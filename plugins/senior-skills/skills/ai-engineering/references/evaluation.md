# Evaluating LLM Systems

Evaluation is what turns prompt-tweaking into engineering. Because LLM output is
non-deterministic, you improve by measuring against a dataset, not by eyeballing a
couple of examples.

## Build an eval set
- Collect **representative** inputs (from real/expected usage) **and edge cases** (ambiguous,
  adversarial, empty, long, multilingual, the failure modes you've seen). Include the hard cases.
- Define the **expected outcome or acceptance criteria** for each — exact answer, required
  facts, format, or a rubric for open-ended tasks.
- Grow it over time: every production failure becomes a new eval case (regression test).
- Keep it versioned, and hold out a set you don't tune against to avoid over-fitting.

## Metrics — measure several together
- **Quality/correctness:** task-appropriate (accuracy, F1, pass/fail against criteria).
- **Faithfulness/grounding** (for RAG): is the answer supported by the provided context, with
  correct citations, and no fabrication? Evaluate retrieval and generation separately.
- **Format validity:** does structured output parse and conform to schema?
- **Safety:** refusal of disallowed requests, resistance to injection, no PII/leakage.
- **Operational:** latency and **cost per call** — a 2% quality gain at 5× cost may not be worth it.

## Grading methods
- **Deterministic/programmatic** where possible: exact match, schema validation, contains-fact,
  regex, numeric tolerance, unit-test-style assertions. Cheapest and most reliable — prefer it.
- **LLM-as-judge** for open-ended quality: give the judge a clear rubric and the reference;
  prefer pairwise comparison or scored rubrics over vague "rate 1–10." **Validate the judge**
  against human labels — it has its own biases (verbosity, position, self-preference).
- **Human review** for the highest-stakes or subjective calls, and to calibrate the above.

## Process
- **Baseline first**, then change one variable (prompt, model, retrieval, params) and re-run the
  full eval. Attribute improvements to the change. Watch for regressions elsewhere.
- **Put evals in CI** so prompt/model changes can't silently degrade quality before shipping.
- **Monitor in production:** sample real traffic, track quality signals, cost, latency, user
  feedback/thumbs, and **drift** over time (models and data change). Close the loop back into evals.

## Common pitfalls
- "It looked good on three examples" — no dataset, no numbers.
- Only measuring quality, ignoring cost/latency; or vice versa.
- Unvalidated LLM-judge taken as ground truth; vague 1–10 scoring.
- Tuning on the whole set (over-fit) with no held-out data.
- No production monitoring → silent quality drift after launch.
- Not separating retrieval vs. generation failures in RAG.
