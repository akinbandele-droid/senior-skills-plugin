# Exploratory Data Analysis (EDA)

Look before you leap. EDA is how you understand the data's shape and relationships, find
the real signal (and the data artifacts), and form hypotheses — *before* committing to a
conclusion. Skipping it is how analysts get fooled by averages and outliers.

## Understand each variable
- **Distributions:** plot them. Shape (normal, skewed, bimodal, uniform), spread, and range. The
  shape dictates which summary stat and which test are appropriate — a mean is misleading on a
  skewed or bimodal distribution.
- **Central tendency & spread:** mean vs. median (median resists outliers/skew), standard deviation/
  IQR. Report the one that honestly represents the data.
- **Categoricals:** frequencies and proportions; watch for rare categories and a dominant class.

## Segment — the average lies
- A single aggregate often hides the real story. **Break down by meaningful segments** (time,
  cohort, region, channel, user type) — the overall trend may reverse or fragment within groups.
- **Simpson's paradox:** a trend in the whole can disappear or flip when you split by a lurking
  variable. Always ask "is this true within each segment, or an artifact of mix?"
- Compare groups fairly (same definitions, same time windows, adequate sample per segment).

## Relationships
- **Between numerics:** scatter plots reveal shape (linear? curved? none?), clusters, and outliers
  that a correlation coefficient alone hides. A single number can mask a non-linear or clustered
  relationship — always look.
- **Over time:** trends, seasonality, cycles, level shifts (often a tracking/pipeline change, not a
  real event — verify). Use appropriate time grain; beware incomplete current periods.
- **Across categories:** compare distributions, not just means, between groups.

## Hunt for anomalies & artifacts
- Spikes, gaps, flat lines, and cliffs are often **data problems** (a logging change, a missing
  day, a backfill, a unit change) rather than real-world events. Investigate before interpreting.
- Note anything that doesn't make sense — it's either a finding or a bug, and you need to know which.

## Form and test hypotheses honestly
- Let EDA generate hypotheses, then **test them** (see `statistics.md`) — don't stop at the first
  chart that confirms what you hoped. **Beware confirmation bias and cherry-picking:** if you slice
  enough ways, something will look significant by chance (multiple-comparisons problem).
- Distinguish exploratory findings (hypothesis-generating, tentative) from confirmatory results
  (tested, reliable). Be clear which you're presenting.

## Practical tips
- Start broad (overview, totals, time series), then drill into what's interesting.
- Visualize liberally during EDA (quick, rough) — the eye catches what tables hide; polish later
  for communication (pair with `dataviz`).
- Keep notes of what you checked and found — EDA is where the real understanding forms.

## Common pitfalls
- Reporting an average that hides a bimodal/skewed reality or a Simpson's-paradox reversal.
- Trusting a correlation coefficient without the scatter plot.
- Reading a data artifact (missing day, backfill, tracking change) as a real trend.
- Cherry-picking the one slice that fits the story; ignoring the multiple-comparisons trap.
- Treating exploratory hunches as confirmed findings.
