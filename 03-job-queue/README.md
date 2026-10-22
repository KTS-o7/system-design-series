# 03 — Job Queue: design, scale, break

## Why this one
Async work is where retries, duplicates, poison messages and ordering show up. A job that runs twice or never is a different kind of bug from a slow request, and the queue hides it until someone checks.

## Prep
- Arpit Bhayani's System Design course: 14 (Message Brokers and Queues), 15 (Message Stream and Kafka Essentials).
- Idea source: [build-it list](https://x.com/RoninClips/status/2026181715094602099) ("A Job Queue -> learn async like a grown engineer").
- Read before v3: [system design do-not list](https://x.com/0xlelouch_/status/2016885208826716192) (async jobs, workflow engines).

## Roadmap
- [ ] v0 — in-process queue + worker pool. Expect: queued jobs lost on crash, no retries, memory grows when producers outrun workers.
- [ ] v1 — Postgres table as the queue (`SELECT ... FOR UPDATE SKIP LOCKED`), retries with backoff, visibility timeout. Expect: a job runs twice when a worker dies mid-job, row-lock contention and table bloat at high throughput.
- [ ] v2 — idempotency keys + dead-letter table. Expect: poison jobs burning retries, retry storm after an outage.
- [ ] v3 — broker with consumer groups (Redis Streams or Kafka, decided in the ADR). Expect: consumer lag, pauses during rebalance, ordering only within a partition.
- [ ] v4 — horizontal workers + autoscaling + observability (queue depth, age of oldest job). Expect: thundering herd when workers come back, cost of idle workers.

Tag per version: `jobqueue-v0`, `jobqueue-v1`... One PR per version.

## Current status
Not started. Open questions for v0 grilling:
1. At-most-once or at-least-once delivery?
2. Job types and durations (ms, seconds, minutes)?
3. Is ordering needed, and are delayed/scheduled jobs in scope (cron email)?

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
