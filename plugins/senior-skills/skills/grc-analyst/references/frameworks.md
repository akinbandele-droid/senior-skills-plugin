# Security & Compliance Frameworks

What the major frameworks/regulations are, what triggers them, and how they differ. Pick
based on what the business must satisfy — then crosswalk so one control set serves many.

## Certifiable security frameworks
- **SOC 2** (AICPA) — an *attestation report* (not a certification) by a CPA firm on controls
  against the **Trust Services Criteria**: Security (always), and optionally Availability,
  Confidentiality, Processing Integrity, Privacy. **Type I** = design at a point in time;
  **Type II** = operating effectiveness over a period (3–12 months) — customers usually want
  Type II. Common demand from B2B SaaS customers. Flexible controls; you define them to meet criteria.
- **ISO/IEC 27001** — international standard for an **ISMS** (Information Security Management
  System). Certifiable by an accredited body. Requires risk assessment, a Statement of
  Applicability against **Annex A** controls, management system (leadership, objectives, internal
  audit, continual improvement). Recertification cycle with surveillance audits. (ISO 27701
  extends it to privacy.)

## US government / sectoral
- **NIST Cybersecurity Framework (CSF 2.0)** — voluntary, outcome-based: functions **Govern,
  Identify, Protect, Detect, Respond, Recover**. Great for organizing a program and communicating
  posture; not certified.
- **NIST SP 800-53** — exhaustive control catalog for US federal systems (and the basis for
  FedRAMP). Control families + baselines by impact level. **NIST SP 800-171** — protecting
  Controlled Unclassified Information (CUI) for federal contractors (tied to CMMC).

## Regulations (legal obligations, not optional)
- **PCI DSS** — contractual standard for anyone storing/processing/transmitting **cardholder
  data**. Prescriptive (network segmentation, encryption, access control, logging, testing).
  Validation level depends on transaction volume (SAQ vs. ROC/QSA).
- **HIPAA** (US) — protects **PHI** (health info). Security Rule (technical/physical/administrative
  safeguards), Privacy Rule, Breach Notification Rule. Applies to covered entities & business associates.
- **GDPR** (EU) — personal data of EU residents. Principles (lawful basis, minimization, purpose
  limitation), data-subject rights (access, erasure, portability), DPIAs, DPO in some cases, and
  **72-hour breach notification**. Large fines. (CCPA/CPRA is the California analog; many others exist.)

## How to work with them
- **Triggers:** customer contracts → SOC 2 / ISO 27001; cards → PCI DSS; health data → HIPAA;
  EU personal data → GDPR; US federal → 800-53/171/FedRAMP/CMMC.
- **Crosswalk / map once:** the frameworks overlap heavily (access control, logging, IR, risk).
  Build a unified control set and map each control to every framework requirement it satisfies —
  collect evidence once, reuse across audits. Mapping references exist (e.g. the Secure Controls
  Framework) to speed this.
- **Compliance ≠ security.** Meeting a framework is a floor and a communication tool, not proof
  you're secure. Drive from real risk; frameworks ensure coverage and give customers/auditors assurance.
- **Scope tightly.** What's in scope determines audit cost and evidence burden; define it deliberately.

## Common pitfalls
- Chasing a certificate without a real risk basis → controls that don't fit the business.
- Treating SOC 2 as a "certification" or Type I as equal to Type II.
- Not crosswalking → duplicate controls and evidence per framework.
- Over-broad scope inflating the audit; or gaps leaving regulated data uncovered.
- Assuming "compliant" means "secure."
