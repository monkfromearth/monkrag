# monkrag Roadmap

The plan for building monkrag, in phases. Each phase has its own file with milestones, a definition of
done, and how we work on it. This index is the map; the phase files are the detail.

## The shape of the project

monkrag is built in two stages, beginner-first: build the honest, readable pipeline to learn how every hop
works, then climb to the version that is measured and genuinely senior.

| Phase | What it is | Status |
|-------|-----------|--------|
| [v1, The Pipeline](./v1-pipeline.md) | Embed a question, search for the nearest passages, stuff them into the prompt, and generate a grounded answer over a small document set. Small in lines, honest in shape. | Planned |
| [v2, The Budget](./v2-budget.md) | A cross-encoder reranker, an embedding cache, and a measured latency and cost budget across the four hops. The headline artifact. | Planned |

## What it is, and is not

monkrag is a self-contained RAG serving system: its own vector index, its own pipeline, its own benchmark
harness. It is not built on any other project's components. The vector index here is written for monkrag and
is unrelated to any separate vector-search work.

## The four hops

Every question monkrag answers runs the same four hops, and the whole point of v2 is to measure where the
time and money go across them:

1. **Embed.** Turn the question into a vector (a list of numbers that captures its meaning).
2. **Search.** Find the passages whose vectors sit closest in meaning (nearest-neighbor search), and return
   the top few.
3. **Augment.** Build the prompt: paste the retrieved passages in as context, within the token budget.
4. **Generate.** Call the language model, which now answers grounded in that context, with citations.

v2 adds a fifth concern that wraps the four: a smarter reranking pass between search and augment, an
embedding cache in front of embed, and a budget that holds the end-to-end p99 latency in check.

## Principles

1. **Beginner-first, then advanced.** v1 is the textbook pipeline, built to learn each hop. v2 is the
   measured, senior angle layered on top. Build v1 first.
2. **Concepts are taught; code is written by hand.** The [knowledge course](../../knowledge) teaches every
   concept (see [AUTHORING.md](../../knowledge/AUTHORING.md) for how it is written). The implementation is
   written by Sameer, by hand, which is where the understanding sticks. See each phase's collaboration mode.
3. **Measured, not just wired.** A RAG pipeline alone reads as glue. The senior artifact is the measurement:
   a reproducible latency and cost budget, with a written verdict on whether each upgrade earns its cost,
   against a baseline.
4. **No hallucinated facts.** Exact model names, context-window sizes, and benchmark numbers are verified
   against primary sources before they reach the README, docs, or the course. Illustrative numbers in a
   demo are labeled as such.

## Status

Early. The brand and the interactive course are live. The serving code begins with v1.
