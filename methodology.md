# Methodology

## Case study template

Every entry in `case-studies/` and `results/` follows the same shape:

1. **Problem** — what was broken or in question, in plain language.
2. **Hypothesis** — what change was expected to help, and why.
3. **Baseline** — the state before the change (described by behavior, not by
   config/code).
4. **Experiment / Intervention** — what was actually changed, described by
   behavior.
5. **Metrics** — real, verifiable numbers when they exist (durations,
   counts, dates, adoption/reversion across time). If no metric was
   captured for a given change, this section says so explicitly rather than
   estimating one.
6. **Result** — what was observed after the change.
7. **Failure analysis / learning** — what went wrong, if anything, and what
   it taught.
8. **Decision** — **PASS**, **FAIL**, or **INCONCLUSIVE**, with the reasoning
   spelled out.

## Number labels

Every figure in this repo is labeled one of four ways, and the label is
never dropped when a number is quoted elsewhere (a README table, a summary
sentence):

- **Measured** — independently re-derived from a primary source (a log,
  a database, a structured telemetry event) as part of writing the entry,
  not recalled or copy-pasted from a prior summary.
- **Estimated** — a real value from the source data, but the source itself
  flags it as an estimate rather than an exact count.
- **Unavailable** — the data needed doesn't exist or couldn't be resolved
  (e.g. a cost figure the pricing table has no entry for). Reported as
  unavailable, never as zero — a run that cost nothing and a run whose cost
  nobody could compute are different facts.
- **Inconclusive** — the data exists but doesn't support a clean verdict
  (mixed signal, confounded comparison, single-sample result).

## Ground rules

- **No implementation.** No source, prompts, agent configuration, config
  file contents, orchestration/retrieval/scoring/context-selection/
  tool-routing logic, or internal component names. Changes are described by
  what they *do*, never by *how*.
- **No fabricated numbers.** Every metric traces back to something
  independently verifiable (a timestamp, a log line, a count). Nothing here
  is a plausible-sounding estimate dressed up as a measurement.
- **No credentials, tokens, internal hostnames, room/channel identifiers,
  ticket numbers, file paths, or other internal infrastructure
  identifiers.** These are scrubbed even when they appear in the source
  material behind a write-up.
- **Honest verdicts.** A stopgap that was later reverted or superseded is
  recorded as such (see the sandbox execution case study) rather than
  quietly reframed as a clean win.
- **Evidence-based verdicts, clearly scoped.** Where a verdict is based on
  "did this change hold across subsequent snapshots" rather than a
  monitored KPI, the write-up says so — that's an adoption signal, not a
  performance benchmark, and the two are never conflated.
