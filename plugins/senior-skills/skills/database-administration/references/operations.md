# Database Operations: Config, Monitoring, Maintenance, Upgrades

The day-to-day and lifecycle work of keeping a database healthy: sensible configuration,
continuous monitoring, routine maintenance, capacity planning, and safe patching/upgrades.

## Configuration
- Don't run production on install defaults. Tune for the workload and hardware: memory, connections
  (+ pooling), durability/checkpoint settings, timeouts, logging, and autovacuum/stats settings.
  (See `performance-tuning.md`.)
- Keep config in version control / IaC where possible (pair with `devops-infra`) so instances are
  reproducible and changes are reviewed. Document non-default settings and why.
- Set safety nets: statement timeouts, idle-session timeouts, and sane resource limits so one bad
  query/session can't take down the instance.

## Monitoring & alerting
Watch continuously and alert on symptoms **before** they become outages:
- **Availability/health:** is it up and accepting connections? Replication healthy and lag bounded?
- **Capacity:** **disk space and growth rate** (a top cause of avoidable outages — alert early),
  memory, CPU, IOPS, connection count vs. max.
- **Performance:** slow queries, lock/blocking waits, cache hit ratio, long-running transactions
  (and transaction-ID wraparound risk on PostgreSQL), deadlocks.
- **Backups:** every backup job's **success/failure** and age (alert if last good backup is too old).
- Keep historical trends (not just point-in-time) to spot creeping problems and plan capacity.

## Routine maintenance
- **Statistics:** ensure they stay current (auto or scheduled) — critical for good plans.
- **Bloat/fragmentation:** vacuum/rebuild as the engine needs; reclaim space; watch index bloat.
- **Data lifecycle:** archive and purge per retention; manage partitions (create future, drop old).
- **Integrity checks** where applicable (e.g. consistency checks) to catch corruption early.
- Schedule maintenance in low-traffic windows; understand locking impact of each operation (prefer
  online/concurrent variants — e.g. create indexes concurrently — to avoid blocking production).

## Capacity planning
- Project storage and compute from growth trends; provision ahead of the wall (disk especially).
- Plan for peak, not average; account for backups, logs, temp, and index overhead in sizing.

## Patching & upgrades
- Stay on **supported versions** (off EOL — no security fixes). Track available patches, especially
  security ones.
- Process: read the release/breaking-change notes → **test in non-prod** on prod-like data → take a
  **backup first** → have a **rollback plan** → schedule a window → execute → verify. Minor patches
  are usually low-risk; **major version upgrades** can change behavior/plans — test thoroughly.
- Major upgrades/migrations: consider logical replication or dump/restore for cross-version moves;
  plan downtime or a low-downtime cutover; validate application compatibility first.

## Change discipline
- Treat production changes (schema, config, maintenance) as risky: backup/verify first, prefer
  reversible and online operations, apply in a window, and have rollback. Review schema changes
  for lock impact (a careless `ALTER` can lock a hot table). Pair with `database-design` for
  migration safety (expand/contract).

## Common pitfalls
- Running on defaults; no safety timeouts (one query stalls everything).
- No disk/replication/backup alerting → avoidable outage or data loss discovered late.
- Skipping statistics/bloat maintenance → degrading performance over time.
- Blocking maintenance in peak hours; non-concurrent index builds locking production.
- Upgrading in prod without testing, backup, or rollback; running EOL versions.
