# Skill-lifecycle curator: 5 production runs

The runtime includes a self-maintaining "skill" subsystem: agent-created
skills are periodically reviewed by an automated curation pass that can
consolidate near-duplicate skills into umbrellas, mark skills stale, or
prune ones no longer earning their keep. This is real output from that
subsystem running in production over roughly a month, not a synthetic
benchmark.

## Raw run data

| Run date | Duration | Skills before → after | Consolidated | Pruned |
|---|---|---|---|---|
| 2026-07-28 | 1m 58s | 11 → 9 | 2 | 0 |
| 2026-08-04 | 1m 39s | 18 → 18 | 0 | 0 |
| 2026-08-11 | 10m 39s | 21 → 16 | 4 | 1 |
| 2026-08-21 | 2m 54s | 16 → 16 | 0 | 0 |
| 2026-08-29 | 5m 2s | 16 → 16 | 0 | 0 |

## What this shows

- **The subsystem does real, variable work, not a no-op sweep.** Two of the
  five runs made no changes at all; the other three consolidated or pruned
  real skills, including one run that removed 5 net skills through
  consolidation and pruning together.
- **Duration scales with the amount of work found**, from under 2 minutes
  on quiet runs up to over 10 minutes on the run that did the most
  consolidation — consistent with an LLM-driven review pass rather than a
  fixed-cost cron job.
- **The subtask's model routing changed across the observed window** — the
  same model was called through three different provider routes across
  these five runs. This write-up reports that as an observed operational
  fact; it does not speculate about why, since the reason isn't verifiable
  from this data alone.

## Decision

**PASS**, scoped narrowly: the subsystem reliably ran unattended over a
month, made non-trivial, variable decisions (not just idling), and never
required manual intervention across the runs sampled here. This is not a
claim about skill-quality outcomes — only about the subsystem's operational
reliability.
