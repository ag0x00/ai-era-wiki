---
type: framework
title: "CSA MAESTRO / CSA Agentic Trust Framework"
created: 2026-04-30
updated: 2026-08-20
tags:
  - frameworks
  - csa
  - agentic-ai
  - zero-trust
  - trust-framework
status: developing
source_url: "https://cloudsecurityalliance.org/blog/2026/02/02/the-agentic-trust-framework-zero-trust-governance-for-ai-agents"
scope_axis:
  - sec-of-ai
adoption_signal: active
last_substantive_update: 2026-02-02
published_by: "[[csa|Cloud Security Alliance (CSA)]]"
current_version: "MAESTRO (Feb 6, 2025); Agentic Trust Framework v1.0 (Feb 2, 2026)"
first_published: "2025-02-06"
scope: "MAESTRO seven-layer threat model; ATF Zero Trust governance (5 elements, 4 maturity levels, 5 promotion gates)"
audience: "Enterprise security architects, AI platform teams, risk and compliance"
aliases:
  - "CSA ATF"
  - "MAESTRO"
  - "CSA Agentic Trust Framework"
related:
  - "[[csa|CSA]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[nist-ai-rmf]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[standards-review-csa-maestro-atf-2026-Q2]]"
  - "[[threat-modeling-for-ai]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[precize-agentic-ai-top10]]"
primary_documents:
  - title: "Agentic AI Threat Modeling Framework: MAESTRO"
    url: "https://cloudsecurityalliance.org/blog/2025/02/06/agentic-ai-threat-modeling-framework-maestro"
    version: "MAESTRO (Feb 6, 2025)"
    retrieved: "2026-06-22"
    scope_in_wiki: "Seven-layer architecture (L1-L7); MAESTRO acronym; cross-layer threats"
  - title: "The Agentic Trust Framework: Zero Trust Governance for AI Agents"
    url: "https://cloudsecurityalliance.org/blog/2026/02/02/the-agentic-trust-framework-zero-trust-governance-for-ai-agents"
    version: "ATF v1.0 (Feb 2, 2026)"
    retrieved: "2026-06-22"
    scope_in_wiki: "5 core elements; 4 maturity levels; 5 promotion gates; ZT (NIST 800-207) basis"
  - title: "agentic-trust-framework (reference implementation)"
    url: "https://github.com/massivescale-ai/agentic-trust-framework"
    retrieved: "2026-06-22"
    scope_in_wiki: "Element definitions; maturity-level autonomy/oversight table; demotion rule"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# CSA MAESTRO / CSA Agentic Trust Framework

This page covers two Cloud Security Alliance agentic-security publications. **MAESTRO** (Multi-Agent Environment, Security, Threat, Risk, and Outcome)[^maestro] is a seven-layer threat-modeling framework published February 6, 2025; it partitions an agentic system into layers and enumerates threats per layer and across layers. The **CSA Agentic Trust Framework** (ATF v1.0, February 2, 2026)[^atf] applies Zero Trust governance to autonomous AI agents: five core elements answer five trust questions, four maturity levels grade earned autonomy, and five promotion gates govern advancement between levels. MAESTRO is a threat-modeling methodology, not a control catalogue; ATF is a governance model, not a maturity model with graded controls. The structure, layer names, and gate names below are verified against the primary sources by [[standards-review-csa-maestro-atf-2026-Q2|the 2026-Q2 standards review]].

**CSAI Foundation** (March 23, 2026): a 501(c)(3) spun from CSA with six strategic programs including an AI Risk Observatory and "Valid-AI-ted" AI-driven audit engine. These are organizational initiatives, not part of MAESTRO or ATF.

