# Specification

A specification captures intent. It defines _what_ a system should achieve and _why_, enabling any competent interpreter—human or AI—to produce a functionally correct implementation.

This is a meta-specification: it specifies how to write specifications, including itself.

## Principles

### 1. Semantic Completeness Over Exhaustive Detail

A specification captures **sufficient semantic information** to enable reconstruction of functionally equivalent implementations, without prescribing unnecessary detail.

**Given** a specification and a competent interpreter (human or AI)  
**When** the interpreter generates an implementation  
**Then** the implementation fulfills all critical requirements  
**And** the implementation may differ in non-critical details from any reference

### 2. Hierarchical Decomposition & Bounded References

Specifications organize information in layers. Each specification owns one bounded context and **references** (not duplicates) others for adjacent concerns.

**Structural layers:**
- **Context**: Boundaries, stakeholders, constraints
- **Architecture**: Components, interactions, quality attributes
- **Domain**: Core rules, workflows, data models
- **Detail**: Implementation constraints (only where necessary)

**Given** a reader at any layer  
**When** they need finer detail  
**Then** the spec either provides it or references another spec that does  
**And** circular references between specs are acceptable when representing genuine interdependencies

### 3. Intent Precedes Mechanism

Specifications prioritize _what_ and _why_ over _how_.

- **What**: Desired outcomes and behaviors
- **Why**: Rationale and constraints
- **How**: Only when mechanisms are truly non-negotiable

### 4. Formalization Where Ambiguity Is Costly

Natural language provides context; formal structures provide precision.

| High-cost ambiguity (formalize) | Low-cost ambiguity (prose) |
| ------------------------------- | -------------------------- |
| Critical rules and invariants   | Motivation and context     |
| Data contracts and interfaces   | Design philosophy          |
| State transitions and flows     | Trade-off explanations     |
| Quantitative requirements       | Examples and metaphors     |

### 5. Verifiable Success Criteria

Every specification MUST include objective criteria to validate implementations.

**Given** a claimed implementation  
**When** validation is performed  
**Then** compliance can be assessed deterministically  
**And** pass/fail is unambiguous to any qualified reviewer

**Criteria types:**
- **Functional**: Does it do what the spec says?
- **Non-functional**: Does it meet quality thresholds?
- **Structural**: Does it follow required patterns?
- **Negative**: Does it avoid prohibited behaviors?

### 6. Evolvability

Specifications are living hypotheses that must be validated against reality.

**Given** an implementation derived from a specification  
**When** the implementation is tested or deployed  
**Then** discrepancies between spec and reality are captured  
**And** the spec is updated to reflect validated understanding

When reality diverges from specification, update the spec first, then implement. The spec is the source of truth for intent.

### Principle Precedence

When principles conflict, resolve in this order:
1. **Verifiable Success Criteria** — without this, nothing else can be validated
2. **Semantic Completeness** — intent must be capturable
3. **Intent Precedes Mechanism** — preserve flexibility
4. **Evolvability** — iterate toward correctness
5. **Hierarchical Decomposition** — manage complexity
6. **Formalization Where Costly** — apply rigor proportionally

## Anti-Patterns

| Anti-Pattern | Problem | Fix |
|--------------|---------|-----|
| Implementation as spec | "Use Redis with LRU" | "Sub-10ms cache for repeated queries" |
| Untestable requirements | "UI should be intuitive" | "New users complete X in <5min" |
| Hidden assumptions | Implicitly assumes conditions | State assumptions explicitly |
| Specification by example only | 50 scenarios, no rule | State the rule, then illustrate |
| Context overloading | One spec covers everything | Split by bounded context |
| Orphaned details | Detail without connection to intent | Every element traces to purpose |

## Success Criteria

This meta-specification must satisfy its own criteria:

**Given** a specification following these principles  
**When** an interpreter (human or AI) implements it  
**Then** the implementation fulfills stated intent  
**And** the spec remains maintainable as the system evolves  
**And** compliance can be verified objectively