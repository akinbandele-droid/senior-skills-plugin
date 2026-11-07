# Database Access Control

Who can connect, authenticate, and do what. Over-privileged accounts and network exposure
are the two failures behind most database breaches — fix those first.

## Authentication
- **No shared accounts.** Each human and each application/service gets its own identity, so actions
  are attributable and revocable. Name service accounts per app.
- **Strong credentials:** long, unique, rotated passwords — or better, integrated auth (OS/Kerberos/
  cloud **IAM** / certificate-based) so the DB trusts a managed identity instead of a static
  password. **MFA** for human/admin access where supported (often via a bastion/proxy).
- **Kill the defaults:** remove/rename default and sample accounts, change all default passwords,
  and disable unauthenticated/"trust" access. Default credentials are the first thing attackers try.
- Enforce password/lockout policy; monitor failed logins.

## Authorization — least privilege
- Grant the **minimum** rights needed, through **roles/groups** rather than direct per-user grants
  (roles are maintainable; ad-hoc grants sprawl and rot).
- **The application account must not be a superuser/DBA.** Give it only the DML it needs on only the
  objects it uses; usually it needs no DDL, no admin functions, no access to other schemas. This
  caps the damage of app compromise or SQL injection.
- **Separate duties and levels:** distinct read-only, read-write, and admin roles; separate the
  people who manage access from those who use it where risk warrants. Restrict dangerous privileges
  (DDL, `GRANT`, `FILE`/`COPY`/`xp_cmdshell`-style, backup/restore, admin procedures).
- Scope to objects: grant on specific schemas/tables/columns; avoid blanket database-wide grants.
  Use **views** or **row-level security / column masking** to expose only the needed rows/columns
  to a given role (e.g. analysts see masked PII).
- **Deny by default;** add access deliberately. Avoid `PUBLIC`/everyone grants.

## Network isolation (the perimeter)
- **The database must not be reachable from the public internet** — the single most important
  network control. Put it in a **private subnet** with no public IP.
- Restrict connectivity by firewall/security group to only the specific application hosts/subnets
  that need it, on the DB port only. Admin access via **bastion/jump host, VPN, or private
  endpoint** — never a database port open to the world.
- **Require TLS** and reject plaintext connections. For cloud managed DBs, use private
  connectivity (private link/VPC) rather than public endpoints; if a public endpoint must exist,
  lock it to known IPs and always TLS.

## Lifecycle & hygiene
- **Recertify access** periodically: remove accounts for departed staff, decommissioned apps, and
  unused roles. Stale privileged accounts are a classic breach vector.
- Store DB credentials in a **secrets manager**, injected at runtime; never in source, config files,
  or connection strings in code. Rotate them, and rotate immediately on suspected exposure.

## Common pitfalls
- App connecting as superuser/`sa`/`root` → injection or app compromise = full DB compromise.
- Database exposed to the internet / open security group on the DB port.
- Default accounts and passwords left enabled; shared admin logins (no attribution).
- Broad `PUBLIC`/database-wide grants; direct per-user grants that never get cleaned up.
- Credentials hardcoded in app config; never rotated; stale departed-user accounts lingering.
- Plaintext connections allowed alongside TLS.
