```markdown
# Document

[CONVERGE](CONVERGE.md) applied to specification and representation alignment.

Documentation is a specific application of convergence where the goal is mutual consistency between specifications and representations (code, artifacts, implementations).

**Input**: Existing specifications, existing representations, [CONSTRAINTS](CONSTRAINTS.md).

**Output**: Updated specifications and representations where each accurately reflects the other.

**Goal**: All gap types resolved; specifications and representations mutually consistent.

## Gap Types

| Gap | Signal | Resolution |
|-----|--------|------------|
| **Factual error** | Spec says X, representation does Y | Fix spec or flag representation bug |
| **Missing content** | Representation exists, not in spec | [DISTILL](DISTILL.md) into spec |
| **Orphaned content** | Spec describes non-existent representation | Remove or mark as target |
| **Misplaced content** | Content in wrong document type | Relocate per document definitions |
| **Stale assumption** | Marked uncertainty now resolvable | Update with verified fact |

## Principles

### 1. Document Boundaries

Content belongs to a single document type. Duplication signals misplacement.

Each document type has a purpose:
- **SPECIFICATION** — What and why (intent, outcomes, contracts)
- **CONSTRAINTS** — Bounds on representation (conventions, tools, formats)
- **MANIFEST** — How to compile spec into representation (patterns, structures)
- **TRAVERSE** — How to work (process, verification, workflow)

If content could move to another document type without loss of meaning, it's in the wrong place.

### 2. Representation as Authority for Current State

For current state, observable representation overrides specification claims. The spec may be wrong; the representation is what exists.

### 3. Specification as Authority for Target State

For target state, specification defines correctness. The representation may be incomplete; the spec defines the goal.

### 4. Incremental Progress

Each iteration should leave artifacts more aligned than before. Wholesale rewrites risk introducing new errors.

## Termination

Per [CONVERGE](CONVERGE.md), documentation convergence terminates when:
- **Aligned** — No gaps remain; specifications and representations are mutually consistent
- **Stable** — Known gaps exist but require external input (decisions, access, information)
- **Blocked** — Fundamental conflict that cannot be resolved without human decision

## Triggers

| Trigger | Reason |
|---------|--------|
| After significant implementation changes | Specs may drift from representation |
| Before major releases | Ensure documentation reflects released state |
| After roadmap transitions | Update phase statuses, validate direction |
| Periodic review | Catch accumulated drift |
| New subsystem added | Create specifications for it |

## Anti-Patterns

- **Drift tolerance**: Allowing specification and representation to diverge indefinitely
- **Wholesale rewrites**: Replacing rather than incrementally aligning
- **Verification without method**: Claiming alignment without checking
- **Duplication as convenience**: Repeating content rather than referencing
- **Implicit document boundaries**: Content placed by convenience, not purpose

```
