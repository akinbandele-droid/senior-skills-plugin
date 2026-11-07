# Open Review

An *open* review earns trust by being inspectable, reproducible, and open to
challenge — the opposite of "trust me, I looked." Anyone should be able to follow
what you did, redo it, and disagree with evidence. This applies whether the
subject is proprietary code reviewed by a peer or open-source code reviewed in public.

## Principles
- **Transparent scope.** State exactly what was reviewed, at what version/commit,
  and what was not. A finding's absence means nothing without the scope.
- **Explicit assumptions & attacker model.** Write down what you treated as
  trusted and who the attacker is. Readers can then judge and challenge them.
- **Reproducible findings.** Every finding carries enough detail (location,
  inputs, steps, commit) for someone else to reproduce it independently.
- **State the method.** Say how you reviewed — manual trace, tools and versions,
  tests run — so the coverage is legible.
- **Declare limitations.** Call out what you couldn't check: areas skipped, time
  box, components you lacked access to, things you couldn't confirm. Hiding gaps
  is the main way reviews mislead.
- **Invite challenge.** Findings are claims, not verdicts. Make it easy to refute
  them; correct the record when refuted.
- **No appeal to authority.** The evidence stands on its own, not on who wrote it.

## Running it
1. **Publish the plan** before (or with) the review: scope, version, assumptions,
   attacker model, method.
2. **Review** per the security-review loop; keep a log of what you examined.
3. **Record each finding** reproducibly (see `findings-and-disclosure.md`), and
   each **non-finding** area you cleared (so coverage is visible).
4. **Separate fact from opinion:** confirmed exploit vs. suspected weakness vs.
   hardening suggestion. Label each.
5. **Open it for response:** let the author/maintainer and other reviewers
   challenge findings; track disputes and resolutions.
6. **Version the review** — note the commit it applies to; a later change can
   invalidate it. Say so.

## Coverage statement (include in every open review)
- What was in scope (and the exact version/commit).
- What was out of scope or not reached, and why.
- Method and tools (with versions).
- Confidence level per area (reviewed thoroughly / spot-checked / not reviewed).
- Residual risk and recommended follow-up.

## For open-source / public review
- Prefer private, coordinated disclosure of exploitable issues first; open the
  full detail after a fix or an agreed timeline (see `findings-and-disclosure.md`).
- When reviewing in public, be precise and non-sensational; avoid dropping a
  working exploit before maintainers can respond.
- Give credit, cite evidence, and keep critique about the code, not the people.

## Why this matters
A review nobody can reproduce or scrutinize is just an opinion. Openness —
stated scope, assumptions, method, and limits — is what lets others rely on the
result and build on it.
