# Database Encryption & Key Management

Protect the data itself so that intercepting traffic, stealing a disk/backup, or even
reaching the database doesn't hand over readable secrets. Layered: transit, at rest, and
field-level for the most sensitive data — and the keys must be managed as carefully as the data.

## In transit
- **Enforce TLS** on all client→DB and DB→DB (replication) connections; configure the server to
  **reject non-TLS** connections, not merely allow TLS. Use strong protocol/cipher versions.
- Validate certificates on the client side where feasible (prevent MITM); manage cert rotation.
- Don't rely on "it's an internal network" — assume the network is hostile (lateral movement is real).

## At rest
- **Encryption at rest** via **TDE** (transparent data encryption), volume/filesystem encryption,
  or the cloud provider's managed encryption. Protects against stolen disks/snapshots and improperly
  decommissioned media. **Encrypt backups** too (they're a full copy of the data — a common leak point).
- At-rest encryption is largely transparent and stops *physical/media* theft — but note it does
  **not** protect against an attacker who has valid DB access (they query decrypted data). That's
  what access control and field-level encryption are for.

## Column / field-level (crown jewels)
- For the most sensitive fields (card numbers, SSNs, secrets, tokens), encrypt or **tokenize** at
  the application/column level so the value is unreadable even to someone with full DB read access
  or a leaked dump. Pick based on need to search/compute on the data (deterministic vs. randomized
  encryption have different security/queryability tradeoffs).
- **Tokenization / vaulting** (replace the value with a token, keep the real value in a separate
  vault) is often the right model for PCI card data — it takes the database out of scope for the raw value.
- **Data masking / redaction** for non-production and for limited-privilege roles (show `****1234`).

## Passwords & secrets stored in the DB
- **Never store passwords reversibly.** Hash with a strong, **salted**, slow algorithm (bcrypt,
  scrypt, Argon2, PBKDF2). Encryption is wrong for passwords — you never need to decrypt them.
- Application secrets/API keys ideally don't live in the app database at all — use a secrets
  manager; if stored, encrypt them with a separately-managed key.

## Key management (the hard part)
- Encryption is only as strong as key protection. Use a **KMS/HSM**; **separate the keys from the
  data** and from the people who have data access (separation of duties) — a key sitting next to the
  data it protects, readable by the same admin, adds little.
- **Rotate** keys; control and **audit** key access; plan key lifecycle (generation, rotation,
  revocation, destruction) and recovery (losing the key = losing the data). Consider customer-managed
  keys for regulated/sensitive data. Pair with `cloud-security` for cloud KMS.

## Regulatory note
- PCI (cardholder data), HIPAA (PHI), and GDPR all effectively expect encryption of sensitive data
  at rest and in transit; encryption can also reduce breach-notification exposure. Map with `grc-analyst`.

## Common pitfalls
- TLS allowed but not enforced (plaintext still accepted); internal traffic unencrypted.
- At-rest encryption assumed to stop an attacker with valid DB access (it doesn't).
- Unencrypted backups/snapshots leaking everything.
- Passwords encrypted (reversible) or weakly/unsalted-hashed instead of bcrypt/Argon2/scrypt.
- Keys stored alongside the data, accessible to the same admins; never rotated; no HSM/KMS.
- No recovery plan for keys (lost key → unrecoverable data).
