# The IR Lifecycle (NIST SP 800-61 / SANS PICERL)

Two standard models describe the same flow. **NIST SP 800-61:** Preparation → Detection &
Analysis → Containment, Eradication & Recovery → Post-Incident Activity. **SANS PICERL:**
Preparation, Identification, Containment, Eradication, Recovery, Lessons Learned. It's a
**cycle** — lessons feed back into preparation.

## Preparation
The phase that decides how well all the others go. Covered in SKILL.md §1 and
`coordination-and-comms.md`. In short: plan, team & roles, tooling and pre-granted access,
retainers (IR firm, counsel), tested backups/DR, playbooks, known notification obligations, and
regular **tabletop exercises**. You cannot build these during a crisis.

## Identification (detection & analysis)
- **Validate:** is this a real incident or a false positive? Distinguish an **event** (any
  observable) from an **incident** (a confirmed/suspected violation or imminent threat). Pull
  context, correlate, and confirm before spinning up the full machine — but don't dawdle on a
  likely-serious one.
- **Scope & triage:** initial blast radius (hosts, accounts, data), entry vector if known, and a
  working timeline. Enough to assign severity and drive containment — full reconstruction continues
  in parallel (`forensics-and-evidence.md`).
- **Severity / classification:** rate by impact (data sensitivity, systems affected, business/
  safety/financial/reputational harm) and urgency. Define levels in advance (e.g. SEV-1…4 or
  Low/Med/High/Critical) with the response each triggers. Severity sets who's woken up and how fast.
- **Declare:** formally invoke the plan — stand up incident command, open the incident log and a
  scoped channel, start the clock, notify per plan. Bias toward declaring; standing down is cheap.

## Containment, Eradication & Recovery
The active phases — see `containment-eradication-recovery.md` for depth. Order matters:
contain (stop spread, preserve evidence) → eradicate (remove presence + root cause) → recover
(restore clean, monitor). Don't eradicate before you understand scope, or you'll miss a foothold.

## Lessons Learned (post-incident)
- A **blameless** review within ~1–2 weeks while memory is fresh. Produce: the authoritative
  timeline, root cause, impact, response effectiveness, dwell time / time-to-detect / time-to-
  contain, and prioritized gaps (detection, visibility, process, prevention, comms).
- Every finding → an **owned, tracked action** across the right skills. Finalize the report
  (technical + executive) and preserve the evidence package. Update the plan and playbooks so the
  next incident is handled better — closing the cycle back to Preparation.

## Severity example (tune to the org)
| Level | Example | Response |
|------|---------|----------|
| **Critical/SEV-1** | Active breach of crown jewels, ransomware spreading, major data exfil | Full IR, exec + legal now, all-hands, clocks running |
| **High/SEV-2** | Confirmed compromise, contained scope, sensitive data at risk | IR team engaged, leadership informed |
| **Medium/SEV-3** | Isolated malware/compromised account, limited impact | Standard handling, normal hours |
| **Low/SEV-4** | Policy violation, blocked attempt, minor issue | Triage/track, no full IR |

## Metrics that matter
Dwell time (compromise → detection), **MTTD** (mean time to detect), **MTTR** (mean time to
respond/recover), time-to-contain, and recurrence. Track to improve, not to blame.

## Common pitfalls
- No preparation → improvising roles, access, and tooling mid-crisis.
- Not declaring (treating a real incident as "just an alert") → slow, uncoordinated response.
- No severity definitions → over- or under-reacting; wrong people engaged.
- Skipping the post-incident review, or doing it as blame → the same incident recurs.
- Treating IR as a one-time event rather than a cycle feeding back into prevention.
