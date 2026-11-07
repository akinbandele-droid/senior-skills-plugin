# Posture, Guardrails & Detection

Make insecure states hard to create (prevent), catch the rest fast (detect), and
keep measuring (posture).

## Secure baselines
- Apply **CIS Benchmarks** / the provider's security baseline for each service.
- Turn on account-wide protections: default encryption, public-access blocks, secure
  defaults for new resources, mandatory logging.
- Codify the baseline so new accounts/subscriptions/projects start compliant (landing
  zone / org blueprint), not hardened after the fact.

## Guardrails (prevent)
- **Policy-as-code at the org level:** SCPs (AWS), Azure Policy, GCP Org Policy — hard
  rules accounts can't override (deny public exposure, deny disabling logs/encryption,
  restrict regions/services, enforce tagging).
- **IaC policy scanning** (tfsec, checkov, OPA/Conftest, Sentinel) in the pipeline so
  insecure infra is blocked **before** apply — pair with `devops-infra`.
- Prefer prevention to detection wherever you can: a config that can't be created
  insecurely needs no alert.

## Posture management (measure)
- **CSPM** / the provider's posture tools (Security Hub, Defender for Cloud, Security
  Command Center) continuously scan for misconfig and drift.
- Triage findings by **real exposure and data impact**, not raw count — a public bucket
  with PII outranks a missing tag. Track to remediation; watch for recurrence/drift.

## Detection & logging
- Enable and **centralize** audit/activity logs (CloudTrail, Activity Log, Cloud Audit
  Logs) across all accounts; protect them from tampering/deletion (separate, locked
  logging account/bucket with immutability).
- Enable the provider threat-detection service (GuardDuty, Defender, SCC threat detection).
- **Alert on** security-relevant events: root/global-admin use, IAM/policy changes,
  public-access changes, security-group opens, new credential creation, anomalous or
  high-risk API calls, disabled logging.
- Route alerts into `incident-response`; every alert needs an owner and a runbook.

## Compliance
- Map controls to the frameworks in scope (PCI/HIPAA/SOC 2/GDPR/ISO). Use the posture
  tools' compliance packs for evidence, but don't confuse "passes the pack" with "secure."

## Common pitfalls
- Detect-only with no prevention → the same misconfig recurs forever.
- Logs in the same account an attacker would compromise (no isolation/immutability).
- Alert overload with no triage; findings tracked but never fixed.
- Treating a compliance checkbox as the goal rather than reduced real risk.
