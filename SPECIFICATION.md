# Specification for Creating Effective Specifications

> **Version**: 1.0.0  
> **Status**: Active  
> **Last Updated**: 2026-01-29

## Table of Contents

1. [Intent](#intent)
2. [Scope & Boundaries](#scope--boundaries)
3. [Stakeholders](#stakeholders)
4. [Quality Attributes](#quality-attributes)
5. [Core Principles](#core-principles)
6. [Structure of a Specification](#structure-of-a-specification)
7. [Specification Boundaries & References](#specification-boundaries--references)
8. [Synthesis from Existing Systems](#synthesis-from-existing-systems)
9. [Calibrating Specificity](#calibrating-specificity)
10. [Anti-Patterns](#anti-patterns)
11. [Constraints & Assumptions](#constraints--assumptions)
12. [Evolvability](#evolvability)
13. [Success Criteria](#success-criteria-self-referential)
14. [References](#references)

---

## Intent

This specification defines how to construct specifications that effectively capture system intent and enable reliable transformation between abstraction levels. It applies to any complex system—software, organizational processes, physical systems, or hybrid domains.

**This is a meta-specification**: it specifies how to write specifications, including itself. Therefore, it must satisfy its own criteria.

## Scope & Boundaries

**This specification covers:**
- Core principles for effective specifications
- Required and optional structural elements
- Calibration of detail level
- Success criteria and anti-patterns

**This specification does NOT cover (and should reference separate specs for):**
- Domain-specific specification templates (e.g., API specs, security policies)
- Specification synthesis processes (extracting specs from existing systems)
- Tooling and format standards (Markdown conventions, diagram notations)
- Governance and versioning workflows

**Target length guidance**: A specification should be readable in one sitting by its intended audience. For a meta-spec like this: ~1500-2500 words. For system specs: scale with complexity, but prefer multiple linked specs over monolithic documents.

## Stakeholders

| Stakeholder | Interest | How This Spec Serves Them |
|-------------|----------|---------------------------|
| **Specification Authors** | Need guidance on creating effective specs | Provides principles, structure, and anti-patterns |
| **Implementers** (human & AI) | Need unambiguous, actionable requirements | Emphasizes testable criteria and clarity |
| **Reviewers & Auditors** | Need to validate spec quality | Provides success criteria and checklists |
| **System Architects** | Need to decompose complexity | Covers hierarchical decomposition and boundaries |
| **Domain Experts** | Need specs to capture business intent | Prioritizes "what" and "why" over "how" |

## Quality Attributes

**Priority order** (highest to lowest): Clarity > Completeness > Brevity > Formality

| Attribute | Target | Rationale |
|-----------|--------|-----------|
| **Clarity** | Unambiguous interpretation by target audience | Primary purpose of any specification |
| **Completeness** | All required elements present; no gaps | Enables reliable implementation |
| **Brevity** | Minimal words for maximum understanding | Respects reader attention; reduces maintenance burden |
| **Formality** | Appropriate rigor for cost of ambiguity | Balances precision with accessibility |

## Core Principles

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
- **Domain**: Business rules, workflows, data models
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

| High-cost ambiguity (formalize)     | Low-cost ambiguity (prose)     |
| ----------------------------------- | ------------------------------ |
| Critical business rules → BDD tests | Motivation and context         |
| Data contracts → Schemas            | Design philosophy              |
| State transitions → Diagrams        | Trade-off explanations         |
| Quantitative requirements → Metrics | Examples and metaphors         |

### 5. Verifiable Success Criteria

Every specification includes objective criteria to validate implementations.

**Given** a claimed implementation  
**When** validation is performed  
**Then** compliance can be assessed deterministically  
**And** pass/fail is unambiguous to any qualified reviewer

**Criteria types:**
- **Functional**: Does it do what the spec says? (BDD tests)
- **Non-functional**: Does it meet quality thresholds? (Metrics)
- **Structural**: Does it follow required patterns? (Architecture review)
- **Negative**: Does it avoid prohibited behaviors? (Anti-pattern checks)

## Structure of a Specification

### Required Sections

#### 1. Intent & Scope
- **Purpose**: What problem does this solve? What value does it create?
- **Scope**: What is included and explicitly excluded?
- **Stakeholders**: Who cares and why?
- **References**: What other specs does this depend on or extend?

#### 2. Quality Attributes
Explicit priorities for non-functional characteristics:
- **Performance**: Throughput, latency, resource efficiency
- **Reliability**: Availability, fault tolerance, data integrity
- **Security**: Confidentiality, integrity, authentication, authorization
- **Maintainability**: Evolvability, understandability, testability
- **Usability**: Accessibility, learnability, efficiency

**Specify trade-off order explicitly**: e.g., "Reliability > Performance > Features" prevents ambiguity when trade-offs arise during implementation.

#### 3. Acceptance Criteria
Formal, executable specifications of critical behaviors:

```gherkin
Feature: [Capability being specified]
  
  Scenario: [Specific case]
    Given [initial context/state]
    When [action or event occurs]
    Then [expected outcome]
    And [additional verifiable outcomes]
```

**Coverage guideline**: The 20% of behaviors representing 80% of value and risk. Prioritize edge cases and failure modes over happy paths.

#### 4. Constraints & Assumptions
- Technical, resource, and regulatory constraints
- Explicit assumptions about operating environment
- What happens when assumptions are violated

### Conditional Sections

| Section | Include When |
|---------|--------------|
| Domain Model | System has rich business logic—concepts, relationships, invariants, workflows |
| Architecture & Interfaces | Multiple components interact; need to specify boundaries and protocols |
| State Diagrams | System has complex state transitions or lifecycle management |
| Examples & Scenarios | Abstract rules need concrete illustration for clarity |
| Anti-patterns | Common mistakes are likely; explicitly state what NOT to do |
| Glossary | Domain terminology may confuse cross-functional readers |
| Migration Path | Modernizing existing system; need transition strategy |
| Security Model | System handles sensitive data or untrusted input |

## Specification Boundaries & References

### When to Split vs. Embed

**Split into separate spec when:**
- The concern has independent stakeholders
- It could evolve on a different cadence
- It exceeds ~500 words of detail
- It applies to multiple parent specs

**Embed when:**
- The detail is essential context for understanding the parent
- Splitting would create orphaned fragments
- The audience is the same

### Reference Format

Specifications reference others explicitly with relationship type:

```markdown
**Depends on**: [Authentication Spec](./auth/SPEC.md) — identity requirements
**Implements**: [API Standards v2.1](../standards/api-v2.1.md) — REST conventions
**Extended by**: [Mobile Client Spec](./clients/mobile.md) — platform-specific details
**Supersedes**: [Legacy Auth Spec](./auth/SPEC-v1.md) — deprecated approach
```

### Handling Circular References

Interdependent specs may reference each other. This is acceptable when:
- Each spec owns distinct concerns
- References are to stable interfaces, not implementation details
- The cycle can be broken for initial reading (one spec is "primary")

## Synthesis from Existing Systems

> **Note**: Detailed synthesis methodology—extracting specs from code, docs, and observability data—belongs in a separate `SYNTHESIS.md` specification covering: information categorization, intent extraction from source types, conflict resolution, and validation loops.

**Key principles for synthesis:**

| Principle | Meaning | Example |
|-----------|---------|---------|
| Observable behavior > stated intent | What the system does reveals truth | Logs show retry logic not documented |
| Recent > historical | Unless explicitly maintaining backward compatibility | Current API contract over legacy docs |
| Formal > informal | Tests and schemas over comments and docs | Schema says `required`, comment says "optional" → required |
| Abstract to intent | Extract the "why" from the "how" | "Uses PostgreSQL" → "Requires ACID-compliant relational storage" |
| Document conflicts | Unresolved contradictions become explicit assumptions | Note conflicting sources, require stakeholder resolution |

**Validation checklist for synthesized specs:**
- [ ] Domain expert confirms it captures their understanding
- [ ] Implementer can generate compliant system without guessing
- [ ] Success criteria are clear and measurable
- [ ] All assumptions are explicit
- [ ] Compliance can be verified objectively

## Calibrating Specificity

### Decision Framework

For each element, ask:
1. **Cost of ambiguity?** High → specify precisely. Low → allow flexibility.
2. **Cost of over-specification?** Locks in details that should evolve? Obscures important requirements?
3. **Blast radius of error?** External APIs → exact. Internal details → minimal.

### The Goldilocks Test

| Level | Example | Problem |
|-------|---------|---------|
| Too vague | "The system should be fast" | Not measurable; open to interpretation |
| Too specific | "All responses in 47.3ms at p99 under 1,247 req/s with exactly 3 DB queries" | Over-constrains implementation; brittle |
| Just right | "API responses <50ms at p99 under 2× average traffic" | Measurable, realistic, leaves room for optimization |

## Anti-Patterns

| Anti-Pattern | Example ❌ | Fix ✓ |
|--------------|-----------|-------|
| Implementation as spec | "Use Redis with LRU, 1GB limit" | "Sub-10ms for repeated queries within 5min" |
| Underspecified criticality | "Should be secure and reliable" | "SOC2 compliant, 99.9% availability" + tests |
| Specification by example only | 50 scenarios, no rule | State rule, then 2-3 examples |
| Hidden assumptions | Assumes stable internet | "Assumes continuous connectivity" |
| Untestable requirements | "UI should be intuitive" | "New users complete X in <5min" |
| Orphaned details | Detailed component, no context | Every detail connects to higher intent |
| Context overloading | One spec covers everything | Split by bounded context, reference |
| Missing version control | No history or changelog | Version header + change rationale |
| Stale specification | Spec diverges from reality | Evolution triggers prompt updates |

## Constraints & Assumptions

### Constraints

- **Language**: Written specifications assume readers share a common working language and technical vocabulary level
- **Medium**: This specification assumes plain-text formats (Markdown) for portability and version control compatibility
- **Scope**: One specification per bounded context; cross-cutting concerns require explicit coordination

### Assumptions

| Assumption | Implication if Violated |
|------------|------------------------|
| Readers have domain context | Add glossary and more examples |
| Specifications are version-controlled | Add manual change tracking |
| Stakeholders can be identified | Escalate to define ownership before proceeding |
| Implementation feedback loops exist | Build validation checkpoints into process |

## Evolvability

Specifications are living artifacts, not write-once documents.

**Given** a specification for an evolving system  
**Then** it supports evolution through:
- **Versioning**: Spec version ↔ system version mapping; track what changed and why
- **Modularity**: Component changes don't require full rewrites; bounded contexts limit blast radius
- **Rationale preservation**: Captured "why" prevents future developers from breaking invariants for reasons already considered
- **Assumption tracking**: As assumptions are validated or invalidated, update explicitly; stale assumptions are silent debt

**Evolution triggers**: When reality diverges from specification—through feature changes, discovered edge cases, or shifted requirements—update the spec first, then implement. The spec is the source of truth for intent.

## Success Criteria (Self-Referential)

This meta-specification must satisfy its own criteria. Validation:

### For Specifications Created Using This Guide

**Given** someone following this specification  
**When** they produce a specification  
**Then** it:
1. Enables reliable implementation (by AI or human)
2. Remains maintainable as the system evolves
3. Communicates effectively among stakeholders
4. Provides objective validation criteria
5. Balances precision with appropriate abstraction
6. References other specs rather than duplicating concerns
7. Is readable in one sitting by its intended audience

### For This Meta-Specification Itself

| Criterion | Target | Validation |
|-----------|--------|------------|
| Word count | 1500-2500 | Automated check |
| Readability | Single sitting (~15min) | User feedback |
| Self-consistency | Follows own principles | Manual review |
| Completeness | All required sections present | Checklist |
| Appropriate references | Defers detail to other specs | Structural review |
| No orphaned details | All elements connect to intent | Traceability |

**Current assessment**: This spec references synthesis process as external, defines its own scope and boundaries, uses formalization appropriately (tables, BDD), and prioritizes intent over mechanism.

---

## Usage Notes

This specification practices what it preaches:
- Hierarchy: context → principles → structure → calibration → validation
- BDD criteria throughout
- Intent before mechanism
- Tables for structured comparisons, prose for explanation
- Explicit anti-patterns and self-referential success criteria

**Adapt, don't follow dogmatically.** Different domains require calibration of these principles.

## References

**Related Standards & Frameworks:**
- [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) — Key words for requirement levels (MUST, SHOULD, MAY)
- [Gherkin Syntax](https://cucumber.io/docs/gherkin/) — BDD specification format
- [C4 Model](https://c4model.com/) — Hierarchical architecture documentation
- [Domain-Driven Design](https://www.domainlanguage.com/dml/) — Bounded contexts and ubiquitous language

**Specifications to Create Alongside This One:**
- `SYNTHESIS.md` — Extracting specifications from existing systems
- `TOOLING.md` — Formats, linting, templates, and automation
- `GOVERNANCE.md` — Versioning workflows, approval processes, ownership