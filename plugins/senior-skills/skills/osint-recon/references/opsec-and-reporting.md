# OPSEC, Legality & Reporting

Staying on the right side of passive/legal, protecting what you gather, and turning recon
into a useful deliverable.

## Staying passive & legal
- **Default passive:** consume data from public/third-party sources; send nothing to the
  target's infrastructure. The moment you scan, brute-force DNS, resolve-to-verify liveness, or
  interact with the target's apps/services, you're doing **active testing** — stop and move
  under the `pentest` authorization gate.
- **Respect terms & law:** scraping, accessing non-public material, and handling personal data
  are governed by law (e.g. computer-misuse and data-protection regimes) and site terms — public
  visibility is not blanket permission. When unsure whether an action is passive/authorized/legal,
  ask the user before doing it.
- **Scope discipline:** third-party and cloud services associated with the org are frequently
  **out of scope** even if they appear — note them, don't probe them.

## Privacy & people
- Focus on the **organization's** security exposure. Don't build detailed profiles of individuals
  beyond what the engagement needs. Minimize collection of personal data; handle and store it
  lawfully; delete what you don't need.

## Data handling (your own recon data is sensitive)
- Keep an organized, access-controlled store; a running log of **source + timestamp + finding +
  confidence**. Recon output (asset lists, exposed secrets) is a roadmap for attackers — protect it.
- **Redact** any discovered secrets/credentials/PII in notes and reports; reference them by type
  and location, not value.

## OPSEC (engagement-dependent)
- For red-team work, your own footprint may matter (don't tip off the target); use appropriate
  sourcing and avoid logging into target-controlled systems that would reveal you. For a
  cooperative defensive review this matters less — clarify with the user.

## The recon report / deliverable
Structure it to be actionable:
1. **Scope & method** — what was assessed, authorization, sources used, passive/active boundary,
   and the date (recon is a point-in-time snapshot).
2. **Attack-surface map** — domains → hosts/services → technologies, with ownership/confidence;
   call out unexpected/forgotten (shadow-IT) assets.
3. **Exposures, prioritized** — leaked credentials/secrets (rotate now), exposed sensitive
   documents/data (take down), exposed services, info leakage — each with evidence (redacted),
   why it matters, and remediation.
4. **Recommendations** — reduce the surface: rotate/revoke leaked secrets, remove exposed files,
   fix public-storage/misconfig, decommission forgotten assets, improve what metadata/info the org
   publishes. Pair with `cloud-security` and `security-review` for the fixes.
5. **Hand-off (if an engagement):** the validated in-scope surface for the `pentest` active phases.

## Common pitfalls
- Drifting from passive gathering into active probing under the "recon" label.
- Using leaked credentials or accessing non-public material "to confirm."
- Over-collecting personal data; ignoring law/terms because data is "public."
- Storing recon data (and found secrets) insecurely or unredacted.
- Reporting a raw asset dump with no prioritization or remediation — no defensive value.
