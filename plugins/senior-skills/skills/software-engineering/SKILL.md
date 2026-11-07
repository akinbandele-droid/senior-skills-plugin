---
name: software-engineering
description: >-
  Engineer software changes with senior-level discipline: understand the real
  problem, explore before editing, design for the existing codebase, implement
  in small verifiable steps, test, and review your own work. Use for any
  non-trivial coding task — new features, refactors, bug fixes, debugging,
  architecture decisions, code review, or cleaning up legacy code. Covers the
  full loop from requirements to a reviewed, tested change.
---

# Software Engineering

A disciplined loop for shipping correct, maintainable changes. The throughline:
understand before you build, match the codebase you're in, change the smallest
surface that solves the problem, and prove it works. Match rigor to the task —
a typo fix is not a redesign — but never skip understanding and verification.

## The loop

1. **Understand** the problem and the constraints.
2. **Explore** the codebase and reproduce the current behavior.
3. **Design** the smallest change that fits the existing architecture.
4. **Implement** in small, coherent steps.
5. **Verify** with tests and by running the code.
6. **Review** your own diff before calling it done.

Core references:
- `references/workflow.md` — each phase in detail, with the questions to answer.
- `references/code-quality.md` — what good code looks like here; naming, structure, errors.
- `references/debugging.md` — a systematic method for bugs you can't immediately see.
- `references/testing.md` — what to test, at what level, and how to make tests trustworthy.

Advanced references (load for the matching task):
- `references/code-review.md` — reviewing a change/PR, and self-review before requesting one.
- `references/refactoring.md` — safely restructuring legacy code without changing behavior.
- `references/api-system-design.md` — designing interfaces and systems; trade-offs and failure modes.
- `references/performance.md` — profiling and optimizing with evidence, not guesses.
- `references/independent-review.md` — adversarial fresh-eyes review for high-stakes changes.

## 1. Understand
- Restate the goal in your own words. Separate the stated request from the
  underlying need; solve the need.
- Surface constraints: performance, compatibility, deadlines, security, the
  users affected. Note explicit non-goals.
- If the request is ambiguous in a way that changes the design, ask one sharp
  question rather than guessing. If it's ambiguous in a way that doesn't, pick
  the sensible default and say so.

## 2. Explore
- Read before you write. Find how similar things are already done in this
  codebase and follow those patterns — libraries, structure, naming, error
  handling, test style. Consistency beats personal preference.
- Trace the actual code path involved, not the one you assume exists.
- For a bug, **reproduce it first**. A bug you can't reproduce is a bug you
  can't confirm you fixed.
- Check tests, types, and docs already present — they encode intent.

## 3. Design
- Choose the smallest change that fully solves the problem and fits the existing
  architecture. Prefer editing/extending over rewriting.
- Think about interfaces and data flow before internals. Make the change easy to
  review: one logical concern per change where possible.
- Consider edge cases, failure modes, and backward compatibility up front.
- Call out trade-offs. When two approaches are viable, recommend one with a
  one-line reason rather than listing both and stopping.

## 4. Implement
- Work in small, coherent steps that each leave the code in a working state.
- Match the surrounding style exactly; don't reformat unrelated code or mix a
  refactor into a fix without saying so.
- Handle errors deliberately — don't swallow them. Validate inputs at
  boundaries. Avoid dead code, speculative abstraction, and commented-out blocks.
- Don't add dependencies, config, or files that weren't asked for or aren't
  needed. Never hardcode secrets.
- See `references/code-quality.md`.

## 5. Verify
- Run the code and the relevant tests. Add or update tests for the behavior you
  changed, including the edge/failure cases.
- Report results honestly: if tests fail or a step was skipped, say so with the
  output. Don't claim something works that you didn't run.
- For bugs, confirm the original reproduction no longer triggers the issue and
  that you didn't break neighbors. See `references/testing.md` and `debugging.md`.

## 6. Review your own work
Before declaring done, read your diff as a reviewer would:
- Does it actually solve the stated problem, including edge cases?
- Is anything left half-done, stubbed, or inconsistent?
- Did I leave debug output, TODOs, or unrelated churn?
- Are names clear, errors handled, and the change the right size?
- Would the next engineer understand *why* from the code and commit message?

For **high-stakes changes** — security-sensitive code, data migrations,
hard-to-reverse or cross-team work — a self-review isn't enough: the author
shares the blind spot that caused the bug. Add an **independent review** (a
fresh-eyes, adversarial pass that tries to break the change, ideally by a
separate reviewer/subagent that didn't write it). See
`references/independent-review.md`.

## Operating principles
- **Correctness first**, then clarity, then performance — optimize only with a
  reason or a measurement.
- **Smallest effective change.** Scope creep is a bug.
- **Match the codebase.** Its conventions win over your defaults.
- **Prove it.** "It should work" isn't done; "I ran it and here's the output" is.
- **Be honest about state.** Name what's unverified, skipped, or risky.
- **Leave it better.** Small, in-scope cleanups are welcome; drive-by rewrites are not.

## Related skills
- **security-review** — for a security-focused review of a change, or when the
  code touches auth, crypto, data handling, or anything internet-facing.
- **ui-ux-design** / **web-design** — when the change has a user-facing interface;
  pair the engineering loop with the design and web-craft skills.
- **pentest** — to validate security of the running system after building it.
- **qa-testing** — dedicated test strategy, exploratory, and non-functional testing beyond
  the everyday dev testing in this skill's `testing` reference.
- **devops-infra** — to ship and operate what you build (CI/CD, IaC, deploy, observability).
- **data-engineering** — when the change centers on the database, schema, or data pipelines.
- **ai-engineering** — when building an LLM/AI-powered feature.
- **technical-writing** — to document the change (README, API docs, design docs, ADRs).
- **database-design** / **sql-development** — when the change involves schema design or
  non-trivial SQL and database logic.
