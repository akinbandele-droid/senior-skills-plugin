---
name: sql-development
description: >-
  Write correct, clear, and performant SQL and database programs: advanced
  queries (joins, subqueries, window functions, CTEs), set-based thinking,
  stored procedures/functions/triggers/views, transactions and concurrency,
  and query optimization and indexing. Use when the user is writing or debugging
  SQL, building reports/analytical queries, writing stored procedures or
  database logic, optimizing a slow query, deciding on indexes, or handling
  transactions/isolation. Engine-aware (PostgreSQL, MySQL, SQL Server, Oracle)
  with portable fundamentals. For schema design use `database-design`; for
  operating the DB use `database-administration`; for securing it use
  `database-security`.
---

# SQL Development

Writing SQL well means thinking in **sets**, not loops: describe the result you want and
let the engine figure out how. Good SQL is correct first (right rows, right NULL handling,
no accidental fan-out), clear second (readable, maintainable), and fast third (tuned by
measuring the plan). Parameterize everything that takes input — string-building is how data
gets breached and plans get thrashed.

## Core areas

1. **Query correctly** — joins, filtering, NULLs, aggregation, dedup at the right grain.
2. **Use the power features** — subqueries, CTEs, window functions, set operations.
3. **Write database programs** — views, stored procedures/functions, triggers — where they belong.
4. **Handle transactions & concurrency** — atomicity, isolation levels, locking, deadlocks.
5. **Optimize** — read the plan, index well, and rewrite the real bottleneck.
6. **Write safely** — parameterize, scope writes, and keep SQL clear and portable where it matters.

References (load the matching one):
- `references/querying.md` — joins, NULLs, aggregation, subqueries, CTEs, window functions, sets.
- `references/programmability.md` — views, stored procedures/functions, triggers — when and how.
- `references/transactions.md` — ACID, isolation levels, locking, deadlocks, concurrency patterns.
- `references/optimization.md` — reading query plans, indexing, and rewriting slow queries.

## 1. Query correctly
- **Think in sets:** operate on whole result sets with declarative SQL instead of row-by-row
  procedural loops (which are slow and un-SQL). Let the optimizer do the work.
- **Joins:** know INNER vs. LEFT/RIGHT/FULL and when each changes the row count; beware **fan-out**
  (a one-to-many join multiplying rows) silently inflating counts/sums — aggregate at the right grain
  or dedup deliberately. Qualify columns; join on indexed keys.
- **NULLs:** `= NULL` is never true (use `IS NULL`); `NOT IN (… NULL …)` returns no rows;
  aggregates skip NULLs; `COUNT(col)` ignores NULLs but `COUNT(*)` doesn't. Handle them explicitly.
- **Aggregation:** understand `GROUP BY`/`HAVING` vs. `WHERE`, and that every non-aggregated select
  column must be grouped. De-duplicate consciously (`DISTINCT` vs. `GROUP BY` vs. the real cause).
  See `references/querying.md`.

## 2. Use the power features
- **Subqueries & CTEs:** use CTEs (`WITH`) to decompose complex logic readably; know whether your
  engine materializes them (perf implications). Correlated subqueries can be clear but costly —
  consider a join/window instead.
- **Window functions** (`OVER (PARTITION BY … ORDER BY …)`): rankings, running totals, moving
  averages, period-over-period, dedup-by-row_number, gaps-and-islands — without collapsing rows or
  self-joining. One of the highest-leverage skills; reach for them before procedural workarounds.
- **Set operations** (`UNION`/`UNION ALL`/`INTERSECT`/`EXCEPT`) — and prefer `UNION ALL` when you
  don't need dedup (cheaper).

## 3. Write database programs (where appropriate)
- **Views** to encapsulate and reuse query logic and to expose a controlled subset (also a security
  tool — see `database-security`). Materialized views for expensive, reused results (mind refresh).
- **Stored procedures/functions** for data-intensive logic that belongs close to the data, set-based
  batch operations, and a controlled API to the data. Keep business logic appropriately placed —
  don't scatter it unmanageably in the DB, but don't ship data to the app to loop over it either.
- **Triggers** sparingly and carefully — powerful but hidden side effects that surprise people and
  hurt performance; prefer explicit logic unless a trigger is clearly right (e.g. audit). See
  `references/programmability.md`.

## 4. Handle transactions & concurrency
- Wrap multi-statement changes in **transactions** so they're atomic (all-or-nothing). Keep
  transactions **short** to reduce lock contention. See `references/transactions.md`.
- Understand **isolation levels** (Read Committed → Serializable) and the anomalies each permits
  (dirty/non-repeatable reads, phantoms, lost updates); pick the level the correctness needs.
- Avoid **deadlocks** (consistent lock ordering, short transactions) and handle them with retry.
  Beware long-running transactions blocking others and bloating the DB.

## 5. Optimize (by measuring)
- **Read the execution plan** (`EXPLAIN [ANALYZE]`) to find the real cost — full scans where an
  index should help, expensive sorts, nested loops over big sets, bad row estimates (stale stats).
- **Index** the columns in `WHERE`/join/`ORDER BY`; mind **composite index column order**
  (leftmost-prefix) and covering indexes; don't wrap indexed columns in functions. Don't over-index
  (write cost). See `references/optimization.md`. Optimize the actual slow query on real-sized data.

## 6. Write safely & clearly
- **Parameterize** all input (prepared statements) — never concatenate user input into SQL
  (injection, and plan-cache churn). See `database-security`.
- **Scope writes:** always a `WHERE` on `UPDATE`/`DELETE` (test with a `SELECT` first); use a
  transaction so you can roll back. Avoid unbounded mass updates that lock hot tables — batch them.
- **Readable SQL:** consistent formatting, meaningful aliases, named columns (no `SELECT *` in app/
  reused queries), and comments on non-obvious logic. Note engine-specific syntax when portability matters.

## Operating principles
- **Think in sets,** not row-by-row loops.
- **Correct first:** right joins/grain, explicit NULL handling, no silent fan-out.
- **Window functions and CTEs** over procedural workarounds and tangled subqueries.
- **Short, correct transactions;** choose the isolation level deliberately; handle deadlocks.
- **Optimize by reading the plan** on real data; index the bottleneck, don't guess.
- **Parameterize always; scope every write;** keep SQL readable and maintainable.

## Related skills
- **database-design** — the schema and indexes your queries run against.
- **database-administration** — server-level tuning, stats, and resources behind query performance.
- **database-security** — parameterization and least privilege that keep data access safe.
- **data-engineering** — SQL in pipelines/ELT and at analytical scale.
- **data-analyst** — analytical SQL to answer questions and build reports.
