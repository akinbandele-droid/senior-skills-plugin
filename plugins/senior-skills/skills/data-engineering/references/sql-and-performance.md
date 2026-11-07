# SQL & Query Performance

Write correct, clear SQL; tune by measuring, not guessing.

## Correct, clear SQL
- Be explicit: name columns (not `SELECT *` in app/pipeline code), qualify joins, and
  understand join types (INNER vs. LEFT and when each changes row counts).
- Watch NULL semantics: `= NULL` is never true (use `IS NULL`); NULLs in `NOT IN` bite;
  aggregates skip NULLs. Beware duplicate-row fan-out from joins before aggregating.
- Use window functions for ranking/running totals instead of self-joins. Use CTEs for
  readability (know whether your engine materializes them).
- Make writes safe: parameterize (never string-concatenate user input — SQL injection),
  and scope `UPDATE`/`DELETE` with a `WHERE` (test with a `SELECT` first).

## Tune by measuring
- **Read the query plan** (`EXPLAIN`/`EXPLAIN ANALYZE`): look for full scans on large
  tables where an index should help, expensive sorts, nested-loop joins over big sets,
  and bad row estimates (often stale statistics — refresh them).
- Optimize the **real** slow query on real-shaped data/volume, not a guess. Measure before
  and after.

## Indexing
- Index columns used in `WHERE`, join conditions, and `ORDER BY`. **Composite index column
  order matters** (leftmost-prefix). Covering indexes can satisfy a query from the index alone.
- Indexes speed reads but cost writes and storage — don't over-index. Watch selectivity
  (an index on a low-cardinality column rarely helps).
- Avoid wrapping indexed columns in functions / implicit casts in `WHERE` (defeats the index).

## Scaling patterns
- **Partitioning** large tables (by time/range) to prune scans and ease retention.
- Appropriate denormalization / materialized views / pre-aggregation for heavy analytics.
- Pagination by keyset (seek) rather than large `OFFSET`. Batch large writes.
- Caching for hot reads; read replicas for read scaling (mind replication lag).

## Common pitfalls
- N+1 query patterns from the app; `SELECT *` pulling huge rows.
- Missing or wrong-order composite indexes; functions on indexed columns.
- Huge `OFFSET` pagination; unbatched massive updates locking tables.
- Optimizing on tiny dev data that hides the real plan at production scale.
- String-concatenated SQL → injection and plan-cache churn.
