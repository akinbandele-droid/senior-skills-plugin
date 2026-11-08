# Performance & Resilience Testing

Correct isn't enough if it's too slow or falls over under load or failure. Performance testing
proves the system meets its speed/scale targets; resilience/chaos testing proves it survives the
failures that *will* happen in production. Both require defining the target first and measuring honestly.

## Define targets first (SLOs)
- You can't pass or fail without a goal. Define, per critical operation: latency targets (as
  **percentiles** — p50/p95/p99, not averages, which hide the tail), throughput (requests/sec),
  concurrency, and resource ceilings. Tie to user experience and SLOs (pair with `devops-infra`).

## Types of performance test
- **Load test:** expected/peak traffic — does it meet targets under normal busy conditions?
- **Stress test:** push *beyond* capacity to find the breaking point and, crucially, **how it fails**
  (graceful degradation vs. collapse) and whether it recovers.
- **Soak / endurance test:** sustained load over hours/days — reveals **memory leaks**, resource
  exhaustion, connection-pool/file-handle leaks, and slow degradation invisible in short runs.
- **Spike test:** sudden surge (flash sale, viral event) — does it absorb, autoscale, shed load, or crash?
- **Scalability test:** how performance changes as you add load/resources (linear? a cliff?).

## Doing it well
- **Test at production-like scale** — data volume, infrastructure, and network. Results on a laptop or
  tiny dataset are meaningless; big-O problems and cache/DB effects only appear at scale.
- **Realistic workload:** model real user behavior and mix (think-time, concurrency, representative
  endpoints and payloads), not a single endpoint hammered in a tight loop.
- **Measure the whole picture:** client-side latency percentiles **and** server-side resources (CPU,
  memory, I/O, GC, DB connections, queue depths) to locate the bottleneck.
- **Profile to the real bottleneck** (CPU/alloc/flame graphs, slow-query logs, DB explain) rather than
  guessing — then fix the dominant constraint and re-measure. (See `software-engineering` performance,
  `data-engineering` SQL.)
- Tools: k6, Gatling, JMeter, Locust, wrk/vegeta. Run in CI as a gate on critical paths; track trends to
  catch **performance regressions** over time.

## Resilience & chaos engineering
- **Chaos engineering:** deliberately inject failures to verify the system degrades gracefully and
  recovers — *before* reality does it for you. It's an experiment, not vandalism:
  1. **Hypothesis:** "if a dependency times out, requests still succeed via cache/fallback within X."
  2. **Inject a real fault** in a controlled way: kill instances, add network latency/packet loss, make a
     dependency error or go away, exhaust CPU/disk, fail a zone.
  3. **Observe** against the hypothesis; **start with a small blast radius** (staging, then a tiny
     production slice) and have an abort/rollback. Widen only as confidence grows.
  4. **Fix** what breaks (missing timeout, no retry/backoff, no circuit breaker, no fallback, cascading failure).
- **Resilience patterns to verify:** timeouts, retries **with backoff + jitter**, circuit breakers,
  bulkheads, graceful degradation/fallbacks, idempotency, and recovery/failover (tie to `devops-infra`
  HA and `incident-response`). Tools: Chaos Monkey/Gremlin/Litmus; or targeted fault injection.

## Common pitfalls
- No defined targets; reporting averages that hide p99 pain.
- Testing on tiny data/infra so real bottlenecks never appear; single-endpoint hammering vs. realistic mix.
- Measuring only client latency, not server resources → can't find the bottleneck.
- Chaos with no hypothesis, no blast-radius control, or no abort — causing a real outage.
- Finding resilience gaps but not fixing the pattern (timeouts/retries/breakers), so they recur.
