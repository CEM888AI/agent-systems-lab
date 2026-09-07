# Auditing a rollback path before you need it

**Date:** 2026-09-02
**Decision:** PASS — latent defect in a safety mechanism found and closed
before it was ever exercised

## Problem

Several weeks earlier, a core context-injection component had been replaced
by an improved version. Following standard practice, the previous
component was left in the codebase, disabled via configuration, as a
documented rollback path in case the replacement needed to be reverted.

Question: is that documented rollback path actually still safe to use?

## Hypothesis

A rollback path that has never been exercised in the mode it would actually
run in is a hypothesis, not a guarantee. It was worth auditing before
treating it as a real safety net.

## Baseline

The legacy component remained present, config-gated off, referenced in
inline documentation as the designated rollback target for the newer
component.

## Experiment / Intervention

Audited the legacy component's behavior specifically under the operating
mode it would actually execute in if re-enabled — not just its general
code path.

## Metrics

This was a manual audit, not an instrumented test. The metric here is
binary and verifiable from the resulting change itself: rollback path
usable (yes/no).

## Result

The audit found that, in the relevant operating mode, the legacy
component's early-exit logic would skip a required state-insertion step and
a fallback mechanism that the current system now depends on. In other
words: if the rollback had ever actually been executed, it would have
**silently** degraded state continuity rather than safely reverting to the
old behavior — the opposite of what a rollback path is for.

## Failure analysis / learning

A rollback path that isn't tested under its real operating conditions can
rot into a false sense of safety. The gap here wasn't in the new
component — it was in an assumption that an untouched, disabled fallback
would still behave the same way once the rest of the system had moved on.

## Decision

**PASS.** The rollback path was retired outright and documented inline as
"do not re-enable," with the reasoning recorded so nobody reaches for it
under pressure later. The previous configuration snapshot was kept for
historical reference only, explicitly not as a live fallback target.

A secondary hardening change landed in the same pass: a chat-integration
surface that had been listening across all rooms/channels was scoped down
to a single, explicitly allow-listed one. That scoping has held, unchanged,
through every subsequent configuration snapshot to date.
