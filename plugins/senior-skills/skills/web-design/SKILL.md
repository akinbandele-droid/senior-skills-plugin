---
name: web-design
description: >-
  Design and build websites for the web medium with craft: responsive layouts
  that work on any screen, clean scalable CSS with design tokens and theming,
  fast-loading pages that hit Core Web Vitals, and semantic, accessible,
  SEO-friendly markup. Use when designing or building a website, landing page,
  marketing site, or web UI — layout, responsive behavior, CSS architecture,
  web typography, performance, or turning a design into production HTML/CSS. For
  general UX/visual fundamentals pair with `ui-ux-design`.
---

# Web Design

Designing for the web means designing for the unknown: any screen size, any
network, any device, with the user in control of their browser. Build layouts
that adapt, styles that scale, and pages that load fast — on a foundation of
semantic, accessible markup. This skill covers the web-specific craft; for the
underlying UX, IA, visual, and accessibility fundamentals, use `ui-ux-design`.

## The build loop

1. **Content & structure first** — semantic HTML that makes sense with no CSS.
2. **Responsive layout** — mobile-first, fluid, content-driven breakpoints.
3. **Style system** — design tokens, a scalable CSS architecture, theming.
4. **Performance** — fast first paint, Core Web Vitals, optimized assets.
5. **Polish & verify** — states, cross-browser, responsive and a11y checks.

Core references (load the matching one):
- `references/responsive-layout.md` — mobile-first, fluid layout, Grid/Flexbox, breakpoints, container queries.
- `references/css-architecture.md` — design tokens (custom properties), naming, theming/dark mode, scalable CSS.
- `references/web-performance.md` — Core Web Vitals, images, fonts, loading strategy, CSS/JS budget.
- `references/web-fundamentals.md` — semantic HTML, progressive enhancement, SEO/meta, cross-browser.

Specialized references (load for the matching task):
- `references/landing-pages.md` — landing-page structure, CTAs, conversion, and A/B testing.
- `references/design-system.md` — tokens → components → patterns, theming, docs, governance.
- `references/animation.md` — motion purpose, timing/easing, micro-interactions, performance, reduced-motion.
- `references/email-html.md` — HTML email: tables, inline CSS, Outlook/VML, dark mode, testing.

## 1. Content & structure first
- Start from the content and the document outline. Write **semantic HTML** that
  is usable and readable with no CSS at all — headings, landmarks, lists, real
  buttons and links. Styling layers on top. See `references/web-fundamentals.md`.
- Progressive enhancement: the core content/function works without JS; JS improves it.

## 2. Responsive layout (mobile-first)
- Design the small screen first, then enhance for larger — it forces priority and
  avoids desktop-only assumptions.
- Prefer **fluid** layouts (%, `fr`, `min()/max()/clamp()`, `ch`/`rem`) over fixed
  pixels; let content reflow rather than scaling down.
- Use **CSS Grid** for 2-D page structure and **Flexbox** for 1-D component layout.
- Add breakpoints **where the content breaks**, not at specific device widths.
- Never cause horizontal scroll; keep tap targets and readable measure at every size.
- See `references/responsive-layout.md`.

## 3. Style system
- Define **design tokens** as CSS custom properties: color roles, spacing scale,
  type scale, radii, shadows, breakpoints. Everything references tokens, not magic values.
- Theme (light/dark, brand) by remapping tokens, not rewriting components.
- Keep CSS scalable and predictable: low specificity, consistent naming, components
  over one-off selectors. See `references/css-architecture.md`.

## 4. Performance (a design constraint, not a cleanup)
- Target **Core Web Vitals**: LCP (loading), INP (responsiveness), CLS (visual
  stability). Reserve space for images/embeds to avoid layout shift.
- Optimize the heavy stuff: responsive/next-gen **images**, subset/`display:swap`
  **fonts**, minimal render-blocking CSS/JS, lazy-load below the fold.
- Ship less: the fastest asset is the one you don't send. See `references/web-performance.md`.

## 5. Polish & verify
- Design all states (hover/focus/active/disabled, empty/loading/error) — see
  `ui-ux-design`'s interaction reference.
- Verify: responsive from ~320px up, keyboard + screen-reader pass, color contrast,
  cross-browser (current Chrome/Firefox/Safari/Edge), and Lighthouse/CWV.
- Respect user preferences: `prefers-color-scheme`, `prefers-reduced-motion`.

## Operating principles
- **Content-out, mobile-first.** Structure and small screens drive the design.
- **Fluid over fixed.** Design for a range, not three mockup widths.
- **Semantic, accessible markup is the foundation** — not a later retrofit.
- **Performance is UX.** A beautiful slow page is a failed page.
- **Tokenize and reuse.** Systems scale; one-off styles rot.
- **The user owns the browser.** Respect zoom, text size, reduced motion, color
  scheme, and no-JS/slow-network conditions.
- **Progressive enhancement.** Works everywhere; better where the platform allows.

## Related skills
- **ui-ux-design** — the general UX, information-architecture, visual, and WCAG
  fundamentals this skill builds on. Start there for the *what/why*, use this for
  the *how* on the web.
- **software-engineering** — for the engineering discipline behind the build
  (testing, review, maintainability).
- **security-review** — for client-side security concerns (XSS, CSP, secrets, auth)
  on anything beyond a static page.
- **graphic-design** — for the underlying visual craft (composition, typography, color,
  brand/identity) and for graphics/assets that go on the site.
