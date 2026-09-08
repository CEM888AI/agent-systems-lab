# Vetta — CEM888 Production Agent Beta

![CEM888 — State decides what is true. Models decide what to do about it.](assets/cem888-state-runtime-poster.webp)

**A production-agent release candidate built on the CEM888 runtime.**

Vetta is not a chatbot demo. She is a persistent working agent designed to operate real tools and systems, carry work across steps and sessions, verify outcomes against reality, and keep the user informed while she works.

This repository publishes **sanitized capability evidence and measured production runs**. It intentionally does **not** publish CEM888 proprietary runtime code, prompts, orchestration logic, retrieval/scoring logic, credentials, private logs, infrastructure details, or anything sufficient to reconstruct the system.

## What Vetta has demonstrated

### Operates real environments
Vetta works beyond chat: she can inspect software projects, use terminal and Git workflows, investigate running systems, interact with browser and desktop environments, and work across authorized machines as part of a larger task.

### Executes multi-step work
She can take an outcome-oriented request, investigate the environment, gather evidence, use multiple tools, update her working hypothesis, and continue toward a result without requiring the user to prescribe every click.

### Executes live authenticated business workflows
In a live LinkedIn business-page run, Vetta navigated the authenticated account, discovered the correct native invite workflow, recovered when a synthetic UI interaction did not change the React application state, changed interaction strategy, and completed **235 company-page invitations** in the production account. See [`case-studies/live-authenticated-browser-growth-workflow.md`](case-studies/live-authenticated-browser-growth-workflow.md).

In a separate live cross-platform brand-operations run, Vetta reconciled current LinkedIn/GitHub positioning against stale Facebook copy, updated the business profiles, connected Facebook and Instagram through Meta Business Suite, created and iterated launch artwork, recovered from multiple live UI failures, and submitted a coordinated launch post to both configured destinations. See [`case-studies/cross-platform-brand-operations.md`](case-studies/cross-platform-brand-operations.md).

### Verifies before claiming completion
CEM888 is designed around a simple production rule: **the agent's statement is not the proof — resulting state is.** Vetta has demonstrated production verification against live state and has stopped rather than making an unsafe or unsupported change when evidence was ambiguous.

### Corrects herself from evidence
In live investigation, Vetta has challenged an initial assumption, gathered physical evidence, and changed her conclusion when evidence contradicted the first hypothesis rather than defending it.

### Avoids unnecessary repetition
In a verified production run, Vetta completed the task with **0 provider retries and 0 duplicate reads/searches**.

### Uses context economically during multi-call work
In that verified run, prompt-cache reuse increased across successive calls and reached approximately **99% on the final two inference calls**.

### Remains visible while working
Vetta reports substantive progress during multi-step work instead of disappearing into a long silent execution loop.

### Preserves continuity
CEM888 separates the persistent agent/runtime layer from the underlying reasoning model so useful state and working continuity do not have to disappear when a model or session changes.

### Remains under user control
The runtime includes user interruption/cancellation behavior so autonomous work does not mean surrendering control of the agent.

## Verified production run — September 7, 2026

A live read-only verification task asked Vetta to establish three facts about a deployed web environment, including one nonexistent artifact.

**Independent result: 3/3 factual answers matched physical state.**

| Metric | Result |
|---|---:|
| Factual checks | **3/3 MATCH** |
| Model/API calls | **4** |
| Tool calls | **4** |
| Failed tool calls | **0** |
| Provider retries | **0** |
| Duplicate reads/searches | **0** |
| Wall time | **46.07 s** |
| Total prompt tokens | **132,430** |
| Cache-hit prompt tokens | **113,152** |
| Cache-miss prompt tokens | **19,278** |
| Aggregate prompt-cache reuse | **85.44%** |
| Output tokens | **5,546** |
| Provider cost | **Unavailable in retained pricing telemetry** |

Per-call cache reuse improved from roughly 68% and 67% on the first two calls to approximately **99% and 99%** on the final two calls.

See [`case-studies/production-verification-cache-efficiency.md`](case-studies/production-verification-cache-efficiency.md) for the existing sanitized evidence record.

## Same-day provider spend evidence

For September 7, DeepSeek's provider billing dashboard recorded **$1.85 total API spend** across the shared CEM888 testing/work account for that day: **$1.83** on `deepseek-v4-flash` and **$0.02** on `deepseek-v4-flash-vision-exp`.

That provider total includes same-day work/testing by **CEM, Ember, and Vetta** and should not be read as a Vetta-only task charge. It is published as account-level provider evidence showing the actual order of magnitude of model spend during a day that included production-style autonomous work plus active engineering tests.

See [`case-studies/cross-platform-brand-operations.md`](case-studies/cross-platform-brand-operations.md) for the evidence classification and limitations.

## Additional live acceptance evidence

| Test | Result | Evidence |
|---|---|---|
| Authenticated browser business workflow | **PASS — 235 live company-page invitations** | [`live-authenticated-browser-growth-workflow.md`](case-studies/live-authenticated-browser-growth-workflow.md) |
| Cross-platform brand operations | **PASS with final-feed verification limitation** | [`cross-platform-brand-operations.md`](case-studies/cross-platform-brand-operations.md) |
| Task continuity across 6 compressions + 1 reset | **PASS — 7 chained sessions, 99.9 min, unattended** | [`task-continuity-across-context-compression.md`](case-studies/task-continuity-across-context-compression.md) |

## Customer-facing capability track

Vetta's beta is being evaluated on work a customer actually cares about, not how many internal plugins or tools exist:

- operate native desktop applications
- execute browser and web workflows
- research across multiple sites and reconcile differences
- inspect and debug software/projects
- use terminal and Git workflows
- perform authorized background computer work
- operate across authorized remote machines
- use credentials through secure paths without exposing secrets
- sustain long multi-tool workflows
- recognize impossible/false-premise work instead of fabricating completion
- resume useful work across sessions
- verify resulting state after execution
- report useful progress while working
- expose measurable latency, model/tool calls, retries, token use and cache reuse

These capabilities will be promoted from the beta track into the **proven** section as sanitized acceptance evidence is collected.

## Why this work matters

A lot of agent failure is not a model-intelligence problem. It is a **runtime control problem**: repeated work, stale context, unnecessary execution, runaway retries, false completion, weak state carry, and expensive reconstruction of information the agent already had.

CEM888 focuses on the layer around the model: **state, control, execution, verification, continuity, and measurable efficiency.**

> **The model provides intelligence. CEM888 provides the working system that makes that intelligence usable for real work.**

## Engineering evidence, not implementation disclosure

Public evidence follows:

**Problem → Test → Measured behavior → High-level result → Limitation**

It does not publish proprietary implementation recipes. Internal component names, plugin counts, source code, prompts, private paths, credentials, raw transcripts/logs, customer data, context-selection logic, memory/retrieval internals and orchestration machinery stay private.

## About

**Chandler Morone — Founder & Agentic AI Engineer, CEM888**

I build production agent runtimes: context/state orchestration, browser and desktop execution, MCP/API integrations, reliability/evals, observability, cost/cache engineering, and the control systems required to make autonomous agents useful outside a demo.

---

**CEM888 — choose your agent, choose your intelligence, keep your continuity.**