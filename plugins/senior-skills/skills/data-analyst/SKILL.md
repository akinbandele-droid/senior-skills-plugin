---
name: data-analyst
description: >-
  Analyze data to answer questions and drive decisions with analytical rigor:
  frame the real question, acquire and clean data, explore it (EDA), apply sound
  statistics, visualize honestly, and communicate actionable insight. Use when
  the user wants to analyze a dataset, answer a business/research question with
  data, do exploratory analysis, write analytical SQL, interpret results or
  statistics, build a report or dashboard narrative, or turn numbers into a
  recommendation. Emphasizes correct interpretation (correlation vs. causation,
  significance, bias) and clear communication. For building the data
  layer/pipelines use `data-engineering`; for chart craft pair with the `dataviz`
  skill; for modeling/ML features use `ai-engineering`.
---

# Data Analyst

Analysis exists to **answer a question and inform a decision** — not to produce charts.
The value is in asking the right question, trusting the data enough to stand behind the
answer, interpreting results honestly, and communicating so someone can act. A beautiful
dashboard built on dirty data or a misread statistic is worse than useless — it's
confidently wrong. Be skeptical, show your work, and lead with the "so what."

## Core loop

1. **Frame the question** — the real decision behind it, the audience, and what "an answer" means.
2. **Acquire** — get the right data from the right sources; understand what each field means.
3. **Clean & validate** — fix quality issues; know the data's limits before trusting it.
4. **Explore (EDA)** — understand distributions, relationships, and anomalies before concluding.
5. **Analyze** — apply the right method and statistics; interpret carefully and honestly.
6. **Communicate** — the insight and recommendation, told clearly for the audience, with caveats.

References (load the matching one):
- `references/analysis-process.md` — framing questions, scoping, the end-to-end workflow, reproducibility.
- `references/data-cleaning.md` — data wrangling, quality, missing data, outliers, validation.
- `references/statistics.md` — descriptive & inferential stats, correlation vs. causation, pitfalls.
- `references/exploratory-analysis.md` — EDA technique, segmentation, finding the real signal.
- `references/communicating-insights.md` — the narrative, actionable recommendations, honest reporting.

## 1. Frame the question (most analyses fail here)
- Find the **real question and decision** behind the request. "Pull me the signups" usually means
  "are we growing, and what should we do?" Clarify what they'll *do* with the answer — that sets
  the whole analysis. See `references/analysis-process.md`.
- Define success: what metric, over what population and time frame, compared to what. Make vague
  terms concrete ("active users" = ? "churn" = ?). Agree the question before pulling data.
- Scope to what the decision needs; note assumptions and constraints up front.

## 2. Acquire the right data
- Identify authoritative sources (pair with `data-engineering` for where/how the data lives) and
  get exactly the fields and grain you need. Prefer the source of truth over a convenient copy.
- **Understand each field's real meaning, units, and provenance** — the definition, how it's
  collected, and known caveats. Misunderstood columns are a top source of wrong analyses.
- Write analytical SQL/queries deliberately (see `data-engineering`'s SQL reference): mind joins
  that fan out rows, NULL semantics, de-duplication, and the grain you're aggregating to.

## 3. Clean & validate (earn your trust in the data)
- Profile the data and fix issues: duplicates, inconsistent formats/categories, wrong types,
  impossible values, and **missing data** (understand *why* it's missing before dropping/imputing).
  Handle **outliers** by investigating, not blindly deleting. See `references/data-cleaning.md`.
- **Sanity-check against reality:** do totals match a known figure? Is the row count plausible?
  Does the date range cover what you think? Garbage in, garbage out — most "surprising findings"
  are data errors. Document every cleaning decision (it affects the result and reproducibility).

## 4. Explore (EDA before conclusions)
- Look before you leap: distributions (shape, spread, skew), central tendency, and **segments** —
  an average often hides the story (Simpson's paradox, bimodal groups). See `references/exploratory-analysis.md`.
- Examine relationships and trends, and hunt for anomalies and data artifacts. Form hypotheses
  from exploration, then test them — don't cherry-pick the one chart that fits a prior belief.

## 5. Analyze & interpret (where rigor matters most)
- Use the method the question needs — comparison, trend, segmentation, cohort/retention, funnel,
  correlation, a significance test, or a model — no heavier than necessary. See `references/statistics.md`.
- **Interpret honestly:** correlation is not causation; a difference may be noise (check
  significance and practical size, not just a p-value); watch confounders, selection/survivorship
  bias, and small samples. State uncertainty and what the data *can't* tell you.
- Validate findings: reproduce, check robustness, and ask "what would make this wrong?" before you believe it.

## 6. Communicate the insight
- **Lead with the answer and the "so what,"** then support it — not a tour of your process. Tailor
  depth and language to the audience (exec vs. analyst). See `references/communicating-insights.md`.
- Visualize honestly (pair with the **`dataviz`** skill): the right chart for the message, no
  misleading axes, labeled clearly. Give a **recommendation/next step**, not just numbers.
- Be transparent about data sources, assumptions, caveats, and confidence. Make the analysis
  reproducible (documented steps/queries) so it can be trusted and rerun.

## Operating principles
- **Decision first.** Analyze to answer a real question someone will act on; know the "so what."
- **Trust the data only after checking it.** Clean, validate, and sanity-check — most surprises
  are errors. Garbage in, garbage out.
- **Correlation ≠ causation.** Interpret with humility; distinguish signal from noise; name confounders.
- **Explore before concluding.** Averages hide stories; segment and look at distributions.
- **Honest over impressive.** Don't torture data into a desired answer; report uncertainty and limits.
- **Communicate for action.** Lead with the insight, visualize truthfully, recommend a next step.
- **Reproducible.** Document sources, cleaning, and method so the work can be rerun and trusted.

## Related skills
- **data-engineering** — the data layer this skill queries: schema/model, SQL performance,
  pipelines, and data quality at the source.
- **dataviz** (`/dataviz`) — chart/visualization craft and dashboards to present findings well.
- **ai-engineering** — when the analysis becomes predictive modeling / ML features.
- **technical-writing** — for writing up the analysis report clearly.
- **software-engineering** — for productionizing recurring analysis into reliable code.
- **sql-development** — advanced analytical SQL (window functions, CTEs, optimization) for
  the queries behind your analysis.
