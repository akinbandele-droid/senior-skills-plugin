# CSS Architecture

Scalable CSS is predictable CSS: low specificity, tokens instead of magic values,
components instead of one-off selectors, and theming by remapping variables.

## Design tokens (CSS custom properties)
Define the system once on `:root`, reference it everywhere:

```css
:root {
  /* color roles — not raw names */
  --color-bg: #ffffff;
  --color-surface: #f6f7f9;
  --color-text: #15181c;
  --color-muted: #5b6673;
  --color-primary: #2f6df6;
  --color-border: #d9dee5;
  --color-danger: #d7263d;

  /* spacing scale (4/8-based) */
  --space-1: 0.25rem; --space-2: 0.5rem; --space-3: 0.75rem;
  --space-4: 1rem;   --space-6: 1.5rem; --space-8: 2rem; --space-12: 3rem;

  /* type scale */
  --text-sm: 0.875rem; --text-base: 1rem; --text-lg: 1.25rem;
  --text-xl: 1.5rem;  --text-2xl: 2rem;  --text-3xl: 3rem;
  --font-body: system-ui, sans-serif;
  --leading-body: 1.5;

  /* radii, shadows, transitions */
  --radius: 0.5rem;
  --shadow-1: 0 1px 2px rgb(0 0 0 / 0.06), 0 2px 8px rgb(0 0 0 / 0.08);
  --ease: 200ms cubic-bezier(.2,.0,.2,1);
}
```
- Reference tokens in every rule (`padding: var(--space-4)`), never hardcode.
- Build **semantic** tokens (`--color-text`) on top of **primitive** ones if the
  system is large (`--blue-600`) — components use semantic, themes remap primitives.

## Theming (light/dark, brand) by remapping
```css
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) { --color-bg:#0f1115; --color-text:#e9edf2; /* … */ }
}
:root[data-theme="dark"] { --color-bg:#0f1115; --color-text:#e9edf2; /* … */ }
```
- Give `body` an explicit background and color from tokens.
- Components stay identical; only the variables change. Never fork a component per theme.

## Keep specificity low and flat
- Prefer a single class per component part; avoid deep descendant selectors and `!important`.
- Avoid styling by element/ID for components; don't chain long selectors.
- A naming convention keeps it legible — BEM (`.card`, `.card__title`, `.card--featured`)
  or a utility approach (Tailwind). Pick one and be consistent.
- Modern isolation tools when useful: CSS Modules / Shadow DOM / `@scope`, cascade
  `@layer` to control order deliberately, and `:where()` to keep specificity at zero.

## Components over pages
- Style reusable components (button, input, card, nav) with all their states and
  variants; compose pages from them. Don't restyle per page.
- Encode spacing *between* things with layout utilities or `gap`, not component margins
  (components shouldn't know their surroundings).

## Modern layout & sizing
- `box-sizing: border-box` globally. Logical properties (`margin-inline`,
  `padding-block`) for i18n. `gap`, `aspect-ratio`, `clamp()`, nesting, and
  `color-mix()` reduce boilerplate.

## Hygiene
- One source of truth for each decision (the token). No duplicated magic numbers.
- Delete dead CSS; keep the stylesheet small (it's render-blocking).
- Comment the *why* for non-obvious rules (hacks, overrides, browser workarounds).
- A short reset/normalize baseline; then tokens; then base elements; then components; then utilities.
