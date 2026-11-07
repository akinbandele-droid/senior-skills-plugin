# Performance Profiling & Optimization

Optimize with evidence, not intuition. Most performance guesses are wrong; the
profiler is the authority. Correctness and readability come first — buy speed
only where it's measured to matter.

## The discipline
1. **Define the goal.** What metric, what target, under what conditions?
   (p99 latency, throughput, memory ceiling, startup time.) Without a target you
   can't know when you're done.
2. **Measure first.** Reproduce the slow path with a realistic workload and
   profile it. Find where time/memory *actually* goes — don't assume.
3. **Find the bottleneck.** Optimize the biggest cost first (Amdahl's law: a 90%
   function is where the wins are; a 2% function isn't worth touching).
4. **Change one thing, re-measure.** Confirm the improvement is real and hasn't
   broken correctness. Keep the before/after numbers.
5. **Stop when the target is met.** Further optimization adds complexity and risk
   for no business value.

## How to measure
- **Profilers** (CPU sampling, allocation, flame graphs) to find hot paths.
- **Benchmarks** that are stable and representative; warm up, run enough
   iterations, control for noise, and measure the right percentile — averages hide tail latency.
- **Realistic data & load:** test at production-like scale and shape; small
   inputs hide O(n²).
- **Production telemetry:** traces and metrics tell you what's slow for real users.

## Common culprits (check these before micro-tuning)
- **Algorithmic complexity** — accidental O(n²), repeated work, missing index.
- **N+1 queries / chatty I/O** — batch, join, or cache.
- **Unnecessary I/O & network round-trips** — the dominant cost in most services.
- **Serialization/allocation churn** — excessive object creation, copies, GC pressure.
- **Missing or wrong caching** — and its invalidation.
- **Lock contention / serialization of parallel work.**
- **Doing work eagerly that could be lazy, or repeatedly that could be memoized.**

## Order of leverage (biggest wins first)
1. Do less work (better algorithm, avoid the call entirely, cache the result).
2. Reduce I/O and round-trips (batch, prefetch, co-locate).
3. Improve data structures and access patterns.
4. Parallelize / make async where safe.
5. Micro-optimize hot loops — last, and only with a profile.

## Guardrails
- Never trade significant readability for speed without a measurement proving it matters.
- Keep a regression benchmark so the win doesn't silently erode later.
- Re-run the correctness tests after every optimization — fast and wrong is worthless.
- Document why a non-obvious optimization exists, so no one "simplifies" it away.
