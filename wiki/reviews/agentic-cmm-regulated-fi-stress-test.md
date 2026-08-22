---
type: review
title: "Agentic AI CMM: Regulated-FI Stress Test"
address: c-000162
origin: produced
created: 2026-05-23
updated: 2026-08-18
tags:
  - reviews
  - cmm
  - maturity-models
  - financial-services
  - adoption
  - sec-of-ai
status: open
scope_axis:
  - sec-of-ai
question: "How do the Agentic AI Security CMM and Reference Architecture hold up when a regulated, single-vendor (all-Microsoft) financial institution at early AI maturity tries to adopt them — where does that customer land, and which requirements and costs are mis-calibrated for the profile?"
why_it_matters: "The CMM names a 'customer-support chatbot' archetype as still-TBD and crosswalks only to EU AI Act / ISO 42001 / NIST AI RMF / AIUC-1 — none of which a US financial institution is examined against. A worked regulated-FI adoption pass exposes calibration gaps that the internal stress tests did not."
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[cmm-calibration-stress-test-2026]]"
  - "[[cmm-known-limitations]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[lethal-trifecta]]"
  - "[[inference-exposure]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[agentgateway]]"
  - "[[tenuo-warrant]]"
  - "[[aiuc-1]]"
  - "[[credential-proxy-pattern]]"
sources: []
---

# Agentic AI Security CMM — Regulated-FI Adoption Stress Test (Credit Union)

A worked adoption pass over the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] and the [[agentic-ai-security-reference-architecture|Reference Architecture]] from the perspective of a regulated, single-vendor financial institution at early AI maturity. The lens is a no-nonsense AI-security leader at a mid-size credit union: an all-Microsoft/Azure shop with full enterprise licensing (E5, Security Copilot, Purview, Defender, Sentinel, Entra, Azure OpenAI), one production pilot (a member-facing customer-service assistant doing RAG over a knowledge base and account FAQs), a 1–2 FTE AI-security capacity, and NCUA / FFIEC / GLBA examination exposure.

This complements the [[cmm-calibration-stress-test-2026|internal calibration stress test]] (which fixed the scoring math against five archetypes) and the [[cmm-known-limitations|known-limitations]] page. Those examined the model from the inside. This examines it from the buyer's side, and it partially fills the CMM's own open gap: agent-archetype tailoring for the customer-support chatbot is listed as still-TBD.

## Worked archetype: where the customer-service RAG bot lands

Per the CMM's deployment-shape table, a chatbot targets L3 across all domains and a RAG application targets L4 in D6 and D7. A realistic self-assessment of the bot *today*:

| Domain | Today | Basis |
|---|---|---|
| D1 Governance | L1→L2 | Draft AI policy; no AI risk committee yet |
| D2 Identity | L2 | Entra app identity; not per-agent Agent ID; not zero-credential |
| D3 Control / least-agency | L1–L2 | Few tools; informal HITL on write actions |
| D4 Runtime guardrails | L2–L3 | Prompt Shields + Content Safety default; Groundedness lifts to L3 |
| D5 Egress | L2 | Destination allowlist; no agent-aware gateway, but little egress need |
| **D6 Data, Memory and RAG** | **L1–L2** | **Member-PII oversharing is the real risk; Purview AI half-deployed** |
| D7 Observability | L2–L3 | Sentinel/Defender ingest; no behavioral baselines or multi-tool red team |
| D8 Supply chain / AI-BOM | L1–L2 | Manual model/version tracking; no AI-BOM |
| D9 Operations | L1–L2 | No formal decommission or HITL-fatigue tracking |

Applying the [[agentic-ai-security-cmm-dependency-rules|effective-score dependency caps]] (D2→D5, D2→D7, D3→D4): weak per-agent identity and control pull egress, observability, and runtime down, which is a fair reflection of reality. The three-number summary lands near **L2 typical / L1 weakest (D6, D8) / L3 strongest (D4)**.

