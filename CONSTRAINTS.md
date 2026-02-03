# Constraints

Bounds on representation.

**Input**: Domain, medium, or quality requirements that restrict the output space.

**Output**: A set of properties the representation must exhibit.

**Scope**: Constraints address the *what*, not the *how*. Process belongs in [TRAVERSE](TRAVERSE.md).

**Requirement**: Constraints are required input to [MANIFEST](MANIFEST.md). An empty set is valid—explicit absence of restriction.

**Domain-agnostic**: This specification does not prescribe categories. Constraints inherit structure from their domain: software systems demand different properties than legal documents, videos, or physical artifacts.

**Composability**: Constraints combine. Non-orthogonal constraints may tension. Resolution strategy is not prescribed here—it belongs in [TRAVERSE](TRAVERSE.md) or surfaces as discussion.

**Specification tension**: Constraints that contradict the specification are not silent errors. They surface for resolution. The representation cannot satisfy both; something must yield.

## Principles

### 1. Restrict, Never Contradict

Constraints narrow the solution space defined by the specification. They cannot expand it or override specification requirements.

### 2. Explicit Scope

Each constraint states what property it bounds: format, quality, dependency, compatibility, style, or other.

### 3. Domain Inheritance

Constraints speak the language of their domain. A constraint set for software may reference languages, runtimes, paradigms. A constraint set for legislation may reference jurisdictions, precedent, drafting conventions.

## Anti-Patterns

- **Implicit constraints**: Unstated assumptions about the output that affect correctness
- **Specification leakage**: Encoding intent as constraint rather than in the specification
- **Process masquerading**: Constraints on how to work, not what to produce
