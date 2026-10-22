# 04 — Realtime Chat: design, scale, break

## Why this one
Long-lived connections break what request/response taught: memory per connection, fan-out to a whole room, reconnects, ordering and presence. One slow client can hold up everyone else in the room.

## Prep
- Arpit Bhayani's System Design course: 16 (Realtime Pubsub), 21 (Client server model and communication protocols), 17 (Load Balancers, for v4).
- Idea source: [freeCodeCamp handbook](https://www.freecodecamp.org/news/how-to-build-a-production-grade-distributed-chatroom-in-go-full-handbook/) on a distributed chat server in Go with persistence and reconnection. Read it after the v0 ADR, not before.

## Roadmap
- [ ] v0 — one server, WebSocket, rooms in memory. Expect: messages lost on restart, memory per connection, one slow client stalling a room broadcast.
- [ ] v1 — persist messages, history on reconnect with a cursor. Expect: gaps and duplicates around reconnect, write load in busy rooms.
- [ ] v2 — 2+ servers with Redis pub/sub fan-out. Expect: messages missed while a node is disconnected (Redis pub/sub keeps nothing), ordering across nodes.
- [ ] v3 — backpressure, presence, per-user rate limit (reuse 02). Expect: presence update storms, reconnect storm after a deploy.
- [ ] v4 — horizontal behind a load balancer + observability. Expect: uneven connection spread, fan-out tail latency in big rooms, cost per connection.

Tag per version: `chat-v0`, `chat-v1`... One PR per version.

## Current status
Not started. Open questions for v0 grilling:
1. Room sizes: 1:1, small groups or large channels?
2. Delivery and ordering guarantee per room?
3. How long is history kept?

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
