# spec

Principles for writing specifications that capture intent, not implementation.

**Start here**: [METHODOLOGY.md](METHODOLOGY.md) — How to adopt this for your project.

## Contents

- [SPECIFICATION.md](SPECIFICATION.md) — What makes a specification valid
- [MANIFEST.md](MANIFEST.md) — Transform specification → representation
- [DISTILL.md](DISTILL.md) — Extract representation → specification
- [CONSTRAINTS.md](CONSTRAINTS.md) — Bounds on representation
- [TRAVERSE.md](TRAVERSE.md) — How transformation proceeds
- [CONVERGE.md](CONVERGE.md) — Iterative loop toward stability
- [DOCUMENT.md](DOCUMENT.md) — Align specification and representation

## Relationships

```
MANIFEST ←――inverse――→ DISTILL
   ↑                      ↑
   |                      |
   +――― used by ――― DOCUMENT ―― uses ――→ CONVERGE ―― uses ――→ TRAVERSE
```

- **MANIFEST** and **DISTILL** are inverse transformations (stochastic, not exact)
- **DOCUMENT** aligns spec ↔ representation by iteratively invoking MANIFEST or DISTILL
- **CONVERGE** loops until stable, using **TRAVERSE** to guide each step

## Usage

Provide these files as context to an interpreter (AI agent or human), then state your intent.

### Bootstrap (no specs yet)

```
Context: DISTILL.md, METHODOLOGY.md, your codebase
Prompt:  "Distill this codebase into SPECIFICATION.md and CONSTRAINTS.md. Declare assumptions."
```

### Align (specs exist)

```
Context: DOCUMENT.md, CONVERGE.md, TRAVERSE.md, your specs, your codebase
Prompt:  "Document: align specifications with codebase. Surface gaps."
```

### Build (specifications → manifestation)

```
Context: MANIFEST.md, your SPECIFICATION.md, your CONSTRAINTS.md
Prompt:  "Manifest this specification."
```

Do not copy these files into your project. Reference them as methodology.
