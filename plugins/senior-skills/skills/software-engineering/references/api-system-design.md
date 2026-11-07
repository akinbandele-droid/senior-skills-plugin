# API & System Design

Design interfaces and systems that are correct, evolvable, and hard to misuse.
Decisions here are expensive to reverse — think before committing, but match the
weight of the design to the stakes.

## Start from requirements
- **Functional:** what must it do? Concrete use cases and the data involved.
- **Non-functional:** expected load (RPS, data size, growth), latency targets,
  availability, consistency needs, security/compliance, cost.
- **Constraints:** existing systems, team, timeline, operational maturity.
Write these down; they drive every trade-off that follows.

## API design
- **Design for the caller.** The interface should make the common case easy and
  the wrong thing hard. Model it on real usage, not internal structure.
- **Clear contracts:** explicit inputs/outputs, types, units, required vs.
  optional, and defined error semantics. Document what each error means.
- **Consistency:** naming, pagination, filtering, auth, and error shape should be
  uniform across endpoints/methods. Follow the platform's conventions (REST/HTTP
  semantics, gRPC, GraphQL) rather than inventing your own.
- **Evolvability:** version deliberately; add fields without breaking callers;
  avoid leaking internal details that lock you in. Prefer additive changes.
- **Idempotency** for retried operations; **pagination** for lists; **limits**
  and validation on all inputs.
- **Least privilege & authz** at the boundary; never trust the client.

## System design
- **Decompose by responsibility and change rate.** Keep boundaries where data
  and logic naturally cohere; minimize chatty cross-boundary calls.
- **Data first:** the data model and access patterns usually dictate the
  architecture. Choose storage to fit the access pattern (read/write ratio,
  query shape, consistency), not by fashion.
- **State & consistency:** be explicit about what's strongly vs. eventually
  consistent. Prefer stateless services; isolate state in stores you can reason about.
- **Failure is normal:** design for partial failure — timeouts, retries with
  backoff, idempotency, circuit breakers, graceful degradation. Define what
  happens when each dependency is down.
- **Scaling:** scale horizontally where possible; find the bottleneck (usually a
  shared store) before adding layers. Cache deliberately, with an invalidation story.
- **Observability from day one:** logs, metrics, traces, and health checks, so
  you can debug it in production.

## Trade-offs & judgment
- Make the trade-offs explicit: consistency vs. availability, latency vs. cost,
  simplicity vs. flexibility. Pick and state why.
- **Favor the simplest design that meets the real requirements.** Don't build for
  imagined scale; do leave clear seams to grow.
- Avoid one-way doors without need — prefer reversible decisions and keep
  options open where the future is uncertain.
- Record key decisions and their rationale (a short ADR) so the next engineer
  understands *why*, not just *what*.

## Review a design against
Correctness, failure modes, security, operability, cost, and whether it can
evolve without a rewrite. If you can't explain how it fails and recovers, it's
not done.
