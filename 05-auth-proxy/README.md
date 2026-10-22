# 05 — Auth Proxy: design, scale, break

## Why this one
A proxy that sits in front of any HTTP service and handles JWTs for machines, sessions for humans, and rate limits per plan (a "Basic" user gets 1000 calls a month). Every request goes through it, so its latency and its failure mode belong to every service behind it. Do 02 first, since v3 builds on that limiter.

## Prep
- Arpit Bhayani's System Design course: 17 (Load Balancers), 18 (Circuit Breakers), 27 (Designing API Rate Limiter).
- Idea source: [small dev project ideas](https://x.com/Hi_Mrinal/status/2021671651432432032), project #0.

## Roadmap
- [ ] v0 — reverse proxy that checks JWTs signed with one static key. Expect: extra hop latency, no way to revoke a token, rotating the key logs everyone out.
- [ ] v1 — sessions for humans in a store + API keys for machines + a revocation list. Expect: the session store becomes a dependency of every request.
- [ ] v2 — cache auth decisions + JWKS key rotation. Expect: revoked tokens still accepted until the cache entry expires, stampede on the key endpoint after a rotation.
- [ ] v3 — monthly per-plan quotas (on 02) + circuit breaker to the upstream. Expect: quota counters drifting across nodes, fail-open vs fail-closed when the store is down, retries making an upstream outage worse.
- [ ] v4 — multiple proxy nodes behind a load balancer + observability. Expect: added p99 per request, upstream connection pool limits, cost.

Tag per version: `authproxy-v0`, `authproxy-v1`... One PR per version.

## Current status
Not started, waits on 02. Open questions for v0 grilling:
1. Which upstream does it protect (one of 01-04)?
2. Who issues tokens: the proxy itself or an external identity provider?
3. Latency budget for the extra hop?

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
