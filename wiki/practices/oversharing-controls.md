---
type: practice
title: "Oversharing Controls for AI Search"
created: 2026-05-01
updated: 2026-09-30
tags:
  - practices
  - oversharing
  - ai-search
  - knowledge-layer
  - copilot
  - glean
status: developing
scope_axis:
  - sec-of-ai
maturity: emerging
addresses_threat: "AI search tools (Microsoft Copilot, Glean, Gemini, custom LLMs) retrieving and combining content that is RBAC-permitted but contextually inappropriate"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[productivity-assistant-deployment-shape]]"
  - "[[gemini-enterprise-control-sheet]]"
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
  - "[[.raw/articles/cyera-ai-security-every-agent-assistant-data-store-2026-08-31.md]]"
  - "https://learn.microsoft.com/en-us/microsoft-365/copilot/copilot-controls/security-governance"
verified: 2026-09-30
verified_against:
  - ".raw/articles/cyera-ai-security-every-agent-assistant-data-store-2026-08-31.md"
  - ".raw/articles/knostic-ai-data-security-2026-05-01.md"
verified_findings: 0
verified_note: "Whole-page source read against Knostic and Cyera archived articles and live Microsoft guidance; Cyera scope remains a qualified vendor claim."
---

# Oversharing Controls for AI Search

**AI oversharing** includes the failure mode where an AI search tool retrieves and combines content that is *technically RBAC-permitted but contextually inappropriate*. The user can open each retrieved fragment individually; the synthesized answer crosses a specified need-to-know boundary. [Microsoft's Copilot security guidance](https://learn.microsoft.com/en-us/microsoft-365/copilot/copilot-controls/security-governance) treats broadly shared source content as a deployment risk, while [Knostic's AI data security article](https://www.knostic.ai/blog/ai-data-security) argues for answer-time checks on sensitive combinations.

The risk occurs when source permissions admit content beyond the intended audience or when individually permitted records yield a prohibited inference. A knowledge-layer product may test or constrain that synthesis where it sees the retrieval and response path. Its presence alone does not establish enforcement inside a vendor-managed assistant.

## Drivers of oversharing

| Driver | Mechanism |
|---|---|
| **Permission inheritance** | OneDrive / SharePoint / Teams broad-share defaults inherit into AI retrieval scope. A document shared with "all employees" is now answerable by Copilot. |
| **Sensitivity-label drift** | Documents labelled "Confidential" but stored in default-access containers; the AI sees the container, not the label. |
| **Composition risk** | Each retrieved fragment is permitted; the joint inference from the combination is not (see [[inference-exposure\|Inference Exposure (and Retrieval Exposure)]]). |
| **Stale embeddings** | A document permission was tightened; the vector store still holds an embedding of the original content. |
| **Cross-source aggregation** | Copilot pulls from M365, plus a third-party connector, plus user history; the assembly exceeds any single corpus's permission scope. |

## Mitigation stack

The Knostic article proposes several controls. A deployment can credit each one only where its enforcement point sees the actual retrieval or output route:

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

Run synthetic but realistic employee prompts against the production AI search to surface oversharing paths *before* a real user finds them. Knostic describes this as prompt simulation; the test still needs representative principals, corpora, and both prohibited and permitted answers.

### 6. Provenance and audit trail

Every disclosure decision logged: who asked, what was retrieved, why it was allowed, what was returned. Enables post-incident reconstruction (see [[ai-bom|AI-BOM: AI Bill of Materials]] §Audit and [[agent-observability|Agent Observability]]).

## Product surfaces (Q2 2026)

- **Knostic** describes knowledge-layer assessment and response controls for Microsoft Copilot, Glean, Gemini, and custom assistants. See [[knostic|Knostic]] for the vendor claim and integration scope.
- **Microsoft Purview and sensitivity labels** govern data in the Microsoft 365 ecosystem. The [Copilot security guidance](https://learn.microsoft.com/en-us/microsoft-365/copilot/copilot-controls/security-governance) calls for source-permission review alongside those controls.
- **DSPM vendors with AI extensions** can help identify sensitive or broadly shared sources. [[cyera-agent-guardian-release|Cyera Agent Guardian Release]] is a dated example; a deployed integration must be tested for the corpus and answer path at issue.

These surfaces answer different questions: source exposure, permitted retrieval, and answer disclosure. A product that inventories source exposure does not necessarily enforce a decision before an answer is shown.

## CMM Mapping

Oversharing controls span [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] domains:
- **D6 Data, Memory & RAG** — sensitivity-label propagation, DSPM feed
- **D3 Control & [[least-agency-principle|Least Agency]]** — answer-time policy enforcement
- **D7 Observability** — disclosure decision audit trail

The mature implementation requires all three.

The [[productivity-assistant-deployment-shape|Productivity Assistant Deployment Shape]] treats cross-corpus retrieval and synthesized disclosure as threats to assess for each enabled route. [[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]] grades the deployed route's corpus reach, current source entitlements, and material inference combinations. The [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] sets the evidence and coverage rules; the CMM assigns no preset score to the productivity-assistant shape.

The [[gemini-enterprise-control-sheet|Gemini Enterprise Control Sheet]] tests source entitlement separately from a prohibited inference made from individually readable records. Its route decision requires a named restriction, paired test, and an effective gate or Hold.

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
