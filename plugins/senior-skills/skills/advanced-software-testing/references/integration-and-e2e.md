# Integration & End-to-End (E2E) Testing

Unit tests prove pieces work alone; **integration and E2E tests prove they work together and
that real user journeys succeed.** They catch the bugs unit tests can't — wiring, contracts,
serialization, config, environment — but they're slower and flakier, so design them deliberately
and keep them few, critical, and deterministic.

## The layers (push each check as low as it will go)
- **Integration tests:** a few real components together — service + real database, module + real
  queue, two internal modules across their boundary. Catch SQL/ORM errors, serialization mismatches,
  transaction behavior, and wiring/config bugs that mocks hide. Use real dependencies via **ephemeral
  containers** (Testcontainers) or in-memory equivalents rather than shared environments.
- **Contract tests:** for independently-deployed services, verify the **consumer and provider agree on
  the interface** without full E2E. **Consumer-driven contracts** (Pact): the consumer records
  expectations; the provider is verified against them in its own pipeline — each side fast and isolated,
  incompatibilities caught before deploy. For events/schemas, check backward/forward **compatibility** in
  CI. This is the right middle layer for microservices (more real than mocks, far cheaper than E2E).
- **End-to-end (E2E):** drive the **whole system** as a user would (UI or public API through to the
  database and back) to validate a complete journey works in a realistic environment.

## E2E done right (or it becomes your worst suite)
- **Keep it to critical journeys** — login, checkout, the core workflow — not every permutation (that's
  unit/integration work). A handful of reliable E2E tests beat hundreds of flaky ones (top of the pyramid).
- **Stability is everything:**
  - **Stable selectors:** target roles/`data-testid`, not brittle CSS/XPath that break on restyle.
  - **Wait on conditions, never fixed `sleep`s** — the #1 cause of E2E flakiness. Use the tool's
    auto-waiting/retry (Playwright, Cypress, modern Selenium) for elements and network.
  - **Control test data and environment:** seed known data, isolate each test's data, reset state
    between runs; don't depend on a shared mutable environment or on test order.
  - **Handle async/network:** stub or await third-party calls deterministically; mock only what you must.
- **Page Object / screenplay pattern:** encapsulate UI interactions behind a stable API so tests read as
  behavior and don't duplicate selectors — one place to update when the UI changes.
- **Cross-browser/device** only where it matters (real audience), it multiplies cost.
- **API-level E2E** (driving the system through its public API, no UI) is faster and more stable than
  UI E2E — prefer it when a UI isn't essential to the journey under test.

## Environments & data
- Run against production-like environments and data shapes; many integration/E2E bugs are environment-
  or data-specific. Never use real PII (see `database-security`); generate or anonymize.
- Make setup/teardown reliable and idempotent so a failed run doesn't poison the next.

## In the pipeline
- Integration and contract tests run in CI on every change; E2E typically on a later stage / before
  deploy (they're slower). Parallelize and shard; quarantine flaky E2E immediately. See `ci-cd-testing.md`.

## Common pitfalls
- Too many slow, flaky UI E2E tests doing what unit/integration should (inverted pyramid).
- `sleep`-based waits and brittle selectors → chronic flakiness, ignored red.
- Shared/mutable test environment and order-dependent data → nondeterministic failures.
- Mocking so much in "integration" tests that nothing real is integrated.
- Real PII in test data; no data isolation or reset between tests.
