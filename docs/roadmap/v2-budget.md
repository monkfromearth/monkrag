# v2, The Budget

The headline phase. v1 proved the pipeline is correct. v2 proves it is fast, and answers the question that
makes monkrag more than glue: where do the milliseconds and the dollars go, and does each upgrade earn its
cost? The deliverable is a reproducible latency and cost budget, with a written verdict, against a baseline.

## Collaboration mode

v2 is pairing. It is measurement-heavy and research-flavored, so the work is collaborative, code included.

## What we are building

Three upgrades to the v1 pipeline, each treated as an experiment that has to earn its place, plus the
harness that measures them:

1. **Reranking.** A second, smarter pass: fetch a wide set of candidates cheaply (the bi-encoder scout),
   then re-score the top of them with a cross-encoder (the careful judge) and keep the best few. Quality up,
   latency up. The experiment: how much recall does it buy, and at what added latency?
2. **Embedding cache.** Do not re-embed what has been seen. Cache query and passage embeddings; measure the
   hit rate and the latency saved on a realistic query stream, including hot queries.
3. **The latency and cost budget.** One p99 number to defend, broken down across embed, search, rerank, and
   generate. A benchmark harness that reports the breakdown and the dollar cost per query, and a written
   verdict on which upgrades are worth their cost.

## Milestones

1. **Benchmark harness.** A reproducible script: a fixed query set, a fixed document set, and a labeled set
   of relevant passages. Reports recall@k, p50 and p99 latency per hop, and cost per query.
2. **Baseline.** Run v1 through the harness. This is the number every v2 upgrade is measured against.
3. **Reranker.** Add the two-stage retrieve-then-rerank path. Measure the recall gained and the latency
   added. Decide where it is worth it.
4. **Cache.** Add the embedding cache. Measure the hit rate and the latency saved on a realistic stream.
5. **Budget writeup.** The artifact: the p99 breakdown across the four hops, the cost per query, and a
   verdict like "reranking buys X recall for Y added latency; here is when that is worth it," all
   reproducible from the harness.

## Definition of done

- The harness runs end to end and reproduces every number in the writeup from a single command.
- A latency budget chart or table shows where the milliseconds go across the four hops, at p50 and p99.
- A written verdict states, with numbers, whether reranking and the cache earn their cost, against the v1
  baseline.
- Honest negative results are kept: if an upgrade does not pay off on this workload, that is reported, not
  hidden.

## Why this is the senior artifact

Most RAG repositories are an unmeasured pipeline. The measurement is the gap. A reproducible budget, an
honest tradeoff verdict, and a baseline to beat are what make monkrag read as research-grade rather than a
tutorial.

## Honesty bar

Every latency or quality number ships with the script that produced it and the baseline it beat. No invented
benchmarks. Model names, context-window sizes, and pricing are verified against primary sources before they
appear in the writeup.
