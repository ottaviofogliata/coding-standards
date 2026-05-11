# C Coding Standard

This document defines the default coding model for safe, secure, clean, maintainable C.

It is intended for both humans and coding agents.

Also known as the C coding standard for this repository.

## Authority Order

Apply the standards in this order:

```text
1. MISRA C:2025
2. SEI CERT C
3. Project C Design Rules
```

Interpretation:

```text
MISRA defines what C is allowed.
CERT defines what C is secure.
Project Design Rules define what C is clean and maintainable.
```

Conflict rule:

```text
MISRA > CERT > Project Design Rules
```

Default language baseline:

```text
C17
```

---

## Agent Instructions

When writing, editing, or reviewing C code:

```text
1. Prefer the safest design, not the cleverest one.
2. Enforce MISRA first.
3. Enforce CERT security rules second.
4. Use project design rules only where they do not conflict.
5. Avoid undefined, unspecified, and implementation-defined behavior.
6. Make ownership, lifetime, errors, and concurrency explicit.
7. Prefer simple code that can be statically analyzed.
8. Keep APIs small, explicit, and easy to test.
9. Avoid hidden global state.
10. If a rule must be broken, document the deviation.
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
Make C deterministic, analyzable, portable, and suitable for high-reliability systems.
```

Use MISRA C:2025 as the primary rule set, unless the project explicitly requires another MISRA C profile.

### MISRA Rules

```text
M-01: Use ISO C17 unless the project profile requires another C standard.
M-02: Do not rely on undefined behavior.
M-03: Avoid unspecified or implementation-defined behavior.
M-04: Initialize every object before use.
M-05: Keep object lifetime obvious and valid.
M-06: Never return pointers to local objects.
M-07: Avoid mutable global state.
M-08: Avoid recursion unless approved.
M-09: Avoid runtime dynamic allocation unless approved.
M-10: If allocation is allowed, define ownership, lifetime, and failure behavior.
M-11: Avoid casts unless necessary and reviewed.
M-12: Avoid pointer arithmetic unless justified.
M-13: Keep pointer use simple and bounded.
M-14: Keep control flow structured.
M-15: Do not use goto, setjmp, or longjmp.
M-16: Minimize macros.
M-17: Prefer static inline functions, const objects, and enums over macros.
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

Apply CERT C to all code, especially code that touches:

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
system calls
```

### CERT Rules

```text
C-01: Validate all external input.
C-02: Treat external data as untrusted.
C-03: Check integer conversions, signs, truncation, and overflow.
C-04: Make overflow impossible or explicitly checked.
C-05: Avoid raw buffers without explicit size tracking.
C-06: Avoid unsafe string and memory functions.
C-07: Wrap unsafe C/library/system APIs behind reviewed abstractions.
C-08: Make ownership explicit.
C-09: Define who allocates and who frees every resource.
C-10: Pair every init/open/acquire with cleanup/close/release.
C-11: Check return values and error states.
C-12: Handle partial reads, partial writes, invalid state, and failure paths.
C-13: Prevent use-after-free, double-free, and dangling pointers.
C-14: Protect shared mutable data.
C-15: Document lock ordering.
C-16: Avoid TOCTOU bugs.
C-17: Avoid shell execution by default.
C-18: Do not expose unsafe APIs across module boundaries.
C-19: Encode security assumptions in checks, tests, or analysis.
C-20: Avoid weak randomness for security-sensitive code.
```

---

## 3. C Design Layer

Purpose:

```text
Make C code modular, explicit, testable, and easy to evolve.
```

C has no classes, RAII, constructors, destructors, or native ownership types.

Therefore, design must be expressed through:

```text
headers
source files
opaque structs
explicit init/deinit functions
clear ownership contracts
small APIs
error-code protocols
```

### Design Rules

```text
D-01: Prefer simple designs over clever abstractions.
D-02: Keep each function focused on one task.
D-03: Keep each module responsible for one clear concept.
D-04: Use .h files for public API only.
D-05: Keep internal functions static inside .c files.
D-06: Hide implementation details with opaque structs where useful.
D-07: Prefer explicit init/deinit functions for owned resources.
D-08: Name ownership-transfer functions clearly.
D-09: Keep public interfaces small and stable.
D-10: Avoid circular dependencies between modules.
D-11: Separate pure logic from side effects.
D-12: Avoid global mutable state.
D-13: Pass dependencies explicitly through context structs when needed.
D-14: Avoid function pointers unless they reduce coupling or model a stable interface.
D-15: Avoid macro-based polymorphism unless approved.
D-16: Use state machines for complex stateful behavior.
D-17: Keep naming precise, domain-oriented, and unsurprising.
D-18: Prefer explicit control flow over hidden magic.
D-19: Avoid premature generalization.
D-20: Optimize only after correctness, safety, and clarity are established.
```

---

## 4. Memory and Ownership

Purpose:

```text
Make manual resource management explicit and reviewable.
```

### Ownership Rules

```text
O-01: Every pointer must be owning, borrowed, or output-only.
O-02: Document ownership in the function contract.
O-03: The allocator and deallocator must be clear.
O-04: The caller must know whether it owns returned memory.
O-05: The callee must not retain borrowed pointers unless documented.
O-06: Set pointers to NULL after freeing when reuse is possible.
O-07: Do not free memory not owned by the current module.
O-08: Avoid shared ownership.
O-09: Prefer fixed-size or caller-provided buffers when practical.
O-10: If dynamic allocation is allowed, handle allocation failure explicitly.
```

