# Color

Color sets mood, creates hierarchy and meaning, and ties a design to its brand. Use it
with discipline — a restrained, intentional palette almost always beats a rainbow.

## Fundamentals
- **Hue** (the color), **saturation/chroma** (intensity), **value/lightness** (light↔dark). Value
  contrast does most of the heavy lifting for hierarchy and legibility — get value right first.
- The **color wheel** and harmony schemes:
  - **Monochromatic** — one hue, varied value/saturation (calm, cohesive, easy).
  - **Analogous** — neighbors on the wheel (harmonious, low tension).
  - **Complementary** — opposites (high contrast, vibrant; use one as accent, not 50/50).
  - **Split-complementary / triadic / tetradic** — more colors, more balancing needed.

## Building a palette
- Start from the **brand** and the **mood/message** (and cultural associations of colors for the
  audience — meanings vary by culture). 
- A reliable structure: **one dominant color + one or two supporting + one accent** for emphasis/
  calls-to-action. Add neutrals (a near-black, a near-white, grays) — most of a design is often
  neutral with color used sparingly for impact.
- Follow a rough **60/30/10** distribution (dominant/secondary/accent) for balance.
- Control saturation: fully-saturated colors everywhere fatigue the eye; mix in muted tones. Keep
  a consistent temperature/feel so the palette reads as a set.

## Color for hierarchy & meaning
- Use color to signal importance (a bright accent pulls the eye to the key element) and to encode
  meaning (status: red/green; categories). **Don't rely on color alone** to convey information —
  pair with text/shape/position for colorblind users and grayscale reproduction.

## Contrast & accessibility
- Ensure enough **value contrast** between text and background for legibility — especially where
  color/type carries information. For screen, target WCAG contrast (≈4.5:1 body, 3:1 large text);
  for print, test in grayscale.
- Check the palette in **grayscale** — if elements vanish or hierarchy collapses, you're relying
  on hue alone. Consider colorblind-safe choices (avoid red/green as the only distinction).

## Color spaces & reproduction (critical — see production.md)
- **RGB / sRGB** — screens (additive light). Design screen work in RGB.
- **CMYK** — process printing (subtractive ink). Design print work in CMYK (or convert carefully);
  bright RGB colors (vivid blues, greens, oranges) can shift/dull in CMYK — preview and adjust.
- **Spot (e.g. Pantone)** — specific premixed inks for exact/brand colors and when limiting to
  1–2 print colors; essential for consistent brand color across materials.
- **Hex** — the sRGB notation used for screen/web. Document brand colors in all needed systems
  (Hex, RGB, CMYK, Pantone) so they match everywhere.

## Common pitfalls
- Too many colors / full saturation everywhere → chaotic, tiring, no hierarchy.
- Relying on hue with no value contrast (fails legibility and grayscale).
- Information conveyed by color alone (colorblind/accessibility failure).
- Designing print in RGB then being surprised when CMYK prints dull/shifted.
- Inconsistent brand color across pieces (no defined values / no spot color where needed).
