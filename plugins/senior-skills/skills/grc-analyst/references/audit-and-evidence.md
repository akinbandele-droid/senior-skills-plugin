# Audit, Evidence & Certification

An audit tests whether controls are designed well and operating as claimed — using
**evidence**. GRC's job is to make that provable continuously, support the audit, and
close findings. "We do it" is not evidence; the artifact that proves it is.

## Evidence (the heart of it)
- For each control, define **what artifact proves it operated**: logs, tickets, approval records,
  config exports, access-review sign-offs, meeting minutes, scan/patch reports, training records,
  screenshots, system reports. Specify the frequency it's produced.
- **Collect continuously**, not in an audit-eve scramble. Automate collection where possible
  (pull from systems); a compliance-automation platform helps, but the controls must be real
  underneath it.
- Evidence must show the control operated **throughout the audit period** (for SOC 2 Type II /
  ISO), not just on audit day. Keep it organized, dated, and traceable to the control/requirement.

## Control testing (do it before the auditor)
- **Design effectiveness:** is the control capable of preventing/detecting the risk if it operates?
- **Operating effectiveness:** did it actually operate, consistently, over the period? Test by
  sampling (e.g. pick N changes/access grants and verify each followed the control).
- Run internal testing / internal audit ahead of the external one; treat failures as risks to
  remediate, not things to hide. ISO 27001 *requires* internal audit and management review.

## Readiness & the audit process
1. **Readiness/gap assessment** — compare current state to the framework; list gaps.
2. **Remediate** — fix gaps (implement controls, start generating evidence); for Type II/ISO,
   controls must run long enough to build a track record before the audit window.
3. **Audit fieldwork** — auditor reviews evidence, interviews owners, samples, tests. GRC liaises:
   provide exactly what's asked, accurately; don't volunteer scope creep; correct auditor
   misunderstandings with facts.
4. **Findings** — address exceptions/nonconformities with corrective action plans and owners;
   track to closure. Understand severity (minor vs. major nonconformity; SOC 2 exceptions/qualified opinion).
5. **Report/certificate** — SOC 2 report, ISO certificate, etc. Then maintain: surveillance audits
   (ISO), annual SOC 2, continuous evidence.

## Working with auditors
- Be honest and precise. Misrepresenting evidence is a serious integrity and legal risk — far
  worse than a finding. If a control failed, show the remediation, don't fake the artifact.
- A finding is normal and fixable; fraud is not. Build a cooperative, factual relationship.

## Continuous compliance
- Compliance is a recurring program: certifications expire, periods roll, regulations and systems
  change. Keep controls running and evidence flowing year-round; monitor control health; update the
  risk register and controls matrix on change.

## Common pitfalls
- Audit-eve evidence scramble; evidence that only covers audit day, not the whole period.
- "We do this" with no artifact; disorganized evidence not traceable to controls.
- Hiding or fabricating evidence for a failed control (integrity/legal disaster).
- No internal testing → surprised by external findings.
- Treating certification as the finish line; letting controls lapse after the report.
- Scope misunderstandings inflating the audit.
