# Content Artifacts Language

## Content and identity

**Artifact**
A bounded byte object stored under a content digest. Its bytes are immutable through the store API; metadata claims remain caller-supplied.

**Content address**
An object identity derived from SHA-256 of exact bytes. A digest establishes byte identity, not authorship, correctness, or evidence truth.

**Artifact reference**
A typed record containing the content digest, byte length, media type, optional content-schema pin, and declared producer/task/attempt context.

**Artifact manifest**
A canonical record grouping exact artifact references with producer metadata and explicit parent-artifact references. It is a provenance description, not an authorization decision.

**Producer lineage**
Declared relationships between a producer, workflow, task, attempt, output artifacts, and parent artifacts. A host must authenticate the producer context before constructing trusted lineage.

## Reading and observations

**Read request**
A bounded request for one exact artifact reference.

**Read observation**
A point-in-time record describing the outcome of one read attempt. `NOT_FOUND` does not establish that the artifact never existed; `UNAVAILABLE` does not establish absence; `INTEGRITY_FAILURE` means the requested bytes could not be accepted.

**Authenticated source identity**
A service identity obtained by the host from an authenticated transport. Passing an identity string to the read port does not authenticate it.

## Session DTOs

**Session reference**
A versioned identity and time window for a workflow/task attempt.

**Session ACL**
An explicit list of expiring subject grants. The contract validates structure; only a host with authenticated subject identity may enforce it.

**Delivery receipt**
A record describing an exact message-delivery attempt and outcome. `UNCERTAIN` is not success and must not be automatically replayed.
