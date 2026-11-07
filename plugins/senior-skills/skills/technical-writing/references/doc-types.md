# Documentation Types

Match the document to the reader's goal. The four **Diátaxis** modes serve distinct
needs and shouldn't be blended; the operational docs below are common concrete forms.

## The four modes (Diátaxis)
| Mode | Serves | Reader is… | Shape |
|------|--------|-----------|-------|
| **Tutorial** | Learning | A beginner, needs confidence | A guided lesson that *works*, start to finish |
| **How-to guide** | A task | Someone who knows basics, has a goal | Numbered steps to accomplish one thing |
| **Reference** | Looking up facts | Someone who needs accuracy | Complete, consistent, dry, structured |
| **Explanation** | Understanding | Someone curious about *why* | Discussion of concepts, trade-offs, context |

- **Tutorial:** hold the reader's hand; guarantee success; minimize choices and digressions;
  concrete, not abstract. Goal: they feel it worked and they can do it.
- **How-to:** assume competence; focus on the goal; list prerequisites; steps + verification.
  Don't teach concepts here — link to explanation.
- **Reference:** describe the machinery accurately and completely (every param, return, error,
  config key). Consistent structure; no opinions or tasks. Readers scan, not read.
- **Explanation:** the "why," alternatives considered, how pieces fit. Read away from the keyboard.

Mixing these is the most common doc failure (a reference padded with tutorial asides helps no one).

## README
The front door. Orient a newcomer fast. Typical order:
1. **What it is** and what problem it solves (one or two sentences).
2. **Quick start** — install + a minimal working example that runs.
3. **Key usage** / common tasks (or links to how-tos).
4. **Links** to deeper docs, reference, contributing, license.
Keep it current; it's the most-read doc. Don't dump everything here — link out.

## ADR (Architecture Decision Record)
A short record of one significant decision. Classic structure:
- **Context** — the forces and constraints at play.
- **Decision** — what was chosen (and the main alternatives rejected).
- **Consequences** — results, trade-offs, follow-ups.
Immutable once accepted; supersede with a new ADR rather than editing history. Captures
the *why* so future readers don't re-litigate or undo it blindly.

## Runbook
Operational doc for handling a specific situation (deploy, failover, an alert firing).
- Trigger/when-to-use, prerequisites/access, numbered recovery steps, verification,
  rollback, and escalation/contacts. Written to be followed under stress — unambiguous,
  copy-pasteable, no missing assumptions. Pair with `devops-infra` observability/alerts.

## Release notes / changelog
What changed, for the reader who must decide whether to upgrade. Group by audience impact:
breaking changes (loudest, with migration steps), new features, fixes, deprecations.
Write for users, not as a raw commit dump. Follow a consistent format and semantic versioning.

## API / code reference
Complete and accurate: every endpoint/function with params, types, returns, errors,
auth, and a working example per common call. Generated from source where possible so it
stays in sync. Consistency and correctness matter more than prose.
