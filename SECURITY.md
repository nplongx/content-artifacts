# Security Policy

## Scope

This library offers local content-addressed storage and byte-integrity checks. It is not an authorization engine, identity provider, OS sandbox, remote storage service, or independent evidence verifier.

## Filesystem limits

The store rejects common symlink/path escape cases, uses private Unix permissions, bounded writes/reads, exclusive staging files, create-only hard-link publication, and digest verification. The implementation uses path-based checks; a hostile process running under the same user can potentially race filesystem entries between checks and opens. Do not expose the private artifact root to untrusted processes or treat the library as an OS isolation boundary. A stronger adversarial model requires dirfd/openat-style operations, platform-specific review, and race-injection tests.

## Trust limits

Producer IDs, workflow/task/attempt metadata, session identities, ACL subjects, and read-source identities are values supplied by the host. The library cannot prove their authenticity. Authenticate identities independently and authorize before invoking read operations.

## Reporting

Use GitHub private vulnerability reporting for this repository when available. Do not include credentials, private artifact bytes, or production evidence in public issues. Include affected version and a minimal deterministic reproduction.

No production security certification or response SLA is claimed for this initial `0.1.x` extraction.
