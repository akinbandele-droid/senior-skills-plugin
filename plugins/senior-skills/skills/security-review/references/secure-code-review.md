# Secure Code Review

Read code the way an attacker would: follow untrusted data from where it enters
to where it's used, and ask what a hostile value does at each step. Map findings
to CWE / OWASP. Confirm with the real code path — scanner hits are leads, not findings.

## Method: source → sink
1. Find **sources** of untrusted input: HTTP params/headers/body, URLs, file
   contents, filenames, CLI args, env vars, DB rows written by users, IPC/queue
   messages, deserialized objects, third-party API responses.
2. Trace each to its **sinks**: query execution, command execution, file paths,
   HTML output, redirects, reflection/eval, serialization, template rendering,
   outbound requests.
3. At every hop check for validation, encoding, and authorization. A tainted
   value reaching a dangerous sink unescaped is the bug.

## Vulnerability-class checklist

### Injection
- SQL/NoSQL — use parameterized queries; flag string-built queries.
- OS command — avoid shelling out with user data; no shell string concatenation.
- Template (SSTI), LDAP, XPath, header/CRLF, log injection.
- XXE — external entities disabled on XML parsers.

### Authentication & session
- Strong password handling (hash with a slow KDF: bcrypt/scrypt/argon2, salted).
- Session tokens random and high-entropy; invalidated on logout; rotated on
  privilege change; proper cookie flags (`HttpOnly`/`Secure`/`SameSite`).
- MFA correctness; no auth bypass via alternate routes; lockout/rate limiting.
- JWT: signature verified, algorithm pinned (no `none`/alg-confusion), expiry checked.

### Authorization
- Every sensitive action checks authz server-side — not just the UI.
- IDOR/BOLA: object access verifies ownership/role, not just authentication.
- No privilege escalation via mass-assignment or hidden parameters.
- Multi-tenant isolation enforced on every data path.

### Cryptography
- Standard, current primitives; no home-grown crypto or ECB mode.
- Proper key management (no keys in code/repo), secure randomness (CSPRNG),
  authenticated encryption, correct IV/nonce handling, TLS for data in transit.

### Secrets & sensitive data
- No hardcoded credentials, API keys, or tokens (check history/config/CI too).
- Secrets from a vault/env, not source. PII/secrets not logged. Least data retained.

### Memory & resource safety (where applicable)
- Buffer bounds, integer overflow, use-after-free, format strings (C/C++).
- Resource limits: bounded allocations, timeouts, no unbounded recursion/loops (DoS).

### Deserialization & parsing
- No deserializing untrusted data into live objects; safe parsers; schema validation.

### SSRF & outbound requests
- User-controlled URLs restricted (allowlist, block internal ranges & metadata endpoints).

### Supply chain / dependencies
- Known-vulnerable dependencies (audit against advisories); pinned versions;
  integrity/lockfiles; typosquat and unexpected transitive deps; build/CI
  scripts that execute untrusted code; overbroad install hooks.

### Configuration & deployment
- Secure defaults, least privilege, verbose errors/stack traces off in prod,
  debug endpoints disabled, security headers set, no secrets in images/logs.

### Error handling & logging
- Fail closed, not open. Don't leak internals in errors. Log security events
  (authz failures, auth events) without logging secrets.

## Reading discipline
- Prioritize by the threat model — spend time where impact × reachability is highest.
- Read the callers and the whole data flow; a function is only safe in context.
- Check what's **missing** (an absent authz check, no validation) as hard as what's present.
- Confirm exploitability before reporting; mark theoretical items as such.
