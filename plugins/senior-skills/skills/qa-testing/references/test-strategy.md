# Test Strategy & The Pyramid

Decide what to test, at which level, and how much — driven by risk, not by chasing a
coverage number.

## The test pyramid
- **Unit (base, many):** test one function/class/module in isolation. Fast, deterministic,
  precise at locating failures. The bulk of your tests. Mock only true external boundaries.
- **Integration (middle, some):** test modules working together — DB, API, service
  boundaries, real wiring. Catch the bugs units can't (contracts, serialization, queries).
- **End-to-end (top, few):** test a full user journey through the real system. High
  confidence, but slow, brittle, and hard to debug — keep them few and reserved for critical
  paths. An **inverted pyramid** (mostly E2E) is slow, flaky, and painful; avoid it.
- Related anti-pattern: the "ice cream cone" (lots of manual + E2E, few unit) — push checks down.

## Risk-based prioritization
- You cannot test everything. Rank by **likelihood × impact**: critical/most-used paths,
  money/data/safety/security-touching flows, and historically buggy or newly-changed areas first.
- Cover, in order: the happy path → the main error paths → boundaries and edge cases →
  the exotic. Spend the most on the highest risk.
- New and changed code deserves more scrutiny than stable, long-unchanged code.

## Levels of testing (beyond the pyramid)
- **Smoke:** does the build basically work? Run first, fast, gate everything else.
- **Regression:** does old behavior still hold after changes? Mostly automated.
- **Acceptance:** does it meet the agreed criteria / user needs?
- **System:** the whole integrated system against requirements.

## Coverage — use it well
- Code coverage shows what code ran during tests, not whether behavior is correct. **High
  coverage ≠ good tests;** 100% coverage can still miss logic and assertions. Use it to find
  *untested* areas, not as a target to game.
- Prefer coverage of **behaviors, paths, and risks** over line percentage. Assertions matter
  more than execution.

## Manual vs. automated
- **Automate:** stable, repetitive, high-value regression and data-driven checks; anything run
  every build. See `automation.md`.
- **Keep human:** exploratory testing, UX/visual judgment, one-off checks, and fast-changing
  areas where automation would thrash. Humans find the surprising bugs; machines guard the known ones.

## Entry/exit & environments
- Define when testing can start (entry) and when quality is sufficient to ship (exit) — tied to
  risk and criteria, not just "tests pass."
- Test on production-like data and environments; many bugs hide in environment/data differences.

## Common pitfalls
- Inverted pyramid: slow, flaky E2E doing the job of unit tests.
- Chasing a coverage % with assertion-free tests.
- Testing only the happy path; ignoring errors, boundaries, and misuse.
- No risk prioritization — even effort everywhere, thin where it matters.
- Testing only on clean dev data that hides real-world cases.
