# Observability

Knowing the state and behavior of a system from its telemetry — so you can detect,
diagnose, and prevent problems rather than guess.

## The three signals
- **Metrics** — numeric time series, cheap to store, good for alerting and trends.
  Cover the **RED** method for services (Rate, Errors, Duration) and **USE** for
  resources (Utilization, Saturation, Errors). Use labels, but beware cardinality.
- **Logs** — discrete events. Make them **structured** (key/value/JSON), leveled,
  and correlated with a trace/request id. Don't log secrets/PII; sample noisy logs.
- **Traces** — the path of a request across services, with spans and timing. Essential
  for latency and dependency problems in distributed systems. Propagate context.

Correlate all three (shared ids) so you can pivot from an alert to the trace to the logs.

## SLIs, SLOs, error budgets
- **SLI**: a measured indicator of user-facing health (e.g. % of requests < 300ms,
  success rate). **SLO**: the target for an SLI over a window. **Error budget**: the
  allowed shortfall — spend it on change velocity; freeze risky changes when exhausted.
- Define SLOs from the user's perspective, not internal convenience.

## Alerting
- **Alert on symptoms tied to SLOs** (users in pain), not on every cause/spike.
- Every alert must be **actionable** and have a **runbook**. If no one acts, it's noise.
- Tune for signal: avoid flapping, dedupe, set sensible thresholds/burn-rate alerts.
- Page only for urgent, human-needed action; route the rest to dashboards/tickets.

## Dashboards
- One view per service answering: is it healthy (SLIs), what's the load, where's the
  error/latency coming from. Keep them curated, not a wall of every metric.

## Health & readiness
- Expose health endpoints distinguishing liveness (restart me) from readiness (route to me).
- Verify dependencies in readiness where appropriate; fail closed sensibly.

## Common pitfalls
- Logging everything (cost, noise, PII leakage) with no structure.
- Alerting on causes → alert fatigue → ignored pages.
- Metrics with unbounded label cardinality blowing up the backend.
- No trace context propagation → blind spots across services.
- Dashboards nobody reads; SLOs defined but not acted on.
