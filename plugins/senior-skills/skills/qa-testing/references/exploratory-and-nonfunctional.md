# Exploratory & Non-Functional Testing

Scripted tests confirm what you thought to check. Exploratory testing and non-functional
testing find what you didn't — and whether the system is good enough beyond "it works."

## Exploratory testing
- **Simultaneous learning, test design, and execution** — a skilled tester investigates the
  system, forms hypotheses about how it might break, and follows the interesting threads. Often
  finds the highest-value bugs that scripted cases miss.
- **Not random clicking.** Structure it with **charters**: a focused mission for a time-boxed
  session (e.g. "explore the checkout flow for ways to be charged the wrong amount," 60 min).
- **Heuristics to provoke bugs:**
  - Vary inputs to extremes and nonsense (boundaries, huge, empty, special chars, paste a novel).
  - Interrupt and resume (refresh, back button, close mid-flow, lose network, double-click).
  - Do things out of the intended order; skip steps; go backwards.
  - Mnemonics like **SFDPOT** (Structure, Function, Data, Platform, Operations, Time) and
    **"CRUD"** per entity to cover ground; **"goldilocks"** (too big / too small / just right).
- **Take notes:** what you tried, what you saw, and questions raised — so bugs are reproducible
  and coverage is visible. Feed confirmed issues into scripted regression tests.

## Non-functional testing
Quality is more than correct behavior. Test the relevant ones for the system's risk:

- **Performance:** does it meet latency/throughput targets?
  - *Load* (expected volume), *stress* (beyond capacity — how it fails), *soak* (sustained —
    leaks/degradation), *spike* (sudden surge). Measure percentiles (p95/p99), not just averages.
    Define targets first; test on production-like scale/data.
- **Accessibility:** usable by people with disabilities — WCAG 2.1/2.2 AA. Automated scanners
  catch some issues; also test keyboard-only, screen reader, contrast, focus, and zoom manually.
  Pair with `ui-ux-design`.
- **Compatibility:** across browsers, devices, OS/versions, screen sizes, and locales as the
  audience requires. Pair with `web-design` for responsive.
- **Reliability/resilience:** behavior under failure — dependency down, timeout, partial outage,
  recovery, retries, graceful degradation. (Chaos-style thinking.)
- **Usability:** can real users accomplish goals without confusion? Observe, don't assume.
- **Security-adjacent sanity:** obvious issues (secrets in responses, missing authz on an
  endpoint, verbose errors) — but hand real security testing to the `pentest` family; it needs
  authorization and different expertise.
- **Internationalization/localization:** unicode, right-to-left, date/number/currency formats,
  translated text length, timezones.

## Common pitfalls
- Calling unstructured clicking "exploratory testing" (no charters, no notes, not reproducible).
- Only functional testing — shipping something correct but slow, inaccessible, or fragile.
- Performance tested on tiny data; reporting averages that hide bad tail latency.
- Treating accessibility as a scanner checkbox rather than real assistive-tech testing.
- Skipping failure/resilience testing until a real outage proves it.
