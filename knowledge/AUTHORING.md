# Authoring the monkrag knowledge course

How every unit and lesson in `knowledge/` is written. Read this before adding or editing a lesson.
The point of this file is that the course stays one voice and one set of decisions no matter who
(or which session) writes the next unit.

---

## 0. The stance (decided)

**monkrag is a TEACHING COURSE.** It teaches the *concepts* a reader needs to build a RAG serving
system themselves: how text becomes vectors, how nearest-neighbor search finds the right passages, how
the prompt gets assembled, how the model answers from sources, and how the whole thing holds together
under a latency and cost budget. It deliberately never ships monkrag's own implementation. The reader
writes the serving code; the course makes the ideas click. (Same mode as the sibling project monktensor.)

## 1. The rule that overrides everything (teaching-course mode)

**This is knowledge, not documentation.** Teach the idea, the math, and worked examples. NEVER paste the
project's real implementation; the reader writes the code, which is where understanding sticks. The test:
"would copying this snippet build the thing for them?" If yes, cut it. (Skip this section if you chose
DOCUMENTATION mode.)

## 2. Voice

- **Plain words first, the precise term second.** Say the real-world meaning, then name the term once.
- **Calm, curious, smart-friend.** No hype, no emojis, ever.
- **Concrete over abstract.** Lead with an analogy or a worked number, not a definition.
- **Worked examples over proofs.** "Nudge it and watch the number move" beats a wall of symbols.
- **One technical term per idea, max.** Three pieces of jargon in a sentence means rewrite it.
- **No em-dashes** in prose (a comma, period, colon, or parentheses instead). They are an AI tell.

## 3. Lesson structure

Each lesson is one `.astro` page using these pieces, in this order:

1. Frontmatter: import `LessonLayout`, `LessonHeader`, `LessonNav`, and the boxes/demos it uses.
2. `<LessonLayout title lessonNumber section>` wrapping everything.
3. `<LessonHeader lessonNumber title description>` with 1-2 `<TagBadge>` (a topic tag + a read-time).
4. 3-6 `<section class="mb-14">` blocks. Rhythm per section: **hook/analogy, the concept, a worked
   example, an `InsightBox`** naming the takeaway.
5. Optional "What it is NOT" section to kill misconceptions; optional `<ExerciseBox>`.
6. A "Key takeaways" section: 3 numbered points.
7. `<LessonNav prevHref prevLabel nextHref nextLabel>` chaining to the neighbours.

Aim for a 8-12 minute read. Longer means split it.

## 4. Components (in `src/components/`)

`LessonLayout` (page shell) · `LessonHeader` · `LessonNav` · `InsightBox` (the "remember this" callout) ·
`ExerciseBox` (work-it-by-hand) · `Formula` (centered math, no KaTeX) · `StepNumber` · `TagBadge`.
Interactive demos live in `src/components/demos/`.

## 5. Visual constraints

- Palette is the `mt-*` Tailwind tokens only (warm beige default; swap the values in `tailwind.config.mjs`
  to your brand). No raw hex in pages.
- Fonts loaded in the layout. Code blocks dark, used sparingly.
- **No emojis anywhere.**
- Every internal link and asset path goes through `import.meta.env.BASE_URL` (the site is served under a
  base path on GitHub Pages). Hardcoding `/foo` breaks in production.
- Brand assets (logo, etc.) must live in **`knowledge/public/`** — Astro serves that dir, not the repo root.

## 6. Animation policy (the heart of this)

Motion has to *teach*, or it doesn't ship.

