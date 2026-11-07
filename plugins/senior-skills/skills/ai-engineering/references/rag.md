# Retrieval-Augmented Generation (RAG)

Give the model knowledge it doesn't have by retrieving relevant content and grounding
the answer in it. RAG reduces hallucination and lets you use fresh/private data — but
its quality is capped by **retrieval quality**: garbage in, garbage out.

## Pipeline
1. **Ingest & chunk** source documents.
2. **Embed** chunks and index them (vector store; often alongside keyword/metadata index).
3. **Retrieve** the most relevant chunks for a query.
4. **Augment** the prompt with retrieved context (and provenance).
5. **Generate** an answer grounded in that context, ideally with citations.

## Chunking
- Split on **semantic boundaries** (sections, paragraphs) not arbitrary character counts;
  keep chunks coherent and self-contained. Size to balance precision (small) vs. enough
  context (large); overlap a little to avoid cutting mid-thought.
- Attach **metadata** (source, title, section, date, permissions) for filtering and citation.

## Retrieval quality (where most problems live)
- **Hybrid search** (semantic embeddings + keyword/BM25) usually beats either alone; add
  **metadata filters** (recency, source, access rights).
- **Re-rank** top candidates with a cross-encoder/reranker for precision.
- Tune `k` (how many chunks) and thresholds; too few misses facts, too many adds noise and cost.
- Handle "no good match": don't force an answer from irrelevant chunks — let the model say it
  lacks the information.
- Match the **embedding model** to your domain/language; keep query and document embeddings
  from the same model/version. Re-embed when you change models.

## Grounding & citations
- Instruct the model to answer **only from the provided context** and to **cite** which chunk
  supports each claim. This both reduces hallucination and makes answers verifiable.
- Show sources to the user. If context is insufficient, say so rather than guessing.

## Freshness & correctness
- Keep the index in sync with sources (incremental re-indexing on change); stale indexes give
  confidently wrong answers. Respect source **access control** in retrieval — never retrieve
  documents the requesting user isn't allowed to see (a real data-leak vector).

## Evaluate RAG specifically
- Measure **retrieval** (did we fetch the right chunks? recall/precision) separately from
  **generation** (faithfulness to context, answer correctness). A bad answer is often a
  retrieval failure, not a model failure — diagnose which. See `evaluation.md`.

## Common pitfalls
- Fixed-size chunking that splits ideas; no metadata; semantic-only retrieval missing exact terms.
- No reranking; wrong `k`; forcing answers from irrelevant context.
- Stale index; mismatched/old embeddings.
- **Ignoring per-user permissions in retrieval → leaking private docs.**
- Treating retrieved document text as instructions (injection) — it's untrusted data.
