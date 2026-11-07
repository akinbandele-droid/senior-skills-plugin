# Containment, Eradication & Recovery

The active response phases. Run them in order, scoped by the investigation, and always
preserving evidence first. The biggest failure modes are tipping off the adversary with
partial action and eradicating before you understand the full footprint.

## Containment
Goal: stop the spread and limit damage while keeping evidence and not alarming the attacker prematurely.

- **Capture volatile evidence first** where feasible (see `forensics-and-evidence.md`) — then contain.
- **Short-term containment** (immediate): network-isolate affected hosts (quarantine via EDR, VLAN,
  or firewall — prefer isolation over power-off to keep memory), disable/suspend compromised
  **accounts and sessions/tokens** (don't just reset a password an attacker can reset back — kill
  active sessions and revoke tokens/keys), block known **C2** domains/IPs, and segment to stop
  lateral movement.
- **Scope it from the investigation.** Contain **all known footholds together.** Isolating one host
  while the adversary holds domain admin or other accounts just warns them — they may detonate
  ransomware, destroy evidence, or go dormant and return. Coordinate a decisive cut.
- **Long-term containment** (temporary hardening to operate safely until eradication): targeted
  patches, tighter access/rules, extra monitoring, rebuilt clean systems to resume critical function.
- **Preserve business continuity** proportionately — total lockdown has its own cost; balance with
  leadership. Beware the attacker watching your containment via the compromised environment (use
  out-of-band comms).

## Eradication
Goal: remove the adversary's presence **completely** and fix the root cause so they can't walk back in.

- Remove: malware/implants, **persistence** (services, scheduled tasks, run-keys, startup items,
  web shells, rogue OAuth grants/mail rules), attacker-created **accounts and keys/tokens**, and
  backdoors identified in the investigation.
- **Reset credentials broadly** — assume password/hash/token theft across the blast radius; include
  service accounts and, in AD compromise, consider the krbtgt double-reset. Rotate exposed secrets/keys.
- **Fix the root cause** — the vulnerability, misconfiguration, or exposed credential that enabled
  entry — or they return the same way. Pair with `cloud-security`/`security-review`/`software-engineering`.
- **Reimage rather than clean** when host integrity is in any doubt — you rarely fully trust a
  "cleaned" compromised box. Verify eradication against the full timeline; don't proceed with
  unexplained footholds outstanding.

## Recovery
Goal: return to normal operations safely and confirm the threat is gone.

- **Restore from known-good** backups/images; verify they predate the compromise and aren't
  themselves infected/encrypted. Validate integrity and function before returning to production.
- **Phased, prioritized return** (critical systems first), coordinated with the business.
- **Heightened monitoring** for a defined period — attackers frequently return, often via the same
  or related TTPs; watch specifically for the IOCs/behaviors from this incident.
- **Confirm** business functions restored and the environment stable, then formally **close** the
  incident (hand to post-incident review) — only when confident the adversary is out.

## Special cases
- **Ransomware:** preserve evidence and scope encryption before acting; isolate aggressively to
  stop spread; recovery usually means restore-from-backup — paying is a legal/business decision
  (sanctions risk) made by leadership/counsel, never a default. See `playbooks.md`.
- **Active, sophisticated adversary:** consider watching briefly to map full scope before a single
  decisive eradication — but weigh the risk of ongoing harm. Often best with external IR support.

## Common pitfalls
- Partial containment that tips off the attacker → destruction or loss of access to track them.
- Eradicating before scoping → a missed foothold and reinfection days later.
- Only killing symptoms, not the root cause/stolen creds → they return.
- Restoring from a compromised backup; returning with no extra monitoring.
- Resetting a password but not revoking active sessions/tokens.
- Destroying evidence during containment.
