# System README template

## Goal
One line: what does this version do?

## Architecture
Diagram + components + data flow + error handling.

## How to run
Commands only, no prose.

## Stress plan
- Load tool + workload (rps, concurrency, duration)
- Chaos cases (kill, delay, duplicate, flood)
- Oracle (correctness + p50/p99 thresholds)
- Who wrote what (LLM vs human)

## Prediction
"I think it breaks at ___ on ___ because ___."

## Breaking point
Numbers + logs/metrics. What broke exactly, where in code/infra.

## Fix + trade-offs
What fixed it, what it cost (latency, complexity, $).

## Next break to chase
Single concrete hypothesis for next version.
