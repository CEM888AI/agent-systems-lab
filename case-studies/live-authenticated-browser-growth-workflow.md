# Live authenticated browser workflow at business scale

**Verdict: PASS**  
**Observed:** September 7, 2026  
**System under test:** Vetta on the live CEM888 agent runtime

## What this tests

This acceptance test measures whether a live CEM888 agent can operate a real, already-authenticated business web account, recover from UI-level failures, and complete a meaningful multi-step business workflow without step-by-step human driving.

The goal was not to demonstrate a toy browser click. It was to test whether the agent could take a business objective, discover the actual controls available in the live account, adapt when the site's UI did not respond to a naive interaction method, and carry the workflow through at useful scale.

## Task

Using the owner's authenticated LinkedIn session and CEM888 company page, the agent was asked to identify relevant AI/engineering connections and send company-page invitations through LinkedIn's native page-invite workflow.

The owner remained available only for an authentication step that required a device-bound passkey. After that, the agent resumed the workflow autonomously.

## Observed behavior

The live agent:

- located the correct logged-in browser/account context;
- identified the relevant company-page admin workflow;
- inspected existing pending invitations before acting;
- discovered the correct company-page route when an assumed slug was wrong;
- found LinkedIn's native page invite controls;
- filtered toward people relevant to AI, engineering, and potential business/runtime partnerships;
- detected when a synthetic DOM click did not actually change the live React application state;
- re-inspected the rendered controls and changed interaction strategy rather than assuming success;
- continued through the live workflow and sent **235 company-page invitations**, owner-confirmed in the production account.

## Result

**PASS.**

The agent completed a real authenticated browser workflow at business scale, including UI discovery, error recovery, targeted selection, and 235 successful live actions.

## Why this matters

A useful business agent has to do more than answer questions or click a single scripted button.

This run demonstrates the ability to:

> take a real business objective, operate an authenticated web application, adapt to changing UI behavior, and carry the task through without continuous human micromanagement.

The important capability is not "LinkedIn automation" specifically. The capability is **autonomous operation of live business software with verification and recovery**.

That same runtime pattern applies to other browser-based business workflows where the account owner has authorized access.

## Human boundary

The one device-bound authentication challenge was completed by the account owner. The agent did not bypass or attempt to defeat that security boundary; it resumed work immediately after the owner completed the required login step.

## Evidence classification

- **Live production account:** yes
- **Real external actions:** yes
- **Scale:** 235 company-page invitations
- **Shape:** 7 chained sessions over 99.9 min — see
  [`task-continuity-across-context-compression.md`](task-continuity-across-context-compression.md)
- **Human step-by-step control:** no
- **Owner intervention:** authentication boundary only, then normal steering
- **Result confirmation:** owner-confirmed in the live account

## What is intentionally withheld

This public write-up does **not** publish the account transcript, recipient identities, credentials, cookies, session identifiers, browser-profile paths, private URLs, internal tool names, CDP/Playwright implementation details, selector strategies, prompts, agent configuration, targeting heuristics, or runtime source code.

The public artifact shows the task class, observed behavior, scale, and result. The implementation remains proprietary.

---

Built and tested by **Chandler Morone** — CEM888.AI
