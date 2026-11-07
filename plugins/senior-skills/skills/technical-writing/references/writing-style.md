# Writing Style for Technical Docs

Clarity is the whole job. Write so the reader understands on the first pass.

## Clarity & concision
- **Short sentences, one idea each.** Break up long ones. Cut filler ("in order to" → "to",
  "it should be noted that" → delete).
- **Active voice, present tense, second person:** "You create a key," not "A key is created."
  It's shorter and says who does what.
- **Plain words over jargon.** Use the simplest word that's accurate. Define necessary terms
  on first use; expand each acronym once. Don't show off vocabulary.
- **Be specific and concrete.** "Returns within 200ms" beats "is fast." Name the exact file,
  flag, and value.
- **Say it once.** Remove redundancy. If two sentences say the same thing, keep the clearer one.

## Lead with the point
- Inverted pyramid: conclusion/most-important first, then supporting detail. Readers may stop early.
- Put prerequisites and "what you'll end up with" at the top of any procedure.
- Topic sentence first in each paragraph; the rest supports it.

## Structure & scannability
- Descriptive headings that stand alone and double as a map. Keep a sensible hierarchy.
- Short paragraphs (2–4 sentences). White space helps.
- **Lists** for sequences (numbered) and sets of options (bulleted). **Tables** for comparisons
  and option/parameter grids. **Code blocks** for anything the reader types or sees returned.
- Visible callouts for warnings, prerequisites, and gotchas — don't bury a data-loss warning mid-paragraph.

## Examples
- Concrete, **complete, and copy-pasteable** — the reader can run it as-is.
- **Realistic but safe:** meaningful names over `foo`/`bar`; never real secrets, tokens, or
  personal data — use obvious placeholders (`YOUR_API_KEY`).
- Show **expected output** so the reader knows it worked.
- Prefer a working example over a paragraph of description; then explain the interesting parts.

## Procedures
- Number the steps; one action per step; put the reason only where it's non-obvious.
- State the starting state, each action, and how to verify success. Include troubleshooting
  for the common failure, and how to undo.

## Consistency
- One term per concept throughout (don't alternate "user"/"account"/"profile" for the same thing).
- Consistent formatting for code, UI labels, commands, and placeholders. Follow a style guide
  if one exists. Consistent voice and heading style.

## Inclusive & accessible
- Neutral, respectful language; avoid idioms that don't translate and unexplained cultural
  references. Use they/them when a person's pronouns are unknown.
- Don't rely on "simply/just/obviously" — it alienates a stuck reader. Describe links by
  destination ("see the install guide"), not "click here." Add alt text for images/diagrams.

## Self-edit checklist
- Can a reader in the target audience do the task after one read?
- Does every command/example actually run? Is every claim true and current?
- Is anything repeated or unnecessary? Cut it.
- Most important point first? Headings scannable? Warnings visible?
- Terminology consistent? Links valid?
