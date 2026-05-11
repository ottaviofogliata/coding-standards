# C++ Coding Standard

Default contract for safe, secure, readable, maintainable C++. It applies to humans and coding agents.

## Authority Model

Apply these references as one contract, not as a priority ladder:

- MISRA C++:2023
- SEI CERT C++
- C++ Core Guidelines
- Project C++ design rules

Safety, security, clarity, and maintainable design are co-requirements. Do not trade one away to satisfy another. If rules appear to conflict, redesign until the code satisfies this contract. Code that cannot satisfy this contract does not merge.

Baseline: C++17. Project profiles may be stricter, not weaker.

## Mandatory Rules

### Language and Behavior

- CPP-L01: Use C++17 and the project toolchain profile.
- CPP-L02: Do not rely on undefined, unspecified, or implementation-defined behavior.
- CPP-L03: Initialize every object before use.
- CPP-L04: Keep object lifetime obvious and valid.
- CPP-L05: Never return pointers or references to local objects.
- CPP-L06: Do not use recursion, `goto`, `setjmp`, or `longjmp`.
- CPP-L07: Do not use runtime dynamic allocation.
- CPP-L08: C-style casts and `reinterpret_cast` are forbidden.
- CPP-L09: Minimize macros; use `constexpr`, `const`, `enum class`, templates, and inline functions for typed constants and simple helpers.
- CPP-L10: Do not hide side effects in expressions.

### Ownership and Interfaces

- CPP-O01: Use RAII for resources.
- CPP-O02: Express ownership with types; owning raw pointers are forbidden.
- CPP-O03: Use unique ownership by default; shared ownership needs explicit lifetime justification.
- CPP-O04: Public APIs document ownership, lifetime, nullability, and exception behavior.
- CPP-O05: Use `std::array` for fixed storage and non-owning view types for non-owning ranges.
- CPP-O06: Do not use containers that allocate at runtime.
- CPP-O07: Public headers expose only stable API.
- CPP-O08: Headers are self-contained and do not rely on transitive includes.
- CPP-O09: Do not put `using namespace` in headers.
- CPP-O10: Template and inline implementation stays in headers or dedicated `.ipp`/`.inl` files.

### Security and Errors

- CPP-S01: Treat files, network, IPC, environment, user input, parsers, plugins, external APIs, credentials, and C APIs as untrusted boundaries.
- CPP-S02: Validate all external input before use.
- CPP-S03: Check integer conversion, sign, truncation, and overflow risks.
- CPP-S04: Do not expose raw buffers across interfaces.
- CPP-S05: Do not use unsafe C, library, or system APIs without a reviewed wrapper.
- CPP-S06: Check every return value and error state.
- CPP-S07: Handle partial reads, partial writes, invalid state, and cleanup failures.
- CPP-S08: Protect shared mutable data and document lock ordering.
- CPP-S09: Prevent TOCTOU bugs and do not execute shells.
- CPP-S10: Exceptions must not cross forbidden API or ABI boundaries.
- CPP-S11: Encode security assumptions in checks, tests, or static analysis.

### Design and Size

- CPP-D01: Each function, class, and module has one clear responsibility.
- CPP-D02: Use composition before inheritance.
- CPP-D03: Use inheritance only for stable polymorphic interfaces.
- CPP-D04: Keep public interfaces small and explicit.
- CPP-D05: Abstractions must reduce coupling.
- CPP-D06: Do not introduce interfaces, factories, or patterns without a concrete need.
- CPP-D07: Separate pure logic from I/O and side effects.
- CPP-D08: Do not use hidden global state or circular dependencies.
- CPP-D09: Optimize only after correctness, safety, and clarity are established.
- CPP-D10: Keep files under 300 logical lines.
- CPP-D11: Keep functions under ~30 logical lines.

## Tooling Gate

- Compile with the project profile plus strict warnings: `-std=c++17 -Wall -Wextra -Wpedantic -Wconversion -Wsign-conversion -Wshadow -Wnon-virtual-dtor -Wold-style-cast -Woverloaded-virtual -Wnull-dereference -Wformat=2`.
- Run static analysis with project-selected MISRA/CERT/Core coverage, at minimum compiler diagnostics plus one analyzer such as `clang-tidy`, `cppcheck`, Coverity, PVS-Studio, Polyspace, Parasoft, Helix QAC, or LDRA.
- Treat C APIs and low-level runtime interfaces as also subject to the C coding standard.

## Definition of Done

Code is acceptable only when:

1. It builds cleanly with the required warnings.
2. Static analysis has no unresolved critical findings.
3. Ownership, lifetime, errors, and boundary validation are explicit.
4. Unsafe APIs, shared state, throwing across forbidden boundaries, and dynamic allocation are absent.
5. Modules, headers, files, classes, and functions stay within this contract.
6. Every mandatory rule passes.
