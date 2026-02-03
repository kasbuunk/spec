# Traverse

How manifestation proceeds.

**Input**: Parameters governing execution: risk appetite, incrementality, verification, conflict resolution.

**Output**: Configured interpreter behavior.

**Scope**: Traverse addresses the *how*, not the *what*. Output properties belong in [CONSTRAINTS](CONSTRAINTS.md).

**Method**: The interpreter executes [MANIFEST](MANIFEST.md) to produce a representation from specification and constraints. Traverse governs that execution.

**Interpreter-agnostic**: Applies to any entity executing manifest—human, agent, hybrid, swarm, or organization.

## Principles

### 1. Incrementality

Manifestation may proceed in steps. Traverse specifies granularity: how large each step, when to checkpoint, when to seek confirmation.

### 2. Risk Appetite

How much uncertainty is tolerable before surfacing for review? Traverse specifies the threshold between autonomy and consultation.

### 3. Conflict Surfacing

When constraints tension or specification gaps appear, traverse specifies behavior: ask, assume, flag for later, or halt.

### 4. Verification Cadence

How often to confirm progress aligns with intent. Traverse specifies frequency and depth of validation.

### 5. Reversibility

Can the interpreter backtrack? Traverse specifies whether to checkpoint state, how far to unwind on failure, and when to abandon a path.

## Instance Requirements

A traverse instance must clarify:

- Step size and checkpoint frequency
- When to proceed autonomously vs. surface for decision
- How to handle ambiguity, gaps, and constraint tension
- Verification expectations at each stage
- Conditions for backtracking or abandoning a path

## Anti-Patterns

- **Unstated risk appetite**: Interpreter cannot calibrate caution without knowing stakes
- **Verification without criteria**: Checkpoints that don't specify what to check
- **Conflict avoidance**: Silently resolving tensions that should surface
- **One-size-fits-all**: Applying uniform process to heterogeneous work
