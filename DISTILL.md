# Distill

Representation → specification. Inverse of [MANIFEST](MANIFEST.md).

The roundtrip manifest(distill(r)) ≈ r is non-deterministic: equivalent, not identical.

**Input**: Artifacts embodying intent; optional context (provenance, authority, recency).

**Output**: A specification satisfying [SPECIFICATION.md](SPECIFICATION.md) that the input would satisfy.

**Correctness**: Manifesting the output produces a representation functionally equivalent to the input.

**Abstraction**: Generalize mechanism to intent. Include implementation details only when they are the requirement.

**Conflict resolution** (decreasing precedence): observable behavior, recent, formal, explicit.

**Incompleteness**: State assumptions explicitly where intent cannot be determined.
