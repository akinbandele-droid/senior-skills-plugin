---
name: devops-infra
description: >-
  Ship and operate software reliably: CI/CD pipelines, Infrastructure-as-Code,
  containers and orchestration, deployment strategies, configuration/secrets,
  and observability. Use when the user is setting up or improving build/test/
  deploy automation, writing Terraform/Pulumi/CloudFormation or Kubernetes/
  Docker, designing release or rollback strategy, or standing up monitoring,
  logging, and alerting. Emphasizes reproducibility, least privilege, and safe
  change. For securing the cloud account itself use `cloud-security`; for the
  application code use `software-engineering`.
---

# DevOps & Infrastructure

Getting software from a commit to running in production — repeatably, observably,
and safely. The guiding ideas: everything-as-code and under version control,
automate the path to production, make changes small and reversible, and know the
state of the system from its telemetry rather than by guessing.

## Core loop

1. **Understand the delivery need** — what ships, how often, what "healthy" means,
   the environments, and the constraints (compliance, uptime, team size).
2. **Codify** — infrastructure, pipelines, and config as versioned code; no
   click-ops for anything that must be reproducible.
3. **Automate the pipeline** — build → test → scan → deploy, gated and repeatable.
4. **Deploy safely** — a strategy that limits blast radius and can roll back.
5. **Observe** — metrics, logs, traces, and actionable alerts tied to SLOs.
6. **Iterate** — feed incidents and toil back into the system; reduce manual steps.

References (load the matching one):
- `references/cicd.md` — pipeline design, stages, gates, caching, supply-chain safety.
- `references/iac.md` — Infrastructure-as-Code patterns, state, modules, drift.
- `references/containers.md` — Docker images and Kubernetes/orchestration essentials.
- `references/observability.md` — metrics/logs/traces, SLIs/SLOs, alerting.

## 1. Understand the delivery need
- What is being deployed (service, static site, batch job, mobile artifact) and
  the release cadence. Define environments (dev/staging/prod) and promotion flow.
- Define "healthy": the SLIs that matter (latency, error rate, saturation) and the
  SLOs/targets. These drive both deploy gating and alerting.
- Note constraints up front: data residency, change windows, approval requirements,
  and who is on call.

## 2. Codify everything
- **IaC** for infrastructure (`references/iac.md`): declarative, versioned, reviewed.
  Keep state secure and locked; use modules for reuse; detect and reconcile drift.
- **Pipeline-as-code** living with the repo. **Config-as-code** with environment
  overlays; never bake environment specifics or secrets into images/artifacts.
- One source of truth per concern. If a human changed it by hand, it will drift.

## 3. Automate the pipeline
- Stages: checkout → build → unit/integration tests → static analysis & security
  scans (SAST/deps/secret-scan/IaC-scan) → artifact build & sign → deploy →
  smoke/verify. Fail fast; make each stage reproducible and cacheable.
- **Supply-chain:** pin dependencies, verify/sign artifacts, generate an SBOM,
  use least-privilege pipeline credentials (prefer OIDC/short-lived over static keys).
- Make the pipeline the *only* path to prod so it can't be bypassed.

## 4. Deploy safely
- Pick a strategy by risk: rolling, blue-green, or canary. Always have a tested,
  fast **rollback** (or roll-forward) path. Decouple deploy from release with
  feature flags where it helps.
- Run migrations backward-compatibly (expand/contract) so deploy and rollback don't
  break the schema. Verify with smoke tests and health checks before shifting traffic.
- Guard prod changes: approvals/change windows where required; never a manual prod
  edit that isn't reflected in code.

## 5. Observe
- Instrument the **three signals**: metrics (rates/errors/durations + saturation),
  logs (structured, correlated), traces (across services). See `references/observability.md`.
- Alert on **symptoms tied to SLOs** (user-facing pain), not every cause. Every
  alert should be actionable and have a runbook. Watch for alert fatigue.

## 6. Iterate & reduce toil
- Post-incident: blameless review, fix the system not the person, automate the manual
  recovery step. Track toil and cut it. Keep environments as close to identical as possible.

## Operating principles
- **Reproducible > clever.** Anyone should recreate the environment from code.
- **Least privilege everywhere** — pipelines, service identities, state access.
- **Small, reversible changes.** Prefer many safe deploys over rare big-bang ones.
- **Secure the path to prod** (supply chain, secrets, access) as part of the pipeline,
  not after — pair with `cloud-security` and `security-review`.
- **Observability is a feature.** If you can't see it, you can't operate it.
- **No manual prod drift.** Change through code and pipeline, always.

## Related skills
- **cloud-security** — harden the cloud account/identities the infra runs in.
- **software-engineering** — build the application the pipeline ships.
- **incident-response** — when an operational issue becomes a security incident to be
  contained, investigated, and recovered from.
- **data-engineering** — deploying and operating data stores and pipelines.
