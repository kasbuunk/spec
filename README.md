# spec

Principles for writing specifications that capture intent, not implementation.

## Contents

- [SPECIFICATION.md](SPECIFICATION.md) — What makes a specification valid
- [MANIFEST.md](MANIFEST.md) — Transform specification → representation
- [DISTILL.md](DISTILL.md) — Extract representation → specification

## Usage

These documents are specifications for processes. An interpreter—human or AI—executes them.

### To manifest

Provide to the interpreter:
1. [MANIFEST.md](MANIFEST.md)
2. Your specification
3. Implementation guidance (optional)

The interpreter produces a representation that satisfies your specification.

### To distill

Provide to the interpreter:
1. [DISTILL.md](DISTILL.md)
2. Representations: source code, documents, recordings, interviews, or other artifacts
3. Context signals (optional): which sources to trust when they conflict

The interpreter produces a specification that captures the intent embodied in the representations.
