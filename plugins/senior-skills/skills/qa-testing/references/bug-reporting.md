# Bug Reporting

A defect report exists to get a bug understood, prioritized, and fixed. The two things
that decide its fate: can the developer **reproduce** it, and do they grasp its **impact**.

## Anatomy of a good bug report
- **Title:** specific and searchable — what's wrong, where. "Checkout: applying a second coupon
  removes the first silently" beats "coupon broken."
- **Environment:** build/version, OS/browser/device, user role/account, config, and data state —
  bugs are often environment-specific.
- **Preconditions:** the state needed before the steps (logged in as X, cart has Y).
- **Steps to reproduce:** numbered, minimal, exact. Someone who's never seen it should follow
  them and hit the bug. Trim to the shortest path that still reproduces.
- **Expected result:** what should happen (and why — tie to the requirement if non-obvious).
- **Actual result:** what actually happened, precisely.
- **Evidence:** screenshot/video, logs, error messages/stack traces, request/response, IDs,
  timestamps. Redact secrets/PII.
- **Severity & impact:** how bad (data loss? blocks a core flow? cosmetic?) and how often/who it
  hits. Keep severity (technical impact) distinct from priority (business urgency).

## Reproducibility
- **Reproduce it yourself** before filing; nail down the exact trigger. Note the reproduction
  rate (always / intermittent / once) — say so honestly if intermittent, and give everything you
  observed (it helps find the race).
- **Minimize:** strip unrelated steps so the core trigger is clear. A 3-step repro gets fixed; a
  20-step one with noise gets deprioritized.
- A bug nobody can reproduce rarely gets fixed — invest here.

## Severity (typical scale)
- **Critical/Blocker:** data loss/corruption, security, crash, core function unusable, no workaround.
- **Major/High:** important function broken or badly wrong; painful workaround.
- **Minor/Medium:** limited-impact issue with a reasonable workaround.
- **Low/Trivial:** cosmetic, edge-case, polish.
Rate by *impact on the user/business*, not how hard it was to find.

## Good practice
- **One bug per report** (don't bundle several — they get partially fixed and lost).
- **Objective and blameless:** describe the behavior, not "the dev messed up." No speculation as
  fact — separate what you observed from what you think causes it.
- **Search first** for duplicates; link related issues.
- Be reader-kind: lead with the one-line impact and repro; details below.

## Verifying fixes
- Re-test the exact repro on the fixed build; also check nearby/related behavior for regressions.
- Confirm the fix addresses the cause, not just your one path. Close with a note on what you verified.

## Common pitfalls
- Vague title/steps; "doesn't work" with no repro. Missing environment or evidence.
- Several bugs in one ticket. Secrets/PII pasted into logs/screenshots.
- Severity based on discovery difficulty, not impact. Stating a guessed cause as fact.
- Not minimizing the repro; closing without verifying the fix or checking for regressions.
