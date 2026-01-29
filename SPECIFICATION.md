# Specification

A specification captures intent. It defines _what_ a system should achieve and _why_, enabling any competent interpreter—human or AI—to produce a functionally correct implementation.

This is a meta-specification: it specifies how to write specifications, including itself.

## Principles

### 1. Semantic Completeness

A specification captures **sufficient semantic information** to enable reconstruction of functionally equivalent implementations, without prescribing unnecessary detail.

Implementations that satisfy all stated requirements are correct, regardless of how they differ in unstated aspects.

### 2. Intent Precedes Mechanism

Specifications state desired outcomes. Mechanisms appear only when they are genuine requirements.

### 3. Precision Where Ambiguity Is Costly

Natural language provides context; formal structures provide precision. Formalize what must be unambiguous. Use prose for motivation and rationale.

### 4. Verifiability

Every requirement has criteria that determine compliance. If compliance cannot be assessed, the requirement is incomplete.

## Anti-Patterns

| Anti-Pattern | Problem |
|--------------|---------|
| Hidden assumptions | Relies on unstated conditions |
| Specification by example only | Examples without underlying rule |
| Orphaned details | Elements without traceable purpose |

## Success Criteria

A specification is complete when:
- An interpreter can produce a compliant implementation without requiring clarification
- Compliance is objectively determinable