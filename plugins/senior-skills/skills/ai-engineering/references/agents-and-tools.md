# Tools, Function-Calling & Agents

Let the model act — fetch live data, call APIs, take steps — while keeping it bounded,
observable, and safe. Add this capability only when the task needs it.

## Tool / function calling
- **Describe tools well:** a clear name, a precise description of when to use it, and a typed
  parameter schema. The model picks and fills tools from these descriptions — vague ones cause
  misuse. Keep each tool narrow and single-purpose.
- **Validate arguments** before executing — the model can hallucinate or supply malformed/unsafe
  values. Enforce types, ranges, and allow-lists in your code, not just the schema.
- **Make side-effecting tools safe:** least privilege (the tool can only do its one job),
  idempotency where possible, and confirmation/human-approval for consequential or irreversible
  actions (payments, deletes, sends, external posts).
- Return clear, structured results and errors so the model can recover or report.

## Agent loops
- An agent = model + tools in a loop: observe → decide → act → observe. Powerful but
  error-compounding and non-deterministic.
- **Bound it:** a termination condition, a max-step/iteration limit, timeouts, and a cost ceiling
  so it can't loop forever or run away on spend.
- **Observe it:** log each step (thought, tool call, args, result) for debugging and audit. You
  can't operate what you can't see.
- **Keep it on rails:** give only the tools needed for the task; don't hand an agent broad or
  dangerous capabilities "just in case." Prefer the least autonomy that solves the problem.
- **Human in the loop** for consequential actions and when confidence is low.

## Orchestration patterns
- **Single prompt** → simplest; use when no live action/knowledge is needed.
- **Chain / workflow** → fixed sequence of steps (often more reliable and cheaper than a free
  agent when the steps are known).
- **Router** → classify then dispatch to a specialized prompt/tool.
- **Agent** → dynamic planning when steps aren't known in advance.
- **Multi-agent** → only when genuinely parallel/specialized; it multiplies cost and failure modes.
Prefer the most constrained pattern that works; don't reach for autonomous multi-agent by default.

## Safety (critical — see ai-safety.md)
- Tool results and fetched content are **untrusted data**, not instructions. A compromised or
  malicious document/page can try to hijack the agent ("prompt injection") into misusing tools.
  Isolate that content, constrain tool scope, and never let unvalidated model output trigger a
  privileged action.
- Enforce the real permission boundary in your code (what the user is allowed to do), independent
  of what the model decides.

## Common pitfalls
- Over-broad or poorly-described tools; unvalidated arguments executed directly.
- Unbounded agent loops (cost/latency/infinite loops); no step logging.
- Giving agents dangerous capabilities without confirmation or least privilege.
- Trusting tool/web content as instructions → injection-driven tool misuse.
- Using an autonomous agent where a fixed workflow would be more reliable and cheaper.
