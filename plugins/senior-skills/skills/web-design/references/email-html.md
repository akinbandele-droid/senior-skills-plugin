# Email HTML (its own strange beast)

HTML email is not the web. Clients render with ancient, inconsistent engines
(Outlook uses Word's engine; Gmail strips things; Apple Mail is near-modern). The
modern CSS from the rest of this skill mostly does **not** apply. Build
conservatively, test widely, and assume everything can be stripped.

## Core constraints
- **Tables for layout.** Flexbox/Grid don't work reliably. Structure with nested
  `<table role="presentation">`; one outer 100% table, an inner fixed-width table
  (usually **600px**) centered for the content.
- **Inline styles.** Many clients strip `<style>` blocks (notably some Gmail
  contexts). Put critical styles inline on elements; use a `<head>` `<style>` only
  for progressive enhancement (media queries, hover) that you can live without.
- **No JS, no forms you rely on, no external CSS.** Links only for actions.
- **Limited units & properties:** stick to pixels, basic colors, standard fonts.
  Avoid modern layout, `rem`, custom properties, backgrounds you depend on, and
  anything you haven't tested.

## Layout rules
- Single-column is safest and best on mobile; multi-column only with table cells
  that stack (via media query + fallback).
- Fixed content width ~600px; full-width colored band = outer table bg, inner 600px table.
- Set widths on `<td>`s; use cellpadding/cellspacing or padding on cells (not margins
  — margins are unreliable).
- Images: set explicit `width`/`height`, `display:block`, and `border:0`. **Always
  provide `alt` text** — many clients block images by default, so the email must
  make sense with images off. Don't build the whole email as one image.

## Typography & color
- Web-safe font stacks with fallbacks (Arial/Helvetica, Georgia, system). Custom
  web fonts work only in some clients — always fall back gracefully.
- Sufficient size (≥14–16px body) and contrast; left-aligned text; generous line-height.
- Don't rely on background images for legibility (Outlook ignores many) — use solid
  bg colors with VML fallback if a bg image is essential.

## Outlook (the hard one)
- Renders with the Word engine: no `max-width`, flaky margins/padding, ignores many
  CSS properties, forces its own line heights.
- Use conditional comments / MSO conditionals and VML for Outlook-specific fixes
  (bulletproof buttons, background images, ghost tables for spacing).
- Build "bulletproof buttons" (padded table cell with bg color + VML for Outlook)
  rather than CSS-styled anchors.

## Mobile & dark mode
- Mobile: use a `<style>` media query to stack columns and enlarge tap targets —
  but the email must still work where the `<style>` is stripped (design the inline
  default to be mobile-friendly: single column, big enough text/targets).
- Dark mode: some clients auto-invert colors unpredictably. Test it; use
  `@media (prefers-color-scheme)` and `meta name="color-scheme"` where supported;
  avoid pure black/white logos that vanish on inversion (add padding/background).

## Deliverability & correctness
- Keep HTML lean; some clients clip large emails (Gmail ~102KB) — stay well under.
- Always include a **plain-text version** (multipart) — better deliverability and a
  fallback. Include a visible unsubscribe link and physical address for marketing
  (CAN-SPAM/GDPR). Set a meaningful preheader (hidden preview text).
- Accessible: real text over images, `alt`, logical order, `lang`, `role="presentation"`
  on layout tables so screen readers skip them.

## Test, don't assume
- Test across real clients before sending: Outlook (several versions), Gmail (web +
  app), Apple Mail, iOS Mail, and dark mode — via a testing service (Litmus/Email on
  Acid) or seed accounts. Rendering differs more than you expect.
- Consider a vetted email framework (MJML or a tested table-based boilerplate) to
  avoid hand-writing the Outlook/VML quirks — but still test the output.
