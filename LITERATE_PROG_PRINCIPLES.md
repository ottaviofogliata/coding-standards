# Literate Programming Principles

A compact, language-agnostic canon for modern Literate Programming.

Use this document as a shared style guide for humans and AI agents writing code
in C, C++, Python, Rust, Go, JavaScript, or any other language.

A literate source file should read as a technical argument: first the intention,
then the reasoning, then the code.

Use this block format for each literate section:

```text
# ── Descriptive Title ─────────────────────────────────────────────
# Prose explaining the reasoning behind the logic that follows.
# ──────────────────────────────────────────────────────────────────
```

The title names the responsibility. The prose explains the reasoning, invariant,
or design choice that makes the following code easier to review. When a language
requires a different comment prefix, keep the same title, prose, and separator
shape and replace only the prefix.

## 01. Explain Intent Before Mechanism

The primary audience of a program is a human reader. The compiler, interpreter,
or runtime is the second audience.

State what a block is meant to achieve before asking the reader to infer it from
the implementation. Explain the goal, not the syntax.

```text
# ── Parse User Configuration ──────────────────────────────────────
# Load external configuration into a trusted internal shape before use.
# ──────────────────────────────────────────────────────────────────
```

## 02. Make Structure Follow The Problem

Order sections by the reader's mental model, not merely by compiler order. The
narrative should move from problem, to concepts, to rules, to execution.

If the language forces a different order, use section titles to restore meaning.

```text
Problem
Domain Model
Validation Rules
Core Algorithm
Error Handling
Public API
Tests
```

## 03. Keep Sections Small And Named

A literate section should cover one responsibility. Large sections hide
decisions and make review harder.

Split a block when it contains more than one idea. Prefer several named sections
over one long anonymous region.

```text
# ── Normalize Input ───────────────────────────────────────────────
# Convert raw input into canonical values once, before any rule runs.
# ──────────────────────────────────────────────────────────────────

# ── Apply Business Rule ───────────────────────────────────────────
# Decide the outcome from canonical values only, not raw input.
# ──────────────────────────────────────────────────────────────────
```

## 04. Name Concepts And Decisions Explicitly

Types, structs, classes, enums, constants, and functions are vocabulary. Choose
names that describe the domain, not an implementation accident.

Important trade-offs should be recorded where they are made. A reader should not
need commit history to learn why a non-obvious design exists.

```text
Bad:  process(data)
Good: classify_support_ticket(ticket)

Bad:  int flag
Good: bool production_is_down
```

## 05. Keep Proof Close To The Code

Examples and tests are part of the explanation. Keep small examples near the
logic they clarify, and write tests as behavioral specifications.

Code, rationale, examples, and tests should evolve together. Generated
documentation must not drift from executable behavior.

```text
test_downtime_is_always_critical
test_invalid_input_fails_before_state_mutation
test_empty_result_is_returned_when_no_match_exists
```

## Final Checklist

Before adding a block, ask:

```text
What is this block for?
Why is it designed this way?
What protects the behavior?
```

A literate program is not verbose code. It is code whose structure, names,
examples, tests, and local prose make the reasoning legible.
