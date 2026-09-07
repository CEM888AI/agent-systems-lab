# Bounded termination: does the STOP condition actually fire?

**Window:** 2026-09-05 00:11 – 2026-09-07 15:19 UTC (~2.6 days), Ember
instance, 79 completed turns
**Decision:** PASS — the mechanism exists, is rarely needed, and worked
correctly the one time it was needed in this window

## Problem

An agent runtime with no hard ceiling on how long a single turn can run is
one bad loop away from an open-ended cost. Having a configured cap is easy
to claim; whether it actually engages correctly in production, on a real
turn, without being disabled or silently bypassed, is the real question.

## Measured

Turn-ending outcomes across all 79 completed turns in the window:

| Outcome | Count |
|---|---|
| Clean response, no cap involved | 75 |
| User-interrupted mid-turn | 2 |
| Interrupted during an in-flight model call | 1 |
| Hit the configured iteration cap | 1 |

The one capped turn, in full:

- **90/90** model calls used
- **4** tool calls in the entire turn
- **10.2 minutes** wall-clock
- **586,649** input tokens, **461,824** of them cache-read
- Ended with an explicit "incomplete" status — not misreported as a normal
  completion

## Result

The cap fired exactly once in 79 turns (1.3%) — evidence this isn't
compensating for constant runaways, and evidence it isn't decorative
either: when a turn burned its entire call budget while only taking 4
real actions (a call-heavy, action-light pattern), the runtime stopped it
and recorded it honestly as incomplete rather than letting it continue
indefinitely or reporting false success.

Separately, both user-interrupted turns and the mid-call interruption were
also recorded with an explicit incomplete/interrupted status rather than
being folded into "success" — the runtime's own bookkeeping distinguishes
"finished" from "stopped" in all three non-clean cases observed.

## Decision

**PASS.** This is deliberately a small, honest finding: one real capped
turn, fully accounted for, not a dramatized "runaway agent" story. The
low frequency (1.3%) is part of the result, not a caveat to hide.

## Limitation

Single instance, single 2.6-day window, one capped-turn sample. This shows
the mechanism *can* and *did* work correctly — it is not a claim about how
often turns approach the cap across all usage.
