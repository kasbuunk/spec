# Manifest Skill

Generate representation from specification (spec → code).

## Description

Takes a SPECIFICATION.md and CONSTRAINTS.md, then produces artifacts (code, configs, tests) that satisfy the specification within the given constraints. This is the inverse of distill—it transforms intent into mechanism.

## Usage

```bash
/manifest --spec <path> [--constraints <path>] [--output <path>] [--traverse <path>]
```

## Arguments

- `--spec <path>` - Path to SPECIFICATION.md (required)
- `--constraints <path>` - Path to CONSTRAINTS.md (default: ./CONSTRAINTS.md)
- `--output <path>` - Where to write artifacts (default: inferred from spec or cwd)
- `--traverse <path>` - Optional TRAVERSE.md for process guidance (default: ./TRAVERSE.md if exists)

## Process

1. **Read inputs**
   - Read MANIFEST.md from this repository
   - Read the specification file (--spec)
   - Read constraints file (--constraints)
   - Read traverse guidance if provided

2. **Parse requirements**
   - Extract what must be achieved (requirements)
   - Extract why it matters (intent)
   - Extract verifiability criteria (how to confirm correctness)
   - Identify unconstrained aspects (interpreter freedom)

3. **Apply constraints**
   - Check for constraint conflicts with specification
   - Surface conflicts for resolution (constraints restrict, never contradict)
   - Apply bounds: language, framework, patterns, conventions

4. **Generate artifacts**
   - Transform intent into mechanism
   - Make interpreter choices for unconstrained aspects
   - Follow traverse guidance for step size and verification
   - Maintain traceability: which code satisfies which requirement

5. **Verify correctness**
   - Check each requirement against its verifiability criteria
   - Run tests if they exist
   - Surface any requirements that cannot be verified

## Interpreter Freedom

For aspects not specified or constrained, the interpreter chooses:
- Variable names (unless naming convention is constrained)
- Code organization (unless structure is constrained)
- Implementation approach (unless algorithm is specified)
- Optimization strategy (unless performance is specified)

Constraints restrict this freedom but never contradict specifications.

## Output

Creates or updates artifacts at the output path:
- Code that satisfies all requirements
- Tests for verifiable criteria
- Configuration files if needed
- Documentation of interpreter choices (where significant)

## Examples

```bash
# Manifest with default constraints
/manifest --spec ./SPECIFICATION.md

# Manifest with custom constraints and output
/manifest --spec ./api/SPECIFICATION.md --constraints ./api/CONSTRAINTS.md --output ./api/src

# Manifest with traverse guidance
/manifest --spec ./SPECIFICATION.md --traverse ./TRAVERSE.md

# Manifest subsystem
/manifest --spec ./auth/SPECIFICATION.md --output ./src/auth
```

## Integration

- **Input from**: SPECIFICATION.md (via /distill or manual), CONSTRAINTS.md
- **Output to**: Code artifacts (can be verified by /document)
- **Called by**: /develop (during implementation), /converge (during iteration)
- **Calls**: /traverse (if iterative generation needed)

## Termination Conditions

- **Success**: All requirements satisfied and verifiable
- **Blocked**: Constraint contradicts specification (requires resolution)
- **Incomplete**: Some requirements cannot be satisfied within constraints

## Correctness Criteria

Per MANIFEST.md:
- Every specification requirement is satisfied
- All constraints are respected
- Verifiability criteria pass
- Unconstrained choices are documented

## Anti-Patterns to Avoid

- Ignoring verifiability criteria
- Allowing constraints to override specification intent
- Making significant interpreter choices without documentation
- Over-implementing (adding features not in spec)
- Under-implementing (missing specified requirements)
