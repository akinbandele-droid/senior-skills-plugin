# Query Optimization & Indexing

Make SQL fast by understanding what the engine does — read the plan, index the real
bottleneck, and verify on production-scale data. Guessing wastes effort and sometimes makes
things worse.

## Read the execution plan first
- Use `EXPLAIN` (estimated) and `EXPLAIN ANALYZE` (actual timings/rows) — the equivalent in your
  engine (SET SHOWPLAN / Query Store / AUTOTRACE / `EXPLAIN FORMAT=JSON`). Look for:
  - **Full/sequential scans** on large tables where an index should apply.
  - Expensive **sorts** and hash operations, and **spills to disk** (work_mem too small).
  - **Nested-loop joins** over large row counts (vs. hash/merge) — often a bad estimate or missing index.
  - **Bad row estimates** (estimated vs. actual far apart) → usually **stale statistics**; refresh
    them. The optimizer is only as good as its stats.
- Optimize the operator that dominates the cost, not a cheap step.

## Indexing
- **Index the columns used in `WHERE`, join conditions, and `ORDER BY`/`GROUP BY`.** An index lets
  the engine seek instead of scan.
- **Composite indexes: column order matters** — usable left-to-right (leftmost-prefix). Put the most
  selective / equality-filtered columns first, range columns later. One good composite index often
  beats several single-column ones.
- **Covering index:** include all columns a query needs so it's answered from the index alone (no
  table lookup) — big win for hot queries.
- **Selectivity:** indexes help when they narrow to few rows; an index on a low-cardinality column
  (e.g. boolean) rarely helps. Partial/filtered indexes for a common subset.
- **Don't defeat the index:** functions or implicit type casts on an indexed column in `WHERE`
  (`WHERE lower(email) = …`, or comparing a string column to a number) force a scan — use a
  matching type or a function/expression index.
- **Cost of indexes:** each one slows writes and uses space, and must be maintained. Don't over-index;
  remove unused ones. Balance read speedup vs. write cost.

## Rewriting queries
- Prefer **set-based** over row-by-row; replace correlated subqueries/self-joins with **joins or
  window functions**. Avoid `SELECT *` (reads/transfers needless columns, defeats covering indexes).
- **SARGable predicates:** write conditions so an index can be used (no wrapping the column in a
  function; range instead of function-on-column). Filter early; reduce rows before expensive joins.
- Pagination: use **keyset/seek** (`WHERE id > @last ORDER BY id LIMIT n`) rather than large
  `OFFSET` (which scans and discards). Batch big writes.
- `EXISTS` vs `IN` vs join — choose by semantics and test; `UNION ALL` over `UNION` when dedup isn't needed.

## Method
1. Reproduce the slow query on **production-scale data** (dev data hides the real plan).
2. Read the plan; find the dominant cost and its cause.
3. Make one change (an index, a rewrite, updated stats). 4. Re-measure. 5. Keep or revert.
- Watch for **parameter sniffing / plan caching** issues (one plan cached for skewed parameters).
  Keep statistics current (server-side maintenance — see `database-administration`).

## Common pitfalls
- Optimizing without reading the plan; tuning the wrong (cheap) step.
- Missing indexes on join/filter columns, or wrong composite column order.
- Functions/implicit casts on indexed columns → scans (non-SARGable).
- Over-indexing (write penalty) or unused indexes left in place.
- Large-`OFFSET` pagination; `SELECT *`; stale statistics causing bad plans.
- Testing on tiny dev data so the production plan is never seen.
