# Test Architecture, Determinism & Quality

A large test suite is software too. Designed well it's fast, reliable, and catches real bugs;
designed poorly it's slow, flaky, and brittle — and people stop trusting it. Aim for **fast,
deterministic, behavior-focused** tests whose effectiveness you actually measure.

## Test doubles (use the right one, deliberately)
- **Dummy** (passed, unused), **stub** (canned answers), **spy** (records calls), **mock** (pre-set
  expectations, asserts interactions), **fake** (working lightweight impl, e.g. in-memory DB).
- **Mock only true external boundaries** (network, third-party APIs, clock, filesystem). **Over-mocking**
  is a top smell: tests pass while the real integration is broken, and they couple to implementation so
  every refactor breaks them. Prefer real objects internally + a few integration tests. Prefer **fakes
  over mocks** for stateful dependencies.
- If you mock a dependency, back it with a **contract test** proving the real one behaves like the mock
  (see `integration-and-e2e.md`), or the mock drifts from reality and hides bugs.

## Determinism & hermeticity (the foundation of trust)
- A test must give the **same result every run, everywhere**, or it's worthless. Control nondeterminism:
  - **Time:** inject a clock; never call `now()` in code under test. Freeze/advance time in tests.
  - **Randomness:** seed it; log the seed for reproducibility.
  - **Concurrency:** wait on conditions, not `sleep`s; avoid real races.
  - **I/O & network:** stub external calls; use local/in-memory or ephemeral containers, not shared envs.
  - **State & order:** each test owns its setup/teardown; **no order dependence**, no shared mutable
    singletons. **Randomize test order in CI** to surface hidden coupling.
- Hermetic tests (no dependence on external/shared/ambient state) are the prerequisite for safe
  parallelization and a trustworthy suite.

## Coverage science (measure effectiveness, not execution)
- **Coverage is necessary, not sufficient.** Line coverage shows what *ran*, not what was *verified* —
  code can be 100% covered by tests with no meaningful assertions.
- Prefer **branch/condition coverage** over line %, and treat coverage as a tool to find **untested**
  areas, not a target to game (Goodhart's law — a coverage quota breeds assertion-free tests).
- **Mutation score** (see `fault-based-testing.md`) is the truest measure of whether tests would catch a
  bug. Use it on critical modules.
- Focus on covering **behaviors, branches, and risks** — especially error paths and edges — over a
  headline percentage.

## Test smells (hunt and fix)
- Assertion-free tests / tests that can't fail; tests that duplicate the implementation's logic (so a
  bug in both passes); **over-mocking**; non-determinism/flakiness; tests coupled to implementation detail
  (break on refactor, miss real bugs); giant unreadable fixtures; slow tests masquerading as unit tests;
  interdependent tests. **Test behavior through stable interfaces, not internals.**

## Test data management
- **Builders/factories/object mothers**: sensible defaults + override the one field that matters; keep
  fixtures minimal and intention-revealing. Realistic but **safe** data — never real PII/secrets (see
  `database-security`); generate or anonymize; seed deterministically; clean up.

## Suite structure
- Respect the pyramid/trophy (see `qa-testing`): many fast unit, a solid integration/contract layer, few
  E2E. **Arrange-Act-Assert**, one behavior per test, assert outcomes not internals. Readable tests are
  documentation.

## Common pitfalls
- Over-mocking → green tests, broken product, refactor-fragile suite; mocks with no contract backing.
- Hidden nondeterminism (real time/random/network/order) → flakiness.
- Chasing a coverage % with weak assertions; ignoring branch coverage and mutation score.
- Tests coupled to implementation; real PII in test data; unreadable mega-fixtures.
