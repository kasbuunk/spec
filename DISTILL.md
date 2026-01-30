# Distill

Representation → specification.

**Input**: Artifacts embodying intent; optional context (provenance, authority, recency).

**Output**: Specification per [SPECIFICATION.md](SPECIFICATION.md) that the input would satisfy.

**Correctness**: Manifesting the output yields functionally equivalent representation.

**Abstraction**: Generalize mechanism to intent. Include implementation only when required.

**Conflict resolution**: Observable behavior > recent > formal > explicit.

**Incompleteness**: State assumptions where intent is underdetermined.

**Inverse**: [MANIFEST](MANIFEST.md). Roundtrip manifest(distill(r)) ≈ r preserves function, not identity.
