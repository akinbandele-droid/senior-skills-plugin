# Data Pipelines (ETL/ELT, Batch & Streaming)

Move and transform data reliably. Pipelines are production software: version them,
test them, make them idempotent, and observe them.

## ETL vs. ELT
- **ETL:** transform before loading — when the target can't transform well, or you must
  cleanse/conform first. **ELT:** load raw, transform in a capable warehouse — simpler,
  keeps raw for reprocessing, leverages warehouse compute. Prefer ELT when the warehouse
  supports it; keep an immutable raw landing zone either way.

## Idempotency & correctness (the core discipline)
- **Idempotent and re-runnable:** reprocessing the same window produces the same result,
  no duplicates. Use upserts/merge keyed on a natural/business key, or atomic
  partition-replace, rather than blind appends.
- **Handle the messy realities:** late-arriving, duplicate, and out-of-order data;
  schema changes; partial failures. Decide the policy explicitly rather than letting it corrupt.
- **Backfills:** plan them (bounded, idempotent, resumable); don't let a backfill double-count.

## Batch orchestration
- Model as a DAG with explicit dependencies; retries with backoff; alert on failure and on
  SLA miss (data late is a failure too). Make steps atomic — a failed step leaves no
  half-written state (write to temp, swap on success).
- Checkpoint progress; make restarts safe. Keep transformations in version control and tested.

## Streaming
- Understand **delivery semantics**: at-least-once (plan for duplicates → idempotent consumers)
  vs. exactly-once (where supported). Use event time vs. processing time and watermarks for
  windowing late data.
- Preserve ordering where it matters (partition by key). Handle poison messages (dead-letter).
  Checkpoint offsets so you resume without loss or replay storms.

## Schema & contracts
- Treat the schema between producers and consumers as a **contract**; evolve it
  backward/forward-compatibly; validate on ingest. Keep a schema registry for streams.
- Never silently coerce or drop fields on a schema change — fail or route to quarantine.

## Observability
- Emit metrics: rows in/out, reject counts, lag/freshness, run duration, SLA status.
- Log lineage (what ran, over what window, producing what). Alert on freshness and
  volume anomalies, not just hard crashes.

## Common pitfalls
- Non-idempotent appends → duplicate/double-counted data on retry.
- No handling of late/out-of-order/duplicate events; silent schema drift.
- Non-atomic writes leaving half-loaded partitions; backfills that double-count.
- No freshness/volume monitoring — stale or partial data goes unnoticed for days.
- Transformations untested and unversioned, living only in a scheduler UI.
