# Responsive Layout

Design for a continuum of screens, not three fixed widths. The page should adapt
fluidly and reflow gracefully from a small phone to a wide desktop.

## Mobile-first
- Write base styles for the small screen; add complexity with `min-width` media
  queries as space allows. This forces content priority and avoids desktop-only assumptions.
- Always include `<meta name="viewport" content="width=device-width, initial-scale=1">`.

## Fluid over fixed
- Prefer relative units: `%`, `fr`, `rem`/`em`, `ch`, `vw/vh`, and `min()/max()/clamp()`.
- Use `clamp(min, preferred, max)` for fluid type and spacing that scales with the
  viewport but stays within sane bounds (e.g. `font-size: clamp(1rem, 0.9rem + 1vw, 1.5rem)`).
- Constrain line length with `max-width` in `ch` (~60–75ch for body) so text stays readable.
- Images/media: `max-width:100%; height:auto;` so they never overflow.

## Layout tools
- **CSS Grid** for two-dimensional page/section structure. Powerful patterns:
  - Auto-responsive card grids with no media queries:
    `grid-template-columns: repeat(auto-fit, minmax(min(100%, 16rem), 1fr));`
  - Named areas for page scaffolding; reflow by redefining areas at breakpoints.
- **Flexbox** for one-dimensional component layout (toolbars, nav, button rows);
  use `flex-wrap` and `gap` so items wrap instead of overflowing.
- Use `gap` for spacing between grid/flex items rather than margins where possible.

## Breakpoints
- Put breakpoints **where the content/layout breaks**, not at named device sizes.
- Keep them few and meaningful; test by slowly resizing and watching where it gets awkward.
- **Container queries** (`@container`) let a component respond to *its own* space,
  not the viewport — ideal for reusable components that live in different widths.

## Common pitfalls
- **No horizontal scroll:** watch fixed widths, large images, long unbroken strings
  (`overflow-wrap: anywhere`), and wide tables (wrap in a scroll container).
- **Tap targets** stay large (~44px) and spaced on touch.
- **Hover isn't available on touch** — don't hide essential actions behind `:hover`;
  gate hover effects with `@media (hover: hover)`.
- Respect `min-height` with `dvh`/`svh` for mobile browser chrome rather than `vh`.
- Test real content, including the longest and shortest strings, not lorem ipsum of one length.

## Responsive images
- `srcset`/`sizes` to serve the right resolution; `<picture>` for art direction or
  format fallbacks. Always set `width`/`height` (or `aspect-ratio`) to reserve space
  and prevent layout shift (CLS).

## Verify
- From ~320px wide up to large desktop, and at 200% browser zoom.
- Portrait and landscape; with system text scaled up.
- Both pointer and touch; keyboard focus order still logical after reflow.