The [[precize-agentic-ai-top10|Precize Top 10 for Agentic AI Vulnerability]] ([first published February 2025](https://github.com/precize/Agentic-AI-Top10-Vulnerability), the same month as MAESTRO) states its own purpose as "the core for OWASP and CSA Red teaming work" — a claim recorded, with its single-author-group limits, on that page. No citation from CSA acknowledging the repository as an input has been located in this pass; the temporal proximity to MAESTRO's own February 2025 publication is consistent with the claim but does not establish it.

## MAESTRO: Seven-Layer Threat Model

MAESTRO[^maestro] expands to **Multi-Agent Environment, Security, Threat, Risk, and Outcome**. It partitions an agentic system into seven layers:

| Layer | Name | Scope |
|---|---|---|
| Layer 1 | Foundation Models | Core models (LLMs) underlying agent function |
| Layer 2 | Data Operations | Processing, storage, RAG pipelines, databases |
| Layer 3 | Agent Frameworks | Toolkits/frameworks used to build agents |
| Layer 4 | Deployment and Infrastructure | Cloud/on-prem systems where agents run |
| Layer 5 | Evaluation and Observability | Monitoring and performance assessment |
| Layer 6 | Security and Compliance | Vertical layer integrating controls across all layers |
| Layer 7 | Agent Ecosystem | Marketplace where agents meet applications and users |

MAESTRO models threats that span layers: supply-chain compromise of one layer affecting others, lateral movement, cross-boundary privilege escalation, inter-layer data leakage, and goal-misalignment cascades.

## CSA Agentic Trust Framework (ATF)

The ATF v1.0[^atf] applies Zero Trust governance to autonomous agents. Its Zero Trust basis is cited as **NIST SP 800-207**[^atfgh]: no default autonomy; trust is earned and continuously verified. The framework has three distinct constructs.

**Five core elements (pillars)** answer five trust questions:

1. **Identity** — "Who are you?"
2. **Behavior** — "What are you doing?"
3. **Data Governance** — "What are you eating? What are you serving?"
4. **Segmentation** — "Where can you go?"
5. **Incident Response** — "What if you go rogue?"

**Four maturity levels** grade earned autonomy. An agent can be demoted; a critical incident triggers immediate demotion to Intern.[^atfgh]

1. **Intern** — observe + report
2. **Junior** — recommend + approve
3. **Senior** — act + notify
4. **Principal** — autonomous within domain

**Five promotion gates** must be passed to advance a level: **Performance, Security Validation, Business Value, Incident Record, Governance Sign-off**.

> [!gap] Gates name criteria categories without thresholds
> The five promotion gates name *what* must be demonstrated, not *how much*. Per-element controls are described, not specified to pass/fail criteria. The 2026-Q2 review confirmed no published gate threshold or scored rubric in either the ATF blog or the GitHub reference.

## Coverage Against OWASP ASI Top 10

This table scores ATF against its five **elements**, not the promotion gates. It is a re-scoring driven by the gate→element correction in [[standards-review-csa-maestro-atf-2026-Q2|the 2026-Q2 review]]: the elements are the constructs that map to risk categories; the gates govern level advancement.

| ASI Category | Coverage |
|---|---|
| ASI01: Agent Goal Hijack | ◐ Partial (Behavior + Incident Response elements) |
| ASI02: Tool Misuse | ◐ Partial (Segmentation element bounds blast radius) |
| ASI03: Identity & Privilege | ● Identity element |
| ASI04: Supply Chain | ○ None |
| ASI05: Unexpected Code Execution (RCE) | ◐ Partial (Segmentation element limits blast radius) |
| ASI06: Memory Poisoning | ◐ Partial (Data Governance element — input/output validation) |
| ASI07: Insecure Inter-Agent | ◐ Partial |
| ASI08: Cascading Failures | ◐ Partial (Segmentation element) |
| ASI09: Human-Agent Trust Exploitation | ○ None (elements bound agent autonomy; no human-trust-exploitation coverage) |
| ASI10: Rogue Agents | ● Behavior + Incident Response elements (containment, demotion) |

## CSAI Foundation Programs (March 23, 2026)

1. **AI Risk Observatory**: centralized risk tracking
2. **Valid-AI-ted**: AI-driven audit engine
3. **AI Controls Matrix expansion**: adding ISO 42001, ISO 27001, and SOC 2 mappings to AI-specific controls; could provide the first unified compliance mapping across multiple standards
4. Three additional programs (details pending)

## Strengths

- The four-level maturity model (Intern → Junior → Senior → Principal), gated by five promotion gates, addresses the "Least Agency" principle with a structured earned-autonomy progression
- Identity and rogue-agent categories (ASI03, ASI10) receive strong coverage via the Identity and Incident Response elements
- CSAI Foundation's AI Controls Matrix expansion could resolve the multi-standard compliance mapping gap
- AI Risk Observatory could become a valuable threat intelligence resource

## Gaps and Shortcomings

- **Newest framework**: limited operational validation
- Supply chain (ASI04) has no coverage in either MAESTRO (named as a cross-layer threat only) or ATF
- Promotion gates name criteria categories without measurable thresholds
- No certifiable standard, guidance only
- The AI Controls Matrix expansion is a roadmap item, not yet delivered

## See Also

- [[csa|Cloud Security Alliance]] (publisher)
- [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]] — risk taxonomy that ATF is designed to govern
- [[nist-ai-rmf|NIST AI Risk Management Framework (AI RMF)]] — governance complement; NIST RMF provides the federal baseline, ATF addresses agentic specifics
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — MAESTRO Layer 2 (Data Operations) → **D6**; Layer 1 (Foundation Models) + Layer 3 (Agent Frameworks) → **D4**; Layer 4 (Deployment and Infrastructure) + Layer 7 (Agent Ecosystem) → **D5**; Layer 5 (Evaluation and Observability) → **D7**; Layer 3 supply-chain threat → **D8**. ATF maps via its five **elements** (Identity → D2, Behavior → D4/D7, Data Governance → D6, Segmentation → D3/D5, Incident Response → D9); the four maturity levels and five promotion gates inform **D3** (gates name criteria categories without thresholds, see CMM)
- [[standards-review-csa-maestro-atf-2026-Q2|CSA MAESTRO and ATF Standards Review]] — primary-source verification of layer/element/gate names and the gate→element correction
- [[threat-modeling-for-ai|Threat Modeling for AI]] — uses MAESTRO as the layered-decomposition lens; [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] maps the seven layers alongside the ASI, T-code, and ATLAS taxonomies

## Notes

[^maestro]: [CSA — Agentic AI Threat Modeling Framework: MAESTRO](https://cloudsecurityalliance.org/blog/2025/02/06/agentic-ai-threat-modeling-framework-maestro), retrieved 2026-06-22. Acronym: Multi-Agent Environment, Security, Threat, Risk, and Outcome. Layers: L1 Foundation Models, L2 Data Operations, L3 Agent Frameworks, L4 Deployment and Infrastructure, L5 Evaluation and Observability, L6 Security and Compliance, L7 Agent Ecosystem.
[^atf]: [CSA — The Agentic Trust Framework: Zero Trust Governance for AI Agents](https://cloudsecurityalliance.org/blog/2026/02/02/the-agentic-trust-framework-zero-trust-governance-for-ai-agents), retrieved 2026-06-22. Five elements: Identity, Behavior, Data Governance, Segmentation, Incident Response. Five gates: Performance, Security Validation, Business Value, Incident Record, Governance Sign-off. Four levels: Intern, Junior, Senior, Principal.
[^atfgh]: [agentic-trust-framework — massivescale-ai (GitHub)](https://github.com/massivescale-ai/agentic-trust-framework), retrieved 2026-06-22. NIST SP 800-207 Zero Trust basis; demotion-to-Intern on critical incident.

<!-- sources:auto -->
## Sources

- [CSA MAESTRO / CSA Agentic Trust Framework](https://cloudsecurityalliance.org/blog/2026/02/02/the-agentic-trust-framework-zero-trust-governance-for-ai-agents)
- [Agentic AI Threat Modeling Framework: MAESTRO](https://cloudsecurityalliance.org/blog/2025/02/06/agentic-ai-threat-modeling-framework-maestro)
- [agentic-trust-framework (reference implementation)](https://github.com/massivescale-ai/agentic-trust-framework)
<!-- /sources -->
