# Converge Skill

Iteratively apply transformations until stable state reached.

## Description

Implements the convergence loop from CONVERGE.md: repeatedly applies /traverse toward a goal until no significant improvement is possible. This is the orchestration layer for iterative refinement processes.

## Usage

```bash
/converge --goal <description> [--state <path>] [--traverse-config <path>] [--threshold <float>] [--max-iterations <int>] [--metric <description>]
```

## Arguments

- `--goal <description>` - Target criteria to achieve (required, e.g., "all tests pass", "spec matches code", "no gaps remain")
- `--state <path>` - Initial state location (default: current working directory)
- `--traverse-config <path>` - Path to TRAVERSE.md (default: ./TRAVERSE.md if exists)
- `--threshold <float>` - Minimum improvement to continue (default: 0.05, meaning 5% progress)
- `--max-iterations <int>` - Safety limit for loop (default: 10)
- `--metric <description>` - How to measure "significant improvement" (default: from goal or traverse config)

## Process

1. **Initialize**
   - Read CONVERGE.md from this repository
   - Read traverse configuration
   - Parse goal criteria and threshold
   - Set iteration counter = 0

2. **Convergence loop**
   ```
   while not terminated:
     1. Assess    - Evaluate current state against goal
     2. Step      - Call /traverse with goal and state
     3. Verify    - Check if goal reached or progress made
     4. Decide    - Continue, terminate, or surface decision
   ```

3. **Assess current state**
   - Measure distance to goal using specified metric
   - Calculate baseline for improvement comparison
   - Check if goal already satisfied (early termination)

4. **Execute step**
   - Invoke /traverse with:
     - `--goal <goal>`
     - `--state <current_state>`
     - `--traverse-config <config>`
   - Receive: new_state, improvement, goal_reached

5. **Verify progress**
   - Calculate improvement: `new_distance - old_distance`
   - Check monotonic progress (should not regress)
   - If regression: apply course correction per traverse config

6. **Termination decision**
   - **Goal reached**: All criteria satisfied → SUCCESS
   - **Stable**: `improvement < threshold` → STABLE
   - **Blocked**: Traverse returned uncertainty → BLOCKED
   - **Max iterations**: Safety limit hit → CAPPED
   - **Regression**: Repeated backsliding → OSCILLATING

7. **Output results**
   - Final state
   - Termination reason
   - Iteration count
   - Progress metrics

## Termination Conditions

Per CONVERGE.md, explicit termination with reason:

- `goal_reached` - All goal criteria satisfied (success)
- `stable` - No step improves state by ≥ threshold (local optimum)
- `blocked` - Unresolvable gap requires external input (needs human)
- `max_iterations` - Safety limit hit (configuration)
- `oscillating` - Steps undoing previous steps (cycle detected)

## Improvement Metrics

How to measure "significant improvement" (context-dependent):

- **Test-based**: `passing_tests / total_tests`
- **Gap-based**: `resolved_gaps / total_gaps`
- **Coverage-based**: `code_coverage_delta`
- **Spec-based**: `requirements_satisfied / total_requirements`
- **Custom**: User-defined metric from goal

## Output Format

Returns JSON with:
```json
{
  "status": "goal_reached|stable|blocked|max_iterations|oscillating",
  "iterations": 7,
  "final_state": "<path or description>",
  "metrics": {
    "initial_distance": 0.8,
    "final_distance": 0.1,
    "total_improvement": 0.7,
    "improvement_per_iteration": [0.3, 0.2, 0.1, 0.05, 0.04, 0.01, 0.0]
  },
  "termination_reason": "<detailed explanation>",
  "unresolved_gaps": ["gap 1 if blocked"]
}
```

## Examples

```bash
# Converge until tests pass
/converge --goal "all tests pass" --threshold 0.1 --metric "test pass rate"

# Converge with custom iteration limit
/converge --goal "code coverage > 85%" --max-iterations 20

# Converge with explicit state
/converge --goal "spec matches implementation" --state ./src/api

# Converge with tight threshold (more iterations)
/converge --goal "no linting errors" --threshold 0.01
```

## Integration

- **Input from**: Goal criteria, initial state, TRAVERSE.md
- **Output to**: Final state + termination report
- **Called by**: /document (during alignment), composite skills (/develop, /troubleshoot)
- **Calls**: /traverse (repeatedly in loop)

## Principles Applied

Per CONVERGE.md:

1. **Local Optimum** - Accepts best reachable state
2. **Monotonic Progress** - Detects and corrects regression
3. **Explicit Termination** - Always declares reason
4. **Proportionate Assessment** - Cost-appropriate verification

## Course Correction

When drift detected:

- **Oscillation**: Detect if step undoes previous → terminate as oscillating
- **Regression**: If state worsens → backtrack or adjust per traverse config
- **Stall**: If `improvement < threshold` for 3 iterations → terminate as stable

## Anti-Patterns to Avoid

- Infinite loops (no termination criteria)
- Oscillation without detection
- Premature termination (stopping before stability)
- Assessment theater (checkpoints without real evaluation)
- Goal drift (changing criteria mid-convergence)
- Implicit termination (no declared reason)

## Advanced Usage

### Nested Convergence

Converge can be used at multiple levels:

```bash
# High-level: converge multiple features
/converge --goal "milestone 1 complete"
  # Internally calls:
  /converge --goal "feature A complete"
  /converge --goal "feature B complete"
```

### Custom Metrics

```bash
# Domain-specific improvement measurement
/converge --goal "API performance optimized" \
  --metric "p95 latency reduction" \
  --threshold 0.05
```
