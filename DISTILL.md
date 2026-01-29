# Distill

Extract the specification that a representation embodies. Given artifacts that manifest intent—source code, products, documents, recordings, interviews, observability data—distill the essential definition of what problem is solved and what value is created.

Inverse of [MANIFEST](MANIFEST.md). The roundtrip is non-deterministic: manifesting a distilled specification produces an equivalent representation, not necessarily an identical one.

## Requirements

**Output** satisfies [SPECIFICATION.md](SPECIFICATION.md).

**Input**:
- One or more representations (required): artifacts that embody intent
- Context signals (optional): metadata about provenance, authority, recency

**Output**:
- A specification that the input representations would satisfy
- The specification captures intent, not mechanism, except where mechanism is the intent

**Correctness**: Manifesting the output specification (with appropriate implementation guidance) would produce a representation functionally equivalent to the input.

**Conflict resolution**: When representations contradict:
- Observable behavior over stated intent
- Recent over historical (unless backward compatibility is explicit)
- Formal over informal
- Explicit over inferred

**Abstraction**: The specification generalizes from specific implementation choices to the intent they serve. Implementation details appear only when they are the requirement (external contracts, regulatory mandates, value-defining characteristics).

**Incompleteness acknowledgment**: Where intent cannot be determined from available artifacts, the specification explicitly states assumptions requiring validation.
