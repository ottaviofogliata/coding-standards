# C Coding Standard

Default contract for safe, secure, readable, maintainable C. It applies to humans and coding agents.

## Authority Model

Apply these references as one contract, not as a priority ladder:

- MISRA C:2025
- SEI CERT C
- Project C design rules

Safety, security, clarity, and maintainable design are co-requirements. Do not trade one away to satisfy another. If rules appear to conflict, redesign until the code satisfies this contract or record a formally approved, local deviation under the Deviation Process. Code outside this contract does not merge.

Baseline: C17. Project profiles may be stricter, not weaker.

## Mandatory Rules

- C-G01: Generated code must satisfy the same contract as handwritten code.

### Language and Behavior

- C-L01: Use C17 and the project toolchain profile.
- C-L02: Do not rely on undefined behavior. Any unavoidable unspecified or implementation-defined behavior must be documented in the project toolchain profile and covered by tests or static analysis.
- C-L03: Initialize every object before use.
- C-L04: Keep object lifetime obvious and valid.
- C-L05: Never return pointers to local objects.
- C-L06: Do not use recursion, `goto`, `setjmp`, or `longjmp`.
- C-L07: Do not use runtime dynamic allocation.
- C-L08: Casts and pointer arithmetic must be local, bounded, and covered by tests or static analysis.
- C-L09: Minimize macros; use `static inline`, `const`, and enums for typed constants and simple helpers.
- C-L10: Do not hide side effects in expressions.

### Ownership and Interfaces

- C-O01: Every pointer is owning, borrowed, or output-only.
- C-O02: Public APIs document ownership, lifetime, nullability, and buffer sizes.
- C-O03: The allocator and deallocator for owned resources are explicit.
- C-O04: Acquisition and cleanup are paired on every path.
- C-O05: Borrowed pointers are not retained beyond the call.
- C-O06: Do not use shared ownership.
- C-O07: Public headers expose only stable API.
- C-O08: Headers are self-contained and do not rely on transitive includes.
- C-O09: Internal helpers stay `static` in implementation files.
- C-O10: Public symbols use a project prefix.

### Security and Errors

- C-S01: Treat files, network, IPC, environment, user input, parsers, plugins, external APIs, credentials, and system calls as untrusted boundaries.
- C-S02: Validate all external input before use.
- C-S03: Check integer conversion, sign, truncation, and overflow risks.
- C-S04: Raw buffers must carry explicit size tracking.
- C-S05: Do not use unsafe string, memory, library, or system APIs without a reviewed wrapper.
- C-S06: Check every return value and error state.
- C-S07: Handle partial reads, partial writes, invalid state, and cleanup failures.
- C-S08: Protect shared mutable data and document lock ordering.
- C-S09: Prevent TOCTOU bugs and do not execute shells.
- C-S10: Encode security assumptions in checks, tests, or static analysis.

### Design and Size

- C-D01: Each function and module has one clear responsibility.
- C-D02: Keep dependencies explicit; avoid hidden global state.
- C-D03: Separate pure logic from I/O and side effects.
- C-D04: Use opaque structs for private state that crosses module boundaries.
- C-D05: Use state machines for complex stateful behavior.
- C-D06: Do not add abstraction, macro polymorphism, or function-pointer indirection without a concrete need.
- C-D07: Optimize only after correctness, safety, and clarity are established.
- C-D08: Keep files under 300 logical lines.
- C-D09: Keep functions under ~30 logical lines.

## Tooling Gate

- Compile with the project profile plus strict warnings: `-std=c17 -Wall -Wextra -Wpedantic -Wconversion -Wsign-conversion -Wshadow -Wstrict-prototypes -Wmissing-prototypes -Wold-style-definition -Wpointer-arith -Wcast-align -Wcast-qual -Wwrite-strings -Wnull-dereference -Wformat=2`.
- Enable supported hardening flags such as `-fstack-protector-strong` and `_FORTIFY_SOURCE`.
- Run sanitizer builds for memory, undefined-behavior, and thread issues.
- Run static analysis with project-selected MISRA/CERT coverage, at minimum compiler diagnostics plus one analyzer such as `clang-tidy`, `cppcheck`, Coverity, PVS-Studio, Polyspace, Parasoft, Helix QAC, or LDRA.

## Deviation Process

Deviations are exceptional and must be explicit before merge.

Each deviation must record:

1. The rule ID and exact code scope.
2. The technical reason the rule cannot be satisfied.
3. The risk assessment and compensating controls.
4. The tests, static-analysis evidence, or review evidence that bounds the risk.
5. The approver and review or expiry condition.

No deviation may allow undefined behavior, unchecked external input, secret exposure, known exploitable vulnerabilities, or unbounded ownership/lifetime ambiguity.

## Definition of Done

Code is acceptable only when:

1. It builds cleanly with the required warnings.
2. Static analysis has no unresolved critical findings.
3. Tests for normal paths, error paths, and boundary inputs pass.
4. Ownership, lifetime, errors, and boundary validation are explicit.
5. Unsafe APIs, shared state, and dynamic allocation are absent or covered by approved deviations.
6. Deviations, if any, are local, approved, documented, and bounded.
7. Modules, headers, files, and functions stay within this contract.
8. Every mandatory rule passes or has an approved deviation.
