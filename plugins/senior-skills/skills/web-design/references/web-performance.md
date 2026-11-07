# Web Performance

Performance is user experience: speed affects bounce, conversion, and
accessibility on slow devices/networks. Treat it as a design constraint with a
budget, not a post-launch cleanup.

## The targets: Core Web Vitals
- **LCP (Largest Contentful Paint)** — loading. Aim **< 2.5s**. Usually the hero
  image or heading; make it load early and fast.
- **INP (Interaction to Next Paint)** — responsiveness. Aim **< 200ms**. Keep the
  main thread free; avoid heavy JS blocking input.
- **CLS (Cumulative Layout Shift)** — visual stability. Aim **< 0.1**. Reserve
  space so nothing jumps.

## Prevent layout shift (CLS)
- Always set `width`/`height` or `aspect-ratio` on images, video, iframes, ads.
- Reserve space for anything injected late (banners, embeds).
- Preload/`font-display: swap` fonts and match fallback metrics to avoid text reflow.
- Don't insert content above existing content after load.

## Images (usually the biggest payload)
- Serve **next-gen formats** (AVIF/WebP) with fallbacks.
- **Responsive images:** `srcset`/`sizes` so phones don't download desktop assets.
- **Lazy-load** below-the-fold images (`loading="lazy"`); do NOT lazy-load the LCP image.
- Compress; strip metadata; size to the largest rendered dimension, not the original.
- Mark the hero/LCP image high priority (`fetchpriority="high"`), don't defer it.

## Fonts
- Subset to needed glyphs/weights; prefer variable fonts over many files.
- `font-display: swap` (or `optional`) so text is visible during load.
- Preload the critical font; self-host or `preconnect` to the font host.
- Consider a good **system font stack** — zero bytes, instant render.

## CSS & JS
- CSS is render-blocking: keep it lean, remove dead rules, inline critical CSS for
  above-the-fold where it helps, load the rest normally.
- Ship less JS. Defer/async non-critical scripts; code-split; avoid large frameworks
  for simple sites. The fastest script is none.
- Avoid long main-thread tasks (they hurt INP); break up work, debounce handlers.
- Minify and compress (gzip/brotli) text assets.

## Loading strategy
- Prioritize above-the-fold/critical-path resources; defer the rest.
- `preconnect`/`dns-prefetch` to required third-party origins; `preload` truly
  critical assets (used soon, same render).
- Cache aggressively with fingerprinted filenames and long `max-age`; use a CDN.
- Minimize third-party scripts — each is a performance and privacy liability; load
  them async and measure their cost.

## Budget & measure
- Set a budget (e.g. total transfer, JS size, LCP target) and hold the line.
- Measure with **Lighthouse** and **lab tools**, but trust **field data** (real
  users, CrUX) for the verdict. Test on a mid-tier phone and throttled network, not
  just your fast laptop.
- Re-check after adding features; performance erodes silently.
