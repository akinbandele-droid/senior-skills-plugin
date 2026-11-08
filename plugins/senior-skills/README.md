# Skills Index

A set of custom, user-level skills (available across all projects) covering the full
lifecycle of designing, building, shipping, securing, and testing software and
interfaces — designed to work together. Each skill is a `SKILL.md` (the core loop +
principles) plus `references/` files loaded on demand for depth.

> These auto-trigger when a task matches, or invoke one explicitly with
> `/<skill-name>`. They live in `~/.claude/skills/`.

## The skills (25)

### Build & engineer

#### 🔧 software-engineering
Engineer changes with senior-level discipline — understand, explore, design,
implement, verify, review.
**References:** workflow · code-quality · debugging · testing · code-review ·
refactoring · api-system-design · performance · independent-review

#### 🗄️ data-engineering
The data layer — schema/data modeling, SQL & query performance, pipelines (ETL/ELT),
and data quality/integrity/governance.
**References:** data-modeling · sql-and-performance · pipelines · data-quality

#### 📊 data-analyst
Analyze data to answer questions & drive decisions — frame the question, acquire/clean,
explore (EDA), apply sound statistics, and communicate actionable insight honestly.
**References:** analysis-process · data-cleaning · exploratory-analysis · statistics · communicating-insights

#### 🤖 ai-engineering
Build reliable LLM/AI features — prompting & context, RAG, tools/agents, evaluation,
and safety/guardrails. (LLM as one component of a well-engineered system.)
**References:** prompting-and-context · rag · agents-and-tools · evaluation · ai-safety

### Databases

#### 🏛️ database-design
Schema & data modeling — normalization, keys, relationships, constraints, choosing a store
(relational/NoSQL), dimensional/OLAP models, and safe schema evolution.
**References:** relational-modeling · choosing-a-store · analytical-modeling · evolution-and-migrations

#### 🧮 sql-development
Correct, clear, performant SQL — joins/NULLs/windows/CTEs, stored programs, transactions &
concurrency, and query optimization/indexing.
**References:** querying · programmability · transactions · optimization

#### 🛢️ database-administration
The DBA/ops role — install & config, backup & recovery, HA/replication/failover, performance
tuning, monitoring, capacity, patching/upgrades.
**References:** backup-and-recovery · high-availability · performance-tuning · operations

#### 🔐 database-security
DB-specific hardening — access control & least privilege, encryption (transit/rest/column),
auditing & activity monitoring, injection & misconfiguration defense.
**References:** access-control · encryption · auditing-and-monitoring · hardening-and-injection

### Design

#### 🎨 ui-ux-design
Design usable, accessible interfaces — users & goals, IA/flows, visual & interaction
fundamentals, WCAG, critique. (General, medium-agnostic.)
**References:** ux-process · visual-design · interaction-design · accessibility · critique

#### 🌐 web-design
The web-specific craft — responsive layout, CSS architecture, performance, and
semantic/accessible markup.
**References (core):** responsive-layout · css-architecture · web-performance · web-fundamentals
**References (specialized):** landing-pages · design-system · animation · email-html

#### 🖼️ graphic-design
Visual communication — layout/composition, typography, color, brand/identity, imagery, and
production-ready files for print and screen (logos, posters, packaging, social/ad graphics).
**References:** composition-and-layout · typography · color · branding-and-identity · production

### Ship & operate

#### 🚀 devops-infra
Ship and operate reliably — CI/CD, Infrastructure-as-Code, containers/orchestration,
deployment strategy, and observability.
**References:** cicd · iac · containers · observability

#### ☁️ cloud-security
Secure the cloud (AWS/Azure/GCP) — IAM & least privilege, data/network protection,
hardening & guardrails, posture and detection. (Design & review.)
**References:** iam · data-and-network · posture-and-guardrails

### Quality & documentation

#### ✅ qa-testing
Quality assurance with rigor — test strategy & the pyramid, case design, automation,
exploratory & non-functional testing, bug reporting.
**References:** test-strategy · test-design · automation · exploratory-and-nonfunctional · bug-reporting

#### 🧪 advanced-software-testing
Testing beyond basics — property-based/metamorphic/model-based, mutation testing & fuzzing,
integration/contract/E2E, performance/load & chaos, test architecture, and testing in CI/CD.
**References:** advanced-techniques · fault-based-testing · integration-and-e2e · performance-and-resilience · test-architecture · ci-cd-testing

#### 📝 technical-writing
Clear, accurate docs that serve the reader — READMEs, API reference, guides, design
docs/ADRs, runbooks, release notes.
**References:** doc-types · writing-style

### Secure, defend & govern (review, operate, verify)

#### 🔒 security-review
Find real, exploitable weaknesses in code and design, run as an open/transparent
adversarial review. (Static & architectural.)
**References:** threat-modeling · secure-code-review · open-review · findings-and-disclosure

#### 🧯 cyber-security-analyst
Defensive / SOC analyst — detection engineering, alert triage & investigation, threat
hunting, incident response, and threat intel. (Blue team; MITRE ATT&CK.)
**References:** detection-engineering · triage-and-investigation · incident-response · threat-hunting · threat-intel

#### 🚨 incident-response
Lead security incidents & DFIR end to end — incident command, forensic acquisition &
evidence, contain/eradicate/recover, playbooks, legal/regulatory notification, post-incident
review. (NIST SP 800-61 / SANS PICERL.)
**References:** lifecycle · forensics-and-evidence · containment-eradication-recovery · playbooks · coordination-and-comms

