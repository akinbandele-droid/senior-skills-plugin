# Generative Test Design Techniques

Example-based tests only check the inputs you thought of. These techniques generate or
systematically derive cases, finding bugs in the space you'd never enumerate by hand.

## Property-based testing (highest value for logic)
- Instead of `assert f(2) == 4`, state a **property true for all valid inputs** and let the framework
  (Hypothesis, fast-check, QuickCheck, jqwik, PropEr, etc.) generate hundreds of cases.
- **Finding good properties:**
  - **Round-trip / inverse:** `decode(encode(x)) == x`, `parse(render(x)) == x`.
  - **Invariants:** output always sorted; length preserved; sum conserved; balance never negative.
  - **Idempotence:** `f(f(x)) == f(x)` (normalization, dedup, saves).
  - **Commutativity/associativity:** order doesn't change the result where it shouldn't.
  - **Oracle/equivalence:** matches a simpler (slower) reference, or the previous version.
  - **Metamorphic relations** (below) when no direct oracle exists.
- **Shrinking:** on failure the framework reduces the input to a *minimal* reproducer — a huge
  debugging aid. Save failing seeds as regression tests.
- Tune generators toward realistic/edge inputs (empty, huge, unicode, boundaries). Great for parsers,
  serializers, math, data structures, state machines, protocols.

## Metamorphic testing (when you have no oracle)
- For code where the "correct" output is hard to state (ML, search, rendering, complex calc), assert
  **relations between related executions**: `sin(x) == sin(π − x)`; adding an item raises count by one;
  reordering inputs doesn't change a set result; a discount never raises the total. Catches bugs via
  consistency even when absolute correctness is unknowable.

## Model-based / stateful testing
- Build a **simplified reference model** of intended behavior, generate random **sequences of
  operations**, and assert the real system stays consistent with the model after each step. Excellent
  for stateful components (caches, queues, allocators, UIs, databases) — finds order-dependent and
  accumulated-state bugs isolated tests miss.

## Combinatorial / pairwise testing
- When many parameters combine, every combination explodes. **Pairwise (all-pairs)** covers every
  *pair* of values, catching most interaction bugs with a fraction of the cases (tools: PICT, AllPairs).
  Use n-wise for higher-risk interactions.

## Snapshot / approval testing
- Capture a complex output (rendered HTML, serialized structure, report) as an approved baseline and
  diff future runs against it. Fast for large outputs — but **review diffs carefully**, keep snapshots
  small and meaningful, and don't snapshot volatile data (timestamps, random ids), or they become
  rubber-stamped noise that passes by being blindly re-approved.

## Data-driven / parameterized testing
- Run one test over a table of input→expected cases (including the boundary/equivalence cases from
  `qa-testing`'s test-design techniques). Keeps cases dense and readable; combine with generators.

## Common pitfalls
- Property tests that merely restate the implementation (not an independent property).
- Generators too loose (never hit edges) or too tight (miss the bug space); not saving shrunk failures.
- Snapshots blindly re-approved; snapshotting volatile output.
- Pairwise where safety-critical interactions need full combinatorial coverage.
