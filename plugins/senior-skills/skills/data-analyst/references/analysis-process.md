# The Analysis Process

A disciplined workflow from a fuzzy request to a trusted, actionable answer. Most failed
analyses fail at the start (wrong question) or the end (unclear communication), not in the math.

## Frame the question (the highest-leverage step)
- **Find the decision behind the request.** Someone asks for a number because they face a choice.
  "How many signups last month?" may really be "is our growth healthy and what do we change?"
  Ask: *what decision will this inform, and what would you do differently depending on the answer?*
- **Make it specific and measurable.** Turn vague terms into definitions everyone agrees on:
  what exactly is an "active user," "churn," "conversion," the population, the time frame, and the
  comparison (vs. last period? vs. a target? vs. a segment?). Ambiguous definitions produce
  unusable or disputed results.
- **Scope to the decision.** Don't boil the ocean — answer what's needed, note what's out of scope,
  and state assumptions and constraints. Agree the question *before* pulling data; re-confirm if the
  data forces a reframe.

## Plan the analysis
- Decide what data and grain you need, what method answers the question (comparison, trend,
  segmentation, cohort, funnel, test, model), and what "good enough to decide" looks like.
- Identify the authoritative data source and anticipate quality risks. Match effort to the stakes
  of the decision.

## Execute (acquire → clean → explore → analyze)
See the dedicated references: acquisition and SQL grain (`../data-engineering` SQL reference),
`data-cleaning.md`, `exploratory-analysis.md`, and `statistics.md`. Keep it iterative — EDA often
reshapes the question. Throughout, **write down what you did and why** (sources, filters, cleaning
rules, transformations, method) so the analysis is reproducible and defensible.

## Reproducibility & rigor
- Prefer **code/queries over manual spreadsheet steps** for anything repeated or important — it's
  auditable, rerunnable, and less error-prone. Version your analysis; parameterize dates/filters.
- Keep a clear path from raw data → cleaned data → analysis → result, so anyone (including future
  you) can retrace and trust it. Save the exact query and the data snapshot/date.
- **Peer review** important findings — a second set of eyes catches a wrong join or a misread field
  before it reaches a decision.

## Validate before you believe it
- Sanity-check against known reality (totals, plausible magnitudes, expected ranges).
- Ask **"what would make this conclusion wrong?"** — a data artifact, a confounder, a bias, too
  small a sample — and check each. Reproduce the key number a second way if you can.
- Distinguish *interesting* from *actionable* and from *reliable*; only promote to a recommendation
  what survives scrutiny.

## Common pitfalls
- Answering the literal request, not the real question/decision behind it.
- Starting to pull data before the question and definitions are agreed.
- Vague metrics ("engagement") that mean different things to different people.
- Manual, undocumented steps → unreproducible, unverifiable, error-prone.
- Skipping validation and shipping a surprising number that's really a data bug.
