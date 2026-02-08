# monkrag

A retrieval-augmented generation (RAG) serving system built from scratch: embed a question, search for the
passages that answer it, ground the model's reply in those sources, rerank, and hold the whole thing under
a latency and cost budget. Built in two stages, a plain pipeline (v1) then a measured budget (v2). This
file is the entry point for any new session working in this repo.

## Reading order for a new session

1. **This file**, what monkrag is and how we work on it.
2. **[docs/roadmap/index.md](./docs/roadmap/index.md)**, the phased plan; start with the current phase
   ([v1](./docs/roadmap/v1-pipeline.md)).
3. **[knowledge/](./knowledge)**, the course that teaches every concept the system is built on. If writing
   or editing course content, read **[knowledge/AUTHORING.md](./knowledge/AUTHORING.md)** first.

## Collaboration mode (the rule that overrides convenience)

- **v1 is learn-by-building.** The serving code is written by hand, by Sameer. The assistant is a teacher
  and reviewer: guide the theory and the stack, explain plain-English-first then the precise term, scaffold,
  ask Socratic questions, review code, unblock. **The assistant does not write the pipeline implementation
  in v1.**
- **The course teaches concepts, never the implementation.** `knowledge/` explains how retrieval and
  grounding work (the ideas, the math, worked examples) but never ships monkrag's own source. Knowledge, not
  documentation. See `knowledge/AUTHORING.md` rule 1.
- **v2 is pairing.** Once v1 works, the measurement-heavy v2 (reranking, cache, the latency and cost budget)
  is collaborative, code included.

## What monkrag is and is not

- It **is** a self-contained RAG system: its own vector index, its own pipeline, its own benchmark harness.
- It is **not** built on any other project's components. monkrag stands alone; the vector index here is
  written for monkrag and is unrelated to any separate vector-search work.

## The headline (why this is not just glue)

A RAG pipeline by itself reads as wiring. The senior artifact here is the **measurement**: v2 produces a
reproducible latency and cost budget. One p99 number to defend, broken down across the four hops, with a
written verdict on whether reranking and the embedding cache earn their cost, against a naive baseline. The
benchmark and the writeup are the point, not the line count.

## Quality bars

- **No hallucinated facts.** Verify model names, context-window sizes, and benchmark numbers against primary
  sources before they reach the README, docs, or course. Illustrative numbers in a demo are labeled as such.
- **Senior-repo bar.** A phase is done with tests, a reproducible benchmark that tells a story, a real
  README, and honest notes on what is left, not just "it runs."
- **Measurement is the deliverable in v2.** A latency or quality claim ships with the script that produced
  it and the baseline it beat.
- **No em-dashes** anywhere (README, docs, course, commits). Use commas, periods, colons, or parentheses.

## Working with the knowledge site

```bash
cd knowledge
bun install
bun run dev      # local dev at /monkrag/
bun run build    # static build into knowledge/dist (what Pages deploys)
```

The site is served under the `/monkrag/` base path. Brand assets live in `knowledge/public/` (Astro serves
that directory). The landing page links to the course (`/learn`) and the docs (`/docs`); the 23 lessons live
under the unit folders. Animations follow the policy in `knowledge/AUTHORING.md` (meaningful, interactive,
reduced-motion safe).

## Stack

- **Knowledge site:** Astro + Tailwind + GSAP, built with bun. Deployed to GitHub Pages.
- **Serving system (arrives with v1):** Python. A sentence-embedding model, monkrag's own vector index, and
  a pluggable LLM backend. v2 adds a cross-encoder reranker, an embedding cache, and a benchmark harness.
- **Brand:** `public/` holds the logo and the light and dark wordmarks; `knowledge/public/` mirrors them for
  the site.