#### 📋 grc-analyst
Governance, Risk & Compliance — frameworks (SOC 2/ISO 27001/NIST/PCI/HIPAA/GDPR), risk
register, policies & controls, audit evidence, and third-party risk.
**References:** frameworks · risk-management · policies-and-controls · audit-and-evidence · third-party-risk

#### 🛰️ osint-recon
Authorized open-source intelligence & reconnaissance — footprinting, passive asset
discovery, leaked-secret & exposure mapping. (Passive-first, authorization-gated.)
**References:** footprinting · exposure-and-leaks · opsec-and-reporting

#### 🛡️ pentest
Authorized penetration testing of *running* systems — networks, hosts, cloud, AD —
scoping, recon, enumeration, controlled exploitation, reporting. (Authorization-gated.)
**References:** methodology · web-app · reporting

#### 🕷️ web-pentest
Authorized testing of *web apps* — WSTG-aligned mapping, auth/session/authz, injection,
business logic, client-side, reporting. (Authorization-gated.)
**References:** methodology · vulnerability-classes · tooling · reporting · example-report

#### 🔌 api-pentest
Authorized testing of *APIs* (REST/GraphQL/gRPC) — schema discovery, token/JWT/OAuth,
BOLA/BFLA, mass assignment, data exposure, resource abuse. (Authorization-gated.)
**References:** methodology · api-top10 · graphql-grpc · reporting

#### 📱 mobile-pentest
Authorized testing of *mobile apps* (iOS/Android) — static + dynamic analysis, traffic
interception, local data/secrets, platform controls, backend. (Authorization-gated.)
**References:** methodology · android · ios · reporting

## How they fit together

```
   graphic-design ─► ui-ux-design ──(foundations)──► web-design
                          │                              │
                          └───────────┐      ┌───────────┘
                                      ▼      ▼
  data-engineering ► data-analyst ► software-engineering ◄ ai-engineering
                                      │  (build it right)
            ┌─────────────────────────┼─────────────────────────┐
            ▼                         ▼                          ▼
       qa-testing            technical-writing            devops-infra ──► cloud-security
      (verify it)             (document it)               (ship & run it)   (secure the cloud)
                                      │
             ┌────────────────────────┼────────────────────────┐
             ▼                        ▼                         ▼
      security-review     cyber-security-analyst ─► incident-response     grc-analyst
    (review code/design)   (detect & hunt, SOC)     (handle & recover)  (risk, controls, audit)
             │                        │
             └─────(then, authorized)─┴─► osint-recon ─► pentest / web- / api- / mobile-pentest
                                          (recon)        (test the running target)
```

- **Design → Build:** `graphic-design` is the visual-communication craft; `ui-ux-design`
  gives UX/visual/accessibility foundations; `web-design` applies them to the web;
  `software-engineering` implements with testing and review discipline, with
  `data-engineering` (building the data layer), `data-analyst` (analyzing data for
  decisions), and `ai-engineering` (LLM/AI features) for those domains.
- **Databases:** a dedicated family for data at the storage layer — `database-design`
  (schema/model), `sql-development` (queries & DB programs), `database-administration`
  (run it in production: backup, HA, tuning), and `database-security` (harden it). They
  sit under `data-engineering` for deep database work and connect to `cloud-security`,
  `devops-infra`, and `incident-response`.
- **Verify & document:** `qa-testing` for test strategy and non-functional quality, with
  `advanced-software-testing` for the deeper techniques (property/mutation/fuzz, E2E,
  performance/chaos) and testing throughout CI/CD; `technical-writing` to document what's built.
- **Ship & operate:** `devops-infra` for CI/CD, IaC, deploy, and observability;
  `cloud-security` to secure the environment it runs in.
- **Secure, defend & govern:** `security-review` inspects code/design before shipping;
  `cyber-security-analyst` detects, triages, and hunts (blue team / SOC) and hands a
  confirmed incident to `incident-response` for forensics, containment, recovery, and
  breach notification; `grc-analyst` governs risk, controls, and compliance; then, under
  authorization,
  `osint-recon` maps the exposed surface (passive) and the pentest family validates the
  live target — `pentest` (systems/networks/cloud), `web-pentest` (web apps),
  `api-pentest` (REST/GraphQL/gRPC), `mobile-pentest` (iOS/Android). Review, defense,
  governance, and offensive testing reinforce each other.
- Each `SKILL.md` ends with a **Related skills** section pointing to its neighbors.

## Shared principles across all skills
- **Understand before you act** — the real goal and constraints first.
- **Smallest effective change / effort matched to stakes.**
- **Prove it** — verify, test, demonstrate, measure; don't claim unverified results.
- **Be honest about state** — name what's unchecked, skipped, theoretical, or risky.
- **Accessible, secure, and maintainable by default**, not as a later pass.
- **Transparency & reproducibility** — document the *why* and make work inspectable.
- **Authorization & scope are hard boundaries** — for the security-testing skills
  (`pentest` family and `osint-recon`), nothing active happens without confirmed
  authorization and an agreed scope.

## Conventions
- `SKILL.md` frontmatter: `name` (kebab-case, matches the folder) + a `description`
  that says *what it does* and *when to use it* (that's what triggers it).
- Keep the `SKILL.md` lean; push depth into `references/*.md` loaded only when needed.
- Author new skills with the `skill-creator` skill for structure and evals.
