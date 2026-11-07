# Infrastructure as Code

Declarative, versioned, reviewed infrastructure — the environment is recreatable
from the repo, and changes go through the same rigor as application code.

## Principles
- **Declarative desired state** over imperative scripts where possible (Terraform,
  Pulumi, CloudFormation, Bicep, Crossplane). Describe *what*, let the tool reconcile.
- **Version controlled & reviewed.** IaC changes get PRs, plans, and review like code.
- **Plan before apply.** Always review the diff/plan; never apply blind to prod.
- **Immutable infrastructure.** Replace rather than mutate in place where feasible
  (rebuild the image/instance, don't hand-patch).
- **Idempotent.** Re-applying unchanged code is a no-op.

## State management
- State is sensitive (may hold secrets) and authoritative — store it remotely,
  encrypted, access-controlled, and **locked** to prevent concurrent corruption.
- Never edit state by hand except as a last resort with backups. Isolate state per
  environment so a prod change can't touch dev and vice versa.
- Separate state/workspaces by environment and by blast-radius boundary.

## Structure & reuse
- **Modules** for reusable components; keep them small, documented, versioned.
- Compose environments from modules with per-env variables — don't copy-paste stacks.
- Keep a clear layering: networking/foundational → platform → application.
- Name and tag resources consistently (owner, env, cost-center) for ops and billing.

## Drift
- Drift = reality diverged from code (usually manual changes). Detect it (scheduled
  plan/drift detection) and reconcile by updating code then applying — not by more
  manual edits. Treat "someone changed it in the console" as an incident to codify.

## Safety
- Least-privilege for the IaC runner's credentials; separate plan (read) from apply.
- Guard destructive changes (resource replacement/deletion) — review plans for
  `destroy`/`replace`; use prevent-destroy on stateful resources (databases, data stores).
- Policy-as-code (OPA/Sentinel/tfsec/checkov) to enforce guardrails automatically
  (encryption on, no public buckets, tagging) — see `cloud-security`.

## Secrets
- Don't hardcode secrets in IaC or state outputs. Reference a secrets manager;
  mark variables sensitive; avoid printing them in plan/apply output.

## Common pitfalls
- Monolithic state with huge blast radius; no locking.
- Hardcoded secrets or env specifics in modules.
- Applying without reviewing the plan; manual console changes causing drift.
- No prevent-destroy on databases → a refactor nukes data.
