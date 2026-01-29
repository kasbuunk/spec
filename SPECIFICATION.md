# Specification for Creating Effective Specifications

## Intent

This specification defines how to construct specifications that effectively capture system intent and enable reliable transformation between abstraction levels. It applies to any complex system—software, organizational processes, physical systems, or hybrid domains—and provides guidance for synthesizing specifications from heterogeneous source materials.

## Core Principles

### 1. Semantic Completeness Over Exhaustive Detail

A good specification captures **sufficient semantic information** to enable reconstruction of functionally equivalent implementations, without prescribing every detail.

**Given** a specification and a competent interpreter (human or AI)  
**When** the interpreter generates an implementation  
**Then** the implementation fulfills all critical requirements  
**And** the implementation may differ in non-critical details from any reference implementation

### 2. Hierarchical Decomposition

Specifications organize information in layers, each appropriate to its audience and purpose.

**Structural hierarchy:**

- **Context layer**: System boundaries, stakeholders, environmental constraints
- **Architecture layer**: Major components, interaction patterns, quality attributes
- **Domain layer**: Business rules, workflows, data models
- **Component layer**: Individual subsystem specifications (created as needed)
- **Detail layer**: Implementation-specific constraints (only where necessary)

**Given** a reader approaching the specification  
**When** they read from top to bottom  
**Then** they encounter progressively finer detail  
**And** they can stop at any layer and have coherent understanding at that level

### 3. Intent Precedes Mechanism

Specifications prioritize _what_ and _why_ over _how_, allowing implementation flexibility where appropriate.

**Requirements should state:**

- Desired outcomes and behaviors (the "what")
- Rationale and constraints (the "why")
- Non-negotiable mechanisms only when truly required (the "how")

### 4. Formalization Where Ambiguity Is Costly

Natural language prose provides context; formal structures provide precision.

**Use formal elements for:**

- Critical business rules → BDD acceptance tests
- Data contracts → Schemas, type definitions
- State transitions → State diagrams, flow charts
- Quantitative requirements → Metrics with thresholds
- Invariants and constraints → Logical assertions

**Use natural language for:**

- Motivation and context
- Design philosophy
- Trade-off explanations
- Examples and metaphors

### 5. Verifiable Success Criteria

Every specification includes objective criteria to validate implementations.

**Given** a claimed implementation of the specification  
**When** validation is performed  
**Then** there exists a deterministic process to assess compliance  
**And** the criteria distinguish between acceptable and unacceptable implementations

## Structure of a Good Specification

### Required Sections

#### 1. Context & Intent (Always Required)

- **Purpose**: What problem does this system solve? What value does it create?
- **Scope**: What is included and explicitly excluded?
- **Stakeholders**: Who cares about this system and why?
- **Success Definition**: How do we know if this system succeeds?

#### 2. Quality Attributes (Always Required)

Explicit priorities for the system's non-functional characteristics:

- Performance requirements (throughput, latency, resource usage)
- Reliability requirements (availability, fault tolerance, data integrity)
- Security requirements (confidentiality, integrity, authentication, authorization)
- Maintainability requirements (evolvability, understandability, testability)
- Usability requirements (accessibility, learnability, efficiency)

**Specify priorities explicitly**: "Reliability > Performance > Feature richness" prevents ambiguity in trade-offs.

#### 3. Domain Model (Required for Domain-Rich Systems)

- Core concepts and their relationships
- Business rules and invariants
- Workflows and processes
- Data lifecycle and ownership

#### 4. Acceptance Criteria (Always Required)

Formal, executable specifications of critical behaviors:

```gherkin
Given [initial context/state]
When [action or event occurs]
Then [expected outcome]
And [additional expectations]
```

**Coverage guideline**: Specify the 20% of behaviors that represent 80% of the value and risk.

#### 5. Architecture & Interfaces (Required for Multi-Component Systems)

- Major components and their responsibilities
- Interaction patterns and protocols
- Data flow and state management
- Integration points and dependencies

#### 6. Constraints & Assumptions (Always Required)

