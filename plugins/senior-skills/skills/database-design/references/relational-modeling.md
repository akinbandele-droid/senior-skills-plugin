# Relational Modeling: Normalization, Keys, Constraints

The relational core: organize data so each fact lives once, relationships are explicit, and
the schema itself guarantees correctness. Normalize first; denormalize only with a reason.

## Normalization
Removing redundancy to eliminate update/insert/delete anomalies (where the same fact stored in
multiple places can disagree).
- **1NF:** atomic values, no repeating groups; each cell one value, each row unique.
- **2NF:** 1NF + no partial dependency on part of a composite key (every non-key attribute depends on
  the whole key).
- **3NF:** 2NF + no transitive dependency (non-key attributes depend only on the key, not on other
  non-key attributes). **3NF is the practical baseline** for OLTP. (BCNF tightens 3NF for certain
  key overlaps.)
- The rule of thumb: *every non-key column depends on the key, the whole key, and nothing but the key.*
- Benefits: one place to update each fact, no contradictory duplicates, smaller/cleaner data.

## Deliberate denormalization
- Introduce controlled redundancy (duplicated columns, precomputed aggregates) **only** for a proven
  read-performance need. Accept the cost: you must now keep the copies in sync (triggers/app/batch),
  and risk inconsistency. Document why. Default to normalized; denormalize as an optimization, not a
  starting point. For analytics, prefer a proper dimensional model over denormalizing OLTP.

## Keys
- **Primary key:** uniquely identifies each row; every table should have one. Options:
  - **Surrogate** (generated — auto-increment/identity/UUID): stable, meaningless, decoupled from
    business changes. UUIDs are good for distributed/merge scenarios (mind index locality — prefer
    time-ordered UUIDv7/ULID over random UUIDv4 for clustered indexes).
  - **Natural** (a real-world identifier): carries meaning, enforces genuine uniqueness — but
    real-world identifiers change and aren't always unique/stable.
  - Common best practice: **surrogate PK + a UNIQUE constraint on the natural key** — stable
    references *and* real-world uniqueness enforced.
- **Foreign keys:** define every relationship with an FK to the referenced PK; choose `ON DELETE`/
  `ON UPDATE` behavior deliberately (`RESTRICT`/`CASCADE`/`SET NULL`) — `CASCADE` is powerful and
  dangerous. FKs prevent orphaned/dangling references.

## Relationships
- **One-to-many:** FK on the many side. **One-to-one:** shared/served by FK + UNIQUE (or same PK).
- **Many-to-many:** a **junction/association table** with FKs to both sides (and any relationship
  attributes). Never fake it with comma-separated lists in a column (breaks 1NF and querying).
- Model hierarchies (adjacency list, nested set, closure table, or `ltree`/recursive CTE) per query needs.

## Constraints & types (integrity by design)
- Enforce rules in the schema: `NOT NULL`, `UNIQUE`, `CHECK` (ranges, allowed values, cross-column
  rules), FKs, and generated/computed columns. These make invalid states impossible regardless of app bugs.
- **Honest types:** `decimal`/`numeric` for money (float causes rounding errors), `timestamptz`
  (store UTC; render per timezone), real date/time types (not strings), booleans (not `'Y'/'N'`),
  enums/lookup tables for fixed sets, right-sized text, and native JSON only where schemaless data
  truly belongs (don't use a JSON blob to avoid modeling).
- Decide NULL semantics per column (unknown vs. not-applicable) and forbid it where it's meaningless.

## Common pitfalls
- No primary key / no foreign keys → duplicate and orphaned rows.
- Repeating groups or comma-separated lists instead of a related table (breaks 1NF).
- Denormalizing from the start "for speed" → anomalies and sync bugs.
- Floats for money; strings for dates/numbers/booleans; JSON blobs to dodge modeling.
- Natural key as PK that later changes, cascading edits everywhere; random UUID PK hurting index locality.
- Validation only in the app, none in the schema → corrupt data when any path misbehaves.
