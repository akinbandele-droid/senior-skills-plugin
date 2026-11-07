# Visual Design

Visual design is applied clarity. Hierarchy, alignment, spacing, and restraint do
most of the work; color and ornament do the least. Build a small system and reuse it.

## Hierarchy — the first job
- Decide what the eye should hit first, second, third on each view, and make the
  visuals say so through size, weight, color, contrast, and position.
- One clear **primary action** per view; secondary actions visibly subordinate.
- If everything is bold, nothing is. Emphasis works by contrast with calm.

## Layout & grid
- Use a grid/columns for alignment and rhythm; align elements to shared edges.
- **Alignment** is the cheapest way to look intentional — few things misaligned
  reads as sloppy instantly.
- Group related things (proximity); separate unrelated things with space.
- Respect a consistent content width and margins; don't let lines of text run too long.

## Spacing
- Use a **spacing scale** (e.g. 4/8-based: 4, 8, 12, 16, 24, 32…), not arbitrary
  values. Consistent spacing is most of what "clean" means.
- Whitespace is active: it creates grouping, focus, and calm. Don't fear it.
- Space *inside* a group < space *between* groups, so grouping reads correctly.

## Typography
- A small **type scale** (e.g. a handful of sizes with clear roles: display,
  heading levels, body, caption). Don't freestyle sizes.
- Limit typefaces (often one or two). Set comfortable line-height (~1.4–1.6 for
  body) and sensible measure (~45–75 chars/line).
- Weight and size carry hierarchy; avoid using many colors to do type's job.
- Left-aligned body text is the safe default for most scripts; avoid justified web text.

## Color
- Define **roles**, not just swatches: background/surface, text, primary/brand,
  secondary, borders, and semantic (success/warning/error/info).
- Keep a restrained palette; use an accent sparingly for emphasis and the primary action.
- Ensure contrast meets WCAG AA (see accessibility.md) — check it, don't eyeball.
- **Never encode meaning in color alone** (add icon/text/shape) — for colorblind
  users and clarity.
- Design light and dark as deliberate role mappings, not an inversion.

## Consistency & components
- Build/reuse components (buttons, inputs, cards, modals) with consistent padding,
  radius, elevation, and states. Reuse before inventing.
- Pick an elevation/shadow and border language and apply it uniformly.
- Follow the platform's conventions and any existing design system/tokens.

## Imagery & icons
- Icons should be consistent in style and weight; pair with labels when meaning
  isn't obvious. Decorative images get empty alt; informative ones get real alt.

## Polish checklist
- Aligned to the grid; consistent spacing from the scale.
- Clear single focal point and hierarchy; primary action obvious.
- Restrained palette, sufficient contrast, meaning not color-only.
- Consistent components and type scale; nothing one-off without reason.
- Looks right at the smallest and largest target sizes (responsive).
