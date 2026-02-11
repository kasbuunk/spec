# Skills Manifest

Metadata for all skills in this methodology.

## Core Skills

### /distill

- **Version**: 1.0.0
- **Status**: Ready
- **Type**: Core transformation
- **Dependencies**: None
- **Calls**: None (leaf skill)
- **Called by**: /document, /develop (future)
- **Input**: Code/artifacts
- **Output**: SPECIFICATION.md
- **Idempotent**: No (may produce different specs with new context)

---

### /manifest

- **Version**: 1.0.0
- **Status**: Ready
- **Type**: Core transformation
- **Dependencies**: SPECIFICATION.md, CONSTRAINTS.md (optional)
- **Calls**: None (leaf skill)
- **Called by**: /document, /converge, /develop (future)
- **Input**: SPECIFICATION.md, CONSTRAINTS.md
- **Output**: Code artifacts
- **Idempotent**: No (interpreter choices vary)

---

### /traverse

- **Version**: 1.0.0
- **Status**: Ready
- **Type**: Core execution unit
- **Dependencies**: TRAVERSE.md (optional)
- **Calls**: May call /distill or /manifest
- **Called by**: /converge
- **Input**: Goal, state, traverse config
- **Output**: New state + metrics (JSON)
- **Idempotent**: No (single step may differ)

---

### /converge

- **Version**: 1.0.0
- **Status**: Ready
- **Type**: Core orchestration
- **Dependencies**: CONVERGE.md, TRAVERSE.md (optional)
- **Calls**: /traverse (repeatedly)
- **Called by**: /document, composite skills (future)
- **Input**: Goal, initial state, threshold
- **Output**: Final state + termination report (JSON)
- **Idempotent**: No (convergence path varies)

---

### /document

- **Version**: 1.0.0
- **Status**: Ready
- **Type**: Core alignment
- **Dependencies**: SPECIFICATION.md, codebase, CONSTRAINTS.md (optional)
- **Calls**: /converge, /distill, /manifest
- **Called by**: Periodic reviews, pre-release, post-implementation
- **Input**: Specs, code, constraints
- **Output**: Aligned specs/code + gap report (JSON)
- **Idempotent**: Should converge to same state

---

## Composite Skills (Future)

### /develop

- **Version**: N/A
- **Status**: Planned
- **Type**: Domain workflow
- **Dependencies**: Issue/ticket system
- **Calls**: /distill → /manifest → /converge → /document
- **Input**: Issue URL or description
- **Output**: Implemented feature + tests
- **Workflow**:
  1. Fetch issue description
  2. /distill existing related code
  3. Update/create spec with new requirements
  4. /manifest implementation
  5. /converge until tests pass
  6. /document to verify alignment

---

### /troubleshoot

- **Version**: N/A
- **Status**: Planned
- **Type**: Domain workflow
- **Dependencies**: None
- **Calls**: /distill → /traverse → /converge → /manifest
- **Input**: Problem description, symptoms
- **Output**: Root cause + fix
- **Workflow**:
  1. /distill relevant subsystem
  2. /traverse to investigate symptoms
  3. /converge on root cause identification
  4. /manifest fix
  5. /converge until problem resolved

---

### /refactor

- **Version**: N/A
- **Status**: Planned
- **Type**: Domain workflow
- **Dependencies**: None
- **Calls**: /distill → modify spec → /manifest → /document
- **Input**: Target path, refactor goal
- **Output**: Restructured code + verification
- **Workflow**:
  1. /distill current implementation
  2. Improve spec structure (extract, generalize)
  3. /manifest from improved spec
  4. /document to verify behavior preserved
  5. /converge on test equivalence

---

### /bug_fix

- **Version**: N/A
- **Status**: Planned
- **Type**: Domain workflow
- **Dependencies**: Bug tracker (optional)
- **Calls**: /distill → /traverse → /manifest → /converge
- **Input**: Bug URL/description, reproduction steps
- **Output**: Fix + verification
- **Workflow**:
  1. /distill affected subsystem
  2. /traverse to reproduce bug
  3. Identify spec violation or implementation error
  4. /manifest fix
  5. /converge until tests pass (including reproduction test)

---

### /optimize

- **Version**: N/A
- **Status**: Planned
- **Type**: Domain workflow
- **Dependencies**: Profiling tools
- **Calls**: /distill → update constraints → /manifest → /converge
- **Input**: Target path, performance metric
- **Output**: Optimized code + metric improvement
- **Workflow**:
  1. Baseline: measure current metric
  2. /distill implementation
  3. Profile and identify bottlenecks
  4. Update CONSTRAINTS.md with performance requirements
  5. /manifest optimized version
  6. /converge on metric target

---

### /review

- **Version**: N/A
- **Status**: Planned
- **Type**: Domain workflow
- **Dependencies**: Git, issue tracker
- **Calls**: /distill → compare → report
- **Input**: PR number/URL, issue URL (optional)
- **Output**: Review report with gaps
- **Workflow**:
  1. Fetch PR diff
  2. Fetch related issue if provided
  3. /distill changes into "implicit spec"
  4. Compare with:
     - Existing SPECIFICATION.md
     - Issue description (if provided)
     - CONSTRAINTS.md compliance
  5. Report gaps, violations, unspecified changes

---

## Skill Dependency Graph

```
document ──┬──► converge ──► traverse ──┬──► distill
           │                            └──► manifest
           ├──► distill
           └──► manifest

develop ────┬──► distill
            ├──► manifest
            ├──► converge
            └──► document

troubleshoot ┬──► distill
             ├──► traverse
             ├──► converge
             └──► manifest

refactor ───┬──► distill
            ├──► manifest
            └──► document

bug_fix ────┬──► distill
            ├──► traverse
            ├──► manifest
            └──► converge

optimize ───┬──► distill
            ├──► manifest
            └──► converge

review ─────┬──► distill
            └──► (comparison logic)
```

## Version History

### 1.0.0 (Current)

- Initial release
- Core 5 skills: distill, manifest, traverse, converge, document
- Skill architecture and composition framework defined
- Composite skills planned but not implemented

## Compatibility

- **Claude Code**: v1.3.0+
- **Methodology**: spec v1.0 (this repository)
- **Format**: Markdown-based skill definitions

## Contributing

When adding new skills:

1. Assign appropriate version (use semver)
2. Document all arguments clearly
3. Specify dependencies and relationships
4. Provide usage examples
5. Define termination conditions
6. Update this manifest
7. Update skills/README.md

## License

Skills inherit license from parent repository.
