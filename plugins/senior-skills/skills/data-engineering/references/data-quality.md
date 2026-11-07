# Data Quality, Integrity & Governance

Trust in data is easy to lose and hard to regain. Validate at the source, test like
code, monitor continuously, and govern access.

## Dimensions of quality
- **Completeness** (nothing missing), **accuracy** (matches reality), **consistency**
  (agrees across systems), **timeliness/freshness** (current enough), **uniqueness**
  (no unintended duplicates), **validity** (conforms to rules/types/ranges).
- Define expected thresholds per dataset — a quality SLA — and measure against them.

## Enforce integrity at the source
- Database constraints (keys, FKs, NOT NULL, UNIQUE, CHECK, types) are the strongest,
  unbypassable guard — use them. Application-only validation gets bypassed.
- Transactions for multi-step consistency; avoid partial/dirty writes.

## Validate in pipelines
- **Validate on ingest and between stages:** schema conformance, type/range checks,
  referential integrity, allowed-value sets, row-count/volume sanity, freshness.
- **Route bad data** to quarantine/dead-letter with a reason, rather than dropping it
  silently or letting it poison downstream. Make rejects visible and countable.
- **Test transformations** like code: unit tests on logic, and assertion tests on outputs
  (e.g. dbt tests / Great-Expectations-style checks: not-null, unique, accepted values,
  relationships).

## Monitor & observe
- Track quality metrics over time; alert on anomalies (a sudden row-count drop, freshness
  breach, spike in nulls/rejects). Detect silent failures, not just crashes.
- Reconcile against source-of-truth counts/totals periodically.

## Governance & lineage
- **Lineage:** know where each dataset comes from and what depends on it — essential for
  impact analysis and debugging. Document datasets (meaning, owner, grain, freshness).
- **Access & privacy:** classify sensitive/PII data; apply least-privilege access,
  masking/tokenization where needed, and retention/deletion policies. Don't propagate
  PII into logs, non-prod, or analytics without controls — pair with `cloud-security`.
- Clear ownership per dataset; a catalog so people find and trust data.

## Migrations & changes to live data
- Expand/contract, reversible where possible; **back up before destructive changes**;
  test on production-shaped data. Never hand-edit prod without a backup and a change record.

## Common pitfalls
- Validation only in the app; none in the database or pipeline.
- Dropping bad rows silently; no reject visibility or count.
- No freshness/volume monitoring → stale/partial data trusted for days.
- No lineage → nobody knows what breaks when a source changes.
- PII sprawling into logs, non-prod, and dashboards unmasked.
