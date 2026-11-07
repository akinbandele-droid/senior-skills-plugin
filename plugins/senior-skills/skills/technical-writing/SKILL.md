---
name: technical-writing
description: >-
  Write clear, accurate technical documentation that serves its reader: READMEs,
  API reference, how-to guides and tutorials, architecture/design docs and ADRs,
  runbooks, and release notes. Use when the user is writing or improving docs,
  a README, API docs, a guide, a design doc, a runbook, or any explanation of a
  technical system — or wants a doc reviewed/restructured. Emphasizes audience
  fit, structure, correctness, and examples that actually run. Pairs with
  `software-engineering` for the code being documented.
---

# Technical Writing

Documentation exists to get a specific reader to a specific outcome — understand a
concept, complete a task, make a decision, or recover from an incident. Good docs
are **accurate, findable, scannable, and example-driven**, written for the reader's
level and goal, not the author's. Treat docs as part of the product and keep them
current with the code.

## Core loop

1. **Identify reader & goal** — who reads this, what they know, what they need to do after.
2. **Choose the doc type** — the shape (tutorial, how-to, reference, explanation, runbook)
   follows the goal.
3. **Outline** — structure before prose; the skeleton carries the logic.
4. **Draft** — clear, direct, active; lead with what matters; show, don't just tell.
5. **Verify** — every command, code sample, and claim actually works and is current.
6. **Edit & test on a reader** — cut, clarify, and check someone can follow it.

References (load the matching one):
- `references/doc-types.md` — the Diátaxis types, READMEs, ADRs, runbooks, release notes.
- `references/writing-style.md` — clarity, structure, scannability, examples, inclusive language.

## 1. Identify reader & goal
- **Audience:** their role and expertise (end user, integrator, operator, new teammate,
  decision-maker). Pitch vocabulary and assumed knowledge to them; don't write for yourself.
- **Goal / job to be done:** what the reader must be able to do or decide after reading.
  Every section should serve that; if it doesn't, cut it.
- **Context of use:** are they learning calmly, or fixing an outage at 3am? That changes
  tone, length, and structure entirely.

## 2. Choose the doc type (don't mix them)
Different goals need different docs — mixing them serves none well (`references/doc-types.md`):
- **Tutorial** — learning-oriented, hand-held, guaranteed to succeed.
- **How-to guide** — task-oriented steps to a goal, for someone who knows the basics.
- **Reference** — information-oriented, complete and accurate lookup (API docs, config).
- **Explanation** — understanding-oriented background, the "why" and trade-offs.
- Plus: **README** (orientation + quick start), **ADR** (a decision + its context),
  **runbook** (operational recovery steps), **release notes** (what changed, for whom).

## 3. Outline first
- Structure before prose. Put the most important thing first (inverted pyramid); readers scan.
- Use descriptive headings that work as a table of contents and let readers jump.
- One idea per section; logical order (prerequisites → steps → verification → troubleshooting).

## 4. Draft for the reader
- **Clear and direct:** short sentences, active voice, second person ("you run…"), present tense.
  Plain words over jargon; define terms on first use; expand acronyms once. (`references/writing-style.md`)
- **Lead with the point**, then detail. State prerequisites up front. Say what success looks like.
- **Show, don't just tell:** concrete, complete, copy-pasteable examples; realistic (not `foo`)
  but safe (no real secrets). Show expected output. A good example beats a paragraph.
- Make it **scannable:** headings, short paragraphs, lists for sequences/options, tables for
  comparisons, code blocks for code. Call out warnings and prerequisites visibly.

## 5. Verify everything
- **Run every command and code sample** exactly as written, in a clean context — the
  single biggest source of doc pain is examples that don't work.
- Check version accuracy, flags, paths, and that the described behavior matches reality.
- No guessing: if you're unsure a claim is true, verify it or mark it clearly, don't assert it.

## 6. Edit & test on a real reader
- Cut ruthlessly: remove filler, redundancy, and anything not serving the goal. Shorter is kinder.
- Read it as the target reader; better, have someone in that audience follow it and note where
  they get stuck — fix those spots.
- Check links, cross-references, and that terminology is consistent throughout.

## Operating principles
- **Reader and goal first.** The best doc is the one that gets *this* reader to *their* outcome.
- **Accurate above all.** A wrong doc is worse than none — it destroys trust and wastes time.
- **Examples that run.** Tested, complete, realistic, safe.
- **Scannable, not a wall.** Structure, headings, and lists; most important first.
- **One doc type per document.** Don't blend tutorial, reference, and explanation.
- **Docs live with the code.** Update them with changes; stale docs mislead. Treat as product.

## Related skills
- **software-engineering** — the system being documented; code comments and docstrings.
- **ui-ux-design** — UX writing/microcopy in the product itself (pair with `/design ux-copy`).
- **devops-infra** — runbooks and operational docs for the systems you operate.
- **ai-engineering** — writing prompts and system instructions is adjacent technical writing.
