# Production run: verified completion + cache efficiency

**Date:** 2026-09-07, 14:26–14:27 UTC
**Agent:** Vetta (one of two active agent instances this lab draws evidence
from; runs on separate infrastructure from the "Ember" instance used in
other case studies)
**Decision:** PASS, with one metric explicitly unavailable — labeled as
such, not shown as zero

## Problem / test

A production task was run end-to-end and checked against 3 independent
factual claims in its output — not just "did the agent respond fluently,"
but "does the resulting output actually match reality." This case study
reports the full resource cost of that run, not just the pass/fail.

## Measured

All figures below were independently re-derived from the runtime's own
structured per-call telemetry for this run (not taken on faith from a
summary) — parsed directly from the raw event stream, matched call-by-call.

| Call | Prompt tokens | Cache hit | Cache miss | Output tokens |
|---|---|---|---|---|
| 1 | 21,917 | 14,848 | 7,069 | 1,332 |
| 2 | 34,702 | 23,168 | 11,534 | 2,467 |
| 3 | 37,511 | 37,120 | 391 | 628 |
| 4 | 38,300 | 38,016 | 284 | 1,119 |

Every value in this table matches the runtime's per-call telemetry exactly.

**Run-level rollup:**

- Model/API calls: **4**
- Tool calls: **4**, 0 failed, 0 flagged as blocked/duplicate
- Provider retries: **0** (every call telemetry-tagged `attempt: 1`)
- Total prompt tokens: **132,430** (113,152 cache-hit + 19,278 cache-miss)
- Output tokens: **5,546**
- Aggregate prompt-cache reuse: **85.44%** (113,152 / 132,430)
- Final two calls: **~99% cache reuse** each (98.96%, 99.26%)
- Wall-clock: **46.07s measured** (turn-start to turn-completion, from the
  runtime's own timestamps — not the operator's stopwatch figure, which was
  ~1s higher, likely measured across a slightly wider window)
- Provider cost: **unavailable** — the model slug in use wasn't resolved in
  the cost-estimation table for this run. This is reported as unavailable,
  not as $0. A run costing nothing and a run whose cost nobody could
  compute are different facts, and conflating them would misrepresent this
  result.
- Factual verification: **3/3 checked claims correct**, as reported by the
  operator for this run. This specific sub-claim is operator-reported
  rather than independently re-derived by me from raw output content — I
  did not read the run's task content to re-check it myself, since doing so
  isn't necessary to verify the resource-usage numbers above and keeps this
  disclosure minimal. It's consistent with the rest of the telemetry: a
  4-call, 4-tool-call run that ended via a normal stop condition, not a
  forced cutoff.

## What this shows

- A model call sequence where the **majority of context weight shifted
  toward cache-hit tokens as the run progressed** (miss share: 32%, 33%,
  1%, 0.7% across the 4 calls) — later calls in the run were served almost
  entirely from cache.
- Zero retries, zero failed tool calls, zero blocked/duplicate tool
  calls in this run — a clean execution, not one padded by recovery
  overhead.
- A concrete example of the "unavailable ≠ free" distinction this lab
  insists on: this run's provider cost literally could not be computed at
  the time it ran, and it's reported that way.

## Limitation

This is a single run, not a distribution. It demonstrates that this
cache-reuse pattern and this level of clean execution are *achievable* in
production, not that they're the average outcome. See
[tool-call-redundancy-guard-coverage.md](tool-call-redundancy-guard-coverage.md)
for a much larger-sample (864-call) look at where redundant work does and
doesn't get caught, on the Ember instance, including the parts that don't
look this clean.

No comparable, independently verifiable same-task run exists yet on the
Ember instance to responsibly turn this into an Ember-vs-Vetta comparison —
that would need a controlled, matched run, not a retrospective pairing.
