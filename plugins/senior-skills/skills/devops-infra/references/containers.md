# Containers & Orchestration

Packaging apps as portable, immutable images and running them reliably.

## Images (Docker/OCI)
- **Small & minimal:** start from a slim/distroless base; multi-stage builds so the
  runtime image carries only what's needed, not build tools.
- **Pin base images by digest**; rebuild regularly to pick up patches.
- **Non-root user**; read-only root filesystem where possible; drop capabilities.
- **No secrets in images or layers** (they persist in history) — inject at runtime.
- Order layers for cache efficiency (deps before app code). One concern per image.
- Scan images for CVEs in the pipeline; sign them; use immutable tags/digests in deploys.
- Define a meaningful `HEALTHCHECK`; handle signals for graceful shutdown.

## Orchestration (Kubernetes & friends)
- **Declarative manifests** (version-controlled); apply via pipeline/GitOps, not kubectl-by-hand.
- **Resource requests/limits** on every workload to protect nodes and enable scheduling.
- **Liveness/readiness/startup probes** so the platform knows real health.
- **Config & secrets** via ConfigMaps/Secrets (or an external secrets operator);
  Secrets are base64, not encrypted at rest by default — enable encryption / use a vault.
- **Horizontal scaling** (HPA) on the right signal; set PodDisruptionBudgets for availability.
- **Rolling updates** with surge/unavailable tuned; readiness-gated; easy rollback to prior revision.

## Security (pair with cloud-security)
- Least-privilege RBAC; separate namespaces per team/env; network policies to restrict traffic.
- Pod security standards (no privileged, no host mounts unless required).
- Workload identity (federated) over long-lived cloud keys in pods.
- Admission control / policy-as-code to enforce the above.

## Networking & state
- Services for stable addressing; Ingress/Gateway for external traffic (TLS termination).
- Prefer stateless workloads; use managed data stores or StatefulSets with care for state.
- Externalize all config so the same image runs in every environment.

## Common pitfalls
- Running as root / fat images with build tools and secrets baked in.
- No resource limits (noisy-neighbor, OOM); no readiness probe (traffic to unready pods).
- Floating `:latest` tags → non-reproducible deploys and surprise changes.
- Treating K8s Secrets as encrypted; over-broad RBAC.
