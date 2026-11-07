# Test Case Design Techniques

Systematic techniques to design cases that find real bugs instead of only confirming the
happy path. Derive cases from requirements **and** from how software fails.

## Black-box techniques (from the spec/behavior)
- **Equivalence partitioning:** group inputs that should behave the same; test one per class
  (valid and invalid classes) rather than every value.
- **Boundary value analysis:** bugs cluster at edges. Test the boundary and just inside/outside
  it (min-1, min, min+1, max-1, max, max+1), zero, empty, and the first/last element. This is
  one of the highest-yield techniques.
- **Decision tables:** for combinations of conditions and rules — enumerate the combinations and
  their expected outcomes so none is missed.
- **State transition:** for stateful systems — test valid transitions, invalid ones, and
  entry/exit; try to reach states "you shouldn't."
- **Pairwise/combinatorial:** when many parameters combine, test all *pairs* to cover
  interactions without a full combinatorial explosion.
- **Error guessing:** use experience to target likely failure spots (nulls, zero, negatives,
  duplicates, special characters, timezones, unicode).

## Think about the ways things break
- **Inputs:** empty, null/undefined, zero, negative, max/overflow, very long, wrong type,
  special/unicode/emoji, leading/trailing spaces, injection-ish strings, malformed formats.
- **State & sequence:** out-of-order actions, double-submit, back button, resume after
  interruption, stale data, concurrent edits, race conditions.
- **Environment:** slow/lost network, timeout, partial failure, retries, low resources,
  different locale/timezone/DST, clock skew.
- **Data:** duplicates, missing relations, huge volumes, precision/rounding (money!),
  encoding, pagination boundaries.
- **The unhappy paths:** every error the code claims to handle — force it and verify the
  handling, don't assume.

## Anatomy of a good case
- **Precondition/setup**, **action/steps**, and a **specific expected result**. The expected
  result is the point — a case that just "runs the feature" with no assertion tests nothing.
- Independent and repeatable (no hidden dependence on another test's leftover state).
- Named for what it verifies, so a failure is self-explanatory.

## From requirement to cases
- For each requirement/acceptance criterion, write at least: one positive case, the relevant
  negative/error cases, and the boundary cases. Trace cases back to requirements so you can see
  coverage gaps.
- Challenge the requirement itself while designing — ambiguities and contradictions found here
  are the cheapest bugs you'll ever fix.

## Common pitfalls
- Only positive/happy-path cases; no negative or boundary coverage.
- Cases with vague or missing expected results.
- Interdependent tests that fail in cascades and can't run alone.
- Exhaustive combinatorial testing where pairwise would do (wasted effort).
- Testing the implementation's current behavior instead of the intended behavior.
