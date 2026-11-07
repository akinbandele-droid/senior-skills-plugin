# Hardening & SQL Injection Defense

Remove the weaknesses attackers exploit: insecure defaults and configuration, and the
classic application-to-database attack, SQL injection. Hardening shrinks the attack surface;
injection defense protects the data even when app code is imperfect.

## Secure configuration (shrink the attack surface)
- **Remove defaults:** delete demo/sample databases and schemas, remove or disable default/sample
  accounts, change every default password. Disable unused features, extensions, stored procedures,
  and network services the DB doesn't need (e.g. dangerous extended procs, `LOAD_FILE`/`COPY PROGRAM`,
  external script execution) — each is attack surface.
- **Follow a benchmark:** apply the **CIS Benchmark** (or vendor hardening guide) for your engine —
  it codifies secure settings for auth, logging, network, and privileges.
- **Limit network exposure** (see `access-control.md`): private network, TLS enforced, tight firewalling.
- **Least privilege for the DB's own OS account** and file permissions; the DB process shouldn't run
  as root/admin, and data/backup files should be tightly permissioned.
- **Patch promptly** — database engines receive critical CVEs; stay on supported versions and apply
  security patches on a schedule (coordinate with `database-administration`). Unpatched DBs are prime targets.
- **Error handling:** don't leak schema, versions, or query details in errors to clients (feeds
  attackers) — log details server-side, return generic errors.

## SQL injection (the classic data-layer attack)
Injection happens when untrusted input is concatenated into SQL, letting an attacker alter the query
— to read/modify arbitrary data, bypass auth, or sometimes run commands. Defense is layered:

- **Parameterized queries / prepared statements are the real fix** (primary, app-side): data is sent
  separately from the SQL text, so input can never change the query structure. Use them everywhere;
  never build SQL by string concatenation with user input. (Enforce in code — pair with
  `sql-development` and `security-review`.)
- **Safe dynamic SQL:** if SQL must be built dynamically (e.g. dynamic ORDER BY/identifiers, which
  can't be parameterized), **allow-list** the permitted values rather than passing input through.
  Validate/escape as a secondary measure, never the primary.
- **Least-privilege app account** (defense in depth): if injection still occurs, a scoped account
  (no DDL, no admin, only its own tables) sharply limits what the attacker can do — no `DROP`, no
  reading other schemas, no OS access. This is the DB's contribution to injection defense.
- **Limit error disclosure** (above) to slow blind-injection and schema discovery.
- Stored procedures help **only** if they themselves use parameters internally (a proc that
  concatenates input is just as vulnerable).

## Verify
- Review data-access code for concatenated SQL; test with injection payloads in a safe environment
  (authorized — see `web-pentest`/`pentest`). Scan configuration against the benchmark. Re-check after
  changes.

## Common pitfalls
- String-concatenated SQL anywhere (even "internal" tools) → injection.
- Relying on input escaping/validation or a WAF instead of parameterization.
- App account over-privileged, so an injection becomes full compromise.
- Default databases/accounts/passwords left in place; unused dangerous features enabled.
- Verbose DB errors exposed to users; unpatched engine; DB process running as root.
- Assuming stored procedures are injection-proof when they concatenate input internally.
