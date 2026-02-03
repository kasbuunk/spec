# spec

Principles for writing specifications that capture intent, not implementation.

## Contents

- [SPECIFICATION.md](SPECIFICATION.md) — What makes a specification valid
- [MANIFEST.md](MANIFEST.md) — Transform specification → representation
- [DISTILL.md](DISTILL.md) — Extract representation → specification
- [CONSTRAINTS.md](CONSTRAINTS.md) — Bounds on representation
- [TRAVERSE.md](TRAVERSE.md) — How transformation proceeds

## Usage

These documents are specifications—they define what valid instances look like. They are not instances themselves. To use them, provide instances that satisfy their respective specifications.

An interpreter—human or AI—executes these processes.

### To manifest

Provide to the interpreter:
1. A specification document satisfying [SPECIFICATION.md](SPECIFICATION.md)
2. A constraints document satisfying [CONSTRAINTS.md](CONSTRAINTS.md)
3. A manifest document that specifies how to form a representation that satisfies the specification within the constraints, satisfying [MANIFEST.md](MANIFEST.md)
4. A traverse instance satisfying [TRAVERSE.md](TRAVERSE.md)

The interpreter proceeds as the traverse instance specifies, producing a representation that satisfies the specification instance within the constraints instance.

### Traverse instance examples

- **One-shot**: Produce complete representation without intermediate review. Suitable for well-defined, low-risk, or exploratory work.
- **Iterative delivery**: Incremental progress with regular checkpoints. Verify alignment before proceeding.
- **Continuous verification**: Each step validated before the next. High-assurance contexts.
- **Exploratory**: Move fast, surface learnings, expect rework. Prototype and discovery phases.

### To distill

Provide to the interpreter:
1. [DISTILL.md](DISTILL.md)
2. Representations: source code, documents, recordings, interviews, or other artifacts
3. Context signals (optional): which sources to trust when they conflict
4. A traverse instance satisfying [TRAVERSE.md](TRAVERSE.md)

The interpreter proceeds as the traverse instance specifies, producing a specification instance that captures the intent embodied in the representations.
