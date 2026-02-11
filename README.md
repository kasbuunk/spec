# spec

Principles for writing specifications that capture intent, not implementation.

**Start here**: [METHODOLOGY.md](METHODOLOGY.md) — How to adopt this for your project.

## Contents

### Methodology Documents

- [SPECIFICATION.md](SPECIFICATION.md) — What makes a specification valid
- [MANIFEST.md](MANIFEST.md) — Transform specification → representation
- [DISTILL.md](DISTILL.md) — Extract representation → specification
- [CONSTRAINTS.md](CONSTRAINTS.md) — Bounds on representation
- [TRAVERSE.md](TRAVERSE.md) — How transformation proceeds
- [CONVERGE.md](CONVERGE.md) — Iterative loop toward stability
- [DOCUMENT.md](DOCUMENT.md) — Align specification and representation
- [METHODOLOGY.md](METHODOLOGY.md) — How to adopt this for your project

### Claude Code Skills

- [skills/distill.md](skills/distill.md) — Extract specs from code
- [skills/manifest.md](skills/manifest.md) — Generate code from specs
- [skills/traverse.md](skills/traverse.md) — Execute single step
- [skills/converge.md](skills/converge.md) — Iterative refinement loop
- [skills/document.md](skills/document.md) — Align specs ↔ code

## Relationships

### Methodology Transformations

```
MANIFEST ←――inverse――→ DISTILL
   ↑                      ↑
   |                      |
   +――― used by ――― DOCUMENT ―― uses ――→ CONVERGE ―― uses ――→ TRAVERSE
```

- **MANIFEST** and **DISTILL** are inverse transformations (stochastic, not exact)
- **DOCUMENT** aligns spec ↔ representation by iteratively invoking MANIFEST or DISTILL
- **CONVERGE** loops until stable, using **TRAVERSE** to guide each step

### Skills Architecture

```
┌──────────────────────────────────────────────┐
│  Your Project                                 │
│  ┌────────────────┐    ┌─────────────────┐  │
│  │ SPECIFICATION  │◄──►│  Code (src/)    │  │
│  └────────┬───────┘    └────────┬────────┘  │
│           │                     │            │
└───────────┼─────────────────────┼────────────┘
            │                     │
            ▼                     ▼
┌───────────────────────────────────────────────┐
│  Claude Code Skills (AI Abstraction Layer)    │
│                                                │
│  /distill    Extract intent from code         │
│  /manifest   Generate code from intent        │
│  /document   Align specs ↔ code               │
│  /converge   Iterate until stable             │
│  /traverse   Execute single step              │
│                                                │
│  Future: /develop, /troubleshoot, /refactor   │
└───────────────────────────────────────────────┘
            │
            ▼
┌───────────────────────────────────────────────┐
│  Methodology (this repository)                │
│  SPECIFICATION, MANIFEST, DISTILL,            │
│  CONVERGE, TRAVERSE, DOCUMENT, CONSTRAINTS    │
└───────────────────────────────────────────────┘
```

The skills act as an **AI abstraction layer**: your project expresses intent in specifications, AI skills transform between intent and implementation, methodology documents define the transformation rules.

## Skills

This repository includes Claude Code skills that implement the methodology:

- **[skills/](skills/)** — Executable skills for Claude Code
  - Core: distill, manifest, traverse, converge, document
  - See [skills/README.md](skills/README.md) for architecture and usage

## Adopting This Methodology in Your Project

### Step 1: Install Claude Code Skills

Add skills to your Claude Code configuration:

```bash
# In your project root or ~/.config/claude-code/skills/
# Create a skills reference file
cat > .claude/skills.json <<EOF
{
  "skills": {
    "spec": {
      "source": "https://github.com/kasbuunk/spec",
      "path": "skills",
      "enabled": ["distill", "manifest", "converge", "document", "traverse"]
    }
  }
}
EOF
```

Or manually reference this repository when starting Claude Code:

```bash
claude-code --context https://github.com/kasbuunk/spec/skills
```

### Step 2: Bootstrap Your Project

If you have **existing code** but no specifications:

```bash
# Extract specifications from your codebase
/distill --paths src --output SPECIFICATION.md

# Review the generated spec and marked assumptions
# Edit SPECIFICATION.md to verify intent matches implementation
```

If you're **starting fresh** with specifications:

```bash
# Create your project specifications
# Edit SPECIFICATION.md and CONSTRAINTS.md manually
# Then generate implementation
/manifest --spec SPECIFICATION.md --constraints CONSTRAINTS.md
```

### Step 3: Establish Project Artifacts

Create these files in your project root:

```
your-project/
├── SPECIFICATION.md      # What your system does and why
├── CONSTRAINTS.md        # Bounds on how to build it
├── TRAVERSE.md           # Optional: custom workflow guidance
└── src/                  # Your implementation
```

**SPECIFICATION.md** - Document your system's intent:
```markdown
# Project Specification

## Purpose
[What does this system achieve?]

## Requirements
[What must it do?]

## Verifiability
[How do we know it works?]
```

**CONSTRAINTS.md** - Define your bounds:
```markdown
# Project Constraints

## Language & Framework
- Language: [Python/Go/TypeScript/etc.]
- Framework: [specific framework if any]

## Architecture Patterns
- [List patterns to follow]

## Conventions
- [Naming, structure, etc.]
```

### Step 4: Daily Workflow

Use skills in your development cycle:

```bash
# Before implementing a feature
/distill --paths src/relevant-module --scope "feature context"

# Implement the feature by updating SPECIFICATION.md, then
/manifest --spec SPECIFICATION.md --constraints CONSTRAINTS.md

# After implementation, verify alignment
/document --fix both

# Iterate until tests pass
/converge --goal "all tests pass" --threshold 0.05

# Before committing, ensure specs match code
/document --fix ask
```

### Step 5: Continuous Alignment

Set up triggers for automatic alignment checks:

```bash
# In pre-commit hook (.git/hooks/pre-commit)
#!/bin/bash
claude-code --skill document --args "--specs SPECIFICATION.md --code src --fix ask"
```

Or run periodic reviews:

```bash
# Weekly: align specs with codebase
/document --specs SPECIFICATION.md --code src --fix both

# Before releases: comprehensive alignment
/document --specs "*.md" --code . --fix ask
```

## Usage Patterns

### With Claude Code Skills

```bash
# Bootstrap: extract specs from existing code
/distill --paths src --output SPECIFICATION.md

# Build: generate code from specs
/manifest --spec SPECIFICATION.md --constraints CONSTRAINTS.md

# Align: synchronize specs with code
/document --fix both

# Iterate: refine until stable
/converge --goal "all tests pass" --threshold 0.05
```

### Without Skills (Manual Context)

If not using Claude Code skills, provide methodology files as context:

```
# Bootstrap (no specs yet)
Context: DISTILL.md, METHODOLOGY.md, your codebase
Prompt:  "Distill this codebase into SPECIFICATION.md and CONSTRAINTS.md. Declare assumptions."

# Align (specs exist)
Context: DOCUMENT.md, CONVERGE.md, TRAVERSE.md, your specs, your codebase
Prompt:  "Document: align specifications with codebase. Surface gaps."

# Build (specifications → manifestation)
Context: MANIFEST.md, your SPECIFICATION.md, your CONSTRAINTS.md
Prompt:  "Manifest this specification."
```

## Key Principles

**Do not copy methodology files** (MANIFEST.md, DISTILL.md, etc.) into your project. Reference them as methodology.

**Do create instances** (your SPECIFICATION.md, your CONSTRAINTS.md) that apply the methodology to your specific project.

**Separate concerns**:
- SPECIFICATION.md = what and why (intent)
- CONSTRAINTS.md = bounds on how (conventions, tools, patterns)
- Implementation = the actual code (manifestation of spec within constraints)
