---
name: ai-engineering
description: >-
  Build reliable LLM/AI-powered features with engineering discipline: prompt and
  context design, retrieval-augmented generation (RAG), tool/function calling and
  agents, evaluation and testing, and safety/guardrails. Use when the user is
  designing or improving an AI feature — prompting, a RAG pipeline, an agent or
  tool-using system, LLM evals, reducing hallucination/latency/cost, or AI safety
  and prompt-injection defense. Treats the LLM as one (non-deterministic)
  component in a well-engineered system. For the surrounding code use
  `software-engineering`; for provider/model specifics consult the `claude-api` skill.
---

# AI Engineering

Building features on large language models well. The LLM is a powerful but
**non-deterministic, fallible** component — it can be wrong, inconsistent, slow,
costly, and manipulable. Good AI engineering is mostly ordinary engineering around
that component: clear specs, retrieval of the right context, tools with guardrails,
**evaluation**, and defense against misuse. Measure, don't vibe.

## Core loop

1. **Define the task & success** — the exact job, inputs/outputs, and how you'll measure "good."
2. **Start simple** — a clear prompt with good context; only add complexity (RAG, tools,
   agents) when a measured gap requires it.
3. **Engineer context** — give the model the right information (retrieval, structured input).
4. **Add capability deliberately** — tools/function-calling and agent loops where needed, bounded.
5. **Evaluate** — build an eval set; measure quality, cost, latency, safety; iterate on numbers.
6. **Harden** — safety, prompt-injection defense, guardrails, fallbacks, and monitoring in prod.

References (load the matching one):
- `references/prompting-and-context.md` — prompt design, context engineering, structured output.
- `references/rag.md` — retrieval-augmented generation: chunking, embeddings, retrieval, grounding.
- `references/agents-and-tools.md` — tool/function calling, agent loops, orchestration, control.
- `references/evaluation.md` — evals, metrics, LLM-as-judge, regression testing.
- `references/ai-safety.md` — prompt injection, guardrails, abuse, privacy, responsible use.

## 1. Define the task & success
- State the job precisely: inputs, desired outputs (format, constraints), and what a
  **good vs. bad** answer looks like with examples. Vague specs produce unmeasurable systems.
- Decide the metrics up front (accuracy/quality, faithfulness, latency, cost per call,
  refusal/safety). You can't improve what you don't measure.
- Pick a model to fit the task (capability vs. latency/cost); keep model choice swappable.
  For current model ids/pricing/params consult the **`claude-api`** skill — don't guess.

## 2. Start simple, add only what's needed
- Begin with a well-crafted prompt and the right context. Many "we need an agent/fine-tune"
  problems are solved by a clearer prompt and better retrieval.
- Add RAG when the model needs knowledge it doesn't have; add tools when it must act or fetch
  live data; add multi-step agents only when the task genuinely needs planning/iteration.
  Each addition adds failure modes — justify it with a measured gap.

## 3. Engineer the context
- The output quality is bounded by the **input context**. Provide clear instructions, relevant
  retrieved facts, few-shot examples where they help, and a defined output schema.
- Manage the context window: include what's relevant, exclude noise, respect ordering and
  limits. See `references/prompting-and-context.md` and `references/rag.md`.
- Prefer **structured output** (JSON schema / tool-call formatting) when the result feeds code;
  validate it and handle parse failures.

## 4. Add capability deliberately
- **Tools/function-calling:** give the model well-described, narrowly-scoped tools; validate
  arguments; make side-effecting tools safe (confirmation, idempotency, least privilege).
- **Agents:** bounded loops with a termination condition, step limits, and observability.
  Keep humans in the loop for consequential actions. See `references/agents-and-tools.md`.

## 5. Evaluate (the heart of it)
- Build an **eval set** of representative and edge cases with expected outcomes. Measure every
  change against it — prompts, models, retrieval — so you improve on evidence, not vibes.
- Use appropriate graders: exact/structured checks where possible; LLM-as-judge for open-ended,
  with its own validation. Track quality, cost, latency, and safety together. Guard against
  regressions in CI. See `references/evaluation.md`.

## 6. Harden for production
- **Prompt injection is the defining risk:** treat all retrieved/tool/user content as untrusted
  data, never as instructions; isolate it; constrain what the model can do; never let model
  output trigger privileged actions without validation. See `references/ai-safety.md`.
- Guardrails (input/output filtering, allowed topics, PII handling), graceful fallbacks on
  model failure/timeout, rate/cost limits, caching, and monitoring (quality drift, cost, abuse).
- Be honest about limits: handle "I don't know," cite sources in RAG, don't present generated
  content as authoritative where correctness matters.

## Operating principles
- **The LLM is one component.** Engineer the system around its non-determinism and fallibility.
- **Measure everything.** Evals over anecdotes; track quality, cost, latency, safety.
- **Simplest thing that works.** Prompt → context/RAG → tools → agents, each only when needed.
- **Untrusted by default.** Retrieved, tool, and user content is data, not instructions
  (prompt-injection defense). Validate structured output and tool args.
- **Ground and cite.** Reduce hallucination with retrieval and verification; admit uncertainty.
- **Human in the loop for consequences.** Don't let a probabilistic system take irreversible
  action unchecked.

## Related skills
- **software-engineering** — the application wrapping the model; testing and review.
- **claude-api** — concrete model ids, pricing, parameters, tool-use and caching specifics.
- **security-review** — threat-model the AI feature (prompt injection, data exfiltration).
- **data-engineering** — the data and retrieval stores behind RAG.
