---
type: practice
title: "Oversharing Controls for AI Search"
created: 2026-05-01
updated: 2026-08-31
tags:
  - practices
  - oversharing
  - ai-search
  - knowledge-layer
  - copilot
  - glean
status: developing
maturity: emerging
addresses_threat: "AI search tools (Microsoft Copilot, Glean, Gemini, custom LLMs) retrieving and combining content that is RBAC-permitted but contextually inappropriate"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[ai-data-security]]"
  - "[[inference-exposure]]"
  - "[[ai-usage-control]]"
  - "[[dspm]]"
  - "[[rag-hardening]]"
  - "[[knostic]]"
  - "[[cyera|Cyera]]"
  - "[[cyera-agent-guardian-release]]"
sources:
  - "[[.raw/articles/knostic-ai-data-security-2026-05-01.md]]"
---

# Oversharing Controls for AI Search

**AI oversharing** is the failure mode where an AI search tool retrieves and combines content that is *technically RBAC-permitted but contextually inappropriate*. The user can open each retrieved fragment individually; the synthesized answer crosses a need-to-know boundary.

This is the single most common AI security failure mode reported in 2026 enterprise Microsoft Copilot, Glean, and Gemini deployments. It is also the primary commercial driver behind a new vendor category that operates at the **knowledge layer** between data and AI answers.

## Drivers of oversharing

| Driver | Mechanism |
|---|---|
| **Permission inheritance** | OneDrive / SharePoint / Teams broad-share defaults inherit into AI retrieval scope. A document shared with "all employees" is now answerable by Copilot. |
| **Sensitivity-label drift** | Documents labelled "Confidential" but stored in default-access containers; the AI sees the container, not the label. |
| **Composition risk** | Each retrieved fragment is permitted; the joint inference from the combination is not (see [[inference-exposure\|Inference Exposure (and Retrieval Exposure)]]). |
| **Stale embeddings** | A document permission was tightened; the vector store still holds an embedding of the original content. |
| **Cross-source aggregation** | Copilot pulls from M365, plus a third-party connector, plus user history; the assembly exceeds any single corpus's permission scope. |

## Mitigation Stack

The Knostic article and other 2026 vendor playbooks converge on a multi-layer mitigation:

### 1. Need-to-know enforcement at the knowledge layer

- **RBAC + ABAC** combined — role *and* attribute (project, sensitivity label, current task) checked together.
- **Sensitivity labels combined with real-time output filters** — labels propagate through retrieval; filters apply at answer time.
- **Middleware guardrails before AI responses are shown** — final-stage redaction or block, with logged decision.

### 2. Dynamic boundaries

Static labels are insufficient because sensitivity is contextual. The Knostic framing: build **dynamic, need-to-know boundaries that reflect role, context, and actual usage, not only static labels.**

### 3. Continuous policy enforcement during a session

[[ai-usage-control|AI Usage Control]] re-evaluates at every turn. A session that started in one project may not stay there.

### 4. DSPM upstream feed

[[dspm|DSPM]] maps where sensitive data lives. Embeddings, caches, logs inherit the sensitivity. Guardrails consume DSPM signals so risky sources are excluded at query time.

### 5. Prompt simulation testing

Run synthetic but realistic employee prompts against the production AI search to surface oversharing paths *before* a real user finds them. This is now a productized capability (Knostic's "prompt simulation" is the canonical commercial example).

### 6. Provenance and audit trail

Every disclosure decision logged: who asked, what was retrieved, why it was allowed, what was returned. Enables post-incident reconstruction (see [[ai-bom|AI-BOM: AI Bill of Materials]] §Audit and [[agent-observability|Agent Observability]]).

## Vendor / Tool Landscape (Q2 2026)

- **Knostic** — pure-play knowledge-layer governance for Microsoft Copilot, Glean, Gemini. See [[knostic|Knostic]].
- **Microsoft Purview + Sensitivity Labels** — built-in for the M365 ecosystem; less effective on cross-source aggregation than dedicated knowledge-layer tooling.
- **Glean's own permissioning** — vendor-internal RBAC enforcement; limited to Glean's own retrieval scope.
- **DSPM vendors with AI extensions** — [[cyera|Cyera]], [[varonis|Varonis]], BigID, others moving into the AI-feed-DSPM space; Cyera's [[cyera-agent-guardian-release|Agent Guardian release]], fetched 2026-08-31, is a dated instance of that movement.

The category is fragmenting. As of Q2 2026 it is not a single market but at least three: knowledge-layer governance, sensitivity-label-management, DSPM-with-AI-extensions.

## CMM Mapping

Oversharing controls span [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] domains:
- **D6 Data, Memory & RAG** — sensitivity-label propagation, DSPM feed
- **D3 Control & [[least-agency-principle|Least Agency]]** — answer-time policy enforcement
- **D7 Observability** — disclosure decision audit trail

The mature implementation requires all three.

## Open Issues

- **Latency budget.** Real-time output filters add inference latency. How much is acceptable for an AI search experience?
- **False-positive rate.** Aggressive filtering frustrates users; under-filtering surfaces sensitive content. Calibration is per-enterprise.
- **Cross-tenant aggregation.** When the AI consumes data from external connectors (CRM, ticketing, third-party APIs), permission inheritance from external systems is non-trivial.
- **Prompt-simulation coverage.** What is "enough" simulation? No published baseline exists.

## See Also

- [[ai-data-security|AI Data Security (Knostic blog, 2026)]] — primary source
- [[knostic|Knostic]] — vendor most directly aligned with this practice
- [[inference-exposure|Inference Exposure (and Retrieval Exposure)]] — the underlying failure mode
- [[ai-usage-control|AI Usage Control (AI-UC / UCON for AI)]] — answer-time policy decision frame
- [[dspm|Data Security Posture Management (DSPM) for AI]] — upstream data classification feed
- [[rag-hardening|RAG Hardening]] — adjacent practice (vector poisoning, retrieval scoring, source-trust attribution)
