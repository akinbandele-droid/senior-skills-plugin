# Transactions, Isolation & Concurrency

When many sessions read and write at once, correctness depends on transactions and isolation.
Get this wrong and you get lost updates, double-processing, or deadlocks under load — bugs
that never show up in single-user testing.

## ACID & transactions
- A transaction groups statements into an **atomic** unit: all commit or all roll back. Use one for
  any multi-statement change that must be consistent (e.g. debit one account, credit another).
- **A**tomicity, **C**onsistency, **I**solation, **D**urability. Wrap related writes; commit on
  success, roll back on error. Handle errors so you never leave a half-applied change.
- **Keep transactions short.** Long transactions hold locks (blocking others), prevent cleanup/
  vacuum (bloat), and raise deadlock odds. Don't do slow app work or wait on user input mid-transaction.

## Isolation levels (and the anomalies they allow)
From weakest to strongest:
- **Read Uncommitted** — can see others' uncommitted changes (**dirty reads**). Rarely appropriate.
- **Read Committed** (common default) — sees only committed data, but a row can change between reads
  (**non-repeatable reads**) and new rows can appear (**phantoms**).
- **Repeatable Read** — same rows read twice are stable; phantoms may still occur (engine-dependent;
  some prevent them).
- **Serializable** — behaves as if transactions ran one at a time; prevents all these anomalies, at
  the cost of more blocking/serialization failures (retry needed).
- Pick the level the **correctness** needs — stronger isn't free (less concurrency). Many engines use
  MVCC so readers don't block writers; understand your engine's actual behavior per level.

## The lost-update trap
- Read-modify-write races: two sessions read a value, both update based on the stale read → one
  update is lost. Fix with:
  - **Atomic in-DB update** (`UPDATE … SET n = n + 1`) instead of read-then-write in the app.
  - **Optimistic concurrency:** a version/timestamp column; update `WHERE version = @read_version`
    and detect 0 rows affected (someone else changed it) → retry.
  - **Pessimistic locking:** `SELECT … FOR UPDATE` to lock the row while you work (hold briefly).
- This is one of the most common real-world concurrency bugs — handle it explicitly.

## Locking & deadlocks
- Writes take locks; understand row vs. table-level locking and what your statements lock.
- **Deadlock:** two transactions each hold a lock the other needs → the engine kills one. Reduce by
  **acquiring locks in a consistent order**, keeping transactions short, and touching rows in a
  predictable sequence. **Expect deadlocks under load and retry** the victim transaction.
- Beware lock escalation and long blockers; monitor blocking (pair with `database-administration`).

## Patterns
- **Idempotency** for retried operations (so a retry after an unknown outcome doesn't double-apply) —
  natural keys / dedup / "insert if not exists". Essential with at-least-once messaging and retries.
- Use `SELECT … FOR UPDATE SKIP LOCKED` for queue-style work distribution.
- Make writes safe: scope every `UPDATE`/`DELETE` with `WHERE`, run in a transaction, batch large
  writes to avoid long locks.

## Common pitfalls
- No transaction around multi-step changes → half-applied, inconsistent data on error.
- Read-modify-write in app code → lost updates under concurrency.
- Wrong isolation assumption (expecting Read Committed to prevent phantoms).
- Long transactions holding locks → blocking, bloat, deadlocks.
- Not retrying deadlock/serialization-failure victims; inconsistent lock ordering causing deadlocks.
- Unbounded mass `UPDATE`/`DELETE` locking a hot table.
