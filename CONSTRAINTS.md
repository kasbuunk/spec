# Constraints

Bounds on representation.

A constraints instance specifies properties the output must exhibit. It restricts the solution space without prescribing how to search it.

**Domain-agnostic**: Constraints inherit structure from their domain. This specification does not prescribe categories.

**Composability**: Constraints combine. Non-orthogonal constraints may tension; resolution surfaces as discussion.

## Principles

### 1. Restrict, Never Contradict

Constraints narrow the solution space defined by the specification. They cannot override specification requirements. Contradiction surfaces for resolution.

### 2. Explicit Property

Each constraint names the property it bounds.

### 3. Domain Language

Constraints speak the vocabulary of their domain.

## Anti-Patterns

- **Implicit constraints**: Unstated assumptions that affect the output
- **Intent as constraint**: Encoding what belongs in the specification
- **Process as constraint**: Encoding how to work, not what to produce
