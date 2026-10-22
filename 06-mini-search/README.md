# 06 — Mini Search Engine: design, scale, break

## Why this one
Crawl a bounded set of pages, build an inverted index, rank results and answer queries. The breaks move between the write path (crawl, index), the read path (query) and freshness, which none of 01-05 have.

## Prep
- Arpit Bhayani's System Design course: 36 (Designing a Web Crawler), 23 (Bloom Filters), 8 (Sharding and Partitioning), 24 (Consistent Hashing).
- Idea source: [build-it list](https://x.com/RoninClips/status/2026181715094602099) ("A Mini Search Engine -> indexing > tutorials").

## Roadmap
- [ ] v0 — in-memory inverted index over a fixed corpus, BM25 ranking. Expect: index rebuilt on every restart, memory grows with the corpus.
- [ ] v1 — crawler (robots.txt, politeness delay, URL dedupe with a Bloom filter) + index on disk. Expect: crawler traps, near-duplicate pages, Bloom false positives silently skipping URLs.
- [ ] v2 — incremental indexing (segments + merge) + query cache. Expect: merge stalls, stale results, cache invalidation after a re-index.
- [ ] v3 — index sharded by document, scatter-gather queries. Expect: the slowest shard sets query latency, partial results when a shard is down.
- [ ] v4 — replicas + observability. Expect: tail latency, freshness vs cost.

Tag per version: `search-v0`, `search-v1`... One PR per version.

## Current status
Not started. Open questions for v0 grilling:
1. Which corpus, and how big?
2. Keyword queries only, or phrases too?
3. Build the index by hand or embed a library? (By hand teaches more; a library gets to the scaling breaks sooner.)

## Per-version log (append)
### v0
- ADR: `docs/adr/v0.md`
- Prediction: ...
- Breaking point: ... (link `docs/breaks/v0.md`)
- Fix + cost: ...

## Layout
- `app/` — service code
- `load/` — k6/Locust scripts (LLM-generated, human-verified)
- `chaos/` — kill, delay, duplicate, flood
- `evals/` — correctness oracles
- `docs/adr/` — 1 file per version
- `docs/breaks/` — evidence per break

## How to run
Nothing to run yet.
