# Manifest

(Specification, constraints) → representation.

**Input**: Specification per [SPECIFICATION.md](SPECIFICATION.md); constraints per [CONSTRAINTS.md](CONSTRAINTS.md).

**Output**: Artifact satisfying the specification within the constraints.

**Correctness**: Per specification's verifiability criteria.

**Freedom**: Unconstrained aspects are interpreter's choice. Constraints restrict, never contradict.

**Inverse**: [DISTILL](DISTILL.md). Roundtrip distill(manifest(s)) ≈ s preserves intent, not form.