- **Metaphor first, not charts.** Find the image that makes the idea click (gears for a chain of rates, a
  cell that fires, water filling a bucket, a ball on a hill, a line that can't split two groups) and animate
  *that*. Reach for a plain chart only when the metaphor is the chart.
- **The test:** "does seeing it move explain the idea better than a sentence?" If no, use a sentence.
- **Direct manipulation, not sliders.** The reader grabs the actual thing on the figure and moves it; the
  relationship redraws live. A slider sitting below the figure is the weak fallback.
- **Discrete where it's a value.** Snap dragged *values* to clean steps (e.g. 0.5) so they land on round
  numbers. Keep *positioning* continuous (dragging a fit-line, a point on a curve, a ball on a surface).
- **One demo per lesson, max.** A lesson is prose with a single illuminating interaction.
- **Respect `prefers-reduced-motion`.** Show the final/static state and skip tweens; every demo must still
  make sense with motion off. Use the `REDUCED` flag from `src/lib/interact.ts`.
- **Self-contained.** Each demo is one file in `src/components/demos/`, owns its markup + client `<script>`
  (import the shared helpers from `../../lib/interact`), and is scoped via a root class + `querySelectorAll`
  so multiple instances don't collide. Animate transforms/opacity/text, not expensive layout properties.

## 7. Adding a lesson, checklist

1. File: `src/pages/<unit>/<slug>.astro`.
2. Follow §2 voice, §3 structure, §1 stance.
3. Wire `LessonNav` to both neighbours.
4. If it earns one, add a single meaningful demo per §6.
5. Update the course map `src/pages/learn.astro`: the lesson row, the unit count, the header total.
6. `bun run build` must pass before the lesson is "done."

## 8. The curriculum (the agreed plan)

Seven units, ~23 lessons. v1 is the honest four-hop pipeline; v2 is the headline: a measured latency
and cost budget. The course teaches the whole arc.

- **0 Why RAG** (3): what RAG is and the hallucination it fixes; why models make things up; the
  end-to-end pipeline as a map (embed, search, augment, generate).
- **1 Embeddings** (3): meaning as geometry; measuring closeness (cosine); chunking a document.
- **2 Retrieval** (4): the vector store; nearest-neighbor search (brute force to approximate); recall@k,
  the metric; hybrid retrieval (keyword plus vector). *Self-contained; monkrag's own index, unrelated to
  any other project.*
- **3 Augmentation** (3): stuffing the context; the token budget; lost in the middle (ordering).
- **4 Generation** (3): the grounded answer and citations; streaming; when it still hallucinates.
- **5 Reranking** (3): why first-pass retrieval is sloppy; bi-encoder vs cross-encoder (scout and judge);
  retrieve-then-rerank, quality vs latency.
- **6 Serving** (4, the headline): the latency budget (p99 across the four hops); the embedding cache;
  does rerank earn its cost (the measured verdict); the whole service under one budget.

Unit folders: `why-rag`, `embeddings`, `retrieval`, `augmentation`, `generation`, `reranking`, `serving`.
Glossary is `src/pages/glossary.astro`. Unit colors (course map + headers), in order: accent, lav, blue,
rose, green, amber, accent (Unit 6 returns to the brand coral, the headline bookend).

## 9. Metaphor playbook (what to animate, by unit)

The image that makes each idea click, animated with direct manipulation. One demo per lesson, only where
it earns one.

- **Grounding (0.1):** drag a document into the model's context; the answer flips from a confident guess
  to a grounded fact with a citation. Same model, the source changes the answer.
- **Semantic map (1.2):** drag a query point on a 2D map of meaning; the nearest passages light up and a
  closeness number updates. Meaning as distance.
- **Chunking (1.3):** drag a slider-free divider to split a page into cards; too-big loses precision,
  too-small loses context.
- **Recall@k (2.3):** drag k; watch how many of the truly-relevant passages you catch vs how much you
  drag in.
- **Lost in the middle (3.3):** reorder retrieved cards in the prompt; the model's attention favors the
  ends, so placement changes the answer.
- **Scout and judge (5.2):** the fast scout grabs 50 candidates cheaply; drag the cross-encoder's cutoff
  and watch precision climb as latency rises.
- **Latency relay (6.1):** a fixed time budget split across embed, search, rerank, generate; drag a
  stage's cost and watch the p99 bar blow the budget. The headline demo.

## 10. Facts and honesty

No hallucinated numbers. Model names, context-window sizes, benchmark figures, and paper claims are
verified against primary sources before they reach a lesson, or they are dropped. When a number is
illustrative (a made-up latency for a demo), say so; never present an invented benchmark as real.
