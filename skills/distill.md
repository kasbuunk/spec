# Distill Skill

Extract specification from representation (code → spec).

## Description

Analyzes existing artifacts (code, documentation, configurations) and produces a SPECIFICATION.md that captures the intent behind the implementation. This is the inverse of manifest—it abstracts from mechanism to intent.

## Usage

```bash
/distill [--paths <paths>] [--output <path>] [--scope <focus>] [--no-assumptions]
```

## Arguments

- `--paths <path1,path2,...>` - Comma-separated paths to analyze (default: current working directory)
- `--output <path>` - Where to write SPECIFICATION.md (default: ./SPECIFICATION.md)
- `--scope <description>` - Optional focus area to distill (e.g., "authentication module", "API layer")
- `--no-assumptions` - Skip marking assumptions (default: false, assumptions are marked)

## Process

1. **Read context**
   - Read DISTILL.md from this repository
   - Read SPECIFICATION.md meta-spec for validity criteria
   - Read target paths specified in --paths

2. **Extract intent**
   - Identify what the system does (observable behavior)
   - Infer why it does it (purpose, constraints, requirements)
   - Generalize from mechanism to intent
   - Apply scope filter if provided

3. **Handle uncertainty**
   - When intent is underdetermined, mark assumptions explicitly
   - Format: "**Assumption**: [description] because [observable signals]"
   - Distinguish confidence levels:
     - High: Observable behavior → clear intent
     - Medium: Patterns suggest intent
     - Low: Multiple interpretations possible

4. **Structure output**
   - Follow SPECIFICATION.md format (principles, requirements, verifiability)
   - Avoid implementation details unless required for correctness
   - Include only semantic information needed for reconstruction

5. **Verification**
   - Check: Could this spec be manifested into functionally equivalent code?
   - Check: Are all requirements verifiable?
   - Check: Are assumptions clearly marked?

## Conflict Resolution Priority

When sources conflict, use this priority (per DISTILL.md):
1. Observable behavior (what the code actually does)
2. Recent changes (newer over older)
3. Formal specifications (existing docs)
4. Explicit comments

## Output

Creates or updates SPECIFICATION.md at the specified output path with:
- Semantic completeness (sufficient to reconstruct)
- Intent-focused descriptions (what and why)
- Marked assumptions where intent is unclear
- Verifiability criteria for each requirement

## Examples

```bash
# Distill entire codebase
/distill

# Distill specific modules
/distill --paths src/auth,src/api --scope "authentication and API"

# Distill to specific location
/distill --paths ./services --output ./services/SPECIFICATION.md

# Distill without assumption marking (not recommended)
/distill --no-assumptions
```

## Integration

- **Input from**: Codebase, existing documentation
- **Output to**: SPECIFICATION.md (can be used by /manifest, /document)
- **Called by**: /document (during alignment), /develop (during bootstrapping)
- **Calls**: None (leaf skill)

## Termination Conditions

- **Success**: SPECIFICATION.md created/updated with all extractable intent
- **Blocked**: Cannot access specified paths
- **Incomplete**: Spec created but contains many assumptions requiring verification

## Anti-Patterns to Avoid

- Copying implementation details into specification
- Specification by example only (examples without underlying rules)
- Silent assumption resolution (always mark uncertain extractions)
- Including mechanism when intent suffices
