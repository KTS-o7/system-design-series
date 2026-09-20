# NN — <system name>: design, scale, break

Copy this folder for each new system: `cp -r 00-template-project 02-<name>` then fill in.

## Why this one
One paragraph: why this system teaches scaling.

## Roadmap
- [ ] v0 — minimal. Expect: <first obvious break>
- [ ] v1 — + persistence. Expect: <pool / contention>
- [ ] v2 — + cache. Expect: <stampede>
- [ ] v3 — + limits / resilience. Expect: <abuse / cascade>
- [ ] v4 — horizontal + observability. Expect: <tail latency / cost>

Tag per version: `<name>-v0`, `<name>-v1`... One PR per version.

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
Commands only.
