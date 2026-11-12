# Contributing

## Preserve the boundary

Keep this package focused on local content-addressed bytes, artifact references/manifests, and read observations. Do not add network transport, database persistence, workflow authorization, provider operations, or session delivery workers without a separate architecture decision.

Read [CONTEXT.md](CONTEXT.md), [architecture](docs/architecture.md), and relevant ADRs before changing public contracts.

## Security-sensitive changes

Filesystem path, symlink, permissions, staging, atomic publication, and digest-read logic require negative tests and explicit race/TOCTOU review. Do not claim hostile-process isolation. Changes to canonical manifest bytes or contract versions require updated golden vectors and an explicit compatibility decision.

## Required checks

Requirements: Rust 1.85 or newer.

```bash
cargo fmt --all -- --check
cargo test --all-targets --locked
cargo clippy --all-targets --locked -- -D warnings
cargo doc --no-deps --locked
```

CI validates Rust 1.85.0 and stable on Ubuntu 24.04.
