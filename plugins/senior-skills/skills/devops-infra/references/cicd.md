# CI/CD Pipelines

Design build/test/deploy automation that is fast, reproducible, and safe to trust
as the only path to production.

## Pipeline stages (typical)
1. **Trigger** — on push/PR/tag; separate PR validation from release pipelines.
2. **Build** — compile/package in a clean, pinned environment; produce an immutable artifact.
3. **Test** — unit → integration; run fast checks first, fail fast. Parallelize.
4. **Analyze & scan** — linters, SAST, dependency/CVE scan, secret scanning, IaC scan,
   license check. Treat high-severity findings as gates.
5. **Artifact** — build once, promote the *same* artifact across environments; sign it;
   emit an SBOM; push to a registry with an immutable tag/digest.
6. **Deploy** — to staging, verify, then promote to prod via the chosen strategy.
7. **Verify** — smoke tests, health checks, key SLIs before declaring success.

## Principles
- **Build once, promote the same artifact.** Rebuilding per environment invites drift.
- **Fast feedback.** Order stages so common failures surface first; cache deps and layers.
- **Deterministic builds.** Pin tool and dependency versions; isolate from the network
  where possible; same input → same output.
- **The pipeline is the only path to prod.** No side-door manual deploys.
- **Idempotent, re-runnable.** A retried stage shouldn't corrupt state.

## Supply-chain security
- Pin dependencies (lockfiles) and base images (by digest, not floating tags).
- **Least-privilege credentials:** prefer short-lived/OIDC-federated tokens over
  long-lived static secrets; scope them to the one job that needs them.
- Sign artifacts and verify signatures before deploy; generate and store an SBOM.
- Scan third-party actions/plugins; pin them too. Protect the pipeline config and
  the registry as production assets.
- Never echo secrets into logs; mask them; rotate on exposure.

## Secrets & config in pipelines
- Inject secrets from a manager/vault at runtime; never commit them or bake into images.
- Environment config via overlays/parameters, not forked pipelines per env.
- Store state and caches in access-controlled, non-public locations.

## Gates & approvals
- Automated gates: tests, coverage threshold, scans, policy checks.
- Human approvals for prod where required; record who/when. Change windows if mandated.

## Common pitfalls
- Flaky tests eroding trust (quarantine and fix, don't ignore).
- Rebuilding artifacts per stage; environment-specific builds.
- Over-broad pipeline credentials; static cloud keys in CI.
- Slow pipelines nobody waits for → people bypass them.
- No rollback path tied to the deploy stage.
