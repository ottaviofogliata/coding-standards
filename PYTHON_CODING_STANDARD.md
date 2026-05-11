# Python Coding Standard

This document defines the default coding model for safe, secure, clean, maintainable Python.

It is intended for both humans and coding agents.

## Authority Order

Use these standards together:

```text
1. Security and safety requirements
2. PEP 8, PEP 257, and Python typing conventions
3. Project Python Design Rules
```

Interpretation:

```text
Security defines what Python must not expose.
PEP standards define what Python should look like.
Project Design Rules define what Python should be structurally.
```

Design principle:

```text
Safety, security, clarity, and maintainable code design are co-requirements.
Do not trade one away to satisfy another; choose the code design that satisfies all.
```

Default language baseline:

```text
Python 3.12+
```

---

## Agent Instructions

When writing, editing, or reviewing Python code:

```text
1. Prefer designs that are safe, secure, clear, and easy to review.
2. Enforce security, safety, PEP, typing, and project code design rules as one combined standard.
3. Treat security, safety, clarity, and maintainability as inseparable design goals.
4. Resolve apparent rule conflicts explicitly instead of weakening design quality.
5. Make input validation, errors, dependencies, and side effects explicit.
6. Prefer simple code that can be tested and statically analyzed.
7. Keep modules, functions, and classes small and cohesive.
8. Avoid hidden global state.
9. Use patterns only when they reduce complexity.
10. If a rule must be broken, document the deviation.
```

---

## 1. Security Layer

Purpose:

```text
Make Python code resistant to malformed input, unsafe execution, injection, data leakage, and misuse.
```

Apply security rules to all code, especially code that touches:

```text
files
network
IPC
environment
user input
credentials
parsers
plugins
external APIs
databases
subprocesses
deserialization
```

### Security Rules

```text
S-01: Validate all external input.
S-02: Treat external data as untrusted.
S-03: Avoid eval, exec, compile, and dynamic import unless approved.
S-04: Avoid shell=True in subprocess calls.
S-05: Pass subprocess arguments as lists, not shell strings.
S-06: Never log secrets, tokens, passwords, or sensitive data.
S-07: Store secrets in approved secret stores or environment-specific configuration.
S-08: Do not hardcode credentials.
S-09: Use safe parsers and safe deserialization.
S-10: Avoid pickle, marshal, and unsafe YAML loading for untrusted data.
S-11: Use parameterized queries for database access.
S-12: Validate paths before file access.
S-13: Prevent path traversal when handling filenames or archives.
S-14: Set explicit timeouts for network calls.
S-15: Handle partial failures, retries, and cancellation where relevant.
S-16: Avoid broad exception swallowing.
S-17: Keep cryptographic choices in reviewed utility modules.
S-18: Use secure randomness for security-sensitive values.
S-19: Keep dependencies minimal and reviewed.
S-20: Encode security assumptions in checks, tests, or static analysis.
```

---

## 2. Python Style Layer

Purpose:

```text
Make Python code readable, idiomatic, typed, and easy to maintain.
```

Use PEP 8, PEP 257, and Python typing conventions as the primary style model.

### Style Rules

```text
P-01: Use Python 3.12+ unless the project requires another version.
P-02: Follow PEP 8 naming and formatting.
P-03: Use clear, domain-oriented names.
P-04: Prefer explicit code over clever code.
P-05: Prefer small functions with clear inputs and outputs.
P-06: Use docstrings for public modules, classes, and functions.
P-07: Keep comments focused on why, not what.
P-08: Avoid mutable default arguments.
P-09: Prefer pathlib over raw string path manipulation.
P-10: Prefer context managers for resources.
P-11: Prefer comprehensions only when they remain readable.
P-12: Keep imports explicit and organized.
P-13: Write code that is easy to test and review.
```

---

## 3. Python Design Layer

Purpose:

```text
Make Python design simple, cohesive, loosely coupled, and easy to evolve.
```

Use clean-code, SOLID, and design-pattern principles as design tools, not as rigid goals.

### Design Rules

```text
D-01: Prefer simple designs over clever abstractions.
D-02: Keep each function focused on one task.
D-03: Keep each class responsible for one clear concept.
D-04: Prefer composition over inheritance.
D-05: Use inheritance only for stable polymorphic interfaces.
D-06: Avoid deep inheritance hierarchies.
D-07: Keep public interfaces small and explicit.
D-08: Hide implementation details behind clear abstractions.
D-09: Depend on abstractions only when they reduce coupling.
D-10: Do not introduce interfaces, factories, or patterns without a concrete need.
D-11: Apply SOLID principles when they improve clarity and testability.
D-12: Use design patterns as tools, not as goals.
D-13: Prefer dependency injection for external services, I/O, clocks, randomness, and system APIs.
D-14: Separate pure logic from side effects.
D-15: Make modules easy to test without global state.
D-16: Avoid circular dependencies.
D-17: Keep naming precise, domain-oriented, and unsurprising.
D-18: Prefer explicit control flow over hidden magic.
D-19: Avoid premature generalization.
D-20: Optimize only after correctness, safety, and clarity are established.
```

---

## 4. Error Handling

Purpose:

```text
Make failure explicit, local, observable, and testable.
```

### Error Rules

