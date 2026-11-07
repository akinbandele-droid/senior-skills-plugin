# Querying: Joins, NULLs, Aggregation, Windows, Sets

The craft of getting exactly the right rows and values. Most SQL bugs are wrong joins,
mishandled NULLs, or wrong grain — not syntax errors. Think in sets.

## Joins
- **Types:** INNER (matches only), LEFT/RIGHT OUTER (keep all of one side, NULLs for non-matches),
  FULL OUTER (keep both), CROSS (cartesian). Picking the wrong one silently changes results.
- **Fan-out (the classic bug):** joining to a one-to-many relationship multiplies rows — a `SUM`
  after it double-counts, a `COUNT` inflates. Know the cardinality of each join; aggregate at the
  right grain (pre-aggregate the many-side, or `COUNT(DISTINCT …)`), don't just paper over it.
- Join on indexed keys; qualify every column with its table/alias; filter in `WHERE` vs. the `ON`
  clause deliberately (on an outer join they differ — a condition in `ON` vs `WHERE` changes which
  rows survive).
- `LEFT JOIN … WHERE right.col IS NULL` is the idiom for "rows with no match" (anti-join).

## NULL handling (a top source of silent bugs)
- NULL means unknown; comparisons with it yield *unknown*, not true: use `IS NULL`/`IS NOT NULL`.
- **`NOT IN (subquery)` returns zero rows if the subquery yields any NULL** — a notorious trap; use
  `NOT EXISTS` instead.
- Aggregates ignore NULLs (`AVG` over NULLs skips them; `COUNT(col)` ≠ `COUNT(*)`).
- `x = y` is unknown if either is NULL — use `IS DISTINCT FROM` / `COALESCE` where you need NULL-safe
  comparison. Be deliberate about NULL vs. empty-string vs. zero.

## Aggregation & grouping
- `WHERE` filters rows **before** grouping; `HAVING` filters **after** (on aggregates). Every
  non-aggregated column in `SELECT` must appear in `GROUP BY`.
- Deduplicate by understanding the cause: `DISTINCT`, `GROUP BY`, or fixing the fan-out join.
  `COUNT(DISTINCT x)` for unique counts. Watch grain when combining aggregates from different levels
  (aggregate separately then join, to avoid cross-multiplication).

## Subqueries & CTEs
- **CTEs (`WITH`)** decompose complex queries into readable named steps; chain them. Know whether
  your engine materializes/optimizes CTEs (affects performance — some treat them as optimization fences).
- **Correlated subqueries** (reference the outer row) are expressive but run per-row — often a join
  or window is faster. Scalar subqueries in `SELECT` likewise. `EXISTS`/`NOT EXISTS` for presence
  tests (NULL-safe, often faster than `IN`).

## Window functions (high leverage)
- `func() OVER (PARTITION BY … ORDER BY … [frame])` computes across related rows **without
  collapsing** them. Uses:
  - **Ranking:** `ROW_NUMBER`, `RANK`, `DENSE_RANK` — e.g. top-N per group, dedup (keep `ROW_NUMBER=1`).
  - **Offsets:** `LAG`/`LEAD` — period-over-period, deltas, gaps.
  - **Running/moving:** `SUM`/`AVG` with a frame — running totals, moving averages.
  - **Distribution:** `NTILE`, `PERCENT_RANK`, `CUME_DIST`.
- Reach for a window function before a self-join or a procedural loop — clearer and faster.

## Set operations
- `UNION` (dedups — costs a sort), `UNION ALL` (keeps all — cheaper, use when no dedup needed),
  `INTERSECT`, `EXCEPT`/`MINUS`. Column count/types must align.

## Common pitfalls
- Wrong join type or unnoticed fan-out → inflated counts/sums.
- `NOT IN` with NULLs returning nothing; `= NULL`; `COUNT(col)` vs `COUNT(*)` confusion.
- Conditions in `WHERE` vs. `ON` of an outer join changing results unexpectedly.
- Self-joins/procedural logic where a window function is cleaner and faster.
- `SELECT *` in reused/app queries (fragile, wasteful); unqualified ambiguous columns.
