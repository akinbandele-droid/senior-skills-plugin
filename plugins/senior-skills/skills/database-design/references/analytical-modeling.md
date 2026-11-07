# Analytical / Dimensional Modeling (OLAP)

Analytics has a different workload from transactions, so it needs a different model. OLTP is
optimized for many small reads/writes with integrity (normalized); **OLAP** is optimized for
large scans and aggregations (dimensional/denormalized). Don't force one schema to do both.

## OLTP vs. OLAP
| | OLTP | OLAP |
|---|---|---|
| Workload | Many small transactions | Few large analytical queries |
| Operations | Insert/update/delete + point reads | Big scans, aggregations, group-by |
| Model | Normalized (3NF) | Dimensional / denormalized (star) |
| Goal | Integrity, concurrency, write speed | Read/scan/aggregate speed |
Serve analytics from a separate analytical store/warehouse fed by pipelines (pair with
`data-engineering`), not by running heavy reporting on the production OLTP database.

## Star schema (the workhorse)
- **Fact table:** the measurements/events at a defined **grain** (one row = one sale line, one page
  view, one shipment). Holds numeric **measures** (amount, quantity) + **foreign keys** to dimensions.
  Keep facts narrow and additive where possible; be explicit about the grain.
- **Dimension tables:** the descriptive context you filter/group by (date, customer, product, store,
  channel). Wide, denormalized, with readable attributes. Shared ("conformed") dimensions let facts
  be compared consistently across the business.
- **Snowflake schema:** dimensions normalized into sub-tables — saves space but adds joins; usually
  star (denormalized dimensions) is preferred for simplicity and scan speed.

## Grain, measures, and additivity
- **Declare the grain first** — everything depends on it; mixing grains in one fact table causes
  double-counting. One grain per fact table.
- Classify measures: **additive** (sum across all dimensions — e.g. revenue), **semi-additive**
  (sum across some, not time — e.g. account balance), **non-additive** (ratios/percentages — don't
  sum; recompute from components). Handle each correctly in the model and in reporting.

## Slowly Changing Dimensions (SCD)
- Dimension attributes change (a customer moves, a product is recategorized). Decide how to handle
  history **per attribute**:
  - **Type 1:** overwrite (no history) — fine when history doesn't matter.
  - **Type 2:** add a new row with effective/expiry dates (or current flag) — preserves history; the
    most common for analytics. Facts point to the version valid at event time.
  - **Type 3:** keep limited prior value in a column (rarely).
- Choosing wrong (e.g. overwriting when the business needs point-in-time history) silently makes past
  reports wrong. Clarify history needs up front.

## Modern variations
- Columnar cloud warehouses (BigQuery/Snowflake/Redshift/ClickHouse) are very tolerant of **wide,
  denormalized "one big table"** models and heavy columns — often simpler and fast enough without a
  strict star. Choose star vs. flat by tooling, reuse, and query patterns.
- Keep a raw/landing layer, then modeled marts (medallion/layered approach) — see `data-engineering`.

## Common pitfalls
- Running analytics on the production OLTP DB (hurts both); normalizing a warehouse (slow, join-heavy).
- Undeclared or mixed grain → double-counting; summing non-additive/semi-additive measures wrongly.
- Ignoring SCD (overwriting history the business needed) → wrong historical reports.
- Over-engineering a snowflake when a simple star or wide table would serve.
