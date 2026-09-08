# Task continuity across context compression and session reset

**Date:** 2026-09-07
**Decision:** PASS — measured, with one claim explicitly scoped out

## Problem

A long-running agent task will exhaust its context. The usual outcomes are that
the run dies, or that an operator has to re-brief the agent and restart. Neither
is acceptable for unattended work. The question is whether a task can survive
its own context boundaries without a human putting it back together.

## What was measured

A single operator goal — bring a company page up to date and issue targeted
follow invitations — was given once and left to run. The runtime crossed seven
sessions completing it.

| | |
|---|---:|
| Sessions in the chain | 7 |
| Wall time, first start to last end | 99.9 min |
| Context compression events survived | 6 |
| Session resets survived | 1 |
| API calls | 182 |
| Tool calls | 384 |
| Cache-miss input tokens | 596,994 |
| Cache-hit input tokens | 15,664,768 |
| Output tokens | 171,332 |
| Owner-confirmed actions completed | 235 |

Each session records the previous one as its parent. The task was briefed once,
at the start. No re-briefing occurred at any boundary.

## Result

**PASS.** The task ran to completion across six context compressions and one
session reset, unattended, without losing the goal.

## Failure analysis / learning

Two of the seven sessions (0.3 min each) recorded zero API calls, zero tool
calls and zero tokens — empty artifacts of the compression handoff, not useful
work. They do not affect the outcome, but they are noise in the session
lifecycle and are recorded here rather than filtered out.

The run is also call-heavy relative to actions: 182 API calls and 384 tool calls
for 235 completed actions. That ratio is not optimised and is not presented as
if it were.

## Cost

| | |
|---|---:|
| Estimated cost, whole run | **$0.3541** |
| Per completed action | **$0.0015** |

Derived from the token counts above against the published rate card effective
2026-08-16. The run fell entirely outside peak windows, so off-peak rates apply
throughout.

This is derived from token counts rather than read off an invoice: the provider
bills at account level and does not attribute cost per run. As a cross-check,
the provider dashboard on 2026-09-07 showed $1.85 for roughly four hours of
shared activity across three agents that day, which is consistent with this
run's share of it. Per-request provider cost attribution is not yet captured;
when it is, this figure will be re-derived against it.

## Scoped out: memory recall

A follow-on task ran immediately afterwards in a session with **no parent** — a
fresh root with no context lineage to the chain above. The operator reports that
the agent applied procedure worked out during the earlier run without being
re-taught it, and the opening brief is confirmed to contain goal and context
rather than the procedure.

That is consistent with recall from persistent memory rather than carried
context, but this write-up does **not** claim it as measured. What telemetry
verifies is the absence of a parent session, not the provenance of what the
agent knew. The stronger claim needs an instrumented test, and until that exists
it is not asserted.

## What is intentionally withheld

The memory architecture, the compression strategy, and the session handoff
mechanism are implementation and are not published.
