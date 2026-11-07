# Choosing a Data Store

Fit the store to the workload and access pattern. Relational is the sensible default; reach
for a specialized store when the data's shape, scale, or access genuinely demands it — not for
novelty. Many systems are **polyglot** (each store doing what it's best at).

## Relational (RDBMS) — the default
- **Best for:** systems of record, complex relationships, transactional integrity (ACID), and
  flexible ad-hoc querying (joins, aggregation). PostgreSQL, MySQL, SQL Server, Oracle.
- Strong schema and constraints enforce correctness; SQL is powerful and portable-ish. Modern RDBMSs
  also handle JSON, full-text, and scale well vertically and with read replicas/partitioning.
- **Choose it unless you have a specific reason not to.** Most "we need NoSQL for scale" cases are
  solved by proper indexing, a read replica, or partitioning first.

## Document stores
- **Best for:** aggregate-oriented data accessed mostly by key/document, flexible/evolving or nested
  structures, and when you rarely need cross-entity joins. MongoDB, DynamoDB (doc-ish), Couchbase.
- You model around **access patterns** (embed what you read together) rather than normalize. Tradeoffs:
  weaker/looser cross-document consistency and joins; duplication you must manage; easy to create a
  mess if the access patterns aren't well understood up front. Not a shortcut around data modeling —
  just a different modeling discipline.

## Key-value stores
- **Best for:** simple, fast lookups by key — caching, sessions, feature flags, counters. Redis,
  Memcached, DynamoDB. Minimal query ability by design; extremely fast and scalable for that shape.

## Wide-column stores
- **Best for:** massive write/read throughput over huge datasets with known query patterns, often
  time-series/event data at scale. Cassandra, HBase, Bigtable. Designed around the queries; limited
  ad-hoc querying; tunable consistency.

## Graph databases
- **Best for:** data where **relationships are first-class** and you traverse them (social graphs,
  recommendations, fraud rings, networks/dependencies). Neo4j, etc. Traversals that would be painful
  multi-join SQL become natural; not for bulk tabular analytics.

## Search engines
- **Best for:** full-text search, relevance ranking, faceting, log/analytics search. Elasticsearch/
  OpenSearch. Usually a secondary index alongside a system-of-record DB, not the source of truth.

## Time-series databases
- **Best for:** high-volume timestamped metrics/events with time-range queries and downsampling/
  retention. InfluxDB, TimescaleDB, Prometheus. (Or partition a relational table by time.)

## Analytical / columnar (OLAP) — see analytical-modeling.md
- **Best for:** large scans and aggregations over big datasets (warehouses/analytics). Snowflake,
  BigQuery, Redshift, ClickHouse, DuckDB. Columnar storage; not for high-frequency single-row OLTP.

## How to choose
- Start from the **access patterns, consistency needs, scale, and query flexibility** required.
- Relational unless a specific need (extreme scale of a known pattern, relationship traversal,
  full-text, schemaless aggregates, time-series) points elsewhere. Consider operational cost and team
  familiarity. Favor fewer stores; add one only when it clearly earns its keep. Consistency model
  (strong vs. eventual) must match the use case (don't put money on eventual consistency casually).

## Common pitfalls
- Choosing NoSQL for "scale" before exhausting indexing/replicas/partitioning on a RDBMS.
- Treating a document store as schemaless freedom → inconsistent, un-queryable data; no access-pattern design.
- Using a KV/wide-column store then needing ad-hoc joins/queries it can't do.
- Too many stores (operational burden, sync/consistency headaches) without justification.
- Accepting eventual consistency where the use case needs strong consistency.
