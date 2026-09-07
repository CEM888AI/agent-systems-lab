# Production Agent Reliability & Control Lab

Real agent systems. Real failures. Measured results.

Production AI agents should be judged by what they actually accomplish, how
efficiently they execute, and whether their resulting state matches their
claims — not just by fluent output. A lot of agent waste is a control
problem, not merely a token-compression problem. That's the working thesis
behind the case studies here — presented as what the evidence supports,
not as something universally proven.

## What I build

I build and operate CEM888, a real, long-running, model-agnostic
multi-agent runtime — not a demo, a system that runs continuously across
multiple deployed agent instances, handling real tasks with real cost and
real failure modes. This repo is the sanitized evidence trail from
operating it: real dates, real telemetry, real numbers, independently
re-derived from source logs rather than taken on faith from a summary.

**This is not** a token compressor, prompt trimmer, proxy, or context
summarizer, and it is not an open-source release of the runtime. No source
code, prompts, agent configuration, or orchestration/context-selection/
retrieval implementation is published here — see
[What remains proprietary](#what-remains-proprietary).

## Measured production results

| Result | Value | Label |
|---|---|---|
| Aggregate prompt-cache reuse on a verified 4-call production run | **85.44%** | Measured |
| Retries / failed / blocked tool calls on that same run | **0 / 0 / 0** | Measured |
| Tool calls blocked outright as duplicates, zero cost (864-call sample) | **10.3%** | Measured |
| Of the redundant calls *not* blocked, true identical re-execution vs. polling | **3.6% true duplication / 65.5% polling** | Measured, classified |
| Bounded-termination cap firing correctly on the one turn that needed it | **1 of 79 turns**, fully accounted, not silently dropped | Measured |
| Provider cost on the flagship verified run | **Unavailable** (model slug unresolved in the cost table at run time) | Explicitly unavailable — not shown as $0 |

Full context, methodology, and limitations for each number are in the
linked case studies below — none of these are meant to be read as a bare
percentage without the caveats attached to it.

## Case studies

| Case study | What it shows | Verdict |
|---|---|---|
| [Production verification + cache efficiency](case-studies/production-verification-cache-efficiency.md) | A verified-correct production run, call-by-call token/cache accounting | PASS |
| [Tool-call redundancy: what gets caught, what doesn't](case-studies/tool-call-redundancy-guard-coverage.md) | Where a duplicate-call guard works, and a specific real gap in its coverage | INCONCLUSIVE (both a real mechanism and a real gap) |
| [Bounded termination in production](case-studies/bounded-termination-in-production.md) | The one time in 79 turns a hard iteration cap fired, fully accounted | PASS |
| [Enabling a deterministic verification layer](case-studies/deterministic-hooks-adoption.md) | A control-layer mechanism adopted and expanded over time, never rolled back | PASS |
| [Auditing a rollback path before you need it](case-studies/rollback-path-audit.md) | A safety-net component audited and found broken before it was ever needed | PASS |
| [Sandbox execution stopgap](case-studies/sandbox-execution-stopgap.md) | An honest record of a fix that didn't stick — reverted a week later | INCONCLUSIVE |

## Results

| Result | What it shows |
|---|---|
| [Session-lifecycle load pattern](results/session-lifecycle-load-pattern.md) | Sessions ending via a reset path carry 3–7.7x more load than clean ones, reproduced across two independent instances — reported as correlation, not causation |
| [Skill-lifecycle curator runs](results/skill-lifecycle-curator-runs.md) | Real duration/throughput/consolidation numbers from a self-maintaining subsystem across 5 production runs |

## Evaluation methodology

Every entry follows the same discipline: **Problem → Test → Measured
behavior → High-level intervention (described behaviorally, never as
implementation) → Result → Limitation.** Numbers are labeled measured,
estimated, unavailable, or inconclusive — never asserted past what the
source data supports. Full ground rules: [`methodology.md`](methodology.md).

## Capabilities demonstrated

The case studies above are direct, cited evidence for:

- Production agent reliability and failure diagnosis
- Context/state engineering and session-lifecycle behavior
- Tool-use efficiency and redundant-call control
- Cache/token economics
- External-state verification (checking that claimed completion matches
  reality)
- Latency, telemetry, and observability instrumentation
- Provider-neutral agent operation (multiple model providers observed in
  the underlying telemetry)

Capabilities I also work on but that don't yet have a published, verified
case study in this repo — browser automation, desktop automation, MCP/API
integrations, multi-step execution — aren't claimed as "production-proven"
above until they have one. Not making that distinction would undercut the
evidence that *is* here.

## What remains proprietary

The runtime's implementation is not published, ever: source code, system
prompts, agent configuration, orchestration logic, context-selection and
retrieval/scoring algorithms, memory implementation, tool-routing
implementation, credentials, internal identifiers, and raw logs/transcripts.
Every result above is described by observed behavior and measured outcome,
not by the mechanism that produced it. See
[`about-the-runtime.md`](about-the-runtime.md) for the boundary in detail.

## About Chandler Morone

Founder & Agentic AI Engineer, CEM888. I build and operate production
agent runtimes — context/state orchestration, browser and desktop
execution, MCP/API integrations, reliability and evals, observability, and
cost/cache optimization — and spend a lot of time debugging real autonomous
agent failures rather than demoing happy paths.
