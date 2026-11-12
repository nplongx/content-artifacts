# ADR-0002: Exclude the SQLite session registry from the artifact extraction

- **Status:** Accepted
- **Date:** 2026-11-12

## Context

The upstream crate combines filesystem artifact storage, artifact contracts, a read-only artifact observation port, and a SQLite session/ACL/delivery registry. The registry uses a host-supplied SQLite connection and defines its own schema and transaction behavior.

## Decision

Extract only artifact storage, artifact/lineage/manifest contracts, read-port contracts, and their fixture tests. Retain session/ACL/delivery structures present in `contracts.rs` as data contracts, but do not extract the SQLite `SessionStore` implementation.

## Consequences

- The standalone package has no SQLite dependency or session persistence API.
- Session DTOs can still be serialized/validated alongside artifact payloads.
- A future session store should be evaluated as a separate repository with a generic schema/connection contract and independent concurrency/recovery tests.
