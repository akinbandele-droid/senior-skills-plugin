# Fault-Based Testing: Mutation Testing & Fuzzing

Two ways to attack your system instead of confirming it: mutation testing attacks your
**tests** (do they catch bugs?), fuzzing attacks your **code** (does it survive hostile
input?). Both find what example-based tests miss.

## Mutation testing (measure real test strength)
- **Idea:** automatically introduce small faults ("mutants") — flip `>` to `>=`, `+` to `−`, `&&` to
  `||`, remove a statement, negate a condition, change a constant — then run the suite. If a test
  **fails**, the mutant is "killed" (good). If all tests still pass, the mutant **survived** → your
  tests don't actually verify that behavior.
- **Mutation score** = killed / total. It measures what coverage cannot: whether tests would *detect a
  bug*, not merely execute the line. 100% line coverage with a low mutation score means weak or
  assertion-free tests.
- **Use it to:** find weak tests, harden critical modules, and target new tests (surviving mutants point
  exactly at untested behavior).
- **Practicalities:** compute-heavy (runs the suite many times) — scope to changed/critical code, run
  **incrementally** in CI, and triage **equivalent mutants** (don't change behavior, can't be killed).
  Tools: Stryker (JS/.NET), PITest (JVM), mutmut/cosmic-ray (Python).

## Fuzzing (find crashes and unhandled inputs)
- **Idea:** feed **malformed, random, or mutated** inputs to find crashes, hangs, memory errors,
  unhandled exceptions, and security bugs — the inputs nobody writes a test for.
- **Types:**
  - **Coverage-guided** (libFuzzer, AFL++, Go native fuzzing, Jazzer): instruments code and evolves
    inputs to reach new branches — very effective at deep states.
  - **Structure-aware / grammar-based:** generate inputs matching a schema/grammar to get past shallow
    parsing into real logic; combine with property assertions (a crash *or* a broken invariant fails).
  - **Mutation- vs. generation-based:** mutate a seed corpus, or generate from a model.
- **Where it pays off:** trust boundaries and anything parsing untrusted input — file/format parsers,
  decoders, deserializers, protocol handlers, APIs, memory-unsafe code.
- **Make failures meaningful:** run with **sanitizers** (ASan/UBSan/MSan) and assertions so latent
  corruption surfaces as a clear crash. Seed with a good corpus, **minimize** crashing inputs to a small
  reproducer, and add each to the regression corpus. For critical surfaces run **continuous fuzzing**.
- Pair with `security-review`/`pentest` for security-oriented fuzzing (authorized).

## Negative & robustness testing (the manual cousin)
- Deliberately exercise error paths and hostile inputs by hand too: invalid types, nulls, overflow,
  truncated data, wrong encodings, out-of-order calls — and assert the code fails *safely* (clear error,
  no corruption). Keep discovered cases as explicit regression tests.

## Common pitfalls
- Chasing mutation score everywhere (expensive) instead of on critical/changed code; not triaging
  equivalent mutants.
- Fuzzing without sanitizers/assertions → silent corruption that looks like "no crash."
- Shallow fuzzing that never clears input validation (use coverage-guided/structure-aware).
- Not saving fuzz/mutation findings as regressions; treating a clean fuzz run as proof of correctness.
