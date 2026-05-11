# C++ Coding Standard

This document defines the default coding model for safe, secure, clean, maintainable C++.

It is intended for both humans and coding agents.

## Authority Order

Use these standards together:

```text
1. MISRA C++:2023
2. SEI CERT C++
3. C++ Core Guidelines
```

Interpretation:

```text
MISRA defines what C++ is allowed.
CERT defines what C++ is secure.
Core Guidelines define what C++ is clean and maintainable.
```

Design principle:

```text
Safety, security, clarity, and maintainable code design are co-requirements.
Do not trade one away to satisfy another; choose the code design that satisfies all.
```

Default language baseline:

```text
C++17
```

---

## Agent Instructions

When writing, editing, or reviewing C++ code:

```text
1. Prefer architectures and code designs that are safe, secure, clear, and easy to review.
2. Enforce MISRA, CERT, and C++ Core Guidelines as one combined standard to follow.
3. Treat security, safety, clarity, and maintainability as inseparable design goals.
4. Resolve apparent rule conflicts explicitly instead of weakening design quality.
5. Avoid undefined, unspecified, and implementation-defined behavior.
6. Make ownership, lifetime, errors, and concurrency explicit.
7. Prefer simple code that can be statically analyzed.
8. Keep design simple, cohesive, and testable.
9. Keep headers self-contained and lightweight.
10. Use patterns only when they reduce complexity.
11. If a rule must be broken, document the deviation.
```

---

## 1. MISRA Layer

Purpose:

```text
Make C++ deterministic, analyzable, portable, and suitable for high-reliability systems.
```

Use MISRA C++:2023 as the primary rule set.

### MISRA Rules

```text
M-01: Use ISO C++17.
M-02: Do not rely on undefined behavior.
M-03: Avoid unspecified or implementation-defined behavior.
M-04: Initialize every object before use.
M-05: Keep object lifetime obvious and valid.
M-06: Never return pointers/references to local objects.
M-07: Avoid mutable global state.
M-08: Avoid recursion unless approved.
M-09: Avoid runtime dynamic allocation unless approved.
M-10: If allocation is allowed, define ownership, lifetime, and failure behavior.
M-11: Avoid C-style casts.
M-12: Avoid reinterpret_cast unless approved.
M-13: Keep pointer use simple and justified.
M-14: Keep control flow structured.
M-15: Do not use goto, setjmp, or longjmp.
M-16: Minimize macros.
M-17: Prefer constexpr, const, enum class, templates, and inline functions.
M-18: Avoid hidden side effects in expressions.
M-19: Keep functions short and cohesive.
M-20: Document every approved deviation.
```

---

## 2. CERT Layer

Purpose:

```text
Make code resistant to malformed input, misuse, memory corruption, races, and exploits.
```

Apply CERT C++ to all code, especially code that touches:

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
C APIs
```

### CERT Rules

```text
C-01: Validate all external input.
C-02: Treat external data as untrusted.
C-03: Check integer conversions, signs, truncation, and overflow.
C-04: Make overflow impossible or explicitly checked.
C-05: Avoid raw buffers when safer abstractions exist.
C-06: Avoid unsafe C string/memory functions.
C-07: Wrap unsafe C APIs behind reviewed abstractions.
C-08: Enforce ownership and resource-lifetime contracts at boundaries.
C-09: Check return values and error states.
C-10: Handle partial reads/writes and failure paths.
C-11: Prevent use-after-free, double-free, and dangling references.
C-12: Protect shared mutable data.
C-13: Document lock ordering.
C-14: Avoid TOCTOU bugs.
C-15: Avoid shell execution by default.
C-16: Do not let exceptions cross forbidden API/ABI boundaries.
C-17: Encode security assumptions in checks, tests, or analysis.
C-18: Apply CERT C when using C APIs or low-level runtime interfaces.
```

---

## 3. C++ Core Layer

Purpose:

```text
Make code readable, idiomatic, testable, and maintainable.
```

Use these rules only when they do not conflict with MISRA or CERT.

### Core Rules

```text
G-01: Prefer RAII.
G-02: Prefer value semantics.
G-03: Prefer const by default.
G-04: Express ownership with types; do not use owning raw pointers.
G-05: Prefer unique ownership; use shared ownership only when lifetime is clear.
G-06: Prefer std::array for fixed-size storage.
G-07: Use std::vector only if dynamic allocation is allowed.
G-08: Use std::span or equivalent for non-owning views when allowed.
G-09: Prefer enum class.
G-10: Prefer constexpr and const over macros.
G-11: Prefer standard algorithms when clear and analyzable.
G-12: Keep interfaces small and explicit.
G-13: Make invalid states unrepresentable where practical.
G-14: Avoid complex inheritance.
G-15: Use override and final intentionally.
G-16: Avoid implicit ownership transfer.
G-17: Avoid surprising side effects.
G-18: Write code that is easy to test and review.
```

---

## 4. Design Layer

Purpose:

```text
Make design simple, cohesive, loosely coupled, and easy to evolve.
```

Use object-oriented and design-pattern principles as design tools, not as rigid goals.

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
D-11: Prefer dependency injection for external services, I/O, clocks, randomness, and system APIs.
D-12: Separate pure logic from side effects.
D-13: Make modules easy to test without global state.
D-14: Avoid circular dependencies.
D-15: Keep naming precise, domain-oriented, and unsurprising.
D-16: Prefer explicit control flow over hidden magic.
D-17: Avoid premature generalization.
D-18: Optimize only after correctness, safety, and clarity are established.
```

