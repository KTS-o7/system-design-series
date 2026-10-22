# 02 — Rate Limiter: design, scale, break

## Why this one
Every other system here needs one (01 v3, 04 v3 and 05 all reuse it). It's small, and the breaks are about time and shared counters: window edges, read-modify-write races, one hot key, clocks that disagree between nodes.

## Prep
- Arpit Bhayani's System Design course: 27 (Designing API Rate Limiter), 11-12 (caching, for the Redis version).
- Idea source: [build-it list](https://x.com/RoninClips/status/2026181715094602099) ("A Rate Limiter -> understand real backend control").

## Roadmap
- [ ] v0 — fixed-window counter in process memory, as middleware in front of a dummy endpoint. Expect: up to 2x the limit in a burst across a window edge, counters gone on restart.
- [ ] v1 — token bucket and sliding-window log on the same workload as v0. Expect: log memory grows with requests per key, burst size turns into a tuning fight.
- [ ] v2 — shared state in Redis behind 2 app instances (`INCR` + `EXPIRE` first, then a Lua script). Expect: read-modify-write race before the script, one extra Redis round trip per request, a fail-open vs fail-closed call when Redis is down.
- [ ] v3 — limits per IP, user and API key, plus hot-key protection. Expect: one abusive key hammering a single Redis key, local pre-checks drifting from the global count.
- [ ] v4 — horizontal + observability. Expect: clock skew between nodes, accuracy vs added latency, cost per million checks.

Tag per version: `ratelimiter-v0`, `ratelimiter-v1`... One PR per version.

## Current status
Not started. Open questions for v0 grilling:
1. Where does it run: in-app middleware, sidecar or gateway?
2. What does a blocked caller get (429 + `Retry-After`)?
3. How exact does the limit need to be, and is approximate fine?

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
