# Code Quality

What good code looks like. The first rule overrides the rest: **match the
codebase you're in.** These are defaults for when the codebase is silent.

## Readability
- Names say what things are/do; the reader shouldn't have to guess. Avoid
  abbreviations that aren't already used in the project.
- Prefer clear and obvious over clever and compact. Code is read far more than written.
- Keep functions focused on one job; extract when a block needs a comment to
  explain *what* (vs. *why*).
- Comments explain *why* and intent, not *what* the code plainly says. Delete
  commented-out code.

## Structure
- Follow the project's existing module boundaries and layering. Don't introduce a
  new pattern when an established one fits.
- Keep related things together; minimize coupling across boundaries.
- Don't abstract until there's real duplication or a real second case —
  premature abstraction costs more than a little repetition.
- Keep public interfaces small and stable; hide internals.

## Correctness & robustness
- Handle errors deliberately. Don't catch-and-ignore; don't let failures pass
  silently. Fail loudly where a human needs to know; degrade gracefully where appropriate.
- Validate inputs at trust boundaries (user input, network, files, external APIs).
- Think about nulls/empties, boundaries, concurrency, and large inputs.
- Make illegal states hard to represent (types, enums, invariants).

## Change hygiene
- One logical concern per change. Don't bundle a refactor with a fix without saying so.
- Don't reformat or churn unrelated lines — it hides the real diff.
- No leftover debug prints, scratch files, or TODOs you intend to do now.
- Keep dependencies minimal and justified; prefer the standard library / existing deps.

## Security & safety basics
- Never hardcode credentials/secrets/tokens. Read from config/secret stores.
- Parameterize queries; escape/encode output; avoid injection sinks.
- Apply least privilege; don't log secrets or PII.
- Be careful with destructive operations — confirm targets before deleting/overwriting.

## Performance
- Correct first. Optimize only with a measurement or a clear algorithmic reason.
- Watch for accidental O(n²), N+1 queries, and unbounded memory growth.
- Don't trade a lot of readability for a little speed without evidence.
