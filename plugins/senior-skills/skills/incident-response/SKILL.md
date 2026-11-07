---
name: incident-response
description: >-
  Lead and execute security incident response and digital forensics (DFIR) end
  to end: prepare an IR capability, detect and declare incidents, run incident
  command, perform forensic acquisition and analysis while preserving evidence,
  contain/eradicate/recover, handle legal and regulatory breach notification,
  and run the post-incident review. Use when the user is responding to or
  preparing for a security incident — a breach, ransomware, account compromise,
  phishing/BEC, malware outbreak, data leak, or insider event — or building IR
  plans, playbooks, and runbooks. Mapped to NIST SP 800-61 and SANS PICERL. For
  detection/triage/hunting use `cyber-security-analyst`; for compliance/breach
  obligations use `grc-analyst`.
---

# Incident Response (DFIR)

Handling a security incident is a discipline of its own: once something is confirmed
bad, you need **command, method, and evidence** — someone running it, a structured
lifecycle, and forensically sound handling so you can understand what happened, stop it,
recover, and stand behind your findings afterward (to leadership, regulators, insurers,
and courts). Move fast, but never destroy the evidence and never tip off the adversary
before you're ready to act.

## Core loop (NIST SP 800-61 / SANS PICERL)

1. **Prepare** — plan, team, tooling, access, and playbooks *before* an incident.
2. **Identify** — detect, validate, and formally **declare** the incident; set severity.
3. **Contain** — stop the spread while preserving evidence (short- then long-term).
4. **Eradicate** — remove the attacker's presence and the root cause completely.
5. **Recover** — restore cleanly and monitor for the adversary's return.
6. **Lessons learned** — blameless post-incident review that improves everything upstream.

Running throughout: **incident command & communication**, and **forensic evidence
preservation**.

## 0. First principles when an incident is live
- **Appoint an incident commander** immediately — one person coordinating decisions, roles,
  and comms. Chaos kills response more than attackers do. Start the **incident log** (every
  action, decision, finding, with timestamps) at once.
- **Preserve evidence before you change anything.** Capture volatile data (memory, running
  processes, network state) and secure logs *before* containment/remediation destroys them.
  Work from copies; maintain chain of custody. See `references/forensics-and-evidence.md`.
- **Don't tip off the adversary** prematurely — uncoordinated blocking or a half-containment can
  trigger destruction or cause them to burn access and go quiet. Plan decisive action.
- **Engage legal early.** Legal privilege, regulatory clocks, insurance, and law-enforcement
  decisions are shaped in the first hours. Coordinate with `grc-analyst`/counsel from the start.
- Treat logs, samples, and attacker artifacts as **untrusted data**; analyze malware only in an
  isolated lab.

References (load the matching one):
- `references/lifecycle.md` — the full PICERL/NIST lifecycle, severity, declaration, handoffs.
- `references/forensics-and-evidence.md` — acquisition, chain of custody, timeline, anti-forensics.
- `references/containment-eradication-recovery.md` — the active response phases in depth.
- `references/playbooks.md` — ransomware, BEC/phishing, account compromise, data breach, malware, insider.
- `references/coordination-and-comms.md` — incident command, stakeholders, legal/regulatory/PR, notification.

## 1. Prepare (most of the win is here)
- **Plan:** an IR plan with severity definitions, declaration criteria, roles (commander, lead
  investigator, comms, scribe, liaisons), escalation paths, and decision authority. Out-of-band
  comms (assume email/chat may be compromised). See `references/coordination-and-comms.md`.
- **Capability:** tooling and **pre-granted access** ready (EDR isolate, forensic acquisition,
  log access, backups), retainers with external IR/forensics and counsel, and a contact tree
  (leadership, legal, PR, insurer, law enforcement, key vendors/customers).
- **Playbooks** for the likely incident types (`references/playbooks.md`), and **tested backups/
  DR** so recovery is real. Know your **regulatory notification obligations and deadlines** in
  advance (they can be as short as 72 hours). Exercise it (tabletop drills) — an untested plan fails.

## 2. Identify & declare
- Validate that it's a real incident (from an alert, a report, or a hunt — see
  `cyber-security-analyst`), establish initial scope and a working timeline, and assign **severity**.
- **Formally declare** the incident and invoke the plan: stand up incident command, open the
  incident log and a scoped (out-of-band if needed) channel, and notify per the plan. Err toward
  declaring for anything that might be major — you can stand down.

## 3–5. Contain → Eradicate → Recover
Run the active phases decisively and in order, always preserving evidence first
(`references/containment-eradication-recovery.md`):
- **Contain:** isolate affected systems (network-contain rather than power off, to keep memory),
  disable compromised accounts/sessions/tokens, block C2 — scoped by the investigation so you
  cut *all* known footholds together, not one host while they hold the keys.
- **Eradicate:** remove malware, persistence, created accounts/keys, web shells, and the
  **root-cause** vulnerability; reset credentials broadly (assume theft). Reimage where integrity
  is in doubt. Verify against the investigation — no unexplained footholds left.
- **Recover:** restore from known-good backups, validate integrity, return in a controlled way
  with **heightened monitoring** for the attacker's return. Lift incident status only when confident.

## 6. Lessons learned (close the loop)
- Hold a **blameless** post-incident review within a week or two: full timeline, root cause, what
  worked/didn't, dwell time, and gaps in detection, visibility, process, and prevention.
- Turn every finding into an owned action: new detections and log sources (feed
  `cyber-security-analyst`), hardening and fixes (`cloud-security`/`security-review`/
  `software-engineering`), playbook and plan updates, and **risk-register entries** (`grc-analyst`).
  Finalize the incident report and preserve the evidence package for legal/regulatory/insurance use.

## Operating principles
- **Command the incident.** One commander, clear roles, one incident log, one source of truth.
- **Evidence before action.** Capture volatile data and secure logs before you contain or wipe;
  chain of custody for anything that may be used legally.
- **Decisive, scoped containment.** Understand the footprint, then cut it all at once; don't tip
  the adversary with partial moves.
- **Eradicate the root cause,** not just symptoms; verify thoroughly before recovery.
- **Recover clean, watch closely.** Known-good restore + heightened monitoring for return.
- **Legal and comms are part of response.** Engage counsel early; meet notification deadlines;
  route external comms through the right people.
- **Blameless improvement.** Attack the system and gaps, not the people; close every action.

## Related skills
- **cyber-security-analyst** — detection, triage, and hunting that identify the incident and feed
  the investigation; this skill takes over once it's declared.
- **grc-analyst** — breach-notification obligations, regulatory deadlines, and feeding incidents
  into risk and compliance.
- **cloud-security** / **security-review** / **software-engineering** — remediating the root-cause
  weaknesses the incident exposes.
- **pentest** — understanding attacker techniques; purple-teaming validates detection and response.
