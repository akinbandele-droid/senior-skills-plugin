# Database Auditing & Activity Monitoring

You can't respond to what you can't see. Auditing records security-relevant activity for
forensics and compliance; monitoring watches it in near-real-time to catch misuse and
exfiltration. Both must survive an attacker who gains database admin.

## What to audit
Capture security-relevant events (balance coverage against volume/performance):
- **Authentication:** failed logins (brute force), successful privileged/admin logins, new-source logins.
- **Privilege & schema changes:** `GRANT`/`REVOKE`, role/user creation, `ALTER`/`DROP` (DDL) —
  attackers create accounts, escalate, and alter structures.
- **Access to sensitive data:** reads/exports of classified tables/columns (for exfil detection and
  compliance). Bulk `SELECT`/export especially.
- **Admin actions & config changes:** security setting changes, audit being disabled (a red flag),
  backup/restore operations.
- Use the engine's native audit facility (pgAudit, MySQL audit, SQL Server Audit, Oracle Unified
  Audit) and/or a database activity monitoring (DAM) tool.

## Protect the audit trail
- **Ship logs off the database** to a separate, access-controlled, **tamper-resistant/immutable**
  store (central log platform / SIEM). An attacker with DB admin can otherwise delete or edit local
  logs to cover tracks — so the authoritative copy must be out of their reach.
- Ensure time sync (accurate timestamps) and sufficient retention for forensics and compliance.
- Alert if auditing is **disabled or stops** — that itself is a strong indicator of compromise.

## Monitoring & detection
Watch for misuse patterns and route to detection/response (`cyber-security-analyst`):
- **Exfiltration signals:** unusual query volume, bulk reads/exports, access to many sensitive rows,
  a normally-narrow account suddenly scanning tables.
- **Abuse/recon:** off-hours admin access, access from new hosts/geos, repeated permission-denied or
  failed logins, enumeration of schema.
- **Privilege misuse:** use of dangerous privileges, new superuser/admin accounts, grants outside change control.
- Baseline "normal" per account/app so anomalies stand out; tune to actionable alerts, not noise.

## Compliance dimension
- PCI, HIPAA, SOX, and GDPR expect audit trails of access to regulated data and of administrative
  activity. Auditing is both a detective control and audit **evidence** — coordinate retention and
  scope with `grc-analyst`.

## Operational care
- Auditing has a performance/volume cost — audit what matters (security-relevant + regulated-data
  access), not every statement, or you'll drown in logs and slow the DB. Review and tune what's captured.

## Common pitfalls
- No auditing, or logs kept only locally where a DB admin/attacker can wipe them.
- Auditing everything → performance hit and unusable noise; or auditing nothing useful.
- No monitoring/alerting on the logs (collected but never watched) — exfil goes unnoticed for months.
- No alert when auditing is turned off; clocks unsynced; retention too short for forensics.
- Treating audit as compliance-checkbox only, never feeding detection/response.