```text
E-01: Raise specific exceptions.
E-02: Do not catch bare Exception unless re-raised or handled explicitly.
E-03: Do not use exceptions for normal control flow.
E-04: Preserve exception context when wrapping errors.
E-05: Validate arguments at module boundaries.
E-06: Fail fast on invalid state.
E-07: Return Result-like objects only when they improve clarity.
E-08: Log errors at the boundary where they are handled.
E-09: Avoid duplicate logging and re-raising.
E-10: Make error states testable.
```

---

## 5. Type Safety

Purpose:

```text
Make contracts visible to humans, agents, linters, and type checkers.
```

### Typing Rules

```text
T-01: Type public functions and methods.
T-02: Type non-trivial internal functions.
T-03: Prefer concrete types when the API requires them.
T-04: Prefer Protocol when structural abstraction is useful.
T-05: Prefer NewType or domain types for security-sensitive identifiers.
T-06: Avoid Any unless justified.
T-07: Avoid cast unless justified.
T-08: Use Optional only when None is a valid state.
T-09: Narrow Optional values before use.
T-10: Use TypedDict, dataclass, or Pydantic-style models for structured records.
T-11: Avoid stringly typed state when enums or domain types are clearer.
T-12: Keep generic types simple.
```

---

## 6. Module and Package Rules

Purpose:

```text
Keep Python modules explicit, stable, lightweight, and easy to analyze.
```

### File Rules

```text
M-01: Use .py for Python source files.
M-02: Every module must have one clear responsibility.
M-03: Keep public APIs small, explicit, and stable.
M-04: Keep implementation details private by convention with leading underscores.
M-05: Do not rely on wildcard imports.
M-06: Do not rely on import-time side effects.
M-07: Avoid circular imports.
M-08: Keep __init__.py lightweight.
M-09: Use __all__ only when the public module surface must be explicit.
M-10: Keep dependency direction clear.
M-11: Separate application code from library code.
M-12: Separate domain logic from I/O and framework glue.
M-13: Keep tests close to the behavior they verify.
M-14: Match file names to the primary module or concept they contain.
M-15: Keep modules lightweight to reduce import-time coupling.
```

Recommended file roles:

```text
.py: source module
__init__.py: package marker and minimal public exports
*_test.py / test_*.py: tests according to project policy
```

---

## 7. Dependency and Environment Rules

Purpose:

```text
Make runtime behavior reproducible, minimal, and reviewable.
```

### Dependency Rules

```text
R-01: Keep dependencies minimal.
R-02: Prefer standard library when it is clear, safe, and sufficient.
R-03: Pin dependency versions according to project policy.
R-04: Separate runtime, development, and test dependencies.
R-05: Do not import optional heavy dependencies at module import time.
R-06: Avoid hidden network or file-system access during import.
R-07: Use virtual environments or project-managed environments.
R-08: Document required environment variables.
R-09: Validate configuration at startup.
R-10: Keep build, lint, test, and type-check commands reproducible.
```

---

## Size Limits

Line limits are soft constraints unless required by the project profile.

Count logical lines of code, excluding blank lines, long comments, and unavoidable boilerplate.

### Size Rules

```text
L-01: Prefer files under 300 logical lines of code.
L-02: Files over 500 logical lines should be split or justified.
L-03: Files over 800 logical lines require an approved deviation.
L-04: Prefer functions under 50 logical lines.
L-05: Functions over 100 logical lines should be split or justified.
L-06: Prefer classes with one clear responsibility.
L-07: Split files by responsibility, not by arbitrary size.
L-08: Do not split code if splitting makes ownership, invariants, or API clarity worse.
```

Recommended thresholds:

```text
< 300 LOC: ideal
300-500:   acceptable
500-800:   justify
> 800:     approved deviation required
```

---

## Tooling Baseline

Recommended formatter and linter:

```text
ruff format
ruff check
```

Recommended type checking:

```text
mypy
pyright
```

Recommended testing:

```text
pytest
coverage
```

Recommended security tools:

```text
bandit
pip-audit
safety
semgrep
```

Recommended `ruff` rule families:

```text
E   pycodestyle errors
F   pyflakes
B   bugbear
I   isort
UP  pyupgrade
SIM simplify
C4  comprehensions
PL  pylint-compatible rules
RUF ruff-specific rules
S   security rules
```

---

## Deviation Format

Any approved rule violation must be documented as:

```text
Rule:
Reason:
Risk:
Mitigation:
Approval:
```

Example:

```text
Rule: S-04
Reason: Shell feature required for a controlled internal maintenance command.
Risk: Command injection.
Mitigation: Constant command template; validated arguments; no user-controlled shell fragments.
Approval: Tech lead review.
```

---

## Definition of Done

Code is acceptable only when:

```text
1. It formats cleanly.
2. Linting has no unresolved critical findings.
3. Type checking passes where configured.
4. Tests pass.
5. External input is validated.
6. Errors are handled explicitly.
7. No unsafe API is used without a wrapper or deviation.
8. Modules are simple, cohesive, and loosely coupled.
9. Public APIs are typed and documented when needed.
10. Dependencies are minimal and reviewed.
11. Files and functions stay within size limits or have a documented reason.
12. Deviations are documented.
```