The reassuring finding sits underneath the score: the path to "good enough" for this archetype is mostly *not* security tooling. It is a governance committee (D1), a data-classification-and-oversharing project in Purview (D6), and switching on logging the org already pays for (D7). The L5 stack — capability [[tenuo-warrant|Warrants]], mesh [[agentgateway|AgentGateway]] sidecars, real-time AI-BOM, AIUC-1 certification — is irrelevant to the next twelve months. The [[lethal-trifecta|lethal-trifecta]] test is the lever that makes this defensible: if the bot is kept from external-comms reach, the trifecta is broken by architecture and a lower D5/D7 score is sound rather than negligent.

## Gaps and calibration issues surfaced

> [!gap] 1. No regulated-FI standards crosswalk (the biggest gap)
> The CMM crosswalks to EU AI Act, ISO 42001, NIST AI RMF, MITRE ATLAS, OWASP ASI, and [[aiuc-1|AIUC-1]]. A US financial institution is examined against none of those. There is no mapping to FFIEC interagency guidance, NCUA third-party-due-diligence expectations, the GLBA Safeguards Rule, or model-risk-management practice (SR 11-7-style, which examiners increasingly borrow). For a model that wants adoption by regulated FIs, this omission outweighs any missing control: an examiner will not accept "the CMM says L4," and the domains do not speak to the obligations the institution is actually graded on.

> [!gap] 2. Microsoft-stack coverage gaps are real but narrow (corrected 2026-05-23)
> The first draft of this point overstated the gap ("no Microsoft AI gateway; must go off-stack for the egress plane"). Verification against current Microsoft documentation corrects it. **Azure API Management's AI Gateway is GA** and is a genuine agent-aware LLM gateway (token-limit and token-metric policies, semantic caching, inline Azure AI Content Safety, backend load-balancing), and it **brokers MCP servers with Entra / OAuth 2.0 / JWT authorization at GA**; [[microsoft-entra-agent-id|Entra Internet Access]] adds network-layer prompt-injection and Shadow-AI egress filtering. So the LLM-traffic gateway and MCP authorization are covered natively. The genuine residual gaps an all-Microsoft buyer fills off-stack are narrow: (a) **MCP tool-integrity / rug-pull defense** — Microsoft's own OWASP-MCP-for-Azure guidance states "there is no single Azure service dedicated to MCP-specific protection"; (b) **per-task capability tokens** — Entra Agent ID issues per-*agent-identity* scoped tokens (OBO), not per-*task* holder-bound Warrant-style grants ([[tenuo-warrant|Tenuo Warrant]]-class); (c) **agent-to-agent (A2A) authorization beyond identity** (message signing, cross-agent ACLs, content-scanning rule packs) is thin. The RA's Egress row also omits APIM AI Gateway entirely and should add it. The lesson, which the wiki owner flagged: confirm a platform capability against current docs before asserting absence.

> [!gap] 3. L5 assumes a procurement cadence regulated FIs cannot follow
> Several L5 criteria point at products that reached GA within weeks of the CMM revision ([[microsoft-entra-agent-id|Agent 365]] May 1, Okta for AI Agents Apr 30). In a regulated institution, "GA three weeks ago" means twelve to eighteen months out after vendor risk assessment, SOC-2 review, and board sign-off. L5 is therefore unreachable on *cadence*, not capability — and the slow cadence is the examiner-approved behavior. A maturity model that effectively penalizes prudent third-party-risk discipline has a calibration problem for the entire regulated sector, not just one institution.

> [!gap] 4. Right-sizing guidance is buried; vendor-neutrality is an integration tax for single-stack shops
> The genuinely useful guidance — apply per application, default L4 with selective L5, dependency-resolved scoring, the architectural-containment carve-out — is real but sits under 400+ lines weighted toward L5/L5+ aspiration. For a single-stack buyer, the prized vendor-neutrality inverts into an integration tax: a 40-tool neutral catalog is work the buyer must redo to find the six controls already in the Microsoft tenant. The hyperscaler-locked framing the RA criticizes (Microsoft ZT4AI, "700+ controls, Azure-locked") is, for this buyer, the more actionable artifact.

