# Cloud IAM & Identity

Identity is the real perimeter in the cloud. Over-permissioned identities and
long-lived keys cause most incidents.

## Least privilege
- Grant only the actions needed, on only the resources needed, under the conditions
  needed. Start from nothing and add; don't start from admin and trim later (you won't).
- Prefer scoped, purpose-built roles over broad managed-admin policies. Avoid wildcards
  (`*`) in actions/resources except where genuinely required and justified.
- Use conditions/constraints (source IP, MFA present, tag-based, time) to tighten further.
- Review access regularly; prune unused permissions and identities. Use access analyzers
  and "last used" data to find over-grants.

## Human identity
- **SSO + MFA** enforced for all humans; centralize in an identity provider. No shared logins.
- **Root / global admin:** lock it down — hardware MFA, no access keys, break-glass only,
  alert on every use. Day-to-day work uses scoped roles, never root.
- Separate duties: the person who approves ≠ the person who deploys for sensitive actions.

## Workload identity (no static keys)
- Workloads should assume roles / use workload identity federation / instance or pod
  identity — **short-lived, automatically rotated** credentials. Avoid embedding static
  access keys in code, images, CI, or config.
- CI/CD: use OIDC federation to assume a scoped role per job rather than stored cloud keys.
- If a static credential is unavoidable, store it in a secrets manager, scope it tightly,
  rotate it, and monitor its use.

## Blast-radius controls
- **Guardrails above accounts:** SCPs (AWS), Azure Policy / Management Group policies,
  GCP Org Policy — set hard ceilings (e.g. deny disabling logging, deny public exposure,
  restrict regions) that individual accounts can't override.
- **Permission boundaries** to cap what a delegated admin can grant.
- Separate accounts/subscriptions/projects per environment and blast-radius boundary;
  isolate prod. Keep a clean account structure.

## Keys & secrets
- Rotate keys; detect and revoke leaked ones fast (secret scanning in repos/CI).
- Never commit credentials; never log them. Prefer federation to keys everywhere.

## Common pitfalls
- Wildcard admin policies attached "to get it working," never tightened.
- Long-lived access keys in CI, laptops, or images.
- Root/global-admin used routinely; no MFA; no alerting on its use.
- No org-level guardrails → one account's mistake is unbounded.
- Cross-account trust that's too permissive (confused-deputy; missing external-id/conditions).
