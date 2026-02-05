# Methodology

How to adopt this specification methodology for a project.

## Required Artifacts

A project following this methodology maintains these instances:

| Document | Purpose | Create When |
|----------|---------|-------------|
| **SPECIFICATION.md** | What the system does and why | Project inception |
| **CONSTRAINTS.md** | Bounds on representation (conventions, patterns, tools) | Project inception |
| **TRAVERSE.md** | How work proceeds (workflow, verification, step size) | When process needs documentation |
| **ROADMAP.md** | Strategic direction, current→target evolution | When evolution is non-trivial |

For large systems, specifications decompose recursively—subsystems have their own SPECIFICATION.md. Constraints and traverse may be shared or per-subsystem.

## Transformations (No Instance Needed)

These are invoked, not instantiated:

| Document | Invocation |
|----------|------------|
| [MANIFEST](MANIFEST.md) | Provide spec + constraints; interpreter produces representation |
| [DISTILL](DISTILL.md) | Provide representations + scope; interpreter produces specification |
| [CONVERGE](CONVERGE.md) | Provide state + goal + traverse; interpreter loops until stable |
| [DOCUMENT](DOCUMENT.md) | Provide specs + representations; interpreter aligns them |

The methodology documents define these transformations. Projects invoke them with their instances.

## Meta-Specifications (Reference Only)

These define validity for the instances above:

| Document | Defines Validity For |
|----------|---------------------|
| [SPECIFICATION](SPECIFICATION.md) | What makes a specification valid |
| [CONSTRAINTS](CONSTRAINTS.md) | What makes constraints valid |
| [TRAVERSE](TRAVERSE.md) | What makes traverse guidance valid |

Projects reference these to ensure their instances are well-formed.

## Minimal Adoption

At minimum, a project needs:

```
project/
├── SPECIFICATION.md   ← What and why
└── CONSTRAINTS.md     ← Bounds on how
```

Add TRAVERSE.md when process guidance matters. Add ROADMAP.md when evolution is strategic.

## Full Adoption

A mature project:

```
project/
├── SPECIFICATION.md        ← Root specification
├── CONSTRAINTS.md          ← Project-wide bounds
├── TRAVERSE.md             ← Development workflow
├── ROADMAP.md              ← Strategic evolution
├── AGENTS.md               ← Agent-specific guidance (optional)
└── subsystem/
    └── SPECIFICATION.md    ← Subsystem specification
```

## Workflow

### Bootstrapping (no specs yet)

When adopting this methodology for an existing system without specifications:

1. Invoke DISTILL on the codebase/artifacts
2. **Declare assumptions** — Mark uncertain extractions explicitly
3. Human verifies assumptions in broader context (domain knowledge, stakeholder intent)
4. Produce initial SPECIFICATION.md and CONSTRAINTS.md

DISTILL is one-way: representation → specification. It extracts, it doesn't align.

### Building (spec → code)

1. Read your SPECIFICATION.md (what to build)
2. Read your CONSTRAINTS.md (how to build it)
3. Follow your TRAVERSE.md (process guidance)
4. Invoke MANIFEST mentally or explicitly

### Evolving (specs exist, code changes)

When specifications exist and evolve with the codebase:

1. Invoke DOCUMENT with specs + representations
2. Follow CONVERGE loop until stable
3. Both spec and code may change
4. **Declare assumptions** — Surface uncertainties for human decision

DOCUMENT is bidirectional: spec ↔ representation. It aligns both directions.

### DISTILL vs DOCUMENT

| Situation | Use | Direction |
|-----------|-----|-----------|
| No specs exist | DISTILL | code → spec (bootstrap) |
| Specs exist, need sync | DOCUMENT | spec ↔ code (align) |
| Adding undocumented feature to spec | DISTILL | code → spec (extract) |
| Verifying spec matches code | DOCUMENT | spec ↔ code (verify) |

**Key distinction**: DISTILL extracts specification from representation. DOCUMENT converges both toward mutual consistency—it may change either.

### Declaring Assumptions

When distilling or documenting, the interpreter encounters ambiguity. Rather than silently resolving it:

1. **Mark assumptions explicitly** — "Assuming X because Y"
2. **Surface for verification** — Human confirms in broader context
3. **Distinguish confidence levels** — Observable behavior vs. inferred intent

Assumptions arise from:
- Underdetermined intent (code does X, unclear if that's required or incidental)
- Missing context (no access to stakeholder knowledge, historical decisions)
- Conflicting signals (code and comments disagree)

## Anti-Patterns

- **Instance everything**: Creating project copies of MANIFEST, DISTILL, CONVERGE, DOCUMENT
- **Skip constraints**: Putting representation patterns in specification
- **Orphan specification**: Specification that drifts from reality without DOCUMENT cycles
- **Implicit methodology**: Following patterns without documenting them
