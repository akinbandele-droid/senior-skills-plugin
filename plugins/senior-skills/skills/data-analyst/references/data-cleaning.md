# Data Cleaning & Validation

Analysts spend most of their time here, and for good reason: **garbage in, garbage out.**
You must earn trust in the data before trusting the analysis. Profile it, fix issues
deliberately, document every decision, and sanity-check against reality.

## Profile first
- Look at the raw data: row count, columns, types, a sample of values, and summary stats per field
  (min/max/mean/distinct/nulls). Understand the **grain** (what one row represents) and the
  **meaning/units** of each field before touching it.
- Check coverage: does the date range, population, and completeness match what you expect? A silent
  partial extract is a classic trap.

## Common quality issues & fixes
- **Duplicates:** exact and near-duplicates (and accidental fan-out from a join). De-dupe on the
  right key; confirm whether duplication is real or an artifact.
- **Inconsistent formats/categories:** "NY"/"New York"/"new york", trailing spaces, mixed date
  formats, units mismatches (cm vs. in), encoding issues. Standardize to a canonical form.
- **Wrong types:** numbers stored as text, dates as strings — cast correctly or math/sorting breaks.
- **Impossible/invalid values:** negative ages, future birthdates, out-of-range codes, placeholder
  values (0, -1, 9999, "N/A") standing in for missing. Define valid ranges and flag violations.
- **Structural issues:** merged cells, multi-row headers, totals rows mixed into data (common in
  exported spreadsheets). Reshape to tidy data (one variable per column, one observation per row).

## Missing data (understand *why* before acting)
- Find missingness and its pattern. **Is it random or systematic?** Systematic missingness (e.g. a
  field only filled for one segment) biases results if you ignore it.
- Options, chosen deliberately: leave as missing (and handle in analysis), drop rows/columns (only
  if little is lost and it's random), or **impute** (mean/median/mode, forward-fill, model-based) —
  noting that imputation invents data and can distort variance/relationships. Document the choice
  and consider its effect on the result. Never silently drop a large chunk.

## Outliers (investigate, don't reflexively delete)
- Detect (distribution, IQR, z-scores) — but an outlier may be a **data error**, a **genuine
  extreme**, or **the most important finding** (fraud, a whale customer, a bug). Investigate the
  cause. Removing real extremes to make a chart prettier is distortion. If you exclude any, say so
  and show the effect.

## Validate against reality
- Reconcile totals against a known/independent figure (e.g. does revenue match finance's number?).
- Check magnitudes are plausible and trends are continuous (a sudden cliff/spike often means a
  pipeline change or missing data, not a real event).
- Cross-check a few records end-to-end against the source system.

## Document everything
- Every cleaning/transform decision changes the result. Keep a record (ideally as code) of what you
  filtered, fixed, dropped, imputed, and excluded — for reproducibility and so reviewers can judge
  whether a decision biased the outcome.

## Common pitfalls
- Trusting the data without profiling it; not understanding what fields mean.
- Placeholder values (0/-1/9999) treated as real numbers → skewed stats.
- Dropping missing/outlier rows blindly → bias and lost signal.
- Ignoring join fan-out (duplicate rows inflating counts/sums).
- Undocumented manual cleaning → unreproducible, unauditable results.
- Not reconciling to a known total → shipping a data-bug "finding."
