# Debugging

A systematic method beats guess-and-check. The goal is to find the true root
cause, not to make the symptom disappear.

## Method
1. **Reproduce reliably.** Find the smallest, deterministic steps that trigger
   the bug. If you can't reproduce it, you can't confirm a fix.
2. **Observe exactly.** Read the full error, stack trace, and logs. Note what
   actually happens vs. what should happen. Don't skim.
3. **Localize.** Narrow where the fault is: bisect (git bisect, binary search in
   code/data), add targeted logging, or use a debugger/breakpoints. Confirm
   assumptions with evidence, not belief.
4. **Form one hypothesis** about the root cause and a prediction it makes.
5. **Test the hypothesis** — change or inspect one thing, check the prediction.
   If wrong, discard it and form another. Change one variable at a time.
6. **Fix the root cause,** not the symptom. Ask *why* the bad state arose.
7. **Verify** the repro no longer triggers it, and check neighbors for regressions.
8. **Add a test** that would have caught it, so it can't silently return.

## Tactics
- Re-read the error message literally — the answer is often right there.
- Check the boring causes first: wrong env/config, stale build/cache, wrong
  branch, typo, off-by-one, null/empty, timezone/encoding, async ordering.
- Make the invisible visible: log inputs/outputs at boundaries, print types and
  sizes, inspect actual values rather than assumed ones.
- Diff against a working state: what changed (recent commits, deps, data, env)?
- Isolate: reproduce in a minimal script to remove surrounding noise.
- Rubber-duck the code path out loud, line by line, against reality.

## When stuck
- State your assumptions explicitly and verify each — the bug is usually in one
  you were sure of.
- Widen the net: is the fault actually upstream (data, config, a dependency)?
- Take the repro back to first principles and re-observe without yesterday's theory.

## Anti-patterns
- Changing many things at once so you can't tell what worked.
- "Fixing" by adding a catch that hides the error.
- Declaring it fixed without reproducing the original failure first.
- Trusting logs/comments over the actual runtime behavior.
