# Workflow in detail

Each phase with the concrete questions to answer before moving on.

## Understand
- What outcome does the user actually want? What's the underlying need behind
  the literal request?
- What are the constraints: performance targets, platforms/versions, security,
  compatibility, deadline, team conventions?
- What is explicitly out of scope?
- What would "done" look like, observably?
- One sharp clarifying question is worth more than a wrong guess — but only ask
  when the answer changes the design.

## Explore
- Where does this functionality live today? Trace the real code path.
- How are similar problems already solved here? (Find the pattern to follow.)
- What libraries/utilities already exist so you don't reinvent them?
- What do the existing tests and types say the intended behavior is?
- For a bug: what are the exact steps to reproduce, and what's the smallest
  reliable repro?
- What could this change break — callers, data shape, public API, migrations?

## Design
- What is the smallest change that fully solves it and fits the architecture?
- What are the interfaces and the data flow? Decide these before internals.
- Which edge cases and failure modes must be handled?
- Is this backward compatible? If not, what's the migration?
- If two approaches are viable, what's the trade-off, and which do you recommend?
- Can the change be split into independently reviewable/mergeable pieces?

## Implement
- Keep each step working and coherent; commit logical units.
- Match surrounding conventions; keep refactors separate from behavior changes.
- Handle errors and validate boundaries; no silent failures.
- No unrequested dependencies, files, or abstraction; no secrets in code.

## Verify
- Run it. Run the tests. Add tests for the changed behavior and its edges.
- Reproduce-then-confirm for bugs; check for regressions in neighbors.
- Report real results, including failures and anything skipped.

## Review
- Re-read the diff as a critical reviewer (see SKILL.md checklist).
- Write a commit message that explains *why*, not just *what*.
- State clearly what's verified vs. assumed, and any follow-ups worth doing.

## Communication
- Lead with the answer/outcome, then the detail.
- Reference code as `path:line` so it's navigable.
- Surface risks and unknowns early, not after they bite.
- When you recommend a follow-up, make it specific and actionable.
