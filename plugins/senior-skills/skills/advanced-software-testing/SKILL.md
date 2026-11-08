---
name: advanced-software-testing
description: >-
  Apply advanced software testing beyond basic unit tests: generative techniques
  (property-based, metamorphic, model-based, combinatorial), fault-based testing
  (mutation testing, fuzzing), integration and end-to-end (E2E) testing, contract
  testing, performance/load and chaos/resilience testing, test architecture and
  determinism, coverage science and test quality, and testing throughout CI/CD
  pipelines (gates, selection, parallelization, flaky-test management, deployment
  and release testing). Use when the user wants deeper testing than example-based
  unit tests — E2E suites, performance/load testing, wiring tests into CI/CD,
  property/fuzz/mutation testing, or hardening a slow/flaky suite. For overall
  QA strategy and the test pyramid use `qa-testing`; for the code use
  `software-engineering`; for pipelines use `devops-infra`.
---

# Advanced Software Testing

Example-based tests check the cases you thought of. **Advanced testing finds the cases you
didn't, proves the system works under real conditions, and runs continuously in the
pipeline.** The mindset: stop asking "does my code pass my tests?" and start asking
"**would my tests fail if the code were wrong — and would they catch it before release?**"
These techniques cost more, so target them by risk; but for critical, high-scale, or
integration-heavy systems they're what separates "it passed" from "it works."

## Capabilities

1. **Generative test design** — property-based, metamorphic, model-based, combinatorial.
2. **Fault-based testing** — mutation testing and fuzzing: attack the code and the tests.
3. **Integration & end-to-end (E2E)** — contract testing and stable full-journey automation.
4. **Performance & resilience** — load/stress/soak/spike and chaos/fault-injection testing.
5. **Test architecture & quality** — doubles, determinism, coverage science, test effectiveness.
6. **Testing in CI/CD** — pipeline gates, test selection, parallelization, flakiness, release testing.

References (load the matching one):
- `references/advanced-techniques.md` — property-based, metamorphic, model-based, combinatorial, snapshot.
- `references/fault-based-testing.md` — mutation testing and fuzzing (coverage-guided, structure-aware).
- `references/integration-and-e2e.md` — integration, contract, and end-to-end testing done reliably.
- `references/performance-and-resilience.md` — load/stress/soak/spike, profiling, chaos engineering.
- `references/test-architecture.md` — doubles, determinism/hermeticity, test data, coverage science, quality.
- `references/ci-cd-testing.md` — the test pipeline: stages, gates, selection, parallelization, deploy/release testing.

## When to reach for advanced testing
- **Generated/fault-based** techniques shine on logic with a large input space or strong invariants
  (parsers, serializers, money/date math, algorithms, protocols, state machines, security surfaces).
- **Integration/contract/E2E** when correctness depends on components working *together* and on real
  user journeys — especially across services and UIs.
- **Performance/chaos** when latency, throughput, and availability are requirements, not hopes.
- **CI/CD testing** always: tests only protect you if they run automatically on every change and gate
  releases. Spend the heavy techniques where a bug is most likely and most costly; keep the
  fundamentals (`qa-testing`) as the base.

## 1. Generative test design
State a **property** true for all inputs and let a framework generate and **shrink** cases
(round-trip, invariants, idempotence, oracle/equivalence); use **metamorphic** relations when
there's no oracle, **model-based** sequences for stateful systems, and **pairwise** for parameter
interactions. See `references/advanced-techniques.md`. Highest value on logic-heavy code.

## 2. Fault-based testing
- **Mutation testing:** inject small faults and confirm a test **fails** — the real measure of test
  strength (surviving mutants = tests that verify nothing). **Fuzzing:** feed malformed/coverage-guided
  inputs to find crashes, hangs, and unhandled cases at trust boundaries. See `references/fault-based-testing.md`.

## 3. Integration & end-to-end (E2E) testing
- **Integration tests** verify modules/services together (DB, APIs, serialization, wiring). **Contract
  tests** keep independently-deployed services compatible without slow E2E. **E2E** validates a full
  user journey through the real system.
- E2E is high-confidence but slow and flaky if done carelessly — keep it to **critical journeys**, make
  it **deterministic** (stable selectors, controlled data/env, wait on conditions not sleeps), and treat
  flakiness as a stop-the-line bug. See `references/integration-and-e2e.md`.

## 4. Performance & resilience
- **Performance testing:** load (expected), stress (beyond capacity), soak (sustained — leaks), spike
  (surge). Define SLOs first; measure **percentiles (p95/p99)**, not averages; test at production-like
  scale; profile to the real bottleneck. **Chaos/resilience:** inject faults (kill nodes, add latency,
  drop dependencies) against a hypothesis to prove graceful degradation and recovery. See
  `references/performance-and-resilience.md`. Pair ops with `devops-infra`.

## 5. Test architecture & quality
- **Doubles** used deliberately (mock only true boundaries; prefer fakes; back mocks with contract tests).
  **Determinism is sacred:** control time, randomness, I/O, and order so every run is reproducible.
- **Measure effectiveness, not just execution:** line coverage shows what ran, **branch/condition
  coverage** and **mutation score** show what's actually verified. Hunt test smells (assertion-free
  tests, over-mocking, implementation-coupled tests). See `references/test-architecture.md`.

## 6. Testing in CI/CD
- Tests protect you only when they **run automatically on every change and gate releases.** Build a
  tiered pipeline: fast unit → integration/contract → E2E/performance, **fail fast**, quality **gates**
  (tests, coverage/mutation thresholds, scans). See `references/ci-cd-testing.md`.
- Keep feedback fast at scale: **test selection/impact analysis**, **parallelization/sharding**, caching,
  and tiered suites. **Manage flakiness** (detect, quarantine, root-cause). Extend testing **past deploy:**
  smoke tests, canary/blue-green verification, and **synthetic monitoring** in production. Pair with `devops-infra`.

## Operating principles
- **Would my test fail if the code were wrong?** That, not coverage %, is the real question.
- **Generate and attack, don't just enumerate.** Properties, fuzzers, and mutation find what you'd never write.
- **Test it together and end-to-end** where integration is the risk — but keep E2E few, critical, and deterministic.
- **Prove it under load and failure.** Percentiles over averages; inject real faults to verify resilience.
- **Determinism is sacred.** Control time/randomness/IO/order; a flaky suite is worse than none.
- **Shift left and gate releases.** Tests run on every change in CI/CD and block bad releases automatically.
- **Risk-targeted and behavior-focused.** Heavy techniques where bugs cost most; test behavior, not internals.

## Related skills
- **qa-testing** — overall strategy, the pyramid, case design, exploratory & non-functional testing,
  bug reporting. This skill is the advanced/technique and automation layer on top of that foundation.
- **software-engineering** — the code under test and everyday developer testing (`testing` reference).
- **devops-infra** — the CI/CD pipelines, environments, and observability these tests run in and extend into.
- **security-review** / **pentest** — security fuzzing and testing at trust boundaries (authorized).
- **data-analyst** — interpreting performance results and test-quality metrics honestly.
