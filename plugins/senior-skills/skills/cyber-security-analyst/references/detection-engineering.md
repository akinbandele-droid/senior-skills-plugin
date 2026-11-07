# Detection Engineering

Turning telemetry into detections that catch real attacker behavior with a manageable
false-positive rate. Detection is a product you build, tune, and maintain — not a pile
of rules you accumulate.

## Log/telemetry sources (know what you have)
- **Endpoint (EDR):** process creation (with command lines & parents), file/registry changes,
  module loads, network connections — the richest source for attacker behavior.
- **Identity:** authentication (success/fail), privilege use, account/group changes, MFA
  events, directory changes. Identity is the modern battleground.
- **Network:** flow/connection logs, DNS, proxy/web, firewall; TLS metadata. Good for C2,
  exfil, lateral movement.
- **Cloud/SaaS:** control-plane audit logs (CloudTrail/Activity/Audit), config changes,
  console logins. **OS/app:** system, auth, web-server, and application logs.
- Map coverage and **gaps** against ATT&CK — you can't detect what you don't log. Check
  retention (can you investigate 90 days back?) and time sync across sources.

## Detection types (defense in depth)
- **Signature/IOC:** known-bad hashes/domains/IPs. Precise but brittle (attackers rotate them).
- **Behavioral/TTP:** patterns of attacker technique (e.g. `rundll32` spawning from Office,
  LSASS access, new service for persistence, mass file encryption). Survives indicator changes —
  invest here.
- **Anomaly/baseline:** deviations from normal (impossible-travel logon, first-time admin,
  unusual data volume). Powerful but noisier; needs good baselines.
- **Correlation:** chains of events across sources that together indicate an attack.

## Map to MITRE ATT&CK
- Classify each detection by tactic (the attacker's goal) and technique (the how). Build a
  coverage map across the kill chain to find and prioritize gaps.
- Favor detections on techniques that are **hard for attackers to change** (behaviors,
  tooling, TTPs) over trivially-changed indicators — see the Pyramid of Pain in `threat-intel.md`.

## Writing a good detection
Each detection should define:
- **What it catches** and the mapped ATT&CK technique(s).
- **Data source(s)** and the exact logic/query.
- **Expected false positives** and how to tune/exclude them (legitimately).
- **Severity** and a **triage/response runbook** — what the analyst should do when it fires.
- **Test evidence:** validate it actually fires on the real technique (atomic tests / purple-team).

## Tuning (the never-ending job)
- A noisy detection that gets ignored is worse than no detection — it hides real alerts and
  burns analysts out. Measure FP rate; tune or suppress legitimately (not by blanket-disabling).
- Watch for detection **decay**: environment changes, new apps, and attacker evasion erode
  detections over time. Review periodically; re-validate against current TTPs.
- Beware tuning so hard you blind yourself — exclude the specific benign cause, not the whole technique.

## Common pitfalls
- Collecting logs but not detecting (a SIEM full of data, few real detections).
- Only IOC/signature detections → missed on any novel or re-tooled attack.
- Alerts with no runbook, no severity, no owner → ignored.
- No ATT&CK coverage map → unknown blind spots. No detection testing → rules that never fire.
- Retention too short to investigate; clocks unsynced across sources.
