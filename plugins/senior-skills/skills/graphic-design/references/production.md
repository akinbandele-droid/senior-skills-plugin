# Production & File Preparation

The design isn't done until it reproduces correctly in its medium. Getting color space,
resolution, bleed, and file format right is part of the craft — mistakes here ruin
otherwise good work and cost money at the printer.

## Raster vs. vector
- **Vector** (SVG, AI, EPS, PDF) — math-defined; scales infinitely with no quality loss. Use for
  logos, icons, type, and anything that must resize or print sharp at any size.
- **Raster** (PNG, JPG, TIFF, PSD) — pixel grid; resolution-dependent. Use for photos and complex
  imagery. Can't enlarge beyond its pixels without going soft/blocky — source at the size needed.

## Resolution
- **Print:** **300 dpi** at final print size (150+ for large-format viewed from distance). An image
  that looks fine on screen (72–96 ppi) can be far too low-res to print — check effective dpi at
  the placed size, not the file's nominal size.
- **Screen:** design to the target **pixel dimensions**; provide **@2x/@3x** assets for high-density
  (Retina) displays so they stay crisp. Don't upscale small images.

## Color space
- **Print → CMYK** (or spot/Pantone). Convert and preview; expect vivid RGB colors to shift/dull —
  adjust. Use spot colors for exact brand color or 1–2-color jobs. Embed/convert to the printer's
  profile; ask the printer for specs.
- **Screen → RGB / sRGB.** Export in sRGB so colors are consistent across devices/browsers.
- Keep brand colors documented in **all** systems (Hex/RGB/CMYK/Pantone) so they match across media.

## Print setup
- **Bleed:** extend any artwork/background that touches the edge **~3mm / 0.125"** past the trim so
  cutting leaves no white slivers. **Trim:** the final cut line. **Safe margin:** keep text/important
  elements well inside the trim (~3–5mm) so they aren't cut off.
- Set the correct **document/trim size** from the start. Mind fold/panel positions for brochures,
  and spine width for booklets. Overprint/knockout and rich-black (not 100%K alone) for large black areas.
- **Package** the file: outline or embed/supply **fonts**, include linked images at full res, and
  export a **print-ready PDF (PDF/X-1a or X-4)** with marks and bleed. Confirm requirements with the
  printer before sending.

## Screen/digital export
- Export the right format: **PNG** (lossless, transparency, UI/graphics), **JPG** (photos, no
  transparency — watch compression artifacts), **SVG** (vector logos/icons for web), **WebP/AVIF**
  (efficient modern web), **PDF** (documents/presentations), **GIF** (simple animation/legacy).
- Optimize file size for the destination (web performance — pair with `web-design`). Use correct
  dimensions; don't ship a 4000px image into a 400px slot.

## File hygiene
- Keep an **editable master** (layered, organized, named layers) plus exported deliverables. Name
  files clearly and version them. Organize assets. Deliver what the client/printer actually needs,
  and keep sources for future edits.
- Double-check before release: spelling/proofs, final size, bleed/safe area, color space, resolution,
  fonts embedded/outlined, and a last look at 100% and at final size/distance.

## Common pitfalls
- Low-res raster placed large → pixelated print. Logo delivered as raster (can't scale/print sharp).
- Designing print in RGB; no bleed (white edges) or elements inside the trim getting cut.
- Fonts not embedded/outlined → substituted/reflowed at the printer. Wrong trim size set late.
- Huge unoptimized images on the web; wrong export format (JPG for a logo, etc.).
- No editable master kept; disorganized, unnamed files.
