<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./public/logo-wordmark-dark.svg" />
    <img src="./public/logo-wordmark.svg" alt="monkrag" width="420" />
  </picture>

  <p><strong>A RAG serving system built from scratch.</strong></p>

  <p>Ask a question, find the passages that answer it, and let the model reply from those sources instead of guessing.</p>

  <p>
    <a href="https://monkfromearth.github.io/monkrag/learn">Course</a> &middot;
    <a href="https://monkfromearth.github.io/monkrag/docs">Docs</a> &middot;
    <a href="./docs/roadmap/index.md">Roadmap</a>
  </p>
</div>

---

## What this is

monkrag is a retrieval-augmented generation (RAG) serving system built from the ground up. The idea is
simple. Instead of asking a language model to answer from memory, where it tends to make things up, you
first find the passages that actually answer the question and let the model reply from those. monkrag is
the system that does it, end to end: it turns a question into a vector, searches for the closest passages
in meaning, builds a prompt around them, and generates a grounded answer with citations.

It is built in two stages. v1 is the honest four-hop pipeline. v2 is the headline: a reproducible latency
and cost budget that says where every millisecond and dollar goes.

## What's different

- **Answers from sources.** Grounding is the whole point. The model replies from passages you retrieved,
  with citations, not from its own memory.
- **Taught, not just shipped.** A full interactive course ships with the code. It teaches the concepts
  (embeddings, retrieval, grounding, reranking, the latency budget) and leaves the implementation to you,
  which is where the understanding sticks. It deliberately never hands you monkrag's own source.
- **Measured, not just wired.** Most RAG repos are an unmeasured pipeline. v1 is the honest pipeline. v2 is
  the real work: a reproducible budget that answers where the milliseconds and dollars go, and whether
  reranking earns its cost, all against a naive baseline.
- **Built to a senior bar.** Reproducible benchmarks, an honest writeup of the tradeoffs, and real docs,
  not a notebook that runs once.

## The two stages

| Stage | What it is | Status |
| ----- | ---------- | ------ |
| **v1, The Pipeline** | Embed a question, search for the nearest passages, stuff them into the prompt, and generate a grounded answer over a small document set. Small in lines, honest in shape. | Planned |
| **v2, The Budget** | A cross-encoder reranker, an embedding cache, and a measured latency and cost budget (p99 across the four hops, plus the "does rerank earn its cost" verdict). | Planned |

Full detail: [docs/roadmap/](./docs/roadmap/index.md).

## Tech stack

| Part | Stack |
| ---- | ----- |
| **Pipeline (v1)** | Python. A sentence-embedding model for queries and passages, monkrag's own vector index for the search, and a pluggable LLM backend for generation. Self-contained, no managed vector database. |
| **Budget (v2)** | A cross-encoder reranker, an embedding cache, and a benchmark harness that measures p99 latency and cost across embed, search, rerank, and generate. |
| **Knowledge course** | [Astro](https://astro.build) + [Tailwind CSS](https://tailwindcss.com) + [GSAP](https://gsap.com) for the demos, built with [bun](https://bun.sh). Deployed to GitHub Pages. |

## Learn it: the course

The [`knowledge/`](./knowledge) folder is a full interactive course, 23 lessons across 7 units with
animated demos, that teaches the concepts behind monkrag. It is live at
**[monkfromearth.github.io/monkrag](https://monkfromearth.github.io/monkrag/)**.

Run it locally:

```bash
cd knowledge
bun install
bun run dev      # http://localhost:4321/monkrag/
```

Authoring the course follows [`knowledge/AUTHORING.md`](./knowledge/AUTHORING.md) (voice, structure, and the
animation policy).

## Repository layout

```
monkrag/
├── knowledge/        the interactive course (Astro site), concepts not implementation
├── docs/roadmap/     phased plan: index + per-phase files
├── public/           brand assets (logo, wordmark, light and dark)
├── README.md
├── LICENSE
└── CLAUDE.md         the boot file for a new working session
```

The serving code (`src/`, tests, benchmarks) arrives with v1. The course and the plan come first so the
ideas are clear before a line is written.

## Status

Early. The brand and the interactive course are live; the serving code begins next. Structure and docs
grow with the project.

## License

MIT (see [`LICENSE`](./LICENSE)).

---

<div align="center">
  <sub>Built by <a href="https://monkfrom.earth">Sameer Khan (@monkfromearth)</a> &middot; part of the <a href="https://github.com/monkfromearth">monk</a> family &middot; <a href="https://github.com/monkfromearth/monkrag">source</a></sub>
</div>
