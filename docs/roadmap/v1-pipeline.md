# v1, The Pipeline

The first phase. Build the honest four-hop RAG pipeline, end to end, over a small document set. The goal is
to learn how every hop works by building it, not to be clever. Small in lines, honest in shape.

## Collaboration mode

v1 is learn-by-building. Sameer writes the pipeline by hand. The assistant teaches the concepts, scaffolds,
asks Socratic questions, and reviews the code, but does not write the implementation. The
[knowledge course](../../knowledge) teaches the ideas; this phase turns them into working code.

## What we are building

A command-line or small-service RAG pipeline that answers a question from a folder of documents:

1. **Ingest and chunk.** Read a set of documents, split them into passages (the chunking tradeoff is real:
   too big loses precision, too small loses context).
2. **Embed.** Turn each passage, and the incoming question, into a vector with a sentence-embedding model.
3. **Index and search.** Store the passage vectors in monkrag's own index and find the top-k nearest to the
   question by cosine similarity. Brute-force search is fine here; it is correct and easy to reason about.
4. **Augment.** Assemble the prompt: the question plus the retrieved passages as context, within the model's
   token budget.
5. **Generate.** Call a language model and return a grounded answer that cites the passages it used.

## Milestones

1. **Chunker.** Split documents into passages with a chosen size and overlap. Keep the source and offset so
   citations can point back.
2. **Embedder.** Wrap a sentence-embedding model behind one small function: text in, vector out. Same
   function for passages and for the query.
3. **Index + search.** Store vectors, return the top-k by cosine similarity. A flat brute-force scan.
4. **Prompt builder.** Compose the context-stuffed prompt; respect a token budget; order the passages.
5. **Generator.** Call the LLM backend, return the answer plus the citations.
6. **End-to-end.** One entry point: question in, grounded answer with citations out, over a real document
   set.

## Stack

- **Python.** A sentence-embedding model for the vectors, monkrag's own flat vector index for search, and a
  pluggable LLM backend for generation (so the model can be swapped without touching the pipeline).
- Self-contained: no managed vector database, no framework that hides the hops.

## Definition of done

- The pipeline answers questions over a sample document set, end to end, with citations that point to the
  right passages.
- Tests cover each hop: the chunker splits as expected, the embedder is deterministic for the same input,
  search returns the known-nearest passages on a tiny fixture, and the prompt builder respects the budget.
- A README section shows a real question, the retrieved passages, and the grounded answer.
- An honest note on what v1 does not do yet (no reranking, no cache, no latency budget), which sets up v2.

## What v1 deliberately leaves for v2

Reranking, the embedding cache, and the measured latency and cost budget. v1 proves the pipeline is
correct; v2 proves it is fast and that each upgrade earns its cost.
