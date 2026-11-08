# Testing in CI/CD

Tests only protect you if they **run automatically on every change and gate releases.** The CI/CD
pipeline is where testing becomes continuous: structured in stages, fast enough that people don't
route around it, enforced as quality gates, and extended past deploy into production verification.
Pair with `devops-infra` for the pipeline/deployment mechanics.

## Pipeline stages (fast feedback first)
Order tests cheapest/fastest → slowest/most-integrated, and **fail fast**:
1. **Static checks** — lint, type-check, format, security/dependency scans (seconds).
2. **Unit tests** — the bulk; fast, parallel, run on every push/PR.
3. **Integration & contract tests** — real DB/services via ephemeral containers (see `integration-and-e2e.md`).
4. **E2E / performance / security tests** — slower; on a later stage, pre-merge or pre-deploy.
A broken fast stage stops the pipeline before wasting time on slow ones. Developers get unit/integration
feedback on every PR; heavier suites gate the release.

## Quality gates (make them enforceable, not advisory)
- **Block merge/deploy** on: failing tests, coverage below a threshold (prefer branch; beware gaming —
  pair with mutation score on critical code), failing contract tests, security/scan findings above a
  severity, and performance regressions beyond a budget.
- Gates must be **meaningful and trusted** — a flaky or rubber-stamp gate gets ignored or bypassed. Keep
  them green-means-go.

## Keep it fast at scale (or people bypass it)
- **Test selection / impact analysis:** run the tests affected by the change (plus a core always-on set)
  rather than everything, every time.
- **Parallelization & sharding:** split the suite across runners/containers; this *requires* hermetic,
  order-independent tests (see `test-architecture.md`).
- **Caching:** dependencies, build artifacts, and test results for unchanged code.
- **Tiered execution:** fast suite on every push; full suite on merge/nightly. Budget the pipeline to a
  time developers tolerate (minutes, not hours) — slow CI is routed around.

## Flaky tests (treat as a stop-the-line bug)
- A test that passes/fails without code change **erodes trust in the whole suite** — red gets ignored,
  real failures slip through. Don't "just re-run."
- **Detect** (track pass/fail history, flakiness dashboards), **quarantine** immediately (move out of the
  gating set, visibly, with a ticket — not silently disabled), **root-cause** (timing/`sleep`s, shared
  state, order dependence, real time/network, resource limits), and **fix**. Auto-retry is a stopgard that
  hides flakiness — use sparingly and never as the cure.

## Testing beyond deploy (continuous testing)
Testing doesn't stop at merge — verify the actual release in the real environment:
- **Smoke tests** post-deploy: a handful of critical-path checks confirming the deployment is healthy
  before (or right after) taking traffic; fail → auto-rollback.
- **Canary / blue-green verification:** route a small slice of traffic (or run against the green
  environment) and watch error rates, latency, and key SLIs; promote only if healthy, else roll back.
- **Synthetic monitoring:** scripted user journeys run continuously against production to catch breakage
  and SLO violations before users report them — E2E tests repurposed as always-on probes.
- **Feature flags** decouple deploy from release and let you test in production safely (dark launch,
  gradual rollout) with a fast kill switch. (See `devops-infra`.)
- Close the loop: production failures and incidents become new regression tests (pair with `incident-response`).

## Common pitfalls
- Slow pipelines (everything, every time) → developers bypass or ignore them.
- Advisory "gates" nobody enforces; or gates gamed by assertion-free tests to hit a coverage number.
- Flaky tests tolerated/auto-retried instead of fixed → the whole suite loses trust.
- Parallelizing non-hermetic tests → new flakiness. No post-deploy smoke/canary → bad releases reach users.
- No synthetic monitoring → production breakage found by customers, not the pipeline.
