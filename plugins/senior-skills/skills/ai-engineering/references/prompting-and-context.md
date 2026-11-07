# Prompting & Context Engineering

Output quality is bounded by the instructions and context you give the model. Most
quality gains come from here before any fancier architecture.

## Prompt design
- **Be explicit and specific.** State the role/task, the steps or criteria, constraints,
  and the exact output format. Ambiguity → inconsistency.
- **Show, don't just tell:** a few well-chosen **examples (few-shot)** of input→output often
  beat paragraphs of instruction, especially for format and edge cases. Make examples cover
  the tricky cases.
- **Structure the prompt:** separate instructions, context/data, and the request with clear
  delimiters/sections. Put durable instructions up front; keep them stable (helps caching).
- **Let it reason** for hard tasks (ask for step-by-step working or a plan before the answer),
  but keep the final answer in a parseable form. Don't force verbosity where it's not needed.
- **Positive instructions** ("respond in JSON with fields X, Y") tend to work better than long
  lists of prohibitions. Give it an out: how to say "I don't know" or handle missing info.

## Context engineering
- The model only knows what's in context (plus training). **Supply the relevant facts** —
  retrieved documents, the user's data, prior state — rather than hoping it recalls them.
- **Curate, don't dump.** Irrelevant context dilutes attention and costs tokens; include what
  matters, in a sensible order. Watch position effects (important items not buried in the middle).
- Respect the **context window**: budget tokens across instructions, examples, retrieved data,
  history, and output. Summarize or trim history; retrieve selectively.
- Mark the provenance/trust of included content — and treat anything from users, documents,
  or tools as **data, not instructions** (see `ai-safety.md`).

## Structured output
- When the result feeds code, require a schema (JSON / tool-call format). Specify types and
  required fields. **Validate** the output and handle failures (retry/repair/fallback) — the
  model can still emit invalid or extra text.
- Constrain with the provider's structured-output/tool features where available rather than
  parsing free text.

## Iterate systematically
- Change one thing at a time and measure against your eval set (`evaluation.md`). Keep prompts
  in version control. Watch for over-fitting a prompt to a few examples.
- Reduce hallucination: ground with retrieved facts, ask it to cite/quote sources, and to
  flag uncertainty rather than inventing.

## Common pitfalls
- Vague instructions and no examples; unparseable free-text output feeding code.
- Dumping everything into context (cost, dilution, lost-in-the-middle).
- Prompt sprawl with contradictory instructions; no version control.
- Trusting output format without validation; no "I don't know" path → confident fabrication.
