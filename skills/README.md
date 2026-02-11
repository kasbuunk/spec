# Skills Directory

Claude Code skills implementing the spec methodology.

## Architecture

Skills are organized in two layers:

### Core Skills (Foundation)

Atomic transformations that implement the methodology:

| Skill | Purpose | I/O |
|-------|---------|-----|
| **distill** | code → spec | Extracts specifications from implementations |
| **manifest** | spec → code | Generates implementations from specifications |
| **traverse** | single step | Executes one transformation step with guidance |
| **converge** | iterative loop | Applies transformations until stable |
| **document** | spec ↔ code | Bidirectional alignment of specs and code |

### Composite Skills (Future)

Domain workflows that orchestrate core skills:

- **develop** - Implement features from issues/tickets
- **troubleshoot** - Diagnose and fix problems
- **refactor** - Restructure code while preserving behavior
- **bug_fix** - Fix specific bugs with verification
- **optimize** - Improve performance/efficiency metrics
- **review** - Compare PRs against specifications and issues

## Skill Relationships

```
┌─────────────────────────────────────────────────┐
│  Composite Skills (Future)                      │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐     │
│  │ develop  │  │troubleshoot│ │ refactor │     │
│  └────┬─────┘  └─────┬──────┘ └────┬─────┘     │
│       └──────────────┼─────────────┘            │
└───────────────────────┼──────────────────────────┘
                        │ orchestrates
┌───────────────────────┼──────────────────────────┐
│  Core Skills          ▼                          │
│  ┌────────┐    ┌──────────┐    ┌──────────┐    │
│  │distill │◄───┤ document ├───►│ manifest │    │
│  └───┬────┘    └─────┬────┘    └────┬─────┘    │
│      │               │              │           │
│      │         ┌─────▼────┐         │           │
│      └────────►│ converge │◄────────┘           │
│                └─────┬────┘                      │
│                      │                           │
│                ┌─────▼────┐                      │
│                │ traverse │                      │
│                └──────────┘                      │
└──────────────────────────────────────────────────┘

Legend:
  ─►  calls/uses
  ◄─► bidirectional
```

## Skill Composition

### Direct Calls

Skills invoke other skills using the `Skill` tool:

```bash
# In converge skill implementation:
Skill(skill="traverse", args="--goal 'tests pass' --step-size medium")
```

### State Passing

Skills output structured JSON consumed by subsequent skills:

```json
{
  "status": "stable",
  "new_state": "/path/to/artifacts",
  "metrics": {"improvement": 0.03},
  "decisions": ["choice 1", "choice 2"]
}
```

### Context Inheritance

Skills inherit project context automatically:
- SPECIFICATION.md (if exists)
- CONSTRAINTS.md (if exists)
- TRAVERSE.md (if exists)

## Usage Patterns

### Bootstrap (No Specs)

Extract specifications from existing code:

```bash
/distill --paths src --output SPECIFICATION.md
```

### Build (Specs Exist)

Generate implementation from specs:

```bash
/manifest --spec SPECIFICATION.md --constraints CONSTRAINTS.md
```

### Align (Both Exist)

Synchronize specs with code:

```bash
/document --fix both
```

### Iterate (Refinement)

Converge toward goal:

```bash
/converge --goal "all tests pass" --threshold 0.05
```

## Implementation Notes

### For Core Skills

Each core skill should:
1. Read methodology document (DISTILL.md, MANIFEST.md, etc.)
2. Read project artifacts (specs, code, configs)
3. Execute transformation per methodology
4. Output results in consistent format
5. Declare termination reason explicitly

### For Composite Skills (Future)

Each composite skill should:
1. Parse domain-specific input (issue URL, PR number, etc.)
2. Orchestrate core skills in appropriate sequence
3. Pass state between skills
4. Surface decisions to user
5. Report comprehensive results

## File Conventions

- Skill definitions: `skills/<name>.md`
- Skill naming: lowercase with underscores (e.g., `bug_fix.md`)
- Invocation: `/skill-name` or `/skill_name`

## Development Workflow

### Adding New Core Skills

1. Identify atomic transformation needed
2. Create `skills/<name>.md` with:
   - Description
   - Arguments
   - Process (step-by-step)
   - Integration points
   - Examples
3. Update this README
4. Test with existing skills

### Adding Composite Skills

1. Identify domain workflow
2. Define orchestration sequence
3. Map to core skill calls
4. Create `skills/<name>.md`
5. Document state flow between skills

## Testing Strategy

Skills should be testable at multiple levels:

1. **Unit**: Single skill invocation
   ```bash
   /distill --paths test/fixtures/simple-api
   ```

2. **Integration**: Skill composition
   ```bash
   /document --specs test/fixtures/spec.md --code test/fixtures/src
   ```

3. **End-to-end**: Full workflow
   ```bash
   # Bootstrap → modify → align
   /distill && edit spec && /document
   ```

## Error Handling

All skills follow consistent error conventions:

- **Blocked**: Cannot proceed without input → surface decision to user
- **Invalid input**: Bad arguments → clear error message
- **Partial success**: Some goals met → detailed report of what succeeded/failed
- **Unexpected state**: Assumptions violated → mark and continue or surface

## Future Enhancements

### Phase 2: Composition

- [ ] Skills can call other skills via `Skill` tool
- [ ] State passing protocol defined
- [ ] Context inheritance implemented
- [ ] Error propagation handled

### Phase 3: Domain Workflows

- [ ] `/develop` - Feature implementation from issues
- [ ] `/troubleshoot` - Problem diagnosis and fixing
- [ ] `/refactor` - Code restructuring with verification
- [ ] `/bug_fix` - Bug fixing with test verification
- [ ] `/optimize` - Performance improvement with metrics
- [ ] `/review` - PR review against specs and issues

### Phase 4: Advanced Features

- [ ] Skill parameters validation
- [ ] Progress indicators for long operations
- [ ] Checkpoint/resume for interrupted workflows
- [ ] Parallel skill execution where possible
- [ ] Skill result caching

## References

- Methodology documents: `../*.md` (SPECIFICATION.md, MANIFEST.md, etc.)
- Core transformations: MANIFEST, DISTILL, CONVERGE, DOCUMENT, TRAVERSE
- Meta-specifications: SPECIFICATION, CONSTRAINTS
