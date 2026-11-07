# Database Programmability: Views, Procedures, Functions, Triggers

Logic that lives in the database. Used well, it keeps data-intensive work close to the data
and provides a controlled interface; used poorly, it hides behavior, scatters business logic,
and creates performance surprises. Choose deliberately.

## Views
- A named, stored query. Use to **encapsulate and reuse** complex logic, present a simplified or
  standardized shape, and **restrict access** (expose only certain columns/rows to a role — a
  security tool, see `database-security`).
- Regular views run their query each time (no stored data) — composing many layered views can
  produce monstrous plans; keep nesting sane.
- **Materialized views** store the result for expensive, frequently-read queries — fast reads at
  the cost of **staleness** and a refresh strategy (on-demand/scheduled/incremental). Use for
  dashboards/reports; know the refresh cost and freshness tradeoff.

## Stored procedures & functions
- **Good uses:** set-based, data-intensive operations that would be slow or chatty if done by
  shipping rows to the app (bulk transforms, batch jobs); a controlled, parameterized API to the
  data; encapsulating multi-statement transactional logic.
- **Functions** (esp. scalar) called per-row in queries can be a hidden performance killer — they
  run for every row and often block optimization/parallelism. Prefer set-based expressions or
  inline/table-valued functions; be wary of scalar UDFs in `WHERE`/`SELECT` over big sets.
- **Keep logic placed sensibly.** Data-close operations belong in the DB; sprawling business rules
  spread across hundreds of procedures become unversioned, untestable, and hard to maintain. Balance
  with application logic; version your DB code (migrations), and test it.
- Always **parameterize** inside procedures; dynamic SQL built by concatenation inside a proc is
  just as injectable (see `database-security`).

## Triggers (use sparingly)
- Code that fires automatically on INSERT/UPDATE/DELETE. Powerful but **hidden** — they cause side
  effects people don't see when reading the DML, can cascade (trigger invoking trigger), and add
  per-row overhead that silently slows writes.
- Legitimate uses: audit trails, enforcing an invariant that can't be a constraint, maintaining a
  derived value. Avoid putting significant business logic in triggers — it surprises developers and
  is hard to debug.
- If used: keep them simple, fast, and well-documented; beware ordering and recursion; consider
  whether a constraint, default, generated column, or application logic is clearer.

## Constraints over code
- Prefer **declarative constraints** (PK/FK/UNIQUE/CHECK/NOT NULL, generated columns) to enforce
  rules — the engine guarantees them cheaply and they can't be bypassed, unlike trigger/app logic.
  Reach for programmability only when declarative options can't express the rule. (See `database-design`.)

## Error handling & transactions
- Handle errors in procedures (TRY/CATCH, exception blocks); don't leave a multi-step change
  half-applied — wrap in a transaction and roll back on failure (see `transactions.md`).

## Common pitfalls
- Scalar UDFs / per-row functions over large sets → severe, hidden slowdowns.
- Business logic buried in triggers → surprising side effects, hard to debug, perf hits.
- Deeply nested views generating unoptimizable plans; materialized views with no refresh plan.
- DB code not in version control / not tested; concatenated dynamic SQL inside procs (injection).
- Using triggers/procedures for rules a simple constraint would enforce better.
