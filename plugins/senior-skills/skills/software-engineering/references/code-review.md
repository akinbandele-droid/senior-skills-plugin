# Code Review

Reviewing someone else's change (or a PR). Goal: catch real problems and help
the change land in good shape — not to impose personal style. Be rigorous on
correctness, generous on taste.

## What to check, in priority order
1. **Correctness** — does it do what it claims? Walk the logic and the edge
   cases: empty/null, boundaries, concurrency, error paths, large inputs. Look
   for off-by-one, inverted conditions, and unhandled failures.
2. **Security** — injection sinks, authz/authn gaps, secrets in code, unsafe
   deserialization, missing input validation at trust boundaries, PII in logs.
3. **Design fit** — does it match the codebase's architecture and patterns, or
   introduce a divergent one without reason? Is the abstraction level right?
4. **Tests** — do they exist, cover the risky paths and the fixed bug, and
   actually fail if the behavior breaks?
5. **Readability & maintainability** — clear names, reasonable function size,
   comments that explain *why*, no dead/commented-out code.
6. **Scope** — is the change focused, or does it bundle unrelated churn/refactors?
7. **Performance** — only where it plausibly matters (hot paths, N+1, unbounded growth).

## How to give feedback
- Distinguish **blocking** (must fix: bugs, security, broken tests) from
  **non-blocking** (nits, suggestions) — label them so.
- Explain *why*, and suggest a concrete alternative. "This can NPE when `x` is
  empty — guard it" beats "this is wrong."
- Ask questions when you might be missing context rather than asserting.
- Praise genuinely good solutions; review is also knowledge-sharing.
- Keep it about the code, never the author.

## Reviewer discipline
- Actually run or trace the critical path; don't rubber-stamp.
- Verify claims in the description against the diff.
- Check what's *missing* (an unhandled case, a test, a migration), not only what's present.
- Prefer a few well-reasoned high-value comments over a flood of nits.
- Confirm the change is the right *size* and solves the stated problem — nothing more, nothing less.

## Self-review (before requesting review)
Read your own diff top to bottom as if it were someone else's:
- Does it solve the problem and the edge cases? Anything half-done or stubbed?
- Debug output, TODOs, unrelated reformatting left in?
- Would the commit message tell the next person *why*?
A clean self-review respects the reviewer's time and catches most issues cheaply.
