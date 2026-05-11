# C++ Coding Standard

This document defines the default coding model for safe, secure, clean, maintainable C++.

It is intended for both humans and coding agents.

Also known as the CPP/CXX coding standard for this repository.

## Authority Order

Apply the standards in this order:

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

Conflict rule:

```text
MISRA > CERT > C++ Core Guidelines
```

Default language baseline:

```text
C++17
```

---

## Agent Instructions

When writing, editing, or reviewing C++ code:

```text
1. Prefer the safest design, not the cleverest one.
2. Enforce MISRA first.
3. Enforce CERT security rules second.
4. Use C++ Core Guidelines only where they do not conflict.
5. Avoid undefined, unspecified, and implementation-defined behavior.
6. Make ownership, lifetime, errors, and concurrency explicit.
7. Prefer simple code that can be statically analyzed.
8. Keep design simple, cohesive, and testable.
9. Keep headers self-contained and lightweight.
10. Use patterns only when they reduce complexity.
11. If a rule must be broken, document the deviation.
```

Every generated solution should satisfy:

```text
safe by default
secure at boundaries
deterministic where possible
easy to review
easy to test
easy to analyze
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
C-08: Make ownership explicit.
C-09: Do not use ambiguous owning raw pointers.
C-10: Use RAII for resources.
C-11: Check return values and error states.
C-12: Handle partial reads/writes and failure paths.
C-13: Prevent use-after-free, double-free, and dangling references.
C-14: Protect shared mutable data.
C-15: Document lock ordering.
C-16: Avoid TOCTOU bugs.
C-17: Avoid shell execution by default.
C-18: Do not let exceptions cross forbidden API/ABI boundaries.
C-19: Encode security assumptions in checks, tests, or analysis.
C-20: Apply CERT C when using C APIs or low-level runtime interfaces.
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
G-04: Avoid owning raw pointers.
G-05: Express ownership with types.
G-06: Prefer unique ownership over shared ownership.
G-07: Avoid shared ownership unless lifetime is clear.
G-08: Prefer std::array for fixed-size storage.
G-09: Use std::vector only if dynamic allocation is allowed.
G-10: Use std::span or equivalent for non-owning views when allowed.
G-11: Prefer enum class.
G-12: Prefer constexpr and const over macros.
G-13: Prefer standard algorithms when clear and analyzable.
G-14: Keep interfaces small and explicit.
G-15: Make invalid states unrepresentable where practical.
G-16: Avoid complex inheritance.
G-17: Use override and final intentionally.
G-18: Avoid implicit ownership transfer.
G-19: Avoid surprising side effects.
G-20: Write code that is easy to test and review.
```

---

## 4. Design Layer

Purpose:

```text
Make design simple, cohesive, loosely coupled, and easy to evolve.
```

Use clean-code, OOP, SOLID, and design-pattern principles as design tools, not as rigid goals.

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

## Minimum Baseline

All C++ code must satisfy:

```text
B-01: Use C++17.
B-02: Enable high compiler warnings.
B-03: Treat warnings as errors in CI where feasible.
B-04: Run static analysis in CI.
B-05: Use sanitizers in test builds where possible.
B-06: Avoid undefined behavior.
B-07: Initialize every object.
B-08: Prefer RAII.
B-09: Avoid owning raw pointers.
B-10: Avoid runtime dynamic allocation unless approved.
B-11: Validate all external input.
B-12: Check all errors and return values.
B-13: Make integer conversions explicit and checked.
B-14: Avoid unsafe C APIs unless wrapped.
B-15: Protect shared mutable state.
B-16: Keep design simple, cohesive, and testable.
B-17: Keep files and functions within size limits.
B-18: Keep headers self-contained, minimal, and free of hidden dependencies.
B-19: Document MISRA/CERT/size/design/header deviations.
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
10. Design patterns are used only when they reduce complexity.
11. Files and functions stay within size limits or have a documented reason.
12. Headers are self-contained, minimal, and do not expose implementation details.
13. Deviations are documented.
```

---

## Final Model

```text
Safety/compliance: MISRA C++:2023
Security:          SEI CERT C++
Design quality:    C++ Core Guidelines
Clean design:      SOLID, clean code, and design patterns when useful
File structure:    .hpp, .cpp, .ipp/.inl rules
Language:          C++17
Precedence:        MISRA > CERT > Core Guidelines > Design/file preferences
```
