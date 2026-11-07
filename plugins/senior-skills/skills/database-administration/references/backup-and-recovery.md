# Backup & Recovery

The most important DBA responsibility. Everything else is recoverable from a backup; the
data is not recoverable from nothing. Design to the business tolerance, protect the
backups, and **prove recovery by testing it.**

## RPO & RTO (design drivers)
- **RPO (Recovery Point Objective):** how much data loss is acceptable (e.g. "at most 5 minutes").
  Drives backup/log frequency and whether you need synchronous replication.
- **RTO (Recovery Time Objective):** how quickly you must be operational again. Drives backup
  method, HA topology, and recovery automation.
- Get these from the business per database — they decide the cost/complexity. A reporting DB and a
  payments DB have very different RPO/RTO.

## Backup types
- **Full:** complete copy — the baseline; slower/larger.
- **Incremental / differential:** only changes since last full/incremental — faster/smaller, but
  recovery needs the chain. (Differential = since last full; incremental = since last any.)
- **Transaction-log / continuous archiving** (WAL/redo/binlog): the stream of changes. Enables
  **point-in-time recovery (PITR)** — restore to just before an error/corruption, not just to the
  last full backup. Essential for low RPO.
- **Physical** (file/block-level, fast for large DBs) vs. **logical** (dump of schema+data,
  portable across versions, slower, good for migration/partial restore). Often use both.
- **Snapshots** (storage/cloud): fast, but ensure they're **application-consistent** (DB quiesced/
  crash-consistent), not just a raw volume snapshot that may be unrecoverable.

## A sound strategy
- Combine a periodic **full** + frequent **incrementals** + **continuous log archiving** to hit RPO
  with PITR. Automate it; monitor that every backup **succeeds** (a silently failing backup is the
  worst case). Alert on failure.
- **3-2-1 rule:** ≥3 copies, on ≥2 media/types, ≥1 off-site. Keep at least one copy **off-box and
  off-site/another region**.
- **Protect backups:** encrypt them (they contain all your data), restrict access, and make them
  **immutable / deletion-protected** (versioning, object-lock, separate account) so ransomware,
  a compromised DB admin credential, or a bad script can't wipe them. Set and enforce **retention**.
- Document a **recovery runbook**: exact steps, order, dependencies, and contacts.

## Test restores (non-negotiable)
- **An untested backup is not a backup.** Regularly restore to a separate environment and verify
  integrity and completeness. Measure the restore time against RTO. Rehearse PITR.
- Test the *whole* path, including retrieving off-site copies and the runbook under pressure.
  Countless "we had backups" disasters are really "our backups were corrupt / incomplete /
  un-restorable / too slow, and we found out during the outage."

## Recovery scenarios to plan for
- Single-row/table human error (fast logical restore or PITR to a point just before).
- Full instance loss (restore full + logs, or fail over to replica).
- Corruption (restore + PITR to before corruption; identify root cause).
- Ransomware/malicious deletion (immutable off-site backups are the backstop).
- Region/DR event (cross-region backup/replica).

## Common pitfalls
- Never testing restores; discovering corrupt/incomplete backups during a real outage.
- Backups on the same box/account that fails or gets compromised (deletable, not off-site).
- No log archiving → can only restore to last full backup (RPO = hours/days).
- Unencrypted backups leaking all data; no retention or immutability.
- Backup jobs failing silently with no monitoring/alerting.
- No documented, rehearsed recovery runbook → slow, error-prone recovery.