- Technical constraints (platforms, languages, frameworks—if truly necessary)
- Resource constraints (budget, time, personnel)
- Regulatory/compliance requirements
- Explicit assumptions about the operating environment

### Optional Sections (Include When Valuable)

- **Examples & Scenarios**: Concrete illustrations of abstract requirements
- **Anti-patterns**: Explicitly undesired approaches or solutions
- **Migration Path**: For modernization, how to transition from current to target state
- **Glossary**: Domain-specific terminology
- **References**: Related documents, standards, or prior art

## Specification Synthesis Process

### From Heterogeneous Sources to Coherent Specification

This process transforms unstructured or semi-structured information into a well-formed specification.

#### Phase 1: Information Gathering & Categorization

**Given** source materials (code, docs, meeting notes, logs, tickets, observability data)  
**When** beginning specification synthesis  
**Then** categorize information by type:

- **Intent signals**: User stories, feature requests, problem statements, meeting decisions about "what" and "why"
- **Behavior evidence**: Test cases, user workflows, API usage patterns, integration tests
- **Structural evidence**: Code architecture, module dependencies, database schemas, deployment diagrams
- **Constraint evidence**: Configuration files, infrastructure specs, compliance docs, error budgets
- **Implicit knowledge**: Code comments explaining "why", commit messages describing rationale, design decision records

#### Phase 2: Intent Extraction

**For each source material type:**

**Source Code:**

- Extract business logic patterns (not just syntax)
- Identify invariants enforced through validation
- Discover state machines and workflows
- Map data models and relationships
- Recognize error handling strategies (revealing assumptions about failure modes)

**Documentation:**

- Distinguish aspirational (intended design) from descriptive (actual behavior)
- Extract decision rationale from design docs
- Harvest examples and use cases
- Identify explicit constraints and requirements

**Meeting Minutes & Discussions:**

- Extract decisions and their context
- Identify unresolved questions (→ become explicit assumptions or research items)
- Capture stakeholder priorities and trade-offs
- Note rejected alternatives (anti-patterns)

**Observability Data:**

- Infer actual usage patterns vs. designed usage
- Identify performance characteristics under real load
- Discover implicit SLAs (what reliability users actually experience)
- Detect failure modes and edge cases

**Tickets & Issues:**

- Map bug patterns to missing requirements or test cases
- Extract feature evolution (what was added, why, when)
- Identify recurring pain points (→ quality attribute requirements)

#### Phase 3: Conflict Resolution

**Given** overlapping or contradictory information from multiple sources  
**When** synthesizing the specification  
**Then** apply resolution priorities:

1. **Observable behavior > Stated intent**: What the system actually does reveals truth
2. **Recent > Historical**: Unless explicitly maintaining backward compatibility
3. **Formal > Informal**: Tests and schemas over comments and docs
4. **Explicit > Inferred**: Direct statements over interpretations
5. **Stakeholder hierarchy**: When conflicts remain, defer to appropriate decision-maker

**Document unresolved conflicts explicitly** as assumptions requiring validation.

#### Phase 4: Abstraction & Generalization

**Given** specific implementation details from source materials  
**When** writing the specification  
**Then** abstract to intent:

- **Pattern**: Implementation uses PostgreSQL → **Intent**: "Requires durable, ACID-compliant relational storage"
- **Pattern**: Code implements specific retry logic → **Intent**: "Must gracefully handle transient downstream failures"
- **Pattern**: UI has specific layout → **Intent**: "Must present information with workflow X in context Y"

**Preserve implementation details only when:**

- Changing them would violate external contracts (API compatibility)
- They're mandated by external constraints (regulatory requirements)
- They're fundamental to the value proposition (performance characteristics)

#### Phase 5: Validation & Refinement

**Given** a draft specification  
**When** validating completeness  
**Then** verify against checklist:

- [ ] Can a domain expert read this and confirm it captures their understanding?
- [ ] Can an implementer generate a compliant system without guessing critical details?
- [ ] Are success criteria clear and measurable?
- [ ] Are all assumptions explicit?
- [ ] Are priorities clear when trade-offs arise?
- [ ] Can compliance be verified objectively?

**Refinement loop:**