> [!gap] 5. No external authority; the L5 AIUC-1 anchor is not regulator-recognized
> The CMM is a synthesis (`attributed_to: Anton Goncharov + Claude`), not a recognized standard, and it says so. That is fine for an internal checklist, but it caps the L5 governance criterion: "AIUC-1 certified against the latest quarterly refresh" is meaningless to an NCUA examiner who has never heard of AIUC-1. ISO 42001 carries more weight; AIUC-1 carries little in this sector.

> [!gap] 6. The cost model under-tells the dominant costs
> The implementation roadmap is framed around control coverage and tooling. For a fully-licensed Microsoft shop the licensing delta is near zero; the dominant costs are elsewhere and largely unaddressed: (a) the **data-governance project** — classifying member data and remediating oversharing in Purview is a multi-quarter, people-owned effort and the true bottleneck (see [[inference-exposure|Inference / Retrieval Exposure]]); (b) **log-ingestion spend** — the RA's own note that agents emit 10–20× human log volume is a recurring Sentinel/Security-Copilot bill that scales with every agent; (c) **headcount** to operationalize one application's governance, logging, and red-team. The expensive, slow work is data governance and labor, not tool purchase.

## Well-calibrated areas

The model earns credit on several points the buyer values. The PDP/PEP/PIP decomposition maps to the ABAC model the institution already runs for human IAM, so the architecture is familiar rather than alien. The `governance ≠ security` distinction is exactly what the board and examiners probe. Dependency-resolved scoring (replacing the single floor) avoids punishing a sensibly contained design. The [[lethal-trifecta|lethal-trifecta]] structural test is the strongest right-sizing instrument in either document. Several per-domain requirements are already satisfied by the Microsoft stack at low marginal cost: D2 zero-credential via Managed Identities (the [[credential-proxy-pattern|credential-proxy]] tool list is moot), D4 runtime via Prompt Shields and Content Safety, D7 ingest via Sentinel and Defender, and PyRIT (Microsoft OSS) covering one leg of the D7 red-team requirement.

## Closure conditions

- A **regulated-FI deployment profile** for the customer-service RAG-bot archetype: the eight controls that matter, the Microsoft product delivering each, the two genuine gaps (D6 oversharing, D5 agent-egress), the log-ingestion cost line, and a column mapping to FFIEC / GLBA / NCUA third-party-risk expectations.
- A **FFIEC / NCUA / GLBA / model-risk crosswalk** added to the [[agentic-cmm-vs-standards-validation|standards crosswalk]], alongside the existing EU-centric and ISO mappings.
- A **single-stack reading** of the RA per major platform (Microsoft-only, AWS-only, GCP-only) that names where the platform has no native control and an off-stack component is unavoidable.
- A **cost-model section** in the CMM roadmap that separates licensing (often near-zero for incumbents) from data-governance labor and log-ingestion run-rate (the real spend).

## Status Notes

Filed 2026-05-23 from a customer-persona stress test of the CMM and RA. Gaps 1 and 6 are the highest-value adds; gap 2 is a finding already latent in the RA's own enterprise-stack table. None requires new external research to act on — they are authoring and crosswalk tasks against artifacts the wiki already holds.

Downstream resolution: gap 6's D6 finding was acted on in [[agentic-ai-security-cmm-d6-data-rag|the D6 Data, Memory and RAG deep dive]], which reframed the domain around answer-time oversharing and inference exposure, made entitlement enforcement the L3 capability, and stated the Purview data-governance project as a multi-quarter labor cost. The findings above stand as filed; this note records where they landed.
