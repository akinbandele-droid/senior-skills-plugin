# Animation & Micro-interactions

Motion should earn its place: clarify relationships, give feedback, guide
attention, and express a little personality — never decorate for its own sake or
make the user wait. The best animation is felt, not noticed.

## Purposes of motion
- **Feedback:** confirm an action happened (button press, toggle, item added).
- **Continuity:** show where things come from and go (a panel slides from the edge
  it's anchored to; a list item animates into its place) so the user keeps context.
- **Orientation:** express hierarchy and spatial relationships during transitions.
- **Status:** communicate progress, loading, and state changes.
- **Delight:** sparing, tasteful moments of personality — rare and purposeful.

## Timing & easing
- **Fast:** most UI transitions **150–300ms**. Micro-interactions can be ~100–200ms;
  larger/spatial transitions a bit longer. Too slow feels sluggish; instant feels abrupt.
- **Easing:** almost never linear (except continuous spinners). Use ease-out for
  things entering (decelerate into place), ease-in for things leaving, ease-in-out
  for moves. Natural motion accelerates and decelerates.
- Consistent durations/easings via tokens (`--ease`, `--dur-fast`) so motion feels
  like one system.

## Micro-interactions
- A micro-interaction has a trigger → feedback → (sometimes) result. Examples:
  hover/focus states, button ripple/press, form validation check, toggle switch,
  like/heart, pull-to-refresh, skeleton-to-content.
- Make interactive feedback **immediate** (on `:active`/`:focus`), short, and subtle.
- They reward and reassure — the user knows the system heard them.

## Performance (cheap motion only)
- Animate only **`transform` and `opacity`** — they're GPU-composited and don't
  trigger layout/paint. Avoid animating width/height/top/left/margin (janky).
- Use `will-change` sparingly and remove it after; prefer CSS transitions/animations
  over JS where possible; for spring/physics or coordinated sequences, a library
  (e.g. a mature animation lib) is fine, but keep the bundle cost in mind.
- Keep animations off the critical render path; never block interaction on motion.
- 60fps or don't ship it — test on a mid-tier phone, not just a fast laptop.

## Accessibility (non-negotiable)
- **Honor `prefers-reduced-motion`:** provide a reduced path — cut or greatly
  simplify non-essential motion (cross-fade instead of large movement).
  ```css
  @media (prefers-reduced-motion: reduce) {
    *, *::before, *::after { animation-duration:.01ms!important; transition-duration:.01ms!important; animation-iteration-count:1!important; }
  }
  ```
- No content flashing more than 3×/second (seizure risk).
- Don't convey essential information by motion alone; don't trap focus during transitions.
- Avoid large parallax/auto-moving content that can cause vestibular discomfort; let users stop it.

## Restraint
- Don't animate everything — competing motion is noise and fatigues users.
- Entrance animations on every element on load gets old fast; be selective.
- If an animation makes a frequent action slower, cut it. Speed > spectacle for
  things people do often.
- Motion should be consistent across the product (same easing/duration language),
  so it reads as intentional, not random.

## Modern tools
- CSS transitions/`@keyframes` for most cases; `transition`/`animation` tokens.
- View Transitions API for same-/cross-document state and page transitions.
- Scroll-driven animations (`animation-timeline`) for scroll effects without JS —
  still keep them subtle and reduced-motion-aware.
