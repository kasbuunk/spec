# Constraints

Bounds on representation.

A constraints instance specifies properties the output must exhibit, in the vocabulary of its domain.

**Composability**: Constraints combine. Conflicts surface for resolution.

## Principles

### 1. Restrict, Never Contradict

Constraints narrow the solution space. They cannot override specification requirements.

### 2. Explicit Property

Each constraint names the property it bounds.

## Anti-Patterns

- **Implicit constraints**: Unstated assumptions that affect the output
- **Intent as constraint**: Encoding what belongs in the specification
- **Process as constraint**: Encoding how to work, not what to produce
