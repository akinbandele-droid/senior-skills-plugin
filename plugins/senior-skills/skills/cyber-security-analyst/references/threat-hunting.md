# Threat Hunting

Proactively searching for adversary activity that existing detections missed — on the
assumption that a sufficiently skilled attacker is already inside and evading alerts.
Hunting is hypothesis-driven and structured, not random log-scrolling.

## Why hunt
- Detections catch the known; hunting finds the unknown and the evasive. It also discovers
  visibility gaps and produces new detections. Measure of success isn't only "found evil" —
  a clean hunt that yields a new detection or a fixed blind spot is a win.

## The loop
1. **Hypothesis** — a specific, testable idea grounded in intel, ATT&CK, or your environment.
   Good: "An adversary is using WMI for lateral movement" or "credential dumping via LSASS
   access is occurring on servers." Bad: "let's look for anything weird."
2. **Scope the data** — what telemetry would show it, over what time range and hosts.
3. **Hunt** — query/analyze for the behavior; filter known-good; investigate outliers.
4. **Resolve** — malicious → escalate to IR; benign → understand why; inconclusive → refine.
5. **Operationalize** — turn a successful hunt technique into an automated **detection**, and
   note any visibility gap found. Document the hunt either way.

## Where hypotheses come from
- **Intelligence-driven:** TTPs of adversaries known to target your sector (`threat-intel.md`);
  hunt for those specific behaviors.
- **ATT&CK-driven:** pick techniques you can't currently detect and hunt them to validate.
- **Anomaly/environment-driven:** your knowledge of "normal" — rare parent/child process pairs,
  unusual admin tool usage, beaconing patterns, new persistence, odd service accounts.
- **Crown-jewel-driven:** start from what matters most and hunt the paths an attacker would take to it.

## Techniques
- **Stack counting / frequency analysis:** rare is interesting — the one host doing something
  no other host does. Aggregate and look at the long tail.
- **Grouping & clustering** of related events; **searching for known TTPs** (LOLBins, suspicious
  command lines, encoded PowerShell, beaconing intervals, impossible travel).
- **Pivot** on any hit like an investigation (see `triage-and-investigation.md`).

## Discipline
- Time-box hunts and record them (hypothesis, data, queries, findings) so they're repeatable and
  coverage is visible. Avoid confirmation bias; a benign result still needs an explanation.
- Hunt where you have data; note where you don't (that gap is a finding).

## Common pitfalls
- "Hunting" with no hypothesis — aimless scrolling that finds nothing reproducibly.
- Not operationalizing wins into detections (hunting the same thing forever by hand).
- Ignoring the visibility gaps the hunt reveals.
- Only hunting what you can already detect (no new coverage).
