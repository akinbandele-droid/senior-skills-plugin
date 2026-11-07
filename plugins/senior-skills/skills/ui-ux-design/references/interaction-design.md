# Interaction Design

Interaction is the conversation between the user and the interface. Make it
obvious what things do, give feedback for every action, and design every state —
not just the one where everything goes right.

## Affordances & signifiers
- Elements should look like what they do: buttons look pressable, links look
  clickable, inputs look editable, disabled looks inert.
- Don't make clickable things look like plain text, or static things look
  interactive. Consistency of signifiers across the product matters.
- Hit targets large enough to tap (aim ~44×44px min on touch).

## Feedback
- Every action gets a visible response — immediately. A click with no feedback
  feels broken.
- Show system status: loading/progress for anything not instant; confirm success;
  surface errors clearly and near the cause.
- Prefer optimistic/instant feedback where safe; otherwise show a spinner/skeleton
  so the user knows it's working.

## Design all the states
For every interactive element and view, design:
- **Default, hover, focus, active, disabled** for controls.
- **Empty** (first use / no data — make it helpful, not a blank void).
- **Loading** (skeletons/spinners; avoid layout jumps).
- **Error** (what went wrong, why, and how to fix it — in plain language).
- **Success / partial / offline** where relevant.
The missing state is usually where the UX falls apart.

## Forms (the hardest common surface)
- Ask for the least possible; group logically; one column is usually best.
- Clear, persistent labels (not placeholder-as-label). Mark required/optional
  consistently. Helpful hints near the field.
- **Inline validation** at the right moment (on blur/submit, not keystroke-nagging);
  specific, polite error messages that say how to fix it.
- Preserve input on error; never clear the form. Sensible input types/keyboards.
- Make the primary submit obvious; prevent double-submit; show progress.

## Navigation
- Users should always know **where they are**, **where they can go**, and **how to
  get back**. Provide clear back/undo paths.
- Keep navigation consistent and predictable across the product.
- Match platform conventions (web back button, iOS/Android patterns).

## Error prevention & recovery
- Prevent errors before validating them: good defaults, constraints, confirmation
  only for destructive/irreversible actions.
- Prefer **undo** over confirmation dialogs for reversible actions.
- When something fails, explain and offer the next step — never a dead end.

## Motion & animation
- Motion should clarify, not decorate: show relationships, continuity, and state
  changes; guide attention.
- Keep it fast (~150–300ms) and purposeful; avoid blocking the user on animation.
- **Respect `prefers-reduced-motion`** — offer a reduced/!no-motion path.

## Responsiveness & input modes
- Design for the range of viewports; reflow rather than shrink unusably.
- Support both pointer and touch; don't hide essential actions behind hover only
  (hover doesn't exist on touch).
- Keyboard and screen-reader operability is part of interaction — see accessibility.md.
