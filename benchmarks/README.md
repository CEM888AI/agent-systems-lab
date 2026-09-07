# Benchmarks — planned

Nothing here yet. The case studies and results published so far come from
real production telemetry and configuration history, not from a repeatable
benchmark suite — that distinction matters, and this section stays empty
until there's a real one to publish.

Two are designed and queued to run:

- **Waste Mechanism Scorecard** — one realistic, bounded multi-step task,
  measuring cache hit rate, repeated-call ratio (using the same
  true-duplication-vs-polling classification as
  [the redundancy case study](../case-studies/tool-call-redundancy-guard-coverage.md)),
  tool/model calls per *externally verified* completed task, retry rate,
  tokens, cost, and wall-clock — with an external completion check, not
  just the runtime's self-reported status.
- **Runaway / wrong-premise test** — a task with a real, checkable
  contradiction built in. Success is the agent identifying the
  contradiction, staying within a normal call budget, and truthfully
  reporting non-completion — not silently working around it or claiming
  false success.

Numbers will be added here once both have run and produced results that
meet this repo's own labeling standard — see [`methodology.md`](../methodology.md).
