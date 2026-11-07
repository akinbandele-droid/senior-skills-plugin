# Accessibility (WCAG)

Accessibility is a baseline requirement, not a feature. Target **WCAG 2.1/2.2
level AA**. Design it in from the start — it also makes the UI clearer for
everyone. The four principles: **Perceivable, Operable, Understandable, Robust.**

## Color & contrast
- Body/normal text: **≥ 4.5:1** contrast against its background. Large text
  (≥24px, or ≥19px bold): **≥ 3:1**.
- UI components and meaningful graphics (icons, input borders, focus rings): **≥ 3:1**.
- **Never convey information by color alone** — add text, icon, pattern, or shape.
  (Error isn't just red; selected isn't just a color.)
- Check contrast with a tool; don't trust your eyes, especially in dark mode.

## Keyboard
- **Everything operable by mouse must work by keyboard** — links, buttons, menus,
  modals, custom widgets. Test by unplugging the mouse.
- Logical tab order that follows reading/visual order; no keyboard traps.
- Visible **focus indicator** on every focusable element (don't remove outlines
  without replacing them with something equally visible, ≥3:1).
- Provide "skip to content"; manage focus when opening/closing modals and on route change.

## Semantics & structure
- Use native HTML elements for their meaning: `button` for actions, `a` for
  navigation, real headings (`h1`–`h6`) in order, lists for lists, `label` tied to inputs.
- Native elements come with accessibility for free — reach for ARIA only to fill
  gaps, and follow the ARIA Authoring Practices for custom widgets. **No ARIA is
  better than wrong ARIA.**
- One logical heading outline per page; landmarks (header/nav/main/footer) for structure.

## Text & content
- Resizable/zoomable to 200% without loss of content or function; no fixed tiny text.
- Meaningful **alt text** for informative images; empty alt (`alt=""`) for
  decorative ones. Don't put essential info only in an image.
- Clear, plain language; descriptive link text ("View invoice", not "click here").
- Labels and instructions for inputs; errors identified in text and programmatically.

## Forms & errors (a11y specifics)
- Every input has a programmatically-associated label.
- Errors: identify the field, describe the problem in text, and link/move focus to it;
  associate the message with the field (`aria-describedby`).
- Don't rely on placeholder as the only label.

## Media & motion
- Captions for video, transcripts for audio.
- Honor **`prefers-reduced-motion`**; avoid content that flashes more than 3×/second.
- No auto-playing audio; let users pause/stop moving content.

## Touch & targets
- Target size ≥ 24×24 CSS px (WCAG 2.2); aim for ~44px on touch. Adequate spacing
  between targets.

## Verify
- Keyboard-only walkthrough of every flow.
- Automated check (axe/Lighthouse) **plus** manual review — automation catches
  ~a third of issues.
- Screen-reader smoke test of key flows (VoiceOver/NVDA).
- Contrast audit of text, icons, and focus states in light and dark.
