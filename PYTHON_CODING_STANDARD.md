# Python Coding Standard

Default contract for safe, secure, readable, maintainable Python. It applies to humans and coding agents.

## Authority Model

Apply these references as one contract, not as a priority ladder:

- Security and safety requirements
- PEP 8, PEP 257, and Python typing conventions
- Project Python design rules

Safety, security, clarity, and maintainable design are co-requirements. Do not trade one away to satisfy another. If rules appear to conflict, redesign until the code satisfies this contract. Code that cannot satisfy this contract does not merge.

Baseline: Python 3.12+. Project profiles may be stricter, not weaker.

## Mandatory Rules

### Security and Boundaries

- PY-S01: Treat files, network, IPC, environment, user input, credentials, parsers, plugins, external APIs, databases, subprocesses, and deserialization as untrusted boundaries.
- PY-S02: Validate external input before use.
- PY-S03: Do not use `eval`, `exec`, `compile`, dynamic import, `pickle`, `marshal`, or unsafe YAML loading for untrusted data.
- PY-S04: Do not use `shell=True`; pass subprocess arguments as lists.
- PY-S05: Never log secrets, tokens, passwords, or sensitive data.
- PY-S06: Do not hardcode credentials; use project secret or configuration mechanisms.
- PY-S07: Validate paths before file access and prevent traversal in filenames and archives.
- PY-S08: Use parameterized queries for database access.
- PY-S09: Set explicit timeouts for network calls.
- PY-S10: Keep cryptographic choices and randomness in reviewed utilities.
- PY-S11: Keep dependencies minimal, pinned by project policy, and audited.
- PY-S12: Encode security assumptions in checks, tests, or static analysis.

### Types and Contracts

- PY-T01: Type public functions, methods, and non-trivial internal functions.
- PY-T02: Do not expose `Any`, `cast`, or broad unions in public APIs; internal uses must be local and narrowed before use.
- PY-T03: Use `Optional` only when `None` is a valid state, and narrow before use.
- PY-T04: Use `Protocol` only when structural abstraction reduces coupling.
- PY-T05: Use domain types, enums, `NewType`, dataclasses, `TypedDict`, or validated models for structured or security-sensitive data.
- PY-T06: Keep generic types simple.
- PY-T07: Public APIs are documented when the contract is not obvious from names and types.

### Errors and Runtime Behavior

- PY-E01: Raise specific exceptions.
- PY-E02: Do not catch `Exception`; catch specific error types and handle them at the boundary.
- PY-E03: Preserve exception context when wrapping errors.
- PY-E04: Do not use exceptions for normal control flow.
- PY-E05: Fail fast on invalid state.
- PY-E06: Log errors only at the boundary where they are handled.
- PY-E07: Do not perform import-time I/O, network access, heavy optional imports, or hidden side effects.
- PY-E08: Validate configuration at startup.
- PY-E09: Make failure states testable.

### Design and Modules

- PY-D01: Each function, class, and module has one clear responsibility.
- PY-D02: Keep public APIs small, explicit, typed, and stable.
- PY-D03: Keep implementation details private by convention with leading underscores.
- PY-D04: Separate domain logic from I/O, framework glue, clocks, randomness, and system APIs.
- PY-D05: Inject external dependencies at I/O, clock, randomness, and system API boundaries.
- PY-D06: Use composition before inheritance; use inheritance only for stable polymorphic interfaces.
- PY-D07: Do not introduce interfaces, factories, SOLID ceremony, or patterns without a concrete need.
- PY-D08: Do not use hidden global state, wildcard imports, circular imports, or import-time side effects.
- PY-D09: Keep `__init__.py` lightweight; use `__all__` only for an explicit public surface.
- PY-D10: Optimize only after correctness, safety, and clarity are established.
- PY-D11: Keep files under 300 logical lines.
- PY-D12: Keep functions under ~30 logical lines.

## Tooling Gate

- Format with `ruff format`.
- Lint with `ruff check`, including at least `E`, `F`, `B`, `I`, `UP`, `SIM`, `C4`, `PL`, `RUF`, and `S` families.
- Type-check with `mypy` or `pyright`.
- Test with `pytest` and project coverage policy.
- Audit dependencies with `pip-audit` and scan code with `bandit` or `semgrep`.

## Definition of Done

Code is acceptable only when:

1. Format, lint, type-check, tests, and security audit pass.
2. External input, paths, subprocesses, secrets, dependencies, and deserialization are controlled.
3. Errors, configuration, side effects, and failure states are explicit.
4. Public APIs are typed and contracts are visible in names, types, or docstrings.
5. Modules, files, classes, and functions stay within this contract.
6. Every mandatory rule passes.