Recommended naming:

```text
create/init/open/acquire: obtains or initializes a resource
destroy/deinit/close/release: releases a resource
borrow/get/view: does not transfer ownership
take/adopt: transfers ownership into the callee
clone/copy/dup: returns a new owned resource
```

---

## 5. Error Handling

Purpose:

```text
Make failure explicit, local, and testable.
```

### Error Rules

```text
E-01: Return explicit status codes for recoverable errors.
E-02: Do not ignore return values.
E-03: Avoid errno as the only error channel across module boundaries.
E-04: Use out-parameters only with clear contracts.
E-05: Validate output pointers before writing through them.
E-06: Clean up all acquired resources on every failure path.
E-07: Prefer one cleanup path when it improves safety and clarity.
E-08: Do not continue after invalid state is detected.
E-09: Use assertions only for programmer errors, not runtime input validation.
E-10: Make error states testable.
```

Suggested pattern:

```c
typedef enum app_status {
    APP_OK = 0,
    APP_ERR_INVALID_ARGUMENT,
    APP_ERR_OVERFLOW,
    APP_ERR_IO,
    APP_ERR_NOMEM
} app_status_t;
```

---

## 6. Header and Source File Rules

Purpose:

```text
Keep C APIs explicit, stable, lightweight, and easy to analyze.
```

### File Rules

```text
H-01: Use .h for C public headers.
H-02: Use .c for C implementation files.
H-03: Every .h file must be self-contained and directly includable.
H-04: Every .h file must expose only the public API required by callers.
H-05: Keep implementation details out of public headers.
H-06: Prefer forward declarations and opaque structs where useful.
H-07: Do not rely on transitive includes.
H-08: Keep internal helpers static in the .c file.
H-09: Avoid public macros unless required and documented.
H-10: Prefer static inline functions over function-like macros when practical.
H-11: Use include guards or #pragma once according to project policy.
H-12: Keep public APIs small, explicit, and stable.
H-13: Avoid defining storage in headers.
H-14: Keep ownership and lifetime visible in public interfaces.
H-15: Avoid circular dependencies between headers.
H-16: Prefer one main module/concept per header.
H-17: Match file names to the primary module or concept they contain.
H-18: Include the matching header first in each .c file.
H-19: Prefix public symbols consistently.
H-20: Keep headers lightweight to reduce compile-time coupling.
```

Recommended file roles:

```text
.h: public or module-level C interface
.c: implementation
```

Example:

```c
#ifndef PROJECT_SENSOR_H
#define PROJECT_SENSOR_H

#include <stddef.h>
#include <stdint.h>

typedef struct sensor sensor_t;

int sensor_init(sensor_t *sensor);
int sensor_read(sensor_t *sensor, int32_t *out_value);
void sensor_deinit(sensor_t *sensor);

#endif
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
S-06: Prefer modules with one clear responsibility.
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

All C code must satisfy:

```text
B-01: Use C17.
B-02: Enable high compiler warnings.
B-03: Treat warnings as errors in CI where feasible.
B-04: Run static analysis in CI.
B-05: Use sanitizers in test builds where possible.
B-06: Avoid undefined behavior.
B-07: Initialize every object.
B-08: Make ownership explicit.
B-09: Avoid ambiguous raw pointers.
B-10: Avoid runtime dynamic allocation unless approved.
B-11: Validate all external input.
B-12: Check all errors and return values.
B-13: Make integer conversions explicit and checked.
B-14: Avoid unsafe APIs unless wrapped.
B-15: Protect shared mutable state.
B-16: Keep modules simple, cohesive, and testable.
B-17: Keep files and functions within size limits.
B-18: Keep headers self-contained, minimal, and free of hidden dependencies.
B-19: Document MISRA/CERT/size/design/header deviations.
```

---

## Compiler Baseline

For GCC/Clang-like toolchains:

```bash
-std=c17 \
-Wall \
-Wextra \
-Wpedantic \
-Wconversion \
-Wsign-conversion \
-Wshadow \
-Wstrict-prototypes \
-Wmissing-prototypes \
-Wold-style-definition \
-Wpointer-arith \
-Wcast-align \
-Wcast-qual \
-Wwrite-strings \
-Wnull-dereference \
-Wformat=2
```

Optional hardening flags, where supported:

```bash
-fstack-protector-strong \
-D_FORTIFY_SOURCE=2
```

---

## Static Analysis Baseline

Recommended `clang-tidy` families where applicable:

```text
cert-*
bugprone-*
readability-*
performance-*
clang-analyzer-*
```

Additional tools:

```text
cppcheck
PVS-Studio
Coverity
Polyspace
Parasoft C/C++test
Helix QAC / QA-C
LDRA
splint
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
8. Modules are simple, cohesive, and loosely coupled.
9. Public headers expose only stable API.
10. Resource acquisition and cleanup are paired.
11. Files and functions stay within size limits or have a documented reason.
12. Headers are self-contained, minimal, and do not expose implementation details.
13. Deviations are documented.
```

---

## Final Model

```text
Safety/compliance: MISRA C:2025
Security:          SEI CERT C
Design quality:    Project C Design Rules
File structure:    .h and .c rules
Language:          C17
Precedence:        MISRA > CERT > Project Design/file preferences
```