---

## 5. Header and Source File Rules

Purpose:

```text
Keep C++ interfaces explicit, stable, lightweight, and easy to analyze.
```

### File Rules

```text
H-01: Use .hpp for C++ public headers.
H-02: Use .cpp for C++ implementation files.
H-03: Use .ipp or .inl only for template or inline implementation details when needed.
H-04: Every .hpp file must be self-contained and directly includable.
H-05: Every .hpp file must expose only the public interface required by callers.
H-06: Keep implementation details out of public headers.
H-07: Prefer forward declarations over includes when safe and clear.
H-08: Do not rely on transitive includes.
H-09: Do not put using namespace in header files.
H-10: Avoid macros in headers unless required and documented.
H-11: Prefer include guards or #pragma once according to project policy.
H-12: Keep public APIs small, explicit, and stable.
H-13: Avoid defining non-inline objects or functions with external linkage in headers.
H-14: Put template definitions in headers or dedicated .ipp/.inl files.
H-15: Keep ownership and lifetime visible in public interfaces.
H-16: Avoid circular dependencies between headers.
H-17: Prefer one main class/concept per header.
H-18: Match file names to the primary type, module, or concept they contain.
H-19: Include the matching header first in each .cpp file.
H-20: Keep headers lightweight to reduce compile-time coupling.
```

Recommended file roles:

```text
.hpp: public or module-level C++ interface
.cpp: implementation
.ipp/.inl: template or inline implementation included by a header
```

---

## Size Limits

Line limits are soft constraints unless required by the project profile.

Count logical lines of code, excluding blank lines, long comments, and unavoidable boilerplate.

### Size Rules

```text
S-01: Prefer files under 300 logical lines of code.
S-02: Files over 500 logical lines should be split or justified.
S-03: Files over 800 logical lines require an approved deviation.
S-04: Prefer functions under 50 logical lines.
S-05: Functions over 100 logical lines should be split or justified.
S-06: Prefer classes with one clear responsibility.
S-07: Split files by responsibility, not by arbitrary size.
S-08: Do not split code if splitting makes ownership, lifetime, or invariants less clear.
```

Recommended thresholds:

```text
< 300 LOC: ideal
300-500:   acceptable
500-800:   justify
> 800:     approved deviation required
```

---

## Compiler Baseline

For GCC/Clang-like toolchains:

```bash
-std=c++17 \
-Wall \
-Wextra \
-Wpedantic \
-Wconversion \
-Wsign-conversion \
-Wshadow \
-Wnon-virtual-dtor \
-Wold-style-cast \
-Woverloaded-virtual \
-Wnull-dereference \
-Wformat=2
```

---

## Static Analysis Baseline

Recommended `clang-tidy` families:

```text
cppcoreguidelines-*
cert-*
bugprone-*
readability-*
performance-*
modernize-*
clang-analyzer-*
```

Additional tools:

```text
cppcheck
PVS-Studio
Coverity
Polyspace
Parasoft C/C++test
Helix QAC / QA-C++
LDRA
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
Rule: M-09
Reason: Dynamic allocation required during startup.
Risk: Allocation failure.
Mitigation: Allocation only before operational phase; failure checked.
Approval: Tech lead review.
```

---

## Definition of Done

Code is acceptable only when:

```text
1. It compiles cleanly.
2. Static analysis has no unresolved critical findings.
3. Ownership and lifetime are explicit.
4. External input is validated.
5. Errors are handled.
6. Shared state is synchronized.
7. No unsafe API is used without a wrapper.
8. The design is simple, cohesive, and loosely coupled.
9. Classes and functions have clear responsibilities.
10. Files and functions stay within size limits or have a documented reason.
11. Headers are self-contained, minimal, and do not expose implementation details.
12. Deviations are documented.
```
