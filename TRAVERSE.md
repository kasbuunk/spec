# Traverse

How manifestation proceeds.

A traverse instance configures how an interpreter executes [manifest](MANIFEST.md)(specification, constraints) → representation. It governs the journey, not the destination.

**Interpreter-agnostic**: Human, agent, hybrid, swarm, or organization.

## Principles

### 1. Step Size

How much work before reassessment. Larger steps risk wasted effort on wrong paths. Smaller steps incur overhead. Traverse specifies the granularity appropriate to the uncertainty.

### 2. Direction Assessment

How to determine if progress moves toward the goal. Traverse specifies the method: automated verification, manual inspection, user feedback, formal proof, simulation, or other. The method's cost must be proportionate to the cost of misdirection.

### 3. Course Correction

How to respond when assessment reveals drift. Traverse specifies: adjust and continue, backtrack to checkpoint, abandon path, or surface for decision. Not all paths require full visibility—sometimes trying is cheaper than planning.

### 4. Uncertainty Tolerance

How much ambiguity the interpreter absorbs before surfacing. High tolerance enables autonomy; low tolerance ensures alignment. Traverse specifies the threshold.

### 5. Verification Depth

How rigorously to confirm correctness. Full verification is rarely economical. Traverse specifies the appropriate level: existence checks, sampling, coverage targets, invariant preservation, or proof. The choice reflects the cost of undetected error.

## Instance Requirements

A traverse instance specifies:

- Step granularity
- Assessment method and frequency
- Correction strategy
- Uncertainty threshold
- Verification approach and depth

## Anti-Patterns

- **Uniform rigor**: Applying equal verification to unequal risk
- **Assessment without method**: Checkpoints that don't specify how to evaluate
- **Implicit autonomy**: Unstated expectations on when to proceed vs. consult
- **Verification theater**: Process that appears rigorous but doesn't catch misdirection
