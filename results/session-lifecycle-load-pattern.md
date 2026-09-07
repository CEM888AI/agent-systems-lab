# Session-ending pattern and load, across two independent instances

**Sources:** Ember instance session table (315 sessions, 2026-07-14 to
2026-09-07) and a second, independently-operating instance (43 sessions,
~3 weeks). Same runtime, two separately running deployments with different
usage patterns — not the same data counted twice.

## What was measured

Every session in the runtime's own bookkeeping records how it ended (clean
close, an explicit reset path, a context-compression event, etc.) along
with real per-session counters: tool calls, model/API calls, input tokens,
cache-read tokens. Grouping by end-reason and averaging those counters
shows whether sessions that end one way carry more load than sessions that
end another way.

## Result

| Instance | End reason | Sessions | Avg tool calls | Avg API calls |
|---|---|---|---|---|
| Ember | clean/still-open | 108 | 7.2 | 30.0 |
| Ember | reset path | 153 | 55.3 (7.7x) | 122.2 (4.1x) |
| Second instance | clean/still-open | 15 | 26.6 | 20.7 |
| Second instance | reset path | 28 | 79.1 (3.0x) | 154.5 (7.5x) |

In both independently-operating instances, sessions that ended via the
reset path carried substantially more tool calls and model calls than
sessions that ended cleanly — a 3x to 7.7x difference depending on the
metric and instance. The direction of the effect reproduced across two
separate deployments with different task mixes; the magnitude didn't
(which is itself informative — it argues against a fixed universal
multiplier and for something workload-dependent).

## What this does not show

This is a correlation between how a session ended and how much load it
carried, observed after the fact. It is **not** a controlled experiment,
and the two end-reason groups aren't matched for task type or complexity —
a session that needs 50 tool calls to do its job is more likely to still
be running (and eventually hit a reset path) than one that needs 3. This
result does not establish that the reset path *causes* the extra load,
only that the two are strongly associated in both datasets checked.

## Decision

**Reported as correlational, not causal.** Worth publishing because it
reproduced independently on two separately-running instances with
different usage patterns — that rules out "this is one weird dataset,"
even though it doesn't rule out confounding by task type. A controlled
follow-up (matched task complexity across both end-reason groups) would be
needed to say more.
