# ADR-0001: Keep content storage separate from authority

- **Status:** Accepted
- **Date:** 2026-11-12

## Context

Artifact metadata contains producer and task-attempt claims. Content hashes can establish exact byte identity but cannot authenticate those claims or establish evidence truth.

## Decision

The library stores bytes, validates declared references/manifests, and emits point-in-time read observations. It does not authenticate producers/readers, authorize access, verify signatures, or decide whether content is trusted evidence.

## Consequences

- Storage can be used by different host applications.
- Hosts must authenticate identities and enforce permissions before reading or accepting producer metadata.
- A valid manifest is not a signed provenance receipt or workflow-completion proof.
