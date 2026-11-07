# Evaluation & Critique

Design isn't done when it's drawn — it's done when it works. Evaluate against the
goals and heuristics, and run critique as a tool for better outcomes, not opinion
jousting.

## Nielsen's usability heuristics (evaluate against these)
1. **Visibility of system status** — the UI keeps the user informed with timely feedback.
2. **Match to the real world** — users' language and mental models, not jargon.
3. **User control & freedom** — easy undo/exit; no dead ends.
4. **Consistency & standards** — within the product and with platform conventions.
5. **Error prevention** — stop mistakes before they happen.
6. **Recognition over recall** — show, don't make them remember.
7. **Flexibility & efficiency** — shortcuts for experts, simple path for novices.
8. **Aesthetic & minimalist design** — no irrelevant clutter competing for attention.
9. **Help users with errors** — plain-language problems + how to fix.
10. **Help & documentation** — available when needed, findable, task-focused.

## Heuristic walkthrough
- Walk each key task end to end as the user, including empty/error/edge paths.
- At each screen ask: Do I know where I am and what to do? Is the primary action
  obvious? What happens if I'm a first-time user / have no data / make a mistake?
- Note violations with the heuristic and a severity (blocker / major / minor / polish).

## Validate with users (when possible)
- Give real tasks, watch, don't lead ("show me how you'd…"). Where they hesitate or
  err is a design problem, not a user problem.
- A handful of users surfaces most major issues. Test early, rough, and often.

## Running a critique
- **Frame it:** state the goal, the user, the constraints, and what feedback you
  want ("is the hierarchy clear?"), so critique is targeted.
- **Critique the design against the goal,** not personal taste. "A new user won't
  know this is the primary action" beats "I'd make it blue."
- Be specific and actionable; separate blocking problems from preferences.
- Explore the *why* before jumping to a fix; there's usually more than one solution.

## Receiving critique
- You are not your design; feedback on it isn't about you.
- Listen for the underlying problem even when the proposed fix is off.
- Ask "what made you expect that?" to get to the real issue.
- Decide with the goals and users as the tiebreaker, and record why.

## Self-review checklist
- Does each screen serve the user's goal with an obvious next step?
- Clear hierarchy and one primary action per view?
- All states designed (empty/loading/error/success/edge)?
- Accessible: contrast, keyboard, focus, semantics, not color-only?
- Consistent with the system and platform conventions?
- Anything that could be removed without loss? (Usually yes.)
- Can an engineer build it from the spec without guessing?
