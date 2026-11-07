# Risk Management

Risk is the foundation of GRC — controls exist to treat risk. The job: identify risks,
analyze and prioritize them consistently, decide how to treat each, and keep a living
record. Aligned with ISO 27005 / NIST SP 800-30 thinking.

## Terms
- **Asset** — something of value (data, system, service, reputation). **Threat** — what could
  cause harm (actor or event). **Vulnerability** — a weakness a threat can exploit.
- **Risk** — the effect of uncertainty on objectives; practically, **likelihood × impact** of a
  threat exploiting a vulnerability against an asset.
- **Inherent risk** (before controls) vs. **residual risk** (after controls). You manage toward
  acceptable residual risk, defined by **risk appetite/tolerance**.

## Assessment process
1. **Identify** — enumerate risks: per asset, per threat, from assessments (pentest, audit,
   incidents), from frameworks, and from change (new system, vendor, regulation). Describe each
   as a clear scenario (threat + vulnerability + asset + consequence), not a vague label.
2. **Analyze** — estimate **likelihood** and **impact**.
   - *Qualitative:* scales (e.g. Low/Med/High or 1–5) via a risk matrix — fast, common, subjective.
   - *Quantitative:* monetary (e.g. ALE = SLE × ARO; or FAIR for defensible loss modeling) —
     harder, better for big decisions. Many orgs blend: qualitative triage, quantify the top risks.
   - Score consistently so risks are comparable; document assumptions.
3. **Evaluate/prioritize** — rank against risk appetite; decide which need treatment and in what order.

## Treatment (the four options)
- **Mitigate/modify** — apply controls to reduce likelihood and/or impact (the usual path).
- **Transfer/share** — insurance, outsourcing, contractual shifting (never transfers all of it —
  reputational/legal often stays).
- **Avoid** — stop the activity causing the risk.
- **Accept** — knowingly tolerate it (within appetite), with documented sign-off by an accountable owner.
Pick based on cost vs. risk reduction; don't spend \$100 to protect \$10. Record the rationale.

## The risk register (living record)
Each entry: id, risk description/scenario, affected asset, inherent rating, existing controls,
residual rating, treatment decision & plan, **owner**, due date, and status. Review on a cadence
and on change. It's the single source of truth for "what are our risks and what are we doing" and
the backbone of leadership reporting and audits.

## Governance
- An accountable **owner** per risk (usually business, not GRC — GRC facilitates). Escalate risks
  exceeding appetite to management for a decision. Tie into a risk committee / management review.
- Risk acceptance is a **decision made by someone with authority**, documented — not a default
  because nobody acted.

## Common pitfalls
- Risk theater: a register that's written once and never revisited or acted on.
- Inconsistent scoring → incomparable risks and misallocated effort.
- Vague risks ("cyber attack") that can't be analyzed or treated.
- Ignoring residual risk / risk appetite; accepting risk implicitly with no owner or sign-off.
- Controls with no mapped risk; top risks with no treatment plan.
- Over-spending on low risks while high ones sit open.
