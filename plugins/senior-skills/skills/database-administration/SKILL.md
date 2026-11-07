---
name: database-administration
description: >-
  Operate and administer databases in production as a DBA: installation and
  configuration, backup and recovery, high availability and replication,
  performance tuning and monitoring, capacity planning, and patching/upgrades.
  Use when the user is running a production database and needs help with
  backup/restore strategy, replication/failover/HA, tuning a slow or overloaded
  database, configuring or sizing an instance, planning an upgrade/migration, or
  setting up monitoring and maintenance. Covers relational engines (PostgreSQL,
  MySQL, SQL Server, Oracle) and the operational principles common to all. For
  schema design use `database-design`; for writing SQL use `sql-development`; for
  securing the database use `database-security`; for pipelines use `data-engineering`.
---

# Database Administration (DBA)

Keeping databases **available, durable, performant, and recoverable** in production. The
database usually holds the most valuable, least-replaceable asset an organization has —
its data — so the DBA's prime directives are *don't lose it* and *keep it serving*.
Everything else (tuning, config) follows from those. Change carefully, test recovery, and
never trust a backup you haven't restored.

## Core responsibilities

1. **Install & configure** — right engine, version, and settings for the workload and hardware.
2. **Back up & recover** — a tested strategy that meets the required RPO/RTO. The #1 job.
3. **Ensure availability** — replication, HA, and failover so the DB survives failures.
4. **Tune & monitor performance** — find and fix bottlenecks; watch health continuously.
5. **Plan capacity & maintain** — storage/growth, routine maintenance, patches, and upgrades.
6. **Secure & control access** — least privilege, auditing (deep dive in `database-security`).

References (load the matching one):
- `references/backup-and-recovery.md` — backup types, RPO/RTO, PITR, and tested restores.
- `references/high-availability.md` — replication, failover, clustering, DR.
- `references/performance-tuning.md` — diagnosing and fixing DB performance at the server level.
- `references/operations.md` — config, monitoring, maintenance, capacity, upgrades/migrations.

## 1. Install & configure
- Choose the engine/version for the workload (OLTP vs. OLAP, scale, features) and support status
  — avoid end-of-life versions. Size memory, CPU, and **storage/IOPS** to the load.
- Configure deliberately: memory (buffer pool/shared_buffers/SGA), connections (and a
  **connection pooler** — don't let thousands of raw connections exhaust the server), checkpoint/
  WAL/redo, and durability settings. Don't run on defaults in production. See `references/operations.md`.
- Separate data/log/temp storage where it helps; plan the filesystem and growth.

## 2. Back up & recover (the prime directive)
- Design to the business **RPO** (how much data loss is tolerable) and **RTO** (how fast you must
  be back). These drive backup frequency, method, and HA. See `references/backup-and-recovery.md`.
- Combine full + incremental/differential + continuous log archiving for **point-in-time recovery**.
  Keep backups **off-box and off-site**, encrypted, with retention, and protected from deletion
  (ransomware/accident) — a backup an attacker or a bad script can delete isn't a backup.
- **Test restores regularly.** An untested backup is a hope, not a recovery plan. Rehearse the full
  recovery runbook and measure against RTO. Most "we have backups" stories end badly at restore time.

## 3. Ensure availability
- Use **replication** (synchronous for zero data loss but latency cost; asynchronous for
  performance but potential loss) for redundancy, read scaling, and failover. Understand
  replication lag and its consequences. See `references/high-availability.md`.
- Design failover (automatic or manual) and **test it**. Plan **disaster recovery** across
  failure domains/regions, not just a local replica. Match the HA topology to the RPO/RTO.
- Know the tradeoffs (CAP/consistency) and avoid split-brain in clusters.

## 4. Tune & monitor performance
- Tune by **measuring**, not guessing (`references/performance-tuning.md`): find the actual
  bottleneck — slow queries (capture them), missing/bad indexes, lock contention, I/O, memory
  pressure, or bad config — then fix the real one.
- Collaborate with developers on query/index issues (`sql-development`), but own the server-level
  levers (config, resources, maintenance). Keep statistics current and tables maintained.
- **Monitor continuously:** availability, replication lag, connections, cache hit ratio, slow
  queries, locks/blocking, disk space and growth, CPU/memory/IO, and backup success. Alert on
  symptoms before they become outages (especially **disk full** and **replication broken**).

## 5. Plan capacity & maintain
- Track growth and project storage/compute needs ahead of the wall. Running out of disk is a
  classic, avoidable, catastrophic outage.
- Routine maintenance: update statistics, manage index bloat/fragmentation and table bloat
  (e.g. vacuum/rebuild), archive/purge old data per retention, and manage partitions.
- **Patch and upgrade** on a plan: test in non-prod, read breaking changes, back up first, and
  have a rollback. Keep off EOL versions. See `references/operations.md`.

## 6. Secure & control access
- Enforce least-privilege accounts and roles, network restriction, encryption, and auditing.
  This is the DBA's duty but has its own depth — see the **database-security** skill.

## Operating principles
- **Protect the data first.** Durability and recoverability outrank everything; test restores.
- **A backup isn't real until you've restored it.** Rehearse recovery to the RTO.
- **Design to RPO/RTO.** Let the business tolerance drive backup and HA, not the other way around.
- **Change carefully.** Test in non-prod, back up before changes, have a rollback; production data
  is unforgiving. Avoid destructive operations without a verified backup.
- **Measure before tuning.** Find the real bottleneck; don't cargo-cult config.
- **Monitor and get ahead.** Disk, replication, and backups especially — alert before outage.
- **Least privilege and encryption** by default (see `database-security`).

## Related skills
- **database-design** — the schema/model the DB stores (normalization, keys, partitioning).
- **sql-development** — query and index tuning at the statement level; stored programs.
- **database-security** — hardening, access control, encryption, and auditing in depth.
- **data-engineering** — pipelines and data modeling that feed and use the database.
- **cloud-security** / **devops-infra** — managed-DB config, IaC, and operating in the cloud.
- **incident-response** — when a data-loss, corruption, or compromise event hits the database.
