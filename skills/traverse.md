# Traverse Skill

Execute single transformation step with process guidance.

## Description

Performs one step of a transformation (distill, manifest, or custom goal), applying process guidance from TRAVERSE.md. This is the atomic unit of work used by /converge for iterative refinement.

## Usage

```bash
/traverse --goal <description> [--state <path>] [--step-size <size>] [--assessment <method>] [--traverse-config <path>]
```

## Arguments

- `--goal <description>` - What to achieve this step (required, e.g., "implement authentication", "extract API spec")
- `--state <path>` - Current state location (default: current working directory)
- `--step-size <small|medium|large>` - Granularity of work (default: medium)
  - `small`: Single function, single test, minimal change
  - `medium`: Feature component, module, related changes
  - `large`: Full feature, subsystem, major refactor
- `--assessment <method>` - How to measure progress (default: "goal criteria satisfied")
- `--traverse-config <path>` - Path to TRAVERSE.md (default: ./TRAVERSE.md if exists)

## Process

1. **Read configuration**
   - Read TRAVERSE.md from this repository
   - Read project TRAVERSE.md if specified
   - Parse step size, assessment method, uncertainty tolerance

2. **Assess current state**
   - Evaluate state against goal
   - Identify gap between current and target
   - Calculate estimated steps needed

3. **Execute transformation**
   - Perform one step toward goal (constrained by step-size)
   - Apply relevant methodology (distill, manifest, or domain-specific)
   - Document interpreter choices made

4. **Assess progress**
   - Measure progress using specified assessment method
   - Calculate improvement metric (0.0 = no progress, 1.0 = goal reached)
   - Check for drift or regression

5. **Handle uncertainty**
   - If uncertainty exceeds tolerance, surface for decision
   - Mark assumptions made during execution
   - Document course correction if applied

6. **Output results**
   - New state (code, specs, artifacts)
   - Progress metrics (improvement, distance to goal)
   - Decisions made and assumptions

## Step Size Guidance

Calibrate granularity to uncertainty:

- **Small steps** when:
  - High uncertainty about approach
  - Expensive cost of misdirection
  - Frequent verification needed
  - Learning new domain/codebase

- **Large steps** when:
  - Clear path to goal
  - Low risk of misdirection
  - Expensive cost of assessment
  - Well-understood domain

## Assessment Methods

Common assessment approaches:

- `test_pass` - Tests passing vs failing
- `spec_satisfaction` - Requirements met vs outstanding
- `gap_closure` - Number of gaps resolved
- `metric_improvement` - Performance/quality metric delta
- `human_review` - Explicit approval checkpoint

## Output Format

Returns JSON with:
```json
{
  "new_state": "<path or description>",
  "improvement": 0.0-1.0,
  "goal_reached": true|false,
  "decisions": ["decision 1", "decision 2"],
  "assumptions": ["assumption 1"],
  "drift_detected": true|false,
  "course_correction": "none|adjusted|backtracked"
}
```

## Examples

```bash
# Small step with test-based assessment
/traverse --goal "add user login" --step-size small --assessment test_pass

# Medium step with spec satisfaction
/traverse --goal "implement API endpoints" --state ./src/api

# Large step with custom assessment
/traverse --goal "refactor authentication" --step-size large --assessment "code coverage > 80%"

# With explicit traverse config
/traverse --goal "optimize query performance" --traverse-config ./TRAVERSE.md
```

## Integration

- **Input from**: Goal description, current state, TRAVERSE.md
- **Output to**: New state + metrics (consumed by /converge)
- **Called by**: /converge (in loop), composite skills (directly)
- **Calls**: May call /distill or /manifest for transformations

## Termination Conditions

- **Success**: One step completed, progress measured
- **Blocked**: Uncertainty exceeds tolerance, needs input
- **Drift**: Regression detected, course correction needed

## Principles Applied

Per TRAVERSE.md:

1. **Step Size** - Calibrated to uncertainty
2. **Direction Assessment** - Progress measured explicitly
3. **Course Correction** - Drift detected and handled
4. **Uncertainty Tolerance** - Ambiguity surfaced appropriately
5. **Verification Depth** - Rigor reflects error cost

## Anti-Patterns to Avoid

- Uniform rigor (equal verification for unequal risk)
- Assessment without method (unmeasurable progress)
- Implicit autonomy (unclear when to proceed vs consult)
- Verification theater (process without actual error detection)
