# 01 — URL Shortener: design, scale, break

## Why this one first
Simple enough to finish, rich enough to break at every layer: races, persistence, pools, cache stampede, abuse, tail latency.

## Roadmap
- [ ] v0 — single process + in-memory map. Expect: restart loss, concurrent-write races.
- [ ] v1 — + Postgres (unique constraint, base62/counter). Expect: pool exhaustion, hot-key contention, duplicate-POST double-create.
- [ ] v2 — + Redis read-through. Expect: stampede / thundering herd on hot keys.
- [ ] v3 — + rate limit + bulkhead / circuit-breaker. Expect: abuse + slow-DB cascade.
- [ ] v4 — horizontal + observability. Expect: tail latency, cost trade-offs.

## Per-version README contract
Each tag (`v0`, `v1`...) must document:
- Architecture (diagram + components + data flow)
- How to run
- Stress plan (what LLM wrote, what workload)
- Prediction (your one-line break guess before running)
- Breaking point (numbers + logs/metrics, what broke exactly)
- Fix + trade-offs / cost
- Next break to chase

## Current status
v0 QA queued. Open questions for grilling:
1. Expected scale (req/s, reads vs writes)?
2. ID scheme (counter vs random base62 vs hash)? Collision + security trade-offs?
3. Latency budget + durability (ok to lose on restart for v0)?

## Layout
- `app/` — service code
- `load/` — k6/Locust scripts (LLM-generated, human-verified)
- `chaos/` — kill, delay, duplicate, flood experiments
- `docs/adr/` — one file per version decision
- `evals/` — correctness oracles when needed
