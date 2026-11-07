# Schema Evolution & Migrations

Schemas change, but data and running code depend on them — so changes must be versioned,
reviewed, reversible, and (usually) **zero-downtime**. The schema is the hardest thing to
change safely once it's live; treat every migration as a careful, backward-compatible step.

## Migrations as code
- Every schema change is a **versioned migration** in source control, reviewed like code, applied
  through a tool (Flyway, Liquibase, Alembic, Rails/Django/EF migrations, etc.) — **never ad-hoc
  manual changes in production** (unreproducible, undocumented, drift, and mistakes).
- Each migration is ordered, idempotent where possible, and ideally has a tested **rollback** (or a
  forward-fix plan). Keep migrations small and focused.
- Apply through the deploy pipeline (pair with `devops-infra`); the same migrations run in every
  environment so schemas stay consistent.

## Backward-compatible change: expand/contract (the key pattern)
Deploying a schema change and a code change atomically is usually impossible (rolling deploys, replicas),
so split breaking changes into compatible steps:
1. **Expand:** add the new structure **additively** — new nullable column / new table / new index —
   which old and new code both tolerate.
2. **Migrate & dual-write/backfill:** new code writes both old and new; backfill existing rows in
   batches. Both shapes valid simultaneously.
3. **Contract:** once all code uses the new shape and you're confident, remove the old
   column/structure in a later, separate migration.
- This lets deploys and rollbacks happen without breaking running code, and avoids a big-bang cutover.
  Renames and type changes become add-new → backfill → switch → drop-old, not a single breaking `ALTER`.

## Zero-downtime & lock awareness
- Know the **locking behavior** of each DDL on your engine — a careless `ALTER`/index build can lock a
  hot table and cause an outage. Use **online/concurrent** variants (e.g. `CREATE INDEX CONCURRENTLY`,
  online schema-change tools like gh-ost/pt-online-schema-change) and avoid long-blocking operations
  in peak hours. Coordinate with `database-administration`.
- Adding a NOT NULL column with a default, or changing a type, can rewrite a whole large table — do it
  in safe steps (add nullable → backfill in batches → add constraint validated separately).
- **Backfill in batches** (not one giant `UPDATE`) to avoid long locks, replication lag spikes, and bloat.

## Protect the data
- **Back up before destructive/irreversible changes;** test the migration on **production-shaped data**
  (volume and edge cases) first. Guard stateful objects (`prevent-destroy`; never a casual `DROP`).
- A dropped column/table is data loss — contract only after a safety window and confirmation nothing
  needs it; consider archiving first.

## Data migrations
- Moving/transforming data (not just structure) is its own risk: make it idempotent and resumable,
  reconcile counts/totals before and after, and keep the source until the target is verified.

## Common pitfalls
- Manual prod schema edits (drift, no history, no rollback).
- Big-bang breaking changes (drop/rename/retype in one step) that break running code and can't roll back.
- Blocking DDL / non-concurrent index builds locking hot tables in peak hours.
- One giant backfill `UPDATE` causing locks, replication lag, and bloat.
- Dropping columns/tables without a safety window or backup → irreversible data loss.
- Not testing migrations on production-scale data (surprise lock/time/space in prod).
