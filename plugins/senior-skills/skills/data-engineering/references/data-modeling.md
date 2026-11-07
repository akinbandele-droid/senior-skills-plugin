# Data Modeling

Model the domain and the workload deliberately — the schema is the hardest thing
to change later.

## Fit the store to the workload
- **Relational (OLTP):** transactional integrity, complex relationships, many small
  reads/writes. The default for systems of record.
- **Analytical/columnar (OLAP, warehouse):** large scans, aggregations, few wide rows read.
- **Document:** flexible/nested, aggregate-oriented access by key; weaker cross-entity joins.
- **Key-value / cache:** fast lookups by key; minimal query.
- **Search:** full-text and relevance. **Time-series:** append-heavy metrics/events.
  **Graph:** relationship-centric traversal. **Stream:** ordered event logs.
- Don't force one store to do all jobs; don't adopt exotic stores without a workload reason.
  Polyglot persistence is fine when each store earns its place.

## Relational / normalization (OLTP)
- Normalize to remove redundancy and update/insert/delete anomalies (aim for 3NF as a
  baseline). One fact in one place → updates can't make the data inconsistent.
- Denormalize **selectively** for proven read performance, accepting the consistency cost
  knowingly (and keep it maintained).
- **Keys:** choose primary keys deliberately. Surrogate keys (generated) decouple from
  changing natural keys; natural keys carry meaning and prevent duplicates. Often use both
  (surrogate PK + unique natural key).
- Define relationships with foreign keys and correct cardinality. Model many-to-many with
  join tables.

## Dimensional / analytical (OLAP)
- **Star schema:** central fact tables (measurements, foreign keys to dimensions) +
  dimension tables (descriptive attributes). Optimized for slicing and aggregation.
- Decide grain explicitly (one row = ?). Handle **slowly changing dimensions** (keep history
  vs. overwrite) intentionally. Wide denormalized tables are common and good for scan speed.

## Types & semantics
- Use precise types: decimal/numeric for money (never float), `timestamptz` for instants
  (store UTC), proper date/time, enums/constrained sets, correct string sizing.
- Encode units and meaning (document them); NULL means "unknown/not applicable" — decide
  its meaning per column and constrain where it shouldn't occur.

## Common pitfalls
- Over-normalizing a warehouse (slow analytics) or under-normalizing OLTP (anomalies).
- Floats for money; naive timestamps with lost timezones; stringly-typed everything.
- No keys/constraints → duplicate and orphaned rows.
- Choosing a trendy store that doesn't fit the access pattern.
- Unclear grain/SCD handling in analytical models.
