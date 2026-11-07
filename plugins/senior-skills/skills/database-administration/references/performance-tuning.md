# Database Performance Tuning (server level)

Diagnose by measuring, fix the real bottleneck, and verify. The DBA owns the
instance/server-level levers; statement-level query and index work overlaps with
`sql-development`. Tuning without measuring is cargo-culting.

## Find the bottleneck first
- Identify what's actually slow and why before changing anything. Classic bottlenecks:
  - **Slow queries** — capture them (slow-query log, `pg_stat_statements`, Query Store, AWR/
    Statspack). Usually the biggest wins are here.
  - **Missing/bad indexes** or stale statistics causing bad plans.
  - **Lock/latch contention & blocking** — sessions waiting on each other.
  - **I/O** — slow disk, too much physical reads (insufficient cache), checkpoint storms.
  - **Memory pressure** — low cache hit ratio, spilling to disk/temp.
  - **CPU** — inefficient plans, too much parallelism, or genuine overload.
  - **Connections** — too many/raw connections exhausting resources (needs pooling).
- Use **wait-event analysis** (what the DB is waiting on) — the most direct route to the true
  constraint. Treat symptoms (high CPU) as clues to the cause, not the fix.

## Server/config levers
- **Memory:** size the buffer pool / shared_buffers / SGA so the working set stays cached; set
  work/sort memory appropriately (too high × many connections = OOM). Cache hit ratio is a guide.
- **Connections & pooling:** cap max connections to what the hardware supports; put a **connection
  pooler** in front (PgBouncer, built-in pools) — thousands of idle/raw connections each cost
  memory and scheduling. Right-size the pool.
- **Checkpoints/WAL/redo:** tune to smooth I/O spikes vs. recovery time. **Parallelism:** tune
  degree to the workload (OLTP vs. analytics).
- **Storage:** fast disks/IOPS for hot data; separate logs/temp where it helps; ensure the OS and
  DB aren't fighting for memory. Keep statistics auto-updating.

## Maintenance that affects performance
- **Keep statistics current** — the optimizer makes bad plans on stale stats (a top cause of sudden
  slowdowns). **Manage bloat/fragmentation** — vacuum/rebuild indexes and reclaim table bloat.
- **Partition** very large tables to prune scans and ease maintenance. Archive/purge old data.

## Workload-level
- Reduce load at the source: cache hot reads (app/Redis), offload reporting to **read replicas**,
  batch writes, and fix N+1/chatty access patterns with the app team.
- Right-size the instance — sometimes the honest fix is more/faster hardware, but confirm it's not
  masking a missing index or runaway query first.

## Method
1. Measure and find the dominant bottleneck (don't guess). 2. Form a hypothesis. 3. Change **one
thing**. 4. Measure again on representative load/data. 5. Keep or revert. Optimize the biggest
constraint; chasing micro-tweaks that aren't the bottleneck wastes effort.

## Common pitfalls
- Tuning config from a blog without measuring your actual bottleneck.
- Oversized per-connection memory × many connections → OOM; no connection pooling.
- Stale statistics and index/table bloat causing bad plans and heavy I/O.
- Reading everything from the primary (no read-replica offload); N+1 app patterns.
- Changing several things at once so you can't tell what helped.
- Adding hardware to mask a missing index or a runaway query.
