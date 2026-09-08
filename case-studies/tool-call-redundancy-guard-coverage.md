# Tool-call redundancy: what gets caught, what doesn't

**Window:** 2026-09-05 00:11 – 2026-09-07 15:19 UTC (~2.6 days), Ember
instance, 864 tool calls across 23 active turns
**Decision:** INCONCLUSIVE overall — real evidence of a working control
mechanism *and* a real, specific gap in its coverage. Reported as both,
not rounded off in either direction.

## Problem

A production agent runtime can generate real cost by calling the same tool
with the same arguments more than once inside a single turn. Whether that
actually happens, how often, and whether anything catches it, is an
empirical question — not something to assert from architecture diagrams.

## Method

Parsed the runtime's structured per-tool-call telemetry for this window and
grouped calls by (turn, arguments). Within each group of repeated calls,
classified everything after the first occurrence into one of three
buckets, using output size as a proxy for "was this actually redundant" —
a deliberately conservative approach, not a difference of exact content:

- **Blocked** — call returned instantly, no execution.
- **True redundant re-execution** — call ran again and returned
  output of *identical* size to a prior call in the same group.
- **Polling-like repeat** — call ran again with the *same* arguments but
  *different*-sized output each time (consistent with checking on
  something that's changing, not re-reading something static).

## Measured

- **89 of 864 tool calls (10.3%)** were blocked outright at zero cost —
  direct evidence the runtime has a working duplicate-suppression
  mechanism.
- Of **362** same-turn, same-argument repeat calls classified in this window:
  - **46** were blocked/prevented (0 cost)
  - **39** were true redundant re-execution — identical output returned
    again — costing **13.0 seconds** of tool time (**3.6%** of all
    measured tool execution time in the window)
  - **277** were polling-like repeats — same call, changing output —
    costing **235.2 seconds** (**65.5%** of tool time). This is *not*
    counted as waste in the strict sense: the output changed each time,
    meaning each call returned new information. It's a separate,
    softer inefficiency (repeatedly polling on a fixed interval instead of
    waiting on an event) worth its own investigation, not the same finding
    as true duplication.
- **Coverage is uneven by tool.** Read/search-style tools were almost
  entirely covered by the guard — the large majority of their repeat
  calls were blocked. One specific tool used for viewing longer reference
  content was not: several of its repeats executed again in full,
  including one document (~104,700 characters) fetched three times inside
  a 10-minute span.

## Result

The guard is real and it works for some call types. It has a measurable,
specific gap for others. The true "identical duplicate work" waste in this
window is small in absolute terms (3.6% of tool time) — smaller than an
initial pass at this data suggested, before separating true duplication
from polling. The polling-pattern share is much larger (65.5%) but is a
different problem with a different fix.

## Decision

**INCONCLUSIVE as a single verdict** — this case study exists specifically
to show the two outcomes at once rather than average them into a clean
PASS or FAIL: the control layer measurably prevents some redundant work,
and measurably misses some, in a way now precise enough to act on.

## Open discrepancy

Two blocked-call figures appear above and they are not the same number: **89**
blocked calls across all 864 tool calls, versus **46** blocked inside the 362
classified same-turn, same-argument repeats. The remaining 43 fall outside the
(turn, arguments) grouping used here. This write-up does not resolve *why* from
the data available — it reports both figures as measured rather than picking
whichever reads better, and flags reconciling them as open work.

## Limitation

Single 2.6-day window on one instance. The classified set (362) covers repeats
matched on exact (turn, arguments); repeats differing in argument serialization
fall outside it and are not counted. Output-size matching is a proxy for
identical content, not a byte-for-byte diff. The "polling vs. duplication"
split is a reasonable but not definitive read of the data.
