# Testing

Tests exist to give you (and the next person) confidence to change code. Write
tests that would fail if the behavior broke, and that you'd trust.

## What to test
- The behavior you changed or added — including the edge and failure cases, not
  just the happy path.
- Boundaries: empty, single, many, max; null/missing; invalid input; concurrency
  where relevant.
- The specific bug you fixed: add a regression test that fails before the fix and
  passes after.
- Contracts at trust boundaries (API responses, parsing, validation).

## At what level
- **Unit** for logic and pure functions — fast, many, precise.
- **Integration** for how components fit (DB, HTTP, queues) — fewer, higher value.
- **End-to-end** for critical user flows — few, slow, high confidence.
- Prefer the lowest level that actually exercises the behavior. Don't push
  everything to e2e, and don't over-mock a unit until it tests nothing real.

## Qualities of good tests
- **Deterministic** — no reliance on time, ordering, network flakiness, or random
  seeds you don't control.
- **Isolated** — each test sets up and tears down its own state; no hidden
  dependence on another test running first.
- **Readable** — the test name states the behavior; arrange/act/assert is clear;
  a failure message tells you what's wrong.
- **Focused** — one behavior per test; assert the meaningful thing, not everything.
- **Honest** — it fails when the behavior is wrong. A test that can't fail is noise.

## Practices
- Follow the project's existing test framework, layout, and conventions.
- When fixing a bug, write the failing test first, then make it pass.
- Don't test implementation details that will churn; test observable behavior.
- Keep fixtures small and meaningful. Mock external systems at the boundary, not
  your own core logic.
- Run the full relevant suite before declaring done; report real results,
  including failures or anything skipped.

## Coverage
- Coverage is a flashlight, not a goal. High coverage of trivial getters proves
  little; a few sharp tests on the risky logic prove a lot.
- Aim to cover the paths where a mistake would actually hurt.