1. Share specification with stakeholders (original developers, users, domain experts)
2. Collect feedback on gaps, ambiguities, errors
3. Update specification
4. If synthesis was AI-assisted, generate test implementation and validate against acceptance criteria
5. Repeat until convergence

## Calibrating Specificity

### Decision Framework for Detail Level

**For each potential specification element, ask:**

1. **What is the cost of ambiguity here?**
    
    - High cost → Specify precisely (e.g., financial calculations, security policies)
    - Low cost → Allow flexibility (e.g., internal data structure choices)
2. **What is the cost of over-specification?**
    
    - Locks in implementation details that should evolve
    - Creates unnecessarily rigid specifications that are hard to maintain
    - Obscures important requirements in a sea of unimportant detail
3. **What is the blast radius of getting this wrong?**
    
    - Affects external APIs or data formats → Specify exactly
    - Internal implementation detail → Specify minimally

### The Goldilocks Test

**Too vague**: "The system should be fast"  
**Too specific**: "All API responses must complete in 47.3ms at p99 under load of 1,247 requests/second with exactly 3 database queries per request"  
**Just right**: "API responses must complete in <50ms at p99 under expected peak load (defined as 2× average traffic), maintaining this SLA while minimizing database round-trips"

## Anti-Patterns to Avoid

**Given** a specification-in-progress  
**When** reviewing for quality  
**Then** check for and eliminate these anti-patterns:

### 1. Implementation Disguised as Specification

❌ "Use a Redis cache with LRU eviction policy and 1GB memory limit"  
✓ "Maintain sub-10ms response times for repeated queries within a 5-minute window"

### 2. Underspecified Criticality

❌ "The system should be secure and reliable"  
✓ "Must achieve SOC2 compliance" + "Must maintain 99.9% availability" + specific acceptance tests

### 3. Specification by Example Only

❌ Providing 50 example scenarios without extracting the underlying rule  
✓ State the rule explicitly, then provide 2-3 examples

### 4. Hidden Assumptions

❌ Spec assumes users have stable internet without stating it  
✓ "Assumes: Users have continuous network connectivity; graceful degradation not required"

### 5. Untestable Requirements

❌ "The UI should be intuitive"  
✓ "New users must complete workflow X without assistance in <5 minutes (measured via usability testing)"

### 6. Orphaned Details

❌ Including detailed specifications for components without explaining their role in the system  
✓ Every detail connects to higher-level intent through clear hierarchy

## Meta-Principle: Evolvability

Specifications are living artifacts, not write-once documents.

**Given** a specification for an evolving system  
**When** the system changes over time  
**Then** the specification should support evolution by:

- **Versioning**: Track which version of spec corresponds to which system version
- **Modularity**: Changes to one component's spec don't require rewriting the entire document
- **Rationale preservation**: Captured "why" prevents future developers from breaking things for understood reasons
- **Assumption tracking**: As assumptions are validated or invalidated, update explicitly

## Success Criteria for This Meta-Specification

**Given** someone using this specification to create specifications  
**When** they produce a specification following this guidance  
**Then** their specification should:

1. Enable reliable transformation to implementation (via AI or human developers)
2. Be maintainable as the system evolves
3. Serve as effective communication among stakeholders
4. Provide objective validation criteria
5. Balance precision with appropriate abstraction
6. Take less time to create than exhaustive implementation documentation
7. Preserve more valuable knowledge than source code alone

**And** when used to synthesize specs from existing systems  
**Then** the process should:

1. Recover critical intent not obvious from code inspection
2. Identify and document implicit assumptions
3. Generalize appropriately from specific implementation choices
4. Resolve contradictions between sources
5. Produce specs sufficient to enable system modernization or reimplementation

---

## Notes on Using This Specification

This specification intentionally practices what it preaches:

- Structured hierarchy (context → principles → process → validation)
- BDD-style acceptance criteria throughout
- Intent stated before mechanism
- Appropriate use of formalization (lists for structure, prose for explanation)
- Explicit anti-patterns
- Meta-level success criteria

It should be **adapted, not followed dogmatically**. Different domains, different systems, and different organizational contexts will require calibration of these principles.