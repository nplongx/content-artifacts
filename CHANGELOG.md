# Changelog

## 0.1.0 — 2026-11-12

Initial standalone package release from immutable source snapshot `29c24cfa2b6ead5704d26cf2746bc384f2938b80`.

- Private filesystem-backed content-addressed artifact store.
- Versioned artifact/source/lineage/manifest contracts and canonical manifest serialization.
- Typed artifact read request, observation, and local read-port implementation.
- Cross-language manifest/read-port/provider-lineage golden fixtures.
- Standalone Cargo manifest, docs, MIT license, and CI.

The SQLite session/ACL/delivery persistence implementation was excluded. Session-related DTOs in the shared contracts module remain available as schemas only.
