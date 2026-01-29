# Manifest

Transform a specification into a representation that satisfies it. The representation may be source code, infrastructure, documentation, media, or any artifact that manifests the specified intent in concrete form.

Inverse of [DISTILL](DISTILL.md). The roundtrip is non-deterministic: manifesting a distilled specification produces an equivalent representation, not necessarily an identical one.

## Requirements

**Input**:
- A specification satisfying [SPECIFICATION.md](SPECIFICATION.md) (required): defines what the representation must achieve
- Implementation guidance (optional): constraints on how to achieve it

**Output**:
- A representation that satisfies all requirements stated in the specification
- Representations may differ in any aspect not constrained by the specification or guidance

**Correctness**: The representation fulfills the specification. Compliance is determined by the specification's own verifiability criteria.

**Constraint preservation**: Implementation guidance restricts the solution space but never contradicts the specification. When guidance conflicts with the specification, the specification takes precedence.

**Maximal freedom**: Absent implementation guidance, the interpreter selects freely from the full space of compliant representations.
