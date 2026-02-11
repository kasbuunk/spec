# Document Skill

Align specification ↔ representation bidirectionally.

## Description

Implements the DOCUMENT transformation: identifies and resolves gaps between specifications and code to achieve mutual consistency. This is convergence applied specifically to spec/code alignment, combining both distill (code → spec) and manifest (spec → code) as needed.

## Usage

```bash
/document [--specs <paths>] [--code <paths>] [--constraints <path>] [--fix <strategy>] [--traverse <path>]
```

## Arguments

- `--specs <path1,path2,...>` - Specification files to align (default: ./SPECIFICATION.md)
- `--code <path1,path2,...>` - Code paths to align (default: current working directory)
- `--constraints <path>` - Path to CONSTRAINTS.md (default: ./CONSTRAINTS.md)
- `--fix <spec|code|both|ask>` - Resolution strategy (default: ask)
  - `spec`: Update specs to match code (distill)
  - `code`: Update code to match specs (manifest)
  - `both`: Update whichever is incorrect
  - `ask`: Prompt for each gap
- `--traverse <path>` - Path to TRAVERSE.md (default: ./TRAVERSE.md if exists)

## Process

1. **Read inputs**
   - Read DOCUMENT.md from this repository
   - Read CONVERGE.md (document uses convergence internally)
   - Read specification files (--specs)
   - Read code files (--code)
   - Read constraints (--constraints)

2. **Initial assessment**
   - Compare spec claims vs code behavior
   - Identify all gaps (see Gap Types below)
   - Categorize by gap type and severity

3. **Convergence loop**
   Use /converge with goal="no gaps remain":

   ```
   while gaps exist:
     for each gap:
       1. Classify gap type
       2. Determine resolution per strategy
       3. Apply fix (distill or manifest)
       4. Verify gap closed
       5. Check for new gaps introduced
   ```

4. **Gap classification**
   Per DOCUMENT.md, identify:
   - **Factual error**: Spec says X, code does Y
   - **Missing content**: Code exists, not in spec
   - **Orphaned content**: Spec describes non-existent code
   - **Misplaced content**: Content in wrong document
   - **Stale assumption**: Marked uncertainty now resolvable

5. **Resolution strategies**

   | Gap Type | Strategy: spec | Strategy: code | Strategy: both | Strategy: ask |
   |----------|----------------|----------------|----------------|---------------|
   | Factual error | Update spec | Fix code bug | Analyze, fix incorrect one | Prompt user |
   | Missing content | /distill code into spec | N/A (code exists) | /distill into spec | Ask if intentional |
   | Orphaned content | Remove from spec | /manifest from spec | Determine if bug or obsolete | Ask intention |
   | Misplaced content | Move to correct doc | N/A | Move to correct doc | Ask where it belongs |
   | Stale assumption | Update with fact | N/A | Update with fact | Ask for clarification |

6. **Apply fixes**
   - **Distill path**: Extract code behavior into spec using /distill
   - **Manifest path**: Generate/fix code from spec using /manifest
   - **Move path**: Relocate content to correct document type

7. **Verify alignment**
   - Re-check all gaps after each fix
   - Ensure no new gaps introduced
   - Confirm mutual consistency

8. **Termination**
   Per CONVERGE.md:
   - **Aligned**: No gaps remain (success)
   - **Stable**: Known gaps require external input
   - **Blocked**: Fundamental conflict needs human decision

## Gap Types

From DOCUMENT.md:

| Gap | Signal | Resolution |
|-----|--------|------------|
| **Factual error** | Spec says X, representation does Y | Fix spec or flag representation bug |
| **Missing content** | Representation exists, not in spec | /distill into spec |
| **Orphaned content** | Spec describes non-existent representation | Remove or mark as target |
| **Misplaced content** | Content in wrong document type | Relocate per document definitions |
| **Stale assumption** | Marked uncertainty now resolvable | Update with verified fact |

## Output Format

Returns gap report and updates files:

```json
{
  "status": "aligned|stable|blocked",
  "gaps_initial": 12,
  "gaps_resolved": 10,
  "gaps_remaining": 2,
  "iterations": 5,
  "changes": {
    "specs_updated": ["SPECIFICATION.md"],
    "code_updated": ["src/auth.js", "src/api.js"],
    "content_moved": ["API patterns SPECIFICATION→CONSTRAINTS"]
  },
  "unresolved_gaps": [
    {
      "type": "factual_error",
      "location": "SPECIFICATION.md:42",
      "issue": "Spec claims async, code is synchronous",
      "reason": "Requires decision: should code be async or spec corrected?"
    }
  ]
}
```

## Examples

```bash
# Document entire project, prompt for decisions
/document

# Auto-fix: update specs to match code
/document --fix spec

# Auto-fix: update code to match specs
/document --fix code

# Document specific subsystem
/document --specs ./api/SPECIFICATION.md --code ./src/api

# Document with all files
/document --specs "SPECIFICATION.md,CONSTRAINTS.md" --code src,tests
```

## Integration

- **Input from**: SPECIFICATION.md, codebase, CONSTRAINTS.md
- **Output to**: Updated specs and/or code, gap report
- **Called by**: /develop (after implementation), periodic review, pre-release
- **Calls**: /converge (internally), /distill (for missing content), /manifest (for orphaned content)

## Principles Applied

From DOCUMENT.md:

1. **Document Boundaries** - Content belongs to single document type
2. **Representation as Authority** - For current state, code overrides spec claims
3. **Specification as Authority** - For target state, spec defines correctness
4. **Incremental Progress** - Each iteration leaves artifacts more aligned

## Triggers for Documentation

From DOCUMENT.md:

| Trigger | Reason |
|---------|--------|
| After significant implementation changes | Specs may drift from representation |
| Before major releases | Ensure documentation reflects released state |
| After roadmap transitions | Update phase statuses, validate direction |
| Periodic review | Catch accumulated drift |
| New subsystem added | Create specifications for it |

## Document Type Boundaries

Content placement per DOCUMENT.md:

- **SPECIFICATION.md** - What and why (intent, outcomes, contracts)
- **CONSTRAINTS.md** - Bounds on representation (conventions, tools, formats)
- **MANIFEST.md** - How to compile spec into representation (patterns, structures)
- **TRAVERSE.md** - How to work (process, verification, workflow)

If content could move to another document without loss of meaning, it's misplaced.

## Anti-Patterns to Avoid

From DOCUMENT.md:

- Drift tolerance (allowing divergence indefinitely)
- Wholesale rewrites (replacing rather than aligning)
- Verification without method (claiming alignment without checking)
- Duplication as convenience (repeating rather than referencing)
- Implicit document boundaries (placement by convenience, not purpose)

## Advanced Usage

### Pre-Release Documentation

```bash
# Before release, ensure everything aligns
/document --fix ask --traverse ./TRAVERSE.md
```

### Subsystem Documentation

```bash
# Document each subsystem separately
/document --specs ./auth/SPECIFICATION.md --code ./src/auth
/document --specs ./api/SPECIFICATION.md --code ./src/api
```

### Continuous Documentation

```bash
# After each significant change
git diff --name-only HEAD~1 | xargs -I {} /document --code {}
```
