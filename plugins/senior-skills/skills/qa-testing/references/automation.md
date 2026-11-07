# Test Automation

Automate to guard known behavior cheaply and repeatably on every change. The goal is a
**fast, reliable** suite the team trusts — not the most tests.

## What to automate (and what not)
- **Automate:** regression checks, stable high-value paths, data-driven cases, anything run
  every build, and tedious setup/verification. The base of the pyramid (unit/integration).
- **Don't rush to automate:** fast-changing UI/features (automation thrashes), one-off checks,
  exploratory work, and subjective UX/visual judgment. Automating a moving target wastes effort.
- ROI test: will this run often enough and stay stable enough to pay back the cost to build and
  maintain it?

## Characteristics of good automated tests (FIRST)
- **Fast** — so people run them constantly; slow suites get skipped.
- **Independent** — no order dependence or shared mutable state; each sets up and tears down its own.
- **Repeatable/deterministic** — same result every run, any environment. No reliance on real
  time, network, randomness, or today's data unless controlled.
- **Self-validating** — a clear pass/fail with real assertions, no human reading output.
- **Timely** — written with (or before) the code, not bolted on much later.

## Structure
- **Arrange–Act–Assert** (or Given–When–Then). One logical behavior per test; assert the
  outcome, not incidental implementation details.
- Test **behavior, not internals** — tests coupled to implementation break on every refactor and
  discourage improvement. Test through stable interfaces.
- Use builders/factories/fixtures for setup; keep tests readable — a test is also documentation.

## Test doubles (mocks/stubs/fakes)
- Mock only **true external boundaries** (network, third-party APIs, clock, filesystem) to keep
  tests fast and deterministic. **Over-mocking** makes tests pass while the real integration is
  broken — prefer real objects internally and a few integration tests over everything mocked.
- Keep contracts honest: if you mock a dependency, have an integration test proving the real one
  behaves as the mock assumes.

## Flakiness — treat as a priority bug
- A flaky test (passes/fails without code change) **erodes trust in the whole suite** — people
  start ignoring red. Fix or quarantine immediately; don't "just re-run."
- Common causes: timing/`sleep` races (wait for conditions, not fixed delays), shared state,
  test-order dependence, real network/time, animation/async in E2E. Make waits explicit and
  deterministic.

## E2E/UI automation specifics
- Keep them few (critical journeys only). Use stable selectors (roles/test-ids, not brittle CSS
  paths). Control test data and environment. Expect higher maintenance; isolate from flakiness.

## In the pipeline
- Run in CI on every change; fast checks first (smoke → unit → integration → E2E). Gate merges
  on them. Keep the full suite under a time budget people tolerate. Pair with `devops-infra`.

## Common pitfalls
- Chasing test count/coverage over reliability; a flaky suite nobody trusts.
- Over-mocking → green tests, broken product. Testing implementation details → brittle tests.
- `sleep`-based waits causing races; order-dependent tests.
- Huge, slow E2E suites doing unit tests' job; brittle UI selectors.
