# Web Fundamentals

The web platform rewards building on its grain: semantic HTML, progressive
enhancement, and respect for the user's browser. Get these right and
accessibility, SEO, and resilience largely follow.

## Semantic HTML
- Use elements for their meaning, not their default look: `header`, `nav`, `main`
  (one per page), `section`, `article`, `aside`, `footer`; headings `h1`–`h6` in
  order; `ul/ol/li`; `button` for actions, `a[href]` for navigation; `figure`,
  `time`, `table` for tabular data.
- A screen reader, a search crawler, and reader mode all rely on this structure.
  The page should make sense as a document with CSS disabled.
- Don't rebuild native controls out of `div`s — you'll lose keyboard, focus, and
  semantics. Reach for custom widgets only when necessary, and follow ARIA APG.
- Labels tied to inputs; `alt` on images; captions/transcripts for media.

## Progressive enhancement
1. **Content** in semantic HTML — works for everyone.
2. **Presentation** in CSS — enhances the layout.
3. **Behavior** in JS — enhances interaction.
- Core content and navigation should work before JS loads/if JS fails. Links and
  form submits should function server-side where feasible; JS makes them nicer.
- This also improves performance (fast first render) and resilience (flaky networks).

## SEO & metadata (for public sites)
- Unique, descriptive `<title>` and `<meta name="description">` per page.
- One `h1` describing the page; logical heading outline; descriptive link text.
- Semantic structure and fast load are ranking and crawlability wins on their own.
- `<meta name="viewport">`; canonical URLs; meaningful, readable URL slugs.
- **Open Graph**/Twitter card tags for shareable link previews.
- Structured data (JSON-LD schema.org) where it fits (articles, products, FAQs).
- `sitemap.xml` and sensible `robots.txt`; alt text; accessible = more indexable.
- Server-render or pre-render content that must be indexed; don't hide key content behind JS only.

## The `<head>` baseline
```html
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Clear, specific page title</title>
<meta name="description" content="One-sentence summary.">
<meta name="color-scheme" content="light dark">
<!-- OG tags, canonical, favicon, preconnect to critical origins -->
```

## Respect the user's browser
- Honor `prefers-color-scheme`, `prefers-reduced-motion`, and `prefers-contrast`.
- Support zoom and user font-size (use `rem`, don't disable zoom in the viewport tag).
- Don't hijack native behavior (scroll, back button, text selection, right-click)
  without a strong reason.
- Degrade gracefully: feature-detect (`@supports`) rather than assume; provide
  fallbacks for newer CSS/JS.

## Cross-browser & resilience
- Test current Chrome, Firefox, Safari, Edge (and iOS Safari — it differs most).
- Use `@supports` for progressive CSS; provide sensible fallbacks.
- HTTPS everywhere; sensible security headers (CSP, etc.); never ship secrets to the client.
- Handle the offline/slow/empty/error cases, not just the fast happy path.

## Verify
- Renders and is navigable with CSS off and with JS off (core content).
- Valid HTML; heading outline makes sense; landmarks present.
- Keyboard + screen-reader pass; contrast and focus visible.
- Link previews (OG) and `<title>`/description correct; Lighthouse SEO + a11y pass.
