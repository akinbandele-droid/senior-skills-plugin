# Third-Party / Vendor Risk Management

Your risk extends to everyone who handles your data or systems — vendors, SaaS providers,
subprocessors, and the supply chain. A vendor's breach is your incident and often your
regulatory and reputational problem. Manage it across the whole relationship lifecycle.

## Why it matters
- Major breaches frequently originate through a third party (compromised vendor, dependency, or
  managed provider). Regulators and customers hold *you* accountable for data you entrust to others.
- You inherit the weakest link's exposure; a vendor with poor security is your risk.

## Lifecycle
1. **Inventory & tier** — maintain a vendor inventory. **Tier by risk**, driven by: data accessed/
   stored (PII/PHI/cardholder/IP?), access to your systems/network, criticality to operations, and
   volume. A payroll SaaS with all employee PII ≫ a stock-photo site. Focus effort on high-tier vendors.
2. **Due diligence (pre-contract)** — assess security before onboarding, proportionate to tier:
   - Review their **SOC 2 / ISO 27001 report/cert**, pentest summaries, and security
     questionnaires (SIG/CAIQ). Check certifications are current and in scope for the service you use.
   - Look at their subprocessors (fourth parties), breach history, data-residency, and financial/
     operational stability. Verify, don't just accept a logo wall.
3. **Contract & terms** — bake in security: a **DPA** (data processing agreement) for personal
   data, security requirements/SLAs, **breach-notification obligations and timelines**, right to
   audit/assess, data handling/return/deletion on exit, subprocessor controls, and liability.
   Coordinate with legal (`/legal` skills) and privacy.
4. **Onboarding** — grant least-privilege access; integrate securely; document the data flows.
5. **Ongoing monitoring** — reassess periodically (annually for high tier, on change), refresh
   their attestations, watch for their breaches/news, and track open issues. Risk isn't static.
6. **Offboarding** — revoke all access, confirm **data return/destruction**, close integrations,
   and retain evidence of secure termination.

## Fourth-party & concentration risk
- Your vendors' vendors (subprocessors) extend the chain — know the critical ones.
- **Concentration risk:** many services depending on one provider (or one cloud region) is a
  single point of failure — factor into BCP/DR.

## Evidence & governance
- Keep records: assessments, attestations collected, risk ratings, contract security terms,
  reassessment dates, and remediation of vendor issues. Auditors check vendor management as a control.
- Feed vendor risks into the main **risk register** with owners and treatment.

## Common pitfalls
- No vendor inventory / no tiering → every vendor treated the same (or ignored).
- Accepting a vendor's claims without reviewing actual attestations/scope.
- One-time assessment at onboarding, never revisited.
- Missing breach-notification clauses and DPAs; no right to assess.
- Ignoring subprocessors and concentration risk.
- Forgetting offboarding — access and data left with a former vendor.
