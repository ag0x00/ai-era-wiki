---
type: concept
title: "AI Usage Control (AI-UC / UCON for AI)"
created: 2026-05-01
updated: 2026-08-18
tags:
  - concepts
  - access-control
  - usage-control
  - ucon
  - ai-data-security
status: developing
complexity: advanced
domain: access-control
aliases:
  - "AI-UC"
  - "AI Usage Control"
  - "UCON for AI"
  - "Usage Control"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[ai-data-security]]"
  - "[[inference-exposure]]"
  - "[[oversharing-controls]]"
  - "[[non-human-identity]]"
  - "[[security-controls-for-ai-stacks]]"
sources:
  - "[[.raw/articles/knostic-ai-data-security-2026-05-01.md]]"
---

# AI Usage Control (AI-UC)

Classic access control answers a binary question at access time: *"Can I open this file?"* **Usage control (UCON)** continues past that point into *"how may I use or disclose what's inside?"* — and re-evaluates the answer continuously while access is in use.

The conceptual basis is Sandhu and Park's [UCON_ABC model](https://profsandhu.com/journals/tissec/ucon-abc.pdf) (Authorizations, Obligations, Conditions). AI-UC applies UCON to **AI answer time** rather than file open time.

## The Three UCON Components Applied to AI

| Component | Classic meaning | AI-UC meaning |
|---|---|---|
| **Authorizations** | Subject + Object + Right matchup | Identity + retrieved-context + intended-use matchup, evaluated continuously through a session |
| **Obligations** | Pre-access requirements (training, accept-EULA) | Just-in-time approvals, training acknowledgements, MFA step-up before sensitive answers are released |
| **Conditions** | Environmental factors (time, location) | Device health, location, time, project context, conversation drift — re-checked at every turn |

The continuous-evaluation property is what makes UCON different from RBAC/ABAC: the answer can be **cut off mid-session** if context shifts, attributes are updated when usage occurs, and obligations like "approve this disclosure" can be inserted dynamically.

## Case for usage control in AI

Standard access control is a **point-in-time gate** at retrieval. By the time an LLM has assembled a response, the gate has long since closed. The retrieved fragments may have been individually authorized, but their *combination* in a generated answer may exceed any single per-document grant. See [[inference-exposure|Inference Exposure (and Retrieval Exposure)]] for the failure mode this addresses.

Usage control runs **at answer time**:

- Has the user accumulated enough context in this session to constitute oversharing? (continuous Authorization)
- Should this answer require an MFA step-up or a manager approval before delivery? (Obligation)
- Has the conversation drifted into a project the current device/location is not approved for? (Condition)

The decision can be made for every turn of every session, not just at session start.

## Concrete Operations

The Knostic article enumerates the operational primitives:

- **Continuous authorization** during a session, not one-time checks
- **Just-in-time approvals** and training acknowledgements before release
- **Limit copying, summarizing, or exporting** of generated answers
- **Apply transformations** in the response stream: masking, selective redaction
- **Adjust attributes when usage occurs** — the system remembers what was disclosed and considers it for future requests
- **Log every usage decision** for audits

This is not a single product category — it spans Zero Trust + DLP + AI guardrails + identity, with a control loop that closes at every model invocation.

## Placement in the stack

| Layer | Role |
|---|---|
| Identity (D2) | Establishes *who* is asking and through what device/session context |
| Control & [[least-agency-principle\|Least Agency]] (D3) | Sets the upper bounds on what the agent may do |
| Runtime (D4) | Hosts the per-turn [[oversight-layer\|policy decision point]] |
| **Usage control** | Continuously decides what the agent may **disclose** in this turn given full session/conversation context |
| Egress (D5) | Enforces the decision at the boundary if the agent attempts to send/copy |
| Data (D6) | Provides classification + sensitivity labels that the policy depends on |
| Observability (D7) | Captures the decision trail for audit and post-hoc review |

AI-UC is most naturally placed at the boundary between Control and Runtime — a continuous policy decision point that runs every model invocation rather than every API call.

The [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] names this model as the generalization behind its answer-time entitlement enforcement and oversharing-remediation control, with Microsoft Purview DSPM for AI and label-aware DLP for M365 Copilot as the GA instances.

The [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] files the shipping instantiation of this control on the data plane (D6): answer-time entitlement enforcement through Microsoft Purview [[dspm|DSPM for AI]] and its oversharing assessments, label-aware DLP for M365 Copilot, and Restricted SharePoint Search as a site-capped stopgap, classified COTS and generally available. Those controls evaluate the entitlements and sensitivity labels the data plane supplies, covering the Authorizations component alone, and the RA marks that set as the load-bearing control against [[inference-exposure|inference exposure]] for closed-corpus member and customer bots. The Control/Runtime placement above describes the full UCON loop, which a deployment reaches once one decision point evaluates session-drift Conditions and mid-session Obligations on every turn.

## Open Issues

- **Latency budget.** Continuous authorization on every turn adds inference latency. How much is tolerable?
- **Decision provenance.** UCON decisions are dynamic; reproducing why a particular answer was redacted six months later is non-trivial.
- **Policy expressiveness.** UCON has a research lineage but few production policy languages targeted at "answer-time" decisions. Cedar, OPA, and Rego are general-purpose; an AI-specific dialect may be needed.
- **Combination with semantic boundary enforcement.** UCON answers the structural question; semantic boundary enforcement evaluates the *meaning* of the candidate output. Both are needed and they are not the same control.

## See Also

- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] — cites AI-UC as the generalization behind its answer-time entitlement enforcement control
- [[ai-data-security|AI Data Security (Knostic blog, 2026)]] — primary source
- [[inference-exposure|Inference Exposure (and Retrieval Exposure)]] — the failure mode AI-UC addresses
- [[oversharing-controls|Oversharing Controls for AI Search]] — operationalization in AI-search products
- [[non-human-identity|Non-Human Identity (NHI)]] — adjacent identity layer
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]]

<!-- sources:auto -->
## Sources

- [knostic.ai](https://www.knostic.ai/blog/ai-data-security)
<!-- /sources -->
