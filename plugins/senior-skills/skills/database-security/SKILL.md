---
name: database-security
description: >-
  Secure databases specifically: access control and least privilege, encryption
  (at rest, in transit, and column/field level), auditing and activity
  monitoring, injection and misconfiguration hardening, sensitive-data
  protection, and secrets handling. Use when the user is hardening a database,
  setting up database roles/permissions, encrypting data, enabling audit logging,
  protecting PII/PHI/cardholder data in a database, preventing SQL injection at
  the data layer, or reviewing a database's security posture. Complements
  `cloud-security` (the surrounding environment) with database-specific depth.
  For running the DB use `database-administration`; for writing queries use
  `sql-development`; for the data model use `database-design`.
---

# Database Security

Databases concentrate an organization's most sensitive data, which makes them the prime
target — and a single misconfiguration or over-privileged account can expose everything.
Securing a database is layered: control **who** can reach and do what (access), protect
the data itself (encryption), **watch** what happens (auditing), and remove the weaknesses
attackers exploit (hardening). Defense in depth, least privilege, and "assume the network
is hostile" throughout.

## Core loop

1. **Know the data** — what sensitive/regulated data lives here and who legitimately needs it.
2. **Control access** — authentication, least-privilege authorization, network isolation.
3. **Protect the data** — encryption in transit, at rest, and (for the crown jewels) in the DB.
4. **Audit & monitor** — log security-relevant activity; detect misuse and exfiltration.
5. **Harden** — secure configuration, patching, injection defense, remove default exposure.
6. **Review & maintain** — posture reviews, access recertification, and keeping it current.

References (load the matching one):
- `references/access-control.md` — authentication, roles, least privilege, network isolation.
- `references/encryption.md` — in transit, at rest, column/field-level, TDE, key management.
- `references/auditing-and-monitoring.md` — audit logging, activity monitoring, detection.
- `references/hardening-and-injection.md` — secure config, SQL-injection defense, misconfig removal.

## 1. Know the data (scope the risk)
- Inventory and **classify** what's stored: PII, PHI, cardholder data (PCI), credentials, secrets,
  financial/IP. The sensitivity drives how strongly it must be controlled, encrypted, and audited.
- Know who *should* access what, and the regulatory obligations (GDPR/HIPAA/PCI — coordinate with
  `grc-analyst`). Minimize: don't store sensitive data you don't need; apply retention/disposal.

## 2. Control access (least privilege)
- **Authenticate** strongly: no shared accounts, strong/rotated credentials or (better) integrated/
  IAM/certificate auth; MFA for human admin access. Disable or rename default/sample accounts and
  change default passwords. See `references/access-control.md`.
- **Authorize least privilege:** grant the minimum rights via **roles**, not broad admin. The app
  account should not be a superuser; separate read vs. write vs. admin; restrict dangerous
  privileges (DDL, `FILE`, admin functions). Give each app/service its own scoped account.
- **Isolate the network:** the database should **never be directly reachable from the internet**.
  Put it in a private subnet; restrict by security group/firewall to only the hosts that need it;
  require TLS. Access via bastion/VPN/private endpoint only.

## 3. Protect the data (encryption)
- **In transit:** enforce TLS for all client and replication connections; reject unencrypted ones.
- **At rest:** enable encryption at rest (TDE / volume / managed encryption) with proper **key
  management** (rotation, access control, separation of duties — the key must not be trivially
  accessible to whoever has the data). Encrypt backups too.
- **Column/field-level** for the crown jewels (e.g. tokenize or encrypt card numbers, secrets),
  so even DB-level access doesn't reveal them in clear. Hash passwords with a strong, salted
  algorithm — never store or reversibly encrypt them. See `references/encryption.md`.

## 4. Audit & monitor
- Enable **audit logging** for security-relevant events: admin actions, privilege/schema changes,
  failed and privileged logins, and access to sensitive data. Send logs to a **separate, tamper-
  resistant** store (an attacker with DB admin shouldn't be able to erase the trail).
- **Monitor** for misuse: anomalous query volume/patterns, bulk reads/exports (exfiltration),
  off-hours admin access, repeated failures. Feed alerts into detection/response
  (`cyber-security-analyst` / `incident-response`). See `references/auditing-and-monitoring.md`.

## 5. Harden
- **Secure configuration:** remove default/demo databases and accounts, disable unused features/
  extensions and remote admin, set secure defaults, and follow a benchmark (CIS) for the engine.
- **Patch** promptly — DB engines get critical CVEs; stay supported and current (coordinate with
  `database-administration`).
- **Defend against SQL injection** at the data layer: parameterized queries/prepared statements
  are the real fix (app side), plus least-privilege app accounts and limited error disclosure to
  contain impact. See `references/hardening-and-injection.md`.

## 6. Review & maintain
- Periodic posture reviews against a benchmark; **recertify access** (remove stale accounts/grants,
  especially for departed staff and old apps); verify encryption, audit, and backup protections.
- Keep secrets (DB credentials, keys) in a **secrets manager**, never in code/config; rotate them.
  Treat database security as ongoing, not a one-time setup.

## Operating principles
- **Least privilege, always.** No app-as-superuser; scoped roles; restrict dangerous rights.
- **Never expose the database to the internet.** Private network, TLS, tight firewalling.
- **Encrypt in transit and at rest; protect the keys.** Field-level for the crown jewels; hash passwords.
- **Assume breach — audit and watch.** Tamper-resistant logs; alert on exfiltration patterns.
- **Harden and patch.** Remove defaults, follow a benchmark, stay current.
- **Parameterize to kill injection;** least privilege limits the blast radius when app code slips.
- **Minimize and protect sensitive data;** classify it, and manage secrets in a vault.

## Related skills
- **cloud-security** — the surrounding environment: IAM, network, KMS, and account posture.
- **database-administration** — operating the DB, including applying patches and backups securely.
- **sql-development** — parameterized queries and safe data access that prevent injection.
- **security-review** / **pentest** — reviewing and testing the database's security (authorized).
- **grc-analyst** — mapping DB controls to PCI/HIPAA/GDPR and producing audit evidence.
- **incident-response** — when the database is breached or data is exfiltrated.
