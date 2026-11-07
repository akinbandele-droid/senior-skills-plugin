---
name: cloud-security
description: >-
  Secure cloud environments (AWS, Azure, GCP) by design and review: identity and
  access (IAM), network and data protection, secrets management, secure
  configuration/hardening, posture and compliance review, logging/detection, and
  cloud threat modeling. Use when the user is designing, hardening, or reviewing
  the security of a cloud account/landing zone, setting up IAM or guardrails,
  reviewing cloud config for misconfigurations, or threat-modeling cloud
  architecture. Mapped to the cloud providers' well-architected/security pillars,
  CIS Benchmarks, and least privilege. For building the infra use `devops-infra`;
  for live testing use the `pentest` family; for code use `security-review`.
---

# Cloud Security

Making a cloud environment secure by default and keeping it that way. The cloud's
shared-responsibility model means the provider secures the infrastructure *of* the
cloud; you secure what you run *in* it — identity, configuration, data, and network.
Most cloud breaches are **misconfiguration and over-permissioned identity**, not
exotic exploits. Favor guardrails that make the secure path the easy path.

## Core loop

1. **Understand the environment** — accounts/subscriptions/projects, workloads,
   data sensitivity, compliance obligations, and the current baseline.
2. **Identity first** — least-privilege IAM, strong auth, no long-lived static keys.
3. **Protect data & network** — encryption, segmentation, no unintended public exposure.
4. **Harden configuration** — secure defaults, benchmark baselines, guardrails as policy.
5. **Detect & respond** — logging, monitoring, and alerting on security-relevant events.
6. **Review continuously** — posture scanning, threat modeling, and remediation.

References (load the matching one):
- `references/iam.md` — identity, least privilege, federation, keys, boundaries.
- `references/data-and-network.md` — encryption, storage exposure, segmentation.
- `references/posture-and-guardrails.md` — benchmarks, policy-as-code, CSPM, detection.

## 1. Understand the environment
- Inventory accounts/subscriptions/projects and the organization structure. Know what
  runs where, who owns it, and how environments (prod/non-prod) are separated.
- Classify data (public → restricted/regulated) — it drives encryption, access, and residency.
- Capture compliance scope (PCI, HIPAA, SOC 2, GDPR, etc.) and existing baseline/controls.

## 2. Identity first (the real perimeter)
- **Least privilege:** grant only needed actions on needed resources; start minimal
  and add. Prefer managed/scoped roles over broad admin. Review and prune regularly.
- **No long-lived static keys** for workloads — use workload identity / role assumption /
  OIDC federation with short-lived credentials. Rotate anything that must be static.
- **Strong human auth:** SSO/MFA enforced; tightly limit and monitor root/global-admin;
  no shared accounts. Use permission boundaries/SCPs/org policies to cap blast radius.
- See `references/iam.md`.

## 3. Protect data & network
- **Encrypt** at rest and in transit by default; manage keys properly (rotation,
  access control, separation of duties); consider customer-managed keys for sensitive data.
- **No unintended public exposure** — the classic cloud breach. Default storage buckets/
  blobs/disks and databases to private; block public access at the account level.
- **Segment** the network (VPCs/VNets, subnets, security groups, private endpoints);
  default-deny; expose the minimum; keep management planes off the public internet.
- See `references/data-and-network.md`.

## 4. Harden configuration
- Apply secure baselines (CIS Benchmarks / provider security baselines). Turn on the
  account-wide protections (public-access blocks, default encryption, guardrails).
- **Guardrails as policy** (SCPs/Azure Policy/Org Policy, policy-as-code in IaC) so
  insecure configs are prevented, not just detected. See `references/posture-and-guardrails.md`.

## 5. Detect & respond
- Enable and centralize audit/activity logging (CloudTrail/Activity Log/Audit Logs) and
  the provider's threat-detection service (GuardDuty/Defender/SCC). Protect logs from tampering.
- Alert on security-relevant events (root use, IAM/policy changes, public-exposure changes,
  anomalous API calls). Feed into `incident-response`.

## 6. Review continuously
- Run CSPM/posture scans and IaC security scans; triage by real exposure and data impact.
- Threat-model the architecture (trust boundaries, data flows, identities) — see
  `security-review` for STRIDE. Track findings to remediation; re-check for drift.

## Operating principles
- **Identity is the perimeter.** Least privilege and short-lived credentials beat network
  controls alone.
- **Misconfiguration is the #1 risk.** Secure defaults + guardrails > hoping people configure right.
- **Prevent with policy, detect as backstop.** Make insecure states impossible where you can.
- **Encrypt and privatize by default.** Public is an explicit, reviewed decision.
- **Assume breach.** Log everything security-relevant, limit blast radius, be ready to respond.
- **Authorization for testing.** Reviewing config you own is fine; *testing* live cloud
  (exploitation, cred attacks) needs the `pentest` authorization gate.

## Related skills
- **devops-infra** — build the infra and bake these controls into IaC/pipelines.
- **pentest** — authorized live testing of the cloud environment.
- **security-review** — threat modeling and reviewing application code/design.
- **cyber-security-analyst** — monitoring the detection/logging you enable here and
  investigating when it fires.
- **incident-response** — containing, eradicating, and recovering from a confirmed
  cloud compromise.
- **grc-analyst** — mapping these controls to frameworks (SOC 2/ISO/etc.) and producing
  audit evidence from them.
- **database-security** — database-specific hardening (access, encryption, auditing,
  injection) for data stores in this environment.
