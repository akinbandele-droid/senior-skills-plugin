---
name: database-design
description: >-
  Design database schemas and data models that are correct, performant, and
  evolvable: conceptual/logical/physical modeling, normalization and deliberate
  denormalization, keys and relationships, constraints and integrity, indexing
  strategy, choosing the right store (relational vs. NoSQL), dimensional/
  analytical models, and schema evolution/migrations. Use when the user is
  designing or reviewing a schema, modeling a domain, choosing primary/foreign
  keys, deciding normalization vs. denormalization, picking a database type, or
  planning schema changes/migrations. Goes deeper than data-engineering's
  modeling overview. For writing SQL use `sql-development`; for operating the DB
  use `database-administration`; for pipelines use `data-engineering`.
---

# Database Design

The schema is the hardest thing to change once data and code depend on it, so design it
deliberately. A good model makes correct data easy and incorrect data impossible (via
constraints), fits how the data is actually accessed, and can evolve without downtime. The
core discipline: **model the domain truthfully, enforce integrity at the schema, and shape
for the workload.**

## Core loop

1. **Model the domain** — entities, attributes, relationships, and their real meaning and rules.
2. **Choose the store** — relational vs. document/KV/graph/column/search, by workload fit.
3. **Design the schema** — normalize, choose keys, define relationships, pick types.
4. **Enforce integrity** — constraints that make bad data impossible at the schema level.
5. **Shape for the workload** — indexing strategy, access patterns, deliberate denormalization.
6. **Plan for change** — safe, reversible schema evolution and migrations.

References (load the matching one):
- `references/relational-modeling.md` — normalization, keys, relationships, constraints, types.
- `references/choosing-a-store.md` — relational vs. NoSQL families and when each fits.
- `references/analytical-modeling.md` — dimensional/star schemas and OLAP vs. OLTP design.
- `references/evolution-and-migrations.md` — schema change, versioned migrations, zero-downtime.

## 1. Model the domain
- Start conceptual: the **entities** (things), their **attributes** (with real meaning, units,
  optionality), and the **relationships** between them (one-to-one, one-to-many, many-to-many) with
  honest cardinality and the business rules that govern them.
- Capture rules the data must always obey (a person has exactly one birthdate; an order has ≥1 line;
  an email is unique) — these become constraints. Ambiguity modeled now becomes corruption later.
- Work from conceptual → logical (tables/columns/keys, engine-agnostic) → physical (engine-specific
  types, indexes, storage). Don't jump to physical before the logical model is sound.

## 2. Choose the store (fit the workload)
- **Relational** is the default for systems of record — integrity, relationships, flexible querying,
  transactions. Reach for **document / key-value / graph / columnar / search / time-series** only
  when the access pattern genuinely calls for it. See `references/choosing-a-store.md`.
- NoSQL trades (often giving up joins, ad-hoc queries, or strong consistency) for scale or a specific
  shape — choose it for a reason, not novelty. Polyglot persistence is fine when each store earns its place.
- In document/NoSQL models you design around **access patterns and queries** (model for how you read),
  which is a different discipline from relational normalization.

## 3. Design the schema (relational)
- **Normalize** to remove redundancy and update/insert/delete anomalies — aim for **3NF** as the
  baseline so each fact lives in exactly one place. See `references/relational-modeling.md`.
- **Keys:** choose a primary key deliberately — surrogate (generated, stable, decoupled from meaning)
  and/or natural (carries meaning, enforces real-world uniqueness); often a surrogate PK plus a unique
  constraint on the natural key. Define **foreign keys** for every relationship.
- **Types:** precise and honest — `decimal`/`numeric` for money (never float), `timestamptz` for
  instants (store UTC), proper date/time, enums/constrained sets, right-sized strings, booleans not flags.
- Model many-to-many with a join/association table; model inheritance/variants deliberately.

## 4. Enforce integrity at the schema
- Put rules where they **can't be bypassed** by any app or script: `PRIMARY KEY`, `FOREIGN KEY`
  (with sensible `ON DELETE`/`UPDATE`), `UNIQUE`, `NOT NULL`, `CHECK`, correct types, and generated
  columns. The database is the last line of defense for data correctness — use it.
- Prefer declarative constraints over application-only validation or triggers (see `sql-development`).
  A constraint is cheap, guaranteed, and self-documenting.

## 5. Shape for the workload
- Know the **access patterns** (the queries, their frequency, read/write ratio, latency needs) and
  design to serve them: the right **indexes** (on filter/join/sort columns; composite order matters —
  pair with `sql-development`'s optimization reference), and partitioning for very large tables.
- **Denormalize deliberately** only for a proven read-performance need, accepting and managing the
  consistency cost knowingly — never as a default. For heavy analytics, use an analytical model
  (`references/analytical-modeling.md`) rather than contorting the OLTP schema.
- Design doesn't end at correctness — a 3NF schema that ignores the query patterns can still be slow.

## 6. Plan for change
- Schemas evolve. Use **versioned, reviewed migrations** (as code, in source control) — never ad-hoc
  manual prod changes. Make changes **backward-compatible (expand/contract)** so deploys and rollbacks
  don't break running code or require downtime. See `references/evolution-and-migrations.md`.
- Protect data on destructive changes (back up first; `prevent-destroy` on stateful objects; test on
  prod-shaped data). Coordinate with `database-administration` for lock impact and timing.

## Operating principles
- **The schema is expensive to change — design it deliberately,** conceptual→logical→physical.
- **Model the domain truthfully;** honest types, relationships, and cardinality.
- **Make bad data impossible:** enforce integrity with constraints at the schema, not just the app.
- **Normalize by default (3NF); denormalize only for a proven need,** cost acknowledged.
- **Fit the store and the schema to the workload** and its access patterns, not to fashion.
- **Evolve safely:** versioned migrations, backward-compatible (expand/contract), backed up, reversible.

## Related skills
- **sql-development** — querying and indexing the schema you design.
- **database-administration** — operating, partitioning, and migrating the schema in production.
- **database-security** — access control (views, RLS, column grants) designed into the schema.
- **data-engineering** — data modeling in pipelines/warehouses (this skill is the deeper dive).
- **software-engineering** — the application and ORM/migrations that depend on the schema.
