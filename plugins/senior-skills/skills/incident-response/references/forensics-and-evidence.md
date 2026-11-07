# Digital Forensics & Evidence Handling (DFIR)

The investigation answers *what happened* and underpins every response decision — and may be
used by regulators, insurers, and courts. So it must be **sound**: capture before you change,
preserve integrity, and document everything. Order of volatility and chain of custody are the
non-negotiables.

## Preserve first (order of volatility)
Containment and remediation destroy evidence. Capture from most to least volatile **before**
acting, where feasible:
1. **Memory (RAM)** and running state — processes, network connections, open files, logged-on
   users, encryption keys, injected/fileless malware. Lost on power-off; often the richest source.
2. **Volatile system state** — ARP/routing, running services, scheduled tasks.
3. **Disk** — a forensic image of the storage.
4. **Logs & artifacts** — local and (critically) **centralized/remote** logs (they may be the only
   copy if the host is wiped or the attacker cleared local logs).
5. **Archival** — backups, archived logs.

- **Network-isolate rather than power off** when you need memory — pulling the plug loses RAM; a
  graceful shutdown lets malware run cleanup. Decide deliberately per case.
- Work on **copies/images**, never the original. **Hash** evidence (e.g. SHA-256) at acquisition
  and verify copies match — proves integrity/no tampering. Use write-blockers for disk imaging.

## Chain of custody
For anything that may support legal/HR/insurance/regulatory action, document unbroken custody:
- **What** the item is, **who** collected it, **when** (timestamped), **how** (method/tool), and
  **where** it's stored — plus every transfer/handler. Store securely with restricted access.
- Gaps or sloppy handling can render evidence inadmissible and undermine findings. Treat even
  "internal only" incidents this way — they can escalate to litigation.

## Analysis — build the timeline
- **Correlate across sources** (endpoint, memory, disk, identity, network, cloud/SaaS, app logs)
  into one authoritative timeline. Normalize timestamps to a single timezone (note skew).
- Reconstruct the attack along the kill chain / MITRE ATT&CK: **initial access & patient zero**,
  execution, persistence, privilege escalation, defense evasion, credential access, discovery,
  **lateral movement**, collection, **C2**, and **exfiltration/impact**.
- Answer the decision-driving questions: how in, what done, what/who affected, how far, how long
  (dwell time), still active?, and **what data was accessed/taken** (central to breach notification).
- Common artifacts: process-execution evidence (EDR, prefetch, shimcache/amcache), event logs
  (auth, PowerShell, Sysmon), registry run-keys/services (persistence), browser/email, file system
  MAC times, memory strings/injected code, and malware behavior (detonated in an **isolated lab** only).

## Anti-forensics & gaps
- Watch for log clearing, timestomping, encrypted/obfuscated payloads, and log gaps — an *absence*
  of expected logs is itself a finding. Don't assume "no evidence" means "nothing happened";
  corroborate across independent sources.
- Beware confirmation bias and premature closure; state confidence and what would confirm/refute.

## Scope, authority & privacy
- Investigate only systems/data you're authorized to examine; handle employee/user data lawfully
  and minimally. Work under legal privilege where counsel directs. Don't "hack back."

## Common pitfalls
- Containing/reimaging before capturing memory and logs — evidence gone forever.
- Working on originals; no hashing; no write-blocker → integrity challengeable.
- Broken/absent chain of custody → inadmissible evidence.
- Single-source conclusions; ignoring log gaps/anti-forensics; unsynced clocks.
- Opening/detonating malware outside an isolated lab; "hacking back" at the attacker.
