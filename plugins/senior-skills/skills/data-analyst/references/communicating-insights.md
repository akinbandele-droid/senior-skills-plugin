# Communicating Insights

An analysis that isn't understood and acted on was wasted. Your job isn't to show how hard
the work was — it's to get the decision-maker to the right conclusion fast, honestly, and
with enough confidence to act. Lead with the answer; make the data tell a story.

## Lead with the "so what"
- **Start with the answer and its implication**, not your methodology. "Churn rose 3pts last
  quarter, driven by the SMB segment — we should prioritize SMB onboarding" beats walking through
  every query first. (BLUF — bottom line up front.)
- Decision-makers want: what's the finding, why does it matter, and what should we do. Supporting
  detail and method come *after*, available for those who want it.

## Know your audience
- **Executives:** headline insight, business impact, and a clear recommendation; minimal jargon;
  one or two decisive visuals. They want the decision, not the derivation.
- **Analysts/technical peers:** method, assumptions, data, and caveats — they'll (rightly) scrutinize.
- **Operators:** the specific, actionable next step for their area.
- Match depth, language, and format to who's reading and what they'll do with it. Define any metric
  you use.

## Tell a story
- Structure: context → the key finding(s) → why it's happening → recommendation → next steps.
- Build a narrative arc, not a data dump. Each chart/number should advance the argument; cut
  anything that doesn't. One main message per slide/section.
- Make it concrete: tie numbers to business meaning ("= \$1.2M/yr", "= 400 customers").

## Visualize honestly (pair with the `dataviz` skill)
- **Right chart for the message:** trends → line; comparisons → bar; parts-of-whole → sparingly;
  relationships → scatter; distributions → histogram/box. Don't use pie charts for many categories
  or dual axes that mislead.
- **No deception:** start bar-chart axes at zero; don't truncate/invert axes to exaggerate; keep
  scales consistent; label axes, units, and sources. Show the comparison that matters.
- Keep it clean: clear title stating the takeaway, direct labels, minimal clutter, accessible color
  (don't rely on color alone). The chart should make the point at a glance.

## Be honest about limits
- State data sources, time frame, assumptions, cleaning decisions, and **caveats/confidence**.
  Distinguish correlation from causation in your wording. Note what the analysis can't conclude.
- Don't overclaim or torture the data to a desired narrative. Credibility is your main asset — one
  "finding" that turns out to be a data bug costs all future trust. If a result is tentative, say so.

## Make it actionable & reproducible
- End with a **recommendation or clear next step**, and ideally the expected impact — not just
  numbers. Flag decisions the data supports vs. judgment calls.
- Provide a path to the underlying analysis (documented queries/notebook) so it can be verified and
  rerun. Anticipate the obvious follow-up questions and answer them preemptively.

## Common pitfalls
- Burying the lede under methodology; presenting a data dump with no clear message.
- Same depth/jargon for every audience; undefined metrics.
- Misleading charts (truncated axes, wrong chart type, 3D/pie overuse).
- Overclaiming (causation, certainty) beyond what the data supports.
- Numbers with no recommendation or next step; no link back to a reproducible analysis.
