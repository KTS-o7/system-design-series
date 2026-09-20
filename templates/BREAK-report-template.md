# Break report vX — <title>

Use this report when a version has a measured break. State the workload first, then what broke, then what fixed it.

## Workload
Tool, rps, concurrency, duration, dataset.

## Prediction
"I thought it would break at ___ because ___."

## What broke exactly
Numbers (p50/p99, errors), logs/metrics excerpt, file:line. Do not summarize without numbers.

## What we tried first
What obvious fix failed and why. A gate that fails on everything does not help; a rewrite that diverges does not land.

## Fix
Smallest change that held under the same workload. Commands + code path.

## Trade-offs + cost
Latency, complexity, $ impact. What is still open.

## Replay
Exact command to reproduce.
