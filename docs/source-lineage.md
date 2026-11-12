# Source Lineage

## Upstream source

- Repository: [nplongx/runalith](https://github.com/nplongx/runalith)
- Original crate: `rust/crates/runalith-artifacts`
- Source revision: [`29c24cfa2b6ead5704d26cf2746bc384f2938b80`](https://github.com/nplongx/runalith/tree/29c24cfa2b6ead5704d26cf2746bc384f2938b80/rust/crates/runalith-artifacts)
- Upstream license: MIT

The original crate directory was clean at the extraction revision.

## Included

- `src/lib.rs`: private content-addressed filesystem store and byte-integrity checks.
- `src/contracts.rs`: artifact/source/lineage/manifest structures, plus session/ACL/delivery DTO schemas.
- `src/read_port.rs`: read request/result/observation contracts and local read adapter.
- Three golden-vector integration tests and their fixture files.

The standalone package is named `content-artifacts` and imports as `content_artifacts`. Its manifest keeps the relevant direct versions/features but removes `rusqlite` and all internal workspace dependencies.

## Excluded

- `src/sessions.rs`: the SQLite session registry, ACL revision store, durable delivery queue, receipts, and expiry observation persistence. This is a separate stateful subsystem and was not needed by artifact storage, contracts, or the read port.
- Any Runalith worker, provider, sandbox, transport, MCP, or trust-service implementation.

## Preserved compatibility identifiers

Wire/schema identifiers such as `application/vnd.runalith.artifact-manifest+json` and `runalith.execution-result` remain unchanged. These strings are data compatibility values; changing them would alter existing consumers.

## Independence statement

The new repository has no workspace path dependency, Git submodule, SQLite dependency, or runtime dependency on Runalith. Source attribution and MIT license terms are preserved.
