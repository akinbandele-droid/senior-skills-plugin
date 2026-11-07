---
name: ui-ux-design
description: >-
  Design usable, accessible, good-looking interfaces with product-designer
  discipline: understand users and goals, structure flows and information,
  apply visual and interaction fundamentals, meet accessibility standards, and
  critique the result. Use for designing or improving any UI — web, mobile, or
  app screens, flows, components, forms, dashboards, design systems — and for UX
  reviews, wireframes, or turning requirements into a usable interface. Covers
  research, IA, interaction, visual design, and WCAG accessibility.
---

# UI/UX Design

Design decisions serve the user's goal, not decoration. Good design is usable
first, accessible always, and beautiful because it's clear — not in spite of
clarity. Match effort to the work: a single button state is not a full product,
but never skip understanding the user or checking accessibility.

## The design loop

1. **Understand** — who the users are, what they're trying to accomplish, and the
   constraints (platform, brand, tech, timeline).
2. **Structure** — the flows and information architecture before any pixels.
3. **Design** — layout, interaction, and visual treatment on that structure.
4. **Make it accessible** — not a final pass; a constraint throughout.
5. **Validate & critique** — test with real tasks; review honestly against goals.
6. **Specify** — hand off clearly: states, tokens, behavior, edge cases.

References (load the matching one):
- `references/ux-process.md` — research, personas, user flows, information architecture.
- `references/visual-design.md` — layout, grid, type, color, spacing, hierarchy, consistency.
- `references/interaction-design.md` — states, feedback, affordances, forms, navigation, motion.
- `references/accessibility.md` — WCAG essentials: contrast, keyboard, semantics, focus, ARIA.
- `references/critique.md` — heuristic evaluation and running/receiving a design critique.

## 1. Understand
- Who is this for, in what context (device, environment, expertise, frequency)?
- What is the user's actual goal and the job they're hiring this UI to do?
- What are the business goals and the hard constraints (brand, platform
  conventions, existing design system, engineering limits)?
- Define success observably: task completion, fewer errors, faster, clearer.
- When the need is ambiguous in a way that changes the design, ask; otherwise pick
  a sensible default and state it. See `references/ux-process.md`.

## 2. Structure first
- Map the **user flow**: the steps to the goal, the decision points, and the
  error/empty/edge paths — not just the happy path.
- Define **information architecture**: what content/actions exist, how they group,
  and their priority. Hierarchy is decided here, before visuals.
- Sketch low-fidelity (wireframe) to settle structure cheaply before investing in polish.

## 3. Design on the structure
- **Layout & hierarchy** — guide the eye to the most important thing first; use
  space, size, and position to express priority. One primary action per view.
- **Visual system** — consistent type scale, spacing scale, color roles, and
  components. Reuse before inventing. See `references/visual-design.md`.
- **Interaction** — every element shows what it is and what it does; every action
  gives feedback; design all states (default, hover, focus, active, disabled,
  loading, empty, error, success). See `references/interaction-design.md`.

## 4. Accessibility (throughout)
Treat WCAG AA as the floor, not a nice-to-have: sufficient color contrast,
full keyboard operability, visible focus, meaningful semantics/labels, and no
information carried by color alone. Design it in from the start — retrofitting is
costly and worse. See `references/accessibility.md`.

## 5. Validate & critique
- Test the design against the real tasks from step 1 — ideally with users, at
  minimum by walking each flow end to end including errors and edges.
- Evaluate against usability heuristics and the stated goals, honestly. See
  `references/critique.md`.
- Iterate: design is revision, not a single pass.

## 6. Specify for handoff
- Document states, behavior, responsive rules, content/empty/error cases, and the
  design tokens (type, color, spacing). Note the *why* behind non-obvious choices.
- Make it buildable without guessing; call out interactions and motion explicitly.

## Operating principles
- **Usability over decoration.** If it's pretty but confusing, it failed.
- **Clarity creates beauty.** Hierarchy, alignment, spacing, and restraint do most
  of the aesthetic work.
- **Consistency.** Reuse patterns and components; follow platform conventions;
  surprise is a cost.
- **Accessible by default.** Design for the full range of users and abilities.
- **Design the unhappy paths.** Empty, loading, error, and edge states are part of
  the design, not afterthoughts.
- **Reduce load.** Fewer choices, clear defaults, progressive disclosure; don't
  make the user think about what you could decide for them.
- **Justify with the goal.** Every choice traces back to a user or business goal.

## Related skills
- **web-design** — the web-specific craft (responsive layout, CSS architecture,
  performance, semantic markup) that builds on these UX/visual/accessibility
  fundamentals. Use this skill for the foundations, web-design for the medium.
- **software-engineering** — to implement the design as production code.
- **qa-testing** — to test the built interface, including accessibility and usability,
  against the design's criteria.
- **graphic-design** — for the visual-communication craft (layout, type, color, brand,
  imagery) and non-interface assets like logos, print, and marketing graphics.
