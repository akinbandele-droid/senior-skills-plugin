---
name: qa-testing
description: >-
  Plan and execute software quality assurance with rigor: test strategy and the
  test pyramid, designing effective test cases, automation (unit/integration/
  E2E), exploratory testing, non-functional testing (performance, accessibility,
  security-adjacent), and clear bug reporting. Use when the user is planning how
  to test a feature or release, designing test cases, deciding what/how to
  automate, doing exploratory or regression testing, setting up QA process, or
  writing bug reports. Goes deeper than everyday dev testing. For writing the
  code under test use `software-engineering`; for security testing use the
  `pentest` family.
---

# QA & Testing

Quality assurance is about building confidence that software does what it should —
and finds out where it doesn't **before users do**. Testing can't prove the absence
of bugs, so spend effort where risk is highest, automate what's repetitive and
stable, and use human exploration for the judgment machines lack. A good tester
thinks adversarially: how could this break, and what did everyone assume?

## Core loop

1. **Understand what & why** — the feature, its users, the risks, and what "quality" means here.
2. **Strategize** — what to test, at what level, how much, manual vs. automated — risk-based.
3. **Design test cases** — effective cases from requirements and the ways things actually fail.
4. **Execute** — automated suites + scripted + exploratory testing; across the test pyramid.
5. **Report** — clear, reproducible defects; quality signal to the team.
6. **Prevent & regress** — turn every escaped bug into a test; keep suites fast and trustworthy.

References (load the matching one):
- `references/test-strategy.md` — the pyramid, levels, risk-based prioritization, coverage.
- `references/test-design.md` — techniques for designing cases that find real bugs.
- `references/automation.md` — what/how to automate; reliable, non-flaky suites.
- `references/exploratory-and-nonfunctional.md` — exploratory testing + performance/a11y/etc.
- `references/bug-reporting.md` — writing defects that get understood and fixed.

## 1. Understand what & why
- Know the feature's purpose, its users, and how they'll really use it (and misuse it).
- **Assess risk:** where is failure most likely and most costly (money, safety, data, trust,
  reputation)? That's where testing effort concentrates. You can't test everything.
- Clarify the acceptance criteria / "definition of done" — you test against an expectation,
  so the expectation must be explicit. Ambiguity found now is a bug found cheaply.

## 2. Strategize (risk-based)
- **Test pyramid** (`references/test-strategy.md`): many fast **unit** tests, fewer
  **integration** tests, few **end-to-end** tests. Push coverage down to the cheapest level
  that proves the point; avoid an inverted, slow, flaky E2E-heavy suite.
- Decide **manual vs. automated**: automate stable, repetitive, high-value regression checks;
  reserve human testing for exploration, UX judgment, and one-off/changing areas.
- Match depth to risk and stakes. Cover the happy path, the error paths, and the edges.

## 3. Design effective test cases
- Derive cases from requirements **and** from how software actually fails
  (`references/test-design.md`): boundary values, equivalence classes, invalid/empty/huge
  inputs, state transitions, decision tables, error handling, concurrency, and the
  "nobody thought of that" cases.
- Each case: clear precondition, action, and **expected result**. A test without a defined
  expectation can't fail meaningfully.
- Prioritize: the highest-risk and most-used paths first.

## 4. Execute across levels
- Run automated unit/integration/E2E suites (in CI — pair with `devops-infra`). Add scripted
  manual cases where automation isn't worth it yet.
- **Exploratory testing** (`references/exploratory-and-nonfunctional.md`): unscripted,
  skilled investigation to find what scripted cases miss — often the most valuable bugs.
- **Non-functional** as relevant: performance/load, accessibility (WCAG — pair with
  `ui-ux-design`), compatibility, resilience, and basic security-adjacent checks (hand real
  security testing to the `pentest` family).

## 5. Report quality
- File **reproducible, specific** defects (`references/bug-reporting.md`): steps, expected vs.
  actual, environment, evidence, severity. A bug that can't be reproduced rarely gets fixed.
- Give the team an honest quality signal: what's tested, what isn't, known risks, and your
  confidence — don't rubber-stamp a release.

## 6. Prevent & keep suites healthy
- Every **escaped bug becomes a regression test** so it can't return.
- Keep suites **fast and trustworthy:** fix or quarantine flaky tests immediately — a suite
  people don't trust is worse than none. Prune redundant/obsolete tests.
- Shift left: get QA thinking into requirements and design, where bugs are cheapest to prevent.

## Operating principles
- **Risk-based.** You can't test everything; test what matters most, most.
- **Think adversarially.** Hunt the edge, the error path, and the unstated assumption.
- **Right level, right test.** Push checks down the pyramid; keep E2E few and meaningful.
- **Reproducible or it didn't happen.** Bugs and tests alike must be deterministic.
- **A trustworthy suite.** Fast, non-flaky, and maintained — flakiness destroys the value.
- **Honest signal.** Report real quality and risk; testing informs decisions, it doesn't bless them.

## Related skills
- **software-engineering** — writing the code and its developer tests (the pyramid's base).
- **devops-infra** — running suites in CI and gating releases on them.
- **ui-ux-design** — usability and accessibility criteria to test against.
- **pentest** / **web-pentest** — authorized security testing, beyond QA's scope.
