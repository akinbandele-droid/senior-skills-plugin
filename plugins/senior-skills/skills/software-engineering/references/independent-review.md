# Independent Review

An independent review is an adversarial, fresh-eyes pass over a change by someone
(or a reasoning pass) that did **not** write it and is not invested in it. Its
job is to find what the author's self-review will miss — because the author sees
what they *intended*, not what they *wrote*.

Use it for high-stakes changes: security-sensitive code, data migrations,
anything hard to reverse, cross-team interfaces, or when the author is uncertain.

## Why it's separate from self-review
- The author shares the blind spots that caused any bug — they'll re-read the
  code with the same wrong assumption.
- Independence means no sunk-cost bias toward the chosen approach, and no
  "I know what this means" shortcuts.
- Treat the change as a claim to be falsified, not confirmed.

## Stance: try to break it
- Assume there is a bug and hunt for it. Ask "how does this fail?" before "does this work?"
- Challenge the premise: is this even the right problem/approach, or did the
  author solve the wrong thing well?
- Re-derive correctness from the code itself; don't trust the description, the
  comments, or the variable names to be true.

## Falsification checklist
- **Inputs:** empty, null, max, negative, malformed, unicode, huge, concurrent.
- **State & ordering:** races, retries, partial failure, re-entrancy, stale reads.
- **Boundaries:** off-by-one, inclusive/exclusive ranges, timezone/encoding, overflow.
- **Error paths:** what happens when each dependency times out, errors, or returns garbage?
- **Security:** can a hostile caller abuse this? Authz on every path? Injection? Secrets?
- **Assumptions:** list every assumption the code makes and find the input that violates it.
- **Blast radius:** what else calls this / shares this data / depends on this contract?
- **Tests:** do they actually fail if the behavior breaks, or do they just pass?

## Method
1. Understand the intended behavior from requirements, independently of the author's framing.
2. Trace the critical paths yourself; run it against adversarial inputs where possible.
3. Verify each claim ("handles X", "is backward compatible") against the actual code.
4. Separate findings: **blocking** (correctness/security/data loss) vs. **non-blocking**.
5. For each finding, give the concrete failing scenario — inputs → wrong result — not a vague worry.

## Output
- Lead with the verdict: ship / fix-then-ship / rethink.
- List blocking findings first, each with a reproduction or precise failure scenario and a suggested fix.
- Note what you verified and what you could not (so the gap is visible).
- Be specific and falsifiable; "might have an issue" isn't actionable — show the case.

## Using subagents for independence (when available)
Spawning a separate reviewer agent that did not see the implementation reasoning
gives genuine independence: brief it with the requirements and the diff, and ask
it to find the failing case. Keep the author context and the reviewer context apart.
