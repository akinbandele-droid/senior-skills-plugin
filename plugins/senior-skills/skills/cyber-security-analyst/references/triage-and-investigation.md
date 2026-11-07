# Alert Triage & Investigation

Triage decides fast whether an alert matters; investigation reconstructs what actually
happened. Both run on evidence and disciplined pivoting, not hunches.

## Triage (minutes, consistent)
For each alert, decide and record:
- **True or false positive?** Does the evidence actually indicate malicious/unexpected activity,
  or a known-benign cause? Don't guess — check the context.
- **Severity** = confidence × potential impact. A possible domain-admin compromise outranks a
  single blocked phishing email. Factor in the asset (crown jewel?) and the user (privileged?).
- **Initial scope:** one host/user, or many? Related alerts clustering?
Quick context to pull: the host and its role, the user and what's normal for them, the full
process tree and command line, parent process, network connections, recent changes/tickets,
and other alerts on the same entity or time window.

- Prioritize by potential impact; work the scariest first. Don't rabbit-hole on a low-severity
  alert while a critical one waits.
- Closing as FP is a decision with a reason — log it. Repeat FPs are a **tuning** task (feed
  back to detection engineering), not something to keep clicking away.

## Investigation (reconstruct the timeline)
Goal: a defensible narrative of what happened, backed by evidence.

- **Pivot across sources.** From one indicator, expand: endpoint (what ran, from where, as whom)
  → identity (what that account did, where else it logged in) → network (what it talked to) →
  logs (what it touched). Each pivot adds a piece.
- **Build a timeline** of events in order. Establish: initial access & entry vector (patient
  zero), execution, persistence, privilege escalation, discovery, lateral movement, collection,
  C2, and exfiltration/impact — i.e. walk it along the kill chain / ATT&CK.
- **Key questions to answer:** How did they get in? What did they do? What/who is affected
  (hosts, accounts, data)? How far did they spread? How long were they in (dwell time)? Is it
  still active? What did they take?
- **Corroborate** — one log line is a lead, not proof; confirm across independent sources.
  Watch for anti-forensics (cleared logs, timestomping) — absence of logs is itself a signal.

## Evidence discipline
- **Capture before you change.** Memory, volatile data, and logs first — containment and
  remediation destroy artifacts. Work from copies; hash and record what you collect.
- **Chain of custody** for anything that may support legal/HR/insurance action: who collected
  what, when, how, and where it's stored.
- Analyze suspicious files/malware only in an **isolated lab**; never "just open it" to check.
- Document as you go: findings, IOCs (hashes, domains, IPs, accounts), and the timeline — this
  drives response and becomes intel.

## Analytic hygiene
- Follow evidence, not the first theory; state confidence and what would confirm/refute it.
- Avoid confirmation bias and premature closure; note alternative explanations.
- Know when you've answered the question well enough to act — perfect reconstruction can wait
  behind stopping an active attacker.

## Common pitfalls
- Closing alerts as FP without checking context; never feeding FPs back to tuning.
- Destroying evidence by containing/reimaging before capturing it.
- Single-source conclusions; ignoring log gaps/anti-forensics.
- Rabbit-holing on low severity; tunnel vision on the first hypothesis.
- Opening/detonating samples outside a lab.
