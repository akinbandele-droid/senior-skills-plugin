# Policies & Controls

Controls are how risk is treated; policies and procedures are how controls are mandated and
operated. Both must be real and followed — not shelfware.

## The document hierarchy
- **Policy** — high-level *what and why*, mandated and approved by leadership (e.g. "access is
  granted on least privilege and reviewed quarterly"). Stable, principle-level.
- **Standard** — specific, mandatory requirements that support a policy (e.g. "passwords ≥ 14
  chars; MFA on all remote access; TLS 1.2+").
- **Procedure** — step-by-step *how* to carry it out (e.g. the joiner/mover/leaver access steps).
- **Guideline** — recommended, non-mandatory good practice.
Keep each clear, owned, version-controlled, approved, dated, communicated, and reviewed on a
cadence. Write them to be *followed* (pair with `technical-writing`) — a policy nobody reads or
can't follow is a finding in waiting.

## Control types
By **function:**
- **Preventive** — stop it happening (access control, MFA, encryption, segregation of duties, input validation).
- **Detective** — detect it when it happens (logging/monitoring, alerts, reviews, reconciliations).
- **Corrective** — fix/recover after (incident response, backups/DR, patching).
(Also deterrent and compensating.) A healthy program layers all three (defense in depth).

By **nature:** administrative (policy, training, process), technical/logical (system-enforced),
physical (locks, badges). And **automated** (system-enforced, consistent, easy to evidence) vs.
**manual** (people-run, more error-prone) — prefer automated where feasible.

## Control families to cover
Governance & risk mgmt; access control & identity; change management; SDLC/secure development;
vulnerability & patch management; logging, monitoring & detection; incident response; business
continuity & disaster recovery (BCP/DR); data protection & privacy (classification, encryption,
retention, disposal); HR security (screening, onboarding/offboarding, training); vendor/third-party
management; asset management; physical & environmental; cryptography; network security.

## The controls matrix (crosswalk)
Maintain a matrix mapping: **requirement(s)** (from each framework) → **control** → **owner** →
**implementation/evidence** → **status/effectiveness**. Benefits:
- One control satisfies many requirements across frameworks — map once, prove once, reuse.
- Shows coverage and gaps at a glance; drives audit evidence; assigns accountability.

## Making controls effective
- **Design** must actually address the risk/requirement; **operation** must happen reliably.
- **Owner** accountable for running each control. Define the **evidence** it produces (so it's
  auditable) and the frequency.
- Prefer controls built into normal workflow (automated, enforced) over ones relying on people
  remembering — those decay. **Segregation of duties** and least privilege where risk warrants.
- Review controls as the environment changes; retire/replace ones that no longer fit.

## Common pitfalls
- Shelfware policies (exist for the auditor, unknown to staff, not followed).
- Controls with no owner, no evidence, or no mapped risk/requirement.
- All-preventive or all-manual control sets (no detection; decay-prone).
- Policy/standard/procedure confusion (vague policies that try to be procedures, or vice versa).
- Not crosswalking → redundant controls and evidence per framework.
