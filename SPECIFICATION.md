# Specification

A specification captures intent. It defines _what_ a system should achieve and _why_, enabling any competent interpreter—human or AI—to produce a functionally correct implementation.

This is a meta-specification: it specifies how to write specifications, including itself.

## Principles

### 1. Semantic Completeness

A specification captures **sufficient semantic information** to enable reconstruction of functionally equivalent implementations, without prescribing unnecessary detail.

**Given** a specification and a competent interpreter  
**When** the interpreter generates an implementation  
**Then** the implementation fulfills all stated requirements  
**And** the implementation may differ in unstated details

### 2. Hierarchical Decomposition

Specifications organize information in layers of abstraction. Each layer provides coherent understanding at its level and references finer detail as needed.

**Given** a reader at any layer  
**When** they need finer detail  
**Then** the specification either provides it or references where to find it

### 3. Intent Precedes Mechanism

Specifications state desired outcomes before constraints on how to achieve them. Mechanisms appear only when they are genuine requirements.

### 4. Formalization Where Ambiguity Is Costly

Natural language provides context; formal structures provide precision. Formalize what must be unambiguous (rules, contracts, thresholds). Use prose for motivation, rationale, and examples.

### 5. Verifiable Success Criteria

Every specification includes criteria that determine whether an implementation is compliant.

**Given** a claimed implementation  
**When** validation is performed  
**Then** compliance is determinable without ambiguity

### 6. Evolvability

Specifications evolve with understanding. When reality diverges from specification, the specification is updated to reflect validated knowledge.

## Anti-Patterns

| Anti-Pattern | Problem |
|--------------|---------|
| Implementation as spec | Prescribes mechanism instead of outcome |
| Untestable requirements | No objective way to verify compliance |
| Hidden assumptions | Relies on unstated conditions |
| Specification by example only | Examples without underlying rule |
| Orphaned details | Elements without traceable purpose |

## Success Criteria

**Given** a specification following these principles  
**When** an interpreter implements it  
**Then** the implementation fulfills stated intent  
**And** compliance is verifiable