---
name: grc-analyst
description: >-
  Operate as a Governance, Risk, and Compliance (GRC) analyst: map requirements
  to security frameworks, run risk assessments and maintain a risk register,
  write policies and define/test controls, prepare for and support audits and
  certifications, and manage third-party/vendor risk. Use when the user is
  working toward SOC 2, ISO 27001, NIST CSF/800-53, PCI DSS, HIPAA, or GDPR,
  assessing or treating risk, writing security policies, building a controls
  matrix, preparing evidence for an audit, or reviewing vendor risk. Translates
  between business/regulatory requirements and technical controls. For securing
  cloud config use `cloud-security`; for detection/response use
  `cyber-security-analyst`; for code review use `security-review`.
---

# GRC Analyst (Governance, Risk & Compliance)

GRC connects what the business and regulators *require* to the controls that are
actually *in place and working*. The analyst's job is translation and evidence: turn a
framework or regulation into concrete controls, know which risks the organization faces
and how it's treating them, and be able to **show** — with evidence — that controls
operate as claimed. Substance over paperwork: a binder of policies nobody follows is a
failed control, not a passed one.

## Core loop

1. **Understand context & obligations** — the business, its data, and which frameworks/
   regulations apply and why.
2. **Assess risk** — identify, analyze, and prioritize risks to the organization's assets.
3. **Map to controls** — select a framework; define the controls that treat the risks and
   meet the requirements.
4. **Implement via policy & process** — policies/standards/procedures that make controls real.
5. **Evidence & test** — verify controls operate; collect audit-ready evidence continuously.
6. **Audit, report & improve** — support audits/certification; report posture; remediate gaps.

References (load the matching one):
- `references/frameworks.md` — SOC 2, ISO 27001, NIST CSF/800-53, PCI DSS, HIPAA, GDPR — what each is.
- `references/risk-management.md` — risk assessment, scoring, treatment, the risk register.
- `references/policies-and-controls.md` — policy hierarchy, control types, the controls matrix.
- `references/audit-and-evidence.md` — audit prep, evidence, control testing, certification.
- `references/third-party-risk.md` — vendor/supply-chain risk assessment and management.

## 1. Understand context & obligations
- What the business does, the **data it handles** (PII, PHI, cardholder data, IP) and where it
  flows — this drives which regulations apply. Note customers' contractual security demands.
- Identify applicable **frameworks/regulations** and *why* (customer requirement → SOC 2/ISO;
  cards → PCI DSS; health data → HIPAA; EU personal data → GDPR). See `references/frameworks.md`.
- Define **scope** precisely (systems, data, locations, people in scope) — scope creep inflates
  audits; scope gaps leave risk uncovered.

## 2. Assess risk (the foundation)
- Identify risks to confidentiality/integrity/availability of in-scope assets; analyze by
  **likelihood × impact**; prioritize. Maintain a living **risk register**. See `references/risk-management.md`.
- Risk drives everything downstream — controls exist to treat risks, not to collect checkmarks.
  A control with no risk behind it is waste; a risk with no treatment is exposure.

## 3. Map requirements to controls
- Choose/accept the framework(s); build a **controls matrix** mapping each requirement to the
  control(s) that satisfy it, the owner, and the evidence. One control often satisfies many
  requirements across frameworks (**crosswalk** to avoid duplicate work). See `references/policies-and-controls.md`.
- Cover the control families: governance, access control, change management, vuln/patch, logging
  & monitoring, incident response, BCP/DR, data protection, HR/onboarding-offboarding, vendor mgmt,
  physical. Mix **preventive, detective, corrective**.

## 4. Implement via policy & process
- Translate controls into the **policy hierarchy**: policies (what/why, mandated) → standards
  (specific requirements) → procedures (how, step-by-step). Keep them clear, approved, versioned,
  communicated, and actually followed — unfollowed policy is a finding waiting to happen.
- Assign every control an **owner** accountable for operating it. Controls are run by people and
  systems in the business, not by the GRC team alone.

## 5. Evidence & test (the proof)
- A control counts only if it's **operating effectively** and you can **show evidence**. Define,
  for each control, what evidence demonstrates it ran (logs, tickets, approvals, config, reports,
  screenshots) and collect it **continuously**, not in an audit-eve scramble. See `references/audit-and-evidence.md`.
- **Test controls** yourself before the auditor does (design + operating effectiveness); treat
  gaps as risks to remediate. Automate evidence collection where possible.

## 6. Audit, report & improve
- Support internal and external audits/certification: readiness assessment, gap remediation,
  evidence packages, auditor liaison, and managing findings to closure. See `references/audit-and-evidence.md`.
- **Report posture** to leadership in business terms: top risks, control health, compliance
  status, open findings, trend. Drive a continual-improvement cycle — compliance is a program,
  not a one-time project (certifications recur; regulations and risks change).

## Operating principles
- **Risk-driven.** Controls treat identified risks; don't collect controls for their own sake.
- **Substance over paperwork.** A control is real only if it operates and is followed —
  evidence, not assertions.
- **Translate both ways.** Make regulation concrete for engineers, and make technical reality
  legible to auditors and leadership.
- **Map once, satisfy many.** Crosswalk frameworks; reuse controls and evidence.
- **Continuous, not point-in-time.** Collect evidence and monitor controls year-round.
- **Honest posture.** Report real gaps and residual risk; don't paper over them — misrepresenting
  compliance is itself a serious risk.
- **Own by name.** Every control, risk, and policy has an accountable owner.

## Related skills
- **cloud-security** — the technical controls (IAM, encryption, logging, config) GRC relies on
  and gathers evidence from.
- **cyber-security-analyst** — the monitoring/detection controls behind incident readiness.
- **incident-response** — when an incident occurs: it drives the breach-notification
  obligations, regulatory deadlines, and risk-register updates this skill owns.
- **security-review** / **pentest** — assessment activities that provide control evidence and
  find risks to register.
- **technical-writing** — drafting clear, well-structured policies, standards, and procedures.
