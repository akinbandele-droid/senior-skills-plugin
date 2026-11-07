# Incident Response Lifecycle

Responding to a confirmed incident in a structured way (NIST SP 800-61 lifecycle:
Preparation → Detection & Analysis → Containment, Eradication & Recovery → Post-Incident).
Move fast, stay proportionate, and preserve evidence.

## Preparation (before anything happens)
- An IR plan with roles, severity definitions, escalation paths, and contacts (on-call,
  leadership, legal, comms/PR, HR, external IR/forensics, law enforcement where relevant).
- Tooling and access ready (EDR isolate, log access, out-of-band comms in case primary comms
  are compromised). Runbooks for common incident types. Backups tested for recovery.
- Know regulatory/contractual **notification obligations** in advance (timers can be tight —
  e.g. GDPR 72h) — coordinate with `grc-analyst`/legal.

## Detection & analysis
- Confirm it's a real incident; classify severity; establish initial scope and the timeline
  (see `triage-and-investigation.md`). Declare the incident and invoke the plan.
- Assign an incident lead; start an incident log (decisions, times, actions) and a scoped comms channel.

## Containment
- **Short-term:** stop the bleeding — isolate affected hosts (network-contain, don't always
  power off: you lose memory), disable/`reset` compromised accounts and sessions/tokens, block
  C2 domains/IPs, revoke keys. Preserve evidence while doing so (capture memory/disk first where feasible).
- **Decisive, not partial.** An attacker who detects partial containment may escalate to
  destruction or burn access. Plan to cut off all known footholds together, informed by the
  investigation's scope — containing one host while they hold domain admin fails.
- **Long-term:** temporary hardening to keep them out while you prepare eradication (patches,
  rules, tighter access).

## Eradication
- Remove the attacker's presence completely: malware, persistence mechanisms (services, tasks,
  startup, implants), created accounts/keys, web shells, and the **root-cause** vulnerability
  (patch/config/credential). Reset compromised credentials broadly (assume password/hash theft;
  consider krbtgt/double-reset in AD).
- Verify eradication against the investigation — don't declare clean while unexplained footholds
  remain. Reimage rather than clean where integrity is in doubt.

## Recovery
- Restore systems from known-good backups/images; validate integrity before returning to prod.
- Return in a controlled way with **heightened monitoring** for the attacker's return (they
  often come back using the same or related TTPs). Confirm business function restored.
- Only lift incident status when confident the threat is out and controls are back.

## Post-incident (learn)
- **Blameless review:** full timeline, root cause, what worked, what didn't, dwell time, and the
  detection/visibility/process gaps. Attack the system, not the people.
- Turn findings into action: new detections, new log sources, runbook fixes, hardening, and
  risk-register updates. Track to completion.
- Preserve the final evidence package and report; support any legal/regulatory/insurance follow-up.

## Communication
- Internal: clear, factual updates to stakeholders on cadence; a single source of truth.
- External: breach notifications and public comms go through legal/comms — analysts supply facts,
  not public statements. Be careful what's said while facts are still emerging.

## Common pitfalls
- No plan/roles → chaotic response. Partial containment tipping off the attacker.
- Eradicating before understanding scope → attacker persists via a missed foothold.
- Wiping evidence during containment; no incident log.
- Restoring from a backup that's also compromised; returning with no extra monitoring.
- Missing a regulatory notification deadline; ad-hoc external comms.
