# System Design Series

Learn systems by designing, scaling, and breaking them on purpose.

## Operating contract
- **You decide, from first principles.** Each version starts with QA grilling (scale, read/write, latency, durability). No code until ADR is locked.
- **Pen-paper friendly.** Sketch first, then lock 1-paragraph ADR in `docs/adr/`.
- **AI is the code bottleneck relief.** I scaffold code + load/chaos harnesses, open a PR. You review everything on GitHub with comments.
- **Predict the break.** Before running stress, you write: "I think it breaks at X on Y because Z." README records actual vs predicted.
- **LLMs write the breaking tools.** k6/Locust load, fuzz/adversarial inputs, chaos (kill, delay, duplicate, hot-key flood). Humans judge if the break matters.

## Repo shape — locked as Option A
- Root `README.md` (this file) = global tracker.
- `NN-name/` per system = self-contained lab.
- Each system: `README.md` + `app/` + `load/` + `chaos/` + `evals/` + `docs/adr/` + `docs/breaks/`.
- Versions are git tags (`shortener-v0`, `shortener-v1`...), NOT subfolders. One PR per version, reviewed on GitHub.

## Systems
| # | System | Folder | Status | Current break to chase | Tags | Blog |
|---|--------|--------|--------|------------------------|------|------|
| 00 | Template (copy me) | [00-template-project](./00-template-project/) | stable | — | — | — |
| 01 | URL Shortener | [01-url-shortener](./01-url-shortener/) | v0 queued — QA next | v0: restart loss + races | `shortener-v0` queued | — (add portfolio `/blog/...` link when published) |
| 02 | Rate Limiter | [02-rate-limiter](./02-rate-limiter/) | planned | v0: 2x burst at window edge + restart loss | — | — |
| 03 | Job Queue | [03-job-queue](./03-job-queue/) | planned | v0: crash loses queued jobs | — | — |
| 04 | Realtime Chat | [04-realtime-chat](./04-realtime-chat/) | planned | v0: restart loss + slow client stalls a room | — | — |
| 05 | Auth Proxy | [05-auth-proxy](./05-auth-proxy/) | planned, needs 02 | v0: no revocation, key rotation logs everyone out | — | — |
| 06 | Mini Search Engine | [06-mini-search](./06-mini-search/) | planned | v0: index rebuilt on restart, memory grows with corpus | — | — |

Order after 01 is a suggestion. 05 v3 and 04 v3 reuse the 02 limiter, so do 02 before them.

## Per-version loop (same every time)
1. QA grill → ADR
2. Scaffold PR → GitHub review → merge
3. LLM stress/chaos → run → capture metrics
4. README: setup → workload → what broke exactly → fix → trade-offs/cost → next break
5. Blog summarizes break, links repo tag (`v0`, `v1`...)

## Templates
See `templates/SYSTEM-README-template.md` for the per-system README skeleton.
