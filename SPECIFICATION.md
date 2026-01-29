# Specification

A specification captures intent. It defines _what_ a system should achieve and _why_, enabling any interpreter to produce a functionally correct implementation. This document specifies how to write specifications, including itself.

## Principles

### 1. Semantic Completeness

A specification captures **sufficient semantic information** to enable reconstruction of functionally equivalent implementations.

Implementations that satisfy all stated requirements are correct, regardless of how they differ in unstated aspects.

### 2. Intent Precedes Mechanism

Specifications state desired outcomes. Mechanisms appear only when required.

### 3. Precision Where Ambiguity Is Costly

Natural language provides context; formal structures provide precision. Formalize what must be unambiguous.

### 4. Verifiability

Every requirement has criteria that determine compliance.

## Anti-Patterns

- **Hidden assumptions**: unstated conditions that affect correctness
- **Specification by example only**: examples without the underlying rule
- **Orphaned details**: elements without traceable purpose
