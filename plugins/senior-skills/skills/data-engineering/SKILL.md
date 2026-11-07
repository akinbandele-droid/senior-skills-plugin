---
name: data-engineering
description: >-
  Design and build the data layer with rigor: data and schema modeling,
  SQL and query performance, data pipelines (ETL/ELT), and data quality,
  integrity, and governance. Use when the user is designing a database schema,
  writing or optimizing SQL/queries, building or fixing a data pipeline or
  warehouse model, choosing a data store, or ensuring data correctness,
  consistency, and reliability. Covers relational and analytical stores,
  batch and streaming. For the services that use the data use
  `software-engineering`; for deploying data infra use `devops-infra`; for
  securing data use `cloud-security`.
---

# Data Engineering

Designing data models and moving data so it stays **correct, consistent, timely,
and usable**. Data outlives application code and is far harder to fix once wrong —
so model deliberately, enforce integrity at the source, and treat pipelines as
production software with tests and observability.

## Core loop

1. **Understand the data & use** — the entities, the questions to answer, access
   patterns, volume, velocity, and freshness needs.
2. **Choose the store & model** — relational vs. analytical vs. document/KV/stream;
   model for the workload (normalized OLTP vs. dimensional/wide OLAP).
3. **Enforce integrity** — constraints, keys, types, and transactions at the source of truth.
4. **Build pipelines** — reliable, idempotent, observable ETL/ELT; tested like code.
5. **Ensure quality** — validate, test, and monitor data; define quality SLAs.
6. **Operate & evolve** — migrations, performance, governance, and lineage over time.

References (load the matching one):
- `references/data-modeling.md` — relational & dimensional modeling, normalization, keys.
- `references/sql-and-performance.md` — writing correct SQL and tuning queries/indexes.
- `references/pipelines.md` — ETL/ELT, batch/stream, idempotency, orchestration.
- `references/data-quality.md` — integrity, validation, testing, observability, governance.

## 1. Understand the data & its use
- Model the **domain**: entities, relationships, cardinality, identifiers, and the real
  meaning/units of each attribute. Ambiguity here becomes corruption later.
- Capture **access patterns and questions**: what reads/writes happen, how often, latency
  and freshness requirements. Design serves the workload — OLTP and OLAP differ sharply.
- Note scale: volume, growth, velocity (batch vs. real-time), and retention.

## 2. Choose the store & model
- **Fit the store to the workload** (`references/data-modeling.md`): relational for
  transactional integrity and complex relationships; columnar/warehouse for analytics;
  document/KV/search/stream/time-series for their specific shapes. Don't force one store
  to do everything; don't reach for exotic stores without a reason.
- **Model deliberately:** normalize OLTP to remove redundancy and anomalies; use
  dimensional/star or wide denormalized models for analytics read speed. Choose keys
  (natural vs. surrogate) and define relationships explicitly.

## 3. Enforce integrity at the source
- Put constraints where they can't be bypassed: primary/foreign keys, NOT NULL, UNIQUE,
  CHECK, correct and precise types (money as decimal, times as timestamptz, enums).
- Use **transactions** for multi-step consistency; understand isolation levels and the
  anomalies they allow. Make writes atomic; avoid partial updates.
- Integrity in the database is a backstop that application bugs can't defeat.

## 4. Build pipelines
- Make them **idempotent and re-runnable** (reprocessing a window gives the same result),
  with clear ownership of late/duplicate/out-of-order data. See `references/pipelines.md`.
- Prefer **ELT** into a capable warehouse where it simplifies, but keep transformations
  tested and versioned. Orchestrate with dependencies, retries, and alerting.
- Handle schema changes and backfills deliberately; never silently drop/reinterpret data.

## 5. Ensure quality
- Validate at ingestion and between stages (schema, ranges, referential integrity,
  freshness, volume anomalies). Test transformations like code. See `references/data-quality.md`.
- Define quality expectations (completeness, accuracy, timeliness, uniqueness) and
  **monitor** them; alert on breaches. Bad data detected late erodes all trust downstream.

## 6. Operate & evolve
- **Migrations** are expand/contract and reversible where possible; back up before
  destructive changes; test on real-shaped data. Never hand-edit prod data without a backup.
- Tune performance by measuring (`references/sql-and-performance.md`): indexes, query
  plans, partitioning. Track **lineage** and document datasets; apply governance and
  access controls (pair with `cloud-security`).

## Operating principles
- **Model for the workload** — OLTP and OLAP are different jobs; serve the real questions.
- **Integrity at the source of truth** — constraints and transactions, not hope.
- **Idempotent, tested pipelines** — reprocessing is safe; data code is production code.
- **Quality is measured, not assumed** — validate and monitor; fail loudly on bad data.
- **Data is forever and shared** — changes are careful, reversible, backed up, documented.
- **Protect sensitive data** — classify, minimize, control access; don't leak PII downstream.

## Related skills
- **software-engineering** — the application code reading/writing the data.
- **devops-infra** — provisioning, deploying, and operating data stores and pipelines.
- **cloud-security** — encrypting, access-controlling, and governing the data.
- **data-analyst** — analyzing the modeled data to answer questions and drive decisions
  (EDA, statistics, insight); this skill builds and serves the data it works on.
- **database-design** / **sql-development** / **database-administration** / **database-security**
  — the dedicated database family: schema design, SQL craft, running the DB in production, and
  securing it; use them for deep database work this skill touches at a higher level.
- **data** (`/data`) — analysis, querying, and visualization once the data is modeled.
