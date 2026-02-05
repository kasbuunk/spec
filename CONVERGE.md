# Converge

Iterative descent toward a stable state.

Convergence applies [TRAVERSE](TRAVERSE.md) in a loop until a stable state is reached—where further steps yield no significant improvement toward the goal.

**Input**: Current state, goal criteria, [TRAVERSE](TRAVERSE.md) configuration.

**Output**: Final state satisfying goal criteria (or explicit acknowledgment of unresolvable gaps).

**Interpreter-agnostic**: Human, agent, hybrid, or organization.

## Loop

```
1. Assess  — Evaluate current state against goal
2. Step    — Apply transformation to move toward goal
3. Verify  — Check if goal is reached or progress was made
4. Repeat  — Continue until stable or blocked
```

## Termination

Convergence terminates when:
- **Goal reached** — All criteria satisfied
- **Stable** — No step improves state
- **Blocked** — Unresolvable gap requires external input

Termination must be explicit. State why convergence stopped.

## Principles

### 1. Local Optimum

Convergence finds the best reachable state, not necessarily global optimum. Accept this limitation or change the goal criteria.

### 2. Monotonic Progress

Each iteration should not regress. If it does, course correct per [TRAVERSE](TRAVERSE.md).

### 3. Explicit Termination

Every convergence ends with a declared reason: success, stable, or blocked. Implicit stopping is an anti-pattern.

### 4. Proportionate Assessment

Assessment cost should be proportionate to step cost. Expensive verification on cheap steps wastes resources; cheap verification on expensive steps risks drift.

## Anti-Patterns

- **Infinite loops**: No termination criteria defined
- **Oscillation**: Steps that undo previous steps without detecting the cycle
- **Premature termination**: Stopping before reaching stability
- **Assessment theater**: Checkpoints without meaningful evaluation
- **Goal drift**: Changing goal criteria mid-convergence without acknowledgment