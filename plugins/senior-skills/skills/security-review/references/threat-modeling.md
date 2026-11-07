# Threat Modeling

Systematically answer four questions: **What are we building? What can go wrong?
What are we going to do about it? Did we do a good job?** The output is a
prioritized list of threats that focuses the rest of the review.

## 1. Model the system
- Draw the data-flow: external entities, processes, data stores, and the flows
  between them.
- Mark **trust boundaries** — every point where data or control crosses from
  less-trusted to more-trusted (network edge, process boundary, user→kernel,
  tenant→tenant, client→server). Bugs cluster at boundaries.
- Note what data and privileges live where, and which flows carry untrusted input.

## 2. Enumerate threats (STRIDE)
For each element/flow crossing a boundary, ask:
- **S**poofing — can an attacker pretend to be someone/something else? (auth)
- **T**ampering — can they modify data or code in transit or at rest? (integrity)
- **R**epudiation — can they deny an action with no trace? (logging/audit)
- **I**nformation disclosure — can they read what they shouldn't? (confidentiality)
- **D**enial of service — can they exhaust or crash it? (availability)
- **E**levation of privilege — can they gain rights they shouldn't have? (authz)

## 3. Attack trees for the crown jewels
- Identify what an attacker actually wants (data, funds, control, disruption).
- Work backwards: root goal → the ways to achieve it → the concrete steps/paths.
- This surfaces chains that single-point checklists miss.

## 4. Prioritize
- Rank by **impact × reachability** (how bad × how easy for a realistic attacker).
- Consider the attacker model: unauthenticated internet user, authenticated
  low-priv user, malicious tenant, insider, supply-chain. Different models expose
  different threats.
- Focus the deep review on the high-rank threats; note the rest as accepted or deferred.

## 5. Decide responses
For each significant threat: **mitigate** (add a control), **eliminate** (remove
the feature/exposure), **transfer** (push to a component designed for it), or
**accept** (document the residual risk and why).

## Keep it honest
- List assumptions explicitly — "the network is trusted", "input is pre-validated
  upstream". Each is a candidate bug.
- Record what you considered and dismissed, with the reason.
- A threat model is a living artifact: revisit it when the design or attacker
  landscape changes.
