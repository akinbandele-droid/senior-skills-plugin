# UX Process: Research, Flows, IA

Structure the problem before designing the surface. Most UI problems are really
unclear goals or bad information architecture wearing a visual disguise.

## Understand users & goals
- **Who** uses this: expertise, context (mobile on the go vs. desk), frequency,
  motivation, constraints (low vision, slow network, one-handed).
- **Jobs to be done:** what outcome are they hiring the product for? Design for the
  job, not the feature.
- **Context of use:** device, environment, interruptions, what happens before/after.
- Capture real quotes/behaviors where you have them; avoid designing for an
  imaginary ideal user.

## Lightweight research
- **Observe or ask** before assuming: what do users do today, where do they get
  stuck, what workarounds exist?
- **Competitive/convention scan:** how do similar products solve this? Follow
  conventions unless you have a clear reason not to — novelty has a usability cost.
- Turn findings into a short, concrete problem statement and success criteria.

## Personas / user model (keep it lean)
- A sentence or two per key user type: goal, context, pain, key constraint.
- Use them to make trade-offs ("which user loses if we do X?"), not as decoration.

## User flows
- Map the path to the goal step by step, including **decision points** and **all
  the branches**: not just success, but empty, error, permission-denied, offline,
  and first-run.
- Minimize steps and decisions on the critical path; defer the rest.
- Each screen should answer: where am I, what can I do, what happens next?

## Information architecture
- Inventory the content and actions; group by the user's mental model, not the org
  chart or the database schema.
- Establish **priority**: what's primary, secondary, tertiary on each view. This
  drives visual hierarchy later.
- Name things in the user's language. Labels and categories are UX.
- Navigation reflects the IA: findable, shallow where possible, predictable.

## Fidelity ladder
1. **Sketch / wireframe** — settle structure, flow, and hierarchy cheaply. No color/polish.
2. **Mid-fidelity** — real layout, content, and states; validate interaction.
3. **High-fidelity** — visual system applied; ready for critique and handoff.
Don't polish what isn't structurally settled — you'll just have to redo it.

## Reduce cognitive load
- Fewer choices per step (progressive disclosure); smart defaults.
- Recognition over recall — show options rather than making users remember.
- Chunk complex tasks; show progress; let users undo rather than confirm everything.
