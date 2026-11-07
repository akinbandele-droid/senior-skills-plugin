# Statistics for Analysts

Enough statistical rigor to interpret data honestly and avoid confidently-wrong conclusions.
The goal isn't fancy methods — it's not fooling yourself (or your audience). The two deadliest
errors: confusing correlation with causation, and mistaking noise for signal.

## Descriptive statistics
- **Center:** mean (sensitive to outliers/skew), median (robust), mode. Choose the one that honestly
  represents the distribution — report median for skewed data (income, durations).
- **Spread:** range, variance, standard deviation, IQR, percentiles. A center without spread is
  half the story. Percentiles (p50/p90/p95) often beat averages for skewed metrics (latency, spend).
- **Shape:** skew and modality (see `exploratory-analysis.md`). Summarize honestly, not flatteringly.

## Correlation vs. causation (the cardinal rule)
- **Correlation does not imply causation.** A relationship between X and Y can be: X causes Y,
  Y causes X, a **confounder** Z drives both, selection effects, or coincidence (spurious — enough
  variables guarantee some correlate).
- To claim causation you need a controlled **experiment** (A/B test / RCT) or careful causal
  inference with strong assumptions — not an observational correlation. State relationships as
  associations unless you have causal evidence. This is the single most common analytical overreach.

## Inferential statistics (sample → population)
- **Sampling:** a sample estimates a population only if it's **representative**. Watch selection
  bias (who's in the data), **survivorship bias** (only survivors are visible — churned/failed cases
  missing), and non-response bias. A biased sample can't be fixed by more data.
- **Uncertainty:** an estimate from a sample has error — report **confidence intervals**, not just a
  point estimate. Small samples give wide, unreliable intervals; be especially humble there.
- **Significance tests & p-values:** a p-value is the probability of seeing data this extreme if the
  null (no effect) were true — **not** the probability the hypothesis is true, nor the effect's
  importance. p < 0.05 is a convention, not truth.
  - **Statistical vs. practical significance:** a tiny, useless difference can be "significant" with
    a big enough sample; a meaningful difference can be non-significant with a small one. Always
    report **effect size**, not just significance.
  - **Multiple comparisons:** test 20 things at p<0.05 and ~1 will "pass" by chance. Correct for it
    or treat such findings as exploratory.

## Choosing an approach
- **Comparing groups:** t-test/ANOVA (means), chi-square (categorical), non-parametric tests for
  skewed data. **Relationships:** correlation/regression (and check the scatter + residuals).
- **Trends over time:** watch seasonality and autocorrelation; don't over-read short windows.
- **A/B tests:** define the metric and sample size **before** running; don't peek-and-stop (inflates
  false positives); check the test was randomized and balanced.
- Use the simplest method that answers the question; a clear comparison often beats a complex model.

## Interpreting honestly
- Report uncertainty and assumptions; say what the data **can't** tell you.
- Beware base rates (a rare event with a "good" test still yields many false positives), averaging
  rates incorrectly, and extrapolating beyond the data's range.
- Prefer "the data is consistent with…" over "the data proves…" unless it truly does.

## Common pitfalls
- Claiming causation from correlation; ignoring confounders.
- Treating p<0.05 as proof / importance; ignoring effect size and CIs.
- Survivorship/selection bias; tiny or unrepresentative samples stated with false confidence.
- Multiple-comparisons / p-hacking; peeking at A/B tests and stopping early.
- Mean on skewed data; extrapolating trends past the observed range.
