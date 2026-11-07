# Design Systems & Component Libraries

A design system is the single source of truth for how a product looks and behaves:
tokens, components, patterns, and the rules for using them. It exists to make UIs
consistent, accessible, and fast to build — not to be an end in itself. Build only
as much system as the product needs.

## Layers
1. **Foundations / tokens** — the atomic decisions: color roles, type scale,
   spacing scale, radii, shadows, z-index, breakpoints, motion durations/easings.
2. **Components** — reusable UI building blocks (button, input, select, checkbox,
   card, modal, tabs, toast, table) with defined variants, sizes, and states.
3. **Patterns** — how components combine for recurring jobs (forms, empty states,
   page scaffolds, data tables, auth flows).
4. **Guidelines** — usage, content/voice, accessibility, do/don't, and when to use
   which component.

## Tokens as the source of truth
- Define once, reference everywhere (see `css-architecture.md` for the CSS setup).
- **Primitive → semantic → component** layering for larger systems:
  `--blue-600` (primitive) → `--color-primary` (semantic) → `--button-bg` (component).
  Themes remap primitives/semantics; components never hardcode.
- Keep tokens platform-agnostic where you can (a JSON/`Style Dictionary` source can
  emit CSS, iOS, Android) so web and native stay in sync.

## Designing good components
- **API first:** decide the props/variants (`variant`, `size`, `state`, slots)
  before styling. Make the common case the default and the wrong thing hard.
- **Every state, always:** default, hover, focus-visible, active, disabled,
  loading, error — plus empty/skeleton where relevant. A component missing states
  spreads inconsistency everywhere it's used.
- **Accessible by construction:** built on semantic elements, keyboard-operable,
  labelled, correct focus management, ARIA per APG for custom widgets. Accessibility
  lives in the component so consumers get it for free.
- **Composable, not monolithic:** small pieces that combine (e.g. `Field` = `Label`
  + `Input` + `HelpText` + `Error`) beat one component with 30 props.
- **Controlled vs. uncontrolled**, sensible defaults, and don't leak internal DOM
  assumptions. Keep the surface area small and stable.

## Variants & theming
- Variants via tokens/data attributes/utility classes, not forked components.
- Support light/dark and brand theming by remapping tokens; one component, many themes.
- Density/size scales (compact vs. comfortable) through spacing tokens.

## Documentation (a system is only as good as its docs)
- Each component: when to use / when not, props, all states, accessibility notes,
  code example, and live interactive examples (Storybook or similar).
- Document the *why* and the rules, so people use it correctly instead of forking it.
- Show real usage patterns, not just the component in isolation.

## Governance & adoption
- **Versioning:** semver; communicate breaking changes; provide migration notes/codemods.
- **Contribution model:** how new components get proposed, reviewed, and added, so the
  system grows deliberately instead of fragmenting.
- **Single source, generated outputs:** avoid parallel definitions that drift.
- Measure adoption; retire one-off components that duplicate the system.

## Pitfalls
- Over-engineering a system bigger than the product needs — start with tokens and
  the few components actually used.
- Components that aren't accessible — the inconsistency scales as fast as the reuse.
- Too many props / god-components; fork-per-use instead of variants; undocumented
  behavior; tokens bypassed with magic values. Each erodes the single source of truth.
