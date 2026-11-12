# Content Artifacts

[![CI](https://github.com/nplongx/content-artifacts/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/nplongx/content-artifacts/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

Private content-addressed artifact storage, immutable artifact manifests, and typed read observations for Rust applications.

> **Status:** `0.1.0`, initial standalone extraction. The API may change before `1.0.0`. The filesystem adapter is defense-in-depth local storage, not a hostile-process sandbox or a production security certification.

## Terminology

See [TERMINOLOGY.md](TERMINOLOGY.md) for shared identifier, lifecycle, evidence, time-unit, and compatibility conventions.

## Capabilities

- Store exact bytes by SHA-256 content address (`sha256:<digest>`).
- Create-only publication using a private directory, unique staging file, hard-link publication, and directory synchronization on Unix.
- Read bytes only after rechecking digest, size, regular-file status, and canonical path containment.
- Validate artifact references and producer/task/attempt metadata.
- Build and persist canonical immutable manifests with explicit parent-artifact lineage.
- Verify all output and parent bytes referenced by a manifest.
- Expose a typed read-only artifact observation port with `AVAILABLE`, `NOT_FOUND`, `UNAVAILABLE`, `INTEGRITY_FAILURE`, and `NOT_AUTHORIZED` dispositions.
- Provide versioned session, ACL, delivery, receipt, and expiry DTOs as contract types only.

## Quick start

```toml
[dependencies]
content-artifacts = { git = "https://github.com/nplongx/content-artifacts", tag = "v0.1.0" }
```

```rust
use content_artifacts::ArtifactStore;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let store = ArtifactStore::open_or_create("./private-artifacts")?;
    let artifact = store.put("text/plain", b"immutable content")?;
    let exact_bytes = store.read(&artifact)?;
    assert_eq!(exact_bytes, b"immutable content");
    Ok(())
}
```

The store caps one object at 32 MiB. On Unix it sets the root/shard directories to `0700` and object files to `0600`.

## Artifact manifests

The `contracts` module defines:

- `ArtifactSourceV1`: host-supplied producer/workflow/task/attempt metadata.
- `ArtifactRefV1`: exact content digest, byte length, media type, schema identity, and source metadata.
- `ArtifactLineageV1`: producer identity and explicit parent artifact references.
- `ArtifactManifestV1`: immutable manifest with deterministic field ordering and canonical JSON bytes.

`put_manifest` validates manifest relationships, reads every referenced object, verifies exact digest/length, then stores the canonical manifest bytes. It does not authenticate the producer or prove that metadata came from a trusted host.

## Read observations

`read_port::ArtifactReadPort` models a local read-only lookup. It binds a request to a host-supplied source-service identity and observation identity/time. `AVAILABLE` includes exact bytes plus a digest-bound observation. Other dispositions do not return bytes.

`NOT_FOUND` is a point-in-time observation, not proof that an artifact was never created. `NOT_AUTHORIZED` can be constructed without touching storage, but the caller must already have made the authentication and authorization decision.

## Boundaries and security

This crate does not:

- authenticate producers, readers, or service identities;
- verify signatures, provision trust roots, or authorize workflow transitions;
- establish truth of evidence or trustworthiness of producer metadata;
- provide a remote network endpoint, database, session registry, or delivery worker;
- provide an OS sandbox against hostile processes running as the same user.

The store rejects common symlink/path escapes and verifies bytes on read. Its path checks are not a guarantee against a hostile same-UID process concurrently replacing filesystem entries between checks and opens. Use a dedicated private directory and do not share it with untrusted processes. Stronger adversarial filesystem isolation requires a separately reviewed dirfd/openat-style implementation and platform-specific testing.

The session-related types in `contracts` describe versioned records only. The upstream SQLite `SessionStore` implementation was deliberately excluded from this artifact-storage extraction.

## Development

Requirements: Rust `1.85` or newer.

```bash
cargo fmt --all -- --check
cargo test --all-targets --locked
cargo clippy --all-targets --locked -- -D warnings
cargo doc --no-deps --locked
```

The current CI matrix tests Rust 1.85.0 and stable on Ubuntu 24.04. Other operating systems/filesystems are not yet validated.

## Documentation

- [Documentation index](docs/README.md)
- [Domain glossary](CONTEXT.md)
- [Architecture and filesystem boundary](docs/architecture.md)
- [Source lineage](docs/source-lineage.md)
- [Architecture decisions](docs/adr/)
- [Contributing](CONTRIBUTING.md)
- [Security policy](SECURITY.md)
- [Changelog](CHANGELOG.md)

## License

MIT. See [LICENSE](LICENSE).

## Compatibility identifiers

The extraction preserves existing schema/media identifiers such as `application/vnd.runalith.artifact-manifest+json` and `runalith.execution-result`. These are serialized compatibility values; repository/package renaming does not silently rename the wire contract.


## Version 1 compatibility identifiers

Existing media types, schema IDs, and golden fixtures retain their historical serialized strings, including identifiers with an older product namespace. They are data-format compatibility values only. Changing them requires a new schema/media version and an explicit migration; they are not package names or runtime dependencies.
