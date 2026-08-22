---
type: framework
title: "Microsoft Responsible AI Standard (RAI)"
created: 2026-04-30
updated: 2026-08-21
tags:
  - frameworks
  - microsoft
  - responsible-ai
  - zero-trust
  - agentic-ai
status: developing
source_url: "https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/microsoft/final/en-us/microsoft-brand/documents/Microsoft-Responsible-AI-Standard-General-Requirements.pdf"
scope_axis:
  - sec-of-ai
adoption_signal: active
last_substantive_update: 2026-03-01
published_by: "Microsoft"
current_version: "RAI Standard v2 (2022)"
first_published: "2022"
scope: "Microsoft's responsible-AI goals standard — seventeen goals across six principles. The control-catalogue and product-plane content lives in ZT4AI and Agent 365, on their own pages."
audience: "Enterprise security architects, Microsoft ecosystem users, Copilot/Azure AI deployers"
aliases:
  - "Microsoft RAI"
  - "Responsible AI Standard"
related:
  - "[[microsoft-zt4ai]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[google-saif]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[nist-ai-rmf]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
coined_by:
  - "[[microsoft]]"
---

# Microsoft Responsible AI Standard (RAI)

**Microsoft RAI** is a responsible-AI goals standard, not a security-control catalogue. The RAI Standard v2 (2022) sets seventeen goals across six principles — fairness, reliability and safety, privacy and security, inclusiveness, transparency, and accountability. Each goal states a required outcome (an impact assessment completed, an AI interaction disclosed, a failure remediated), and defers the enforcing control to Microsoft's Security Development Lifecycle, the Privacy Standard, and, since 2026, the [[microsoft-zt4ai|Zero Trust for AI (ZT4AI)]] control catalogue and the [[microsoft-entra-agent-id|Agent 365]] control plane.

> [!contradiction] This page previously conflated RAI with ZT4AI and Agent 365
> Earlier revisions described RAI as "the most control-rich AI security framework available — 700+ specific controls" and dated Agent 365 to "March 9, 2026". Both belong elsewhere. The 700+-control figure is the [[microsoft-zt4ai|ZT4AI]] catalogue (and even there it is the whole Zero-Trust Workshop, not the AI pillar). Agent 365 launched at Ignite 2025 (2025-11-18) and reached GA 2026-05-01. RAI v2 itself defines seventeen goals. See [[standards-review-microsoft-rai-agent-365-2026-Q2|the RAI / Agent 365 standards review]] and [[standards-review-microsoft-zt4ai-2026-Q2|the ZT4AI review]].

## The seventeen goals

RAI v2's General Requirements enumerate goals under five of the six principles (Inclusiveness is operationalized through accessibility and fairness requirements rather than a distinct lettered series):

- **Accountability** — A1 Impact Assessment, A2 Oversight of significant adverse impacts, A3 Fit for purpose, A4 Data governance and management, A5 Human oversight and control
- **Transparency** — T1 System intelligibility for decision making, T2 Communication to stakeholders, T3 Disclosure of AI interaction
- **Fairness** — F1 Quality of service, F2 Allocation of resources and opportunities, F3 Minimization of stereotyping, demeaning, and erasing outputs
- **Reliability & Safety** — RS1 Reliability and safety guidance, RS2 Failures and remediations, RS3 Ongoing monitoring, feedback, and evaluation
- **Privacy & Security** — PS1 Privacy Standard compliance

The Accountability and Transparency goals carry the load: they anchor the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] governance domain (D1), with A5 and the human-oversight requirements reaching operations and human factors (D9). The standard names no agent-identity, least-agency, egress, or runtime control — those map from ZT4AI and Agent 365, not from RAI.

## Location of the security substance

RAI is the goals layer. The technical controls and the management plane are separate instruments with their own pages:

- **[[microsoft-zt4ai|Zero Trust for AI (ZT4AI)]]** — the control catalogue (Prompt Shields, Purview answer-time entitlement, Defender runtime protection, the Agent Governance Toolkit). Reviewed in [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]].
- **[[microsoft-entra-agent-id|Agent 365]]** — the agent-management control plane (registry, access control, visualization, interoperability, security), launched at Ignite 2025 (2025-11-18), GA 2026-05-01.

## OWASP ASI Top 10 Mapping

Microsoft published detailed mapping (March 30, 2026) of OWASP ASI categories to specific Copilot Studio mitigations. Microsoft AI Red Team members served on the OWASP Expert Review Board for the ASI Top 10. This is the most operationalized ASI implementation of any vendor.

## FIDES Research Framework

The **[FIDES]()** research framework (May–September 2025, still active) demonstrated **zero successful prompt injection attacks** using information-flow control with dynamic taint-tracking, evaluated on the [AgentDojo]() benchmark. This is the strongest published defensive result against prompt injection. FIDES is not a productized capability yet.

## Coverage Against OWASP ASI Top 10

| ASI Category | Coverage |
|---|---|
| ASI01: Agent Goal Hijack | ● Specific controls |
| ASI02: Tool Misuse | ◐ Partial |
| ASI03: Identity & Privilege | ● Specific controls (Entra) |
| ASI04: Supply Chain | ◐ Partial |
| ASI05: Unexpected Code Execution (RCE) | ◐ Partial (Defender AI runtime protection, sandboxing) |
| ASI06: Memory Poisoning | ◐ Partial |
| ASI07: Insecure Inter-Agent | ◐ Partial |
| ASI08: Cascading Failures | ◐ Partial |
| ASI09: Human-Agent Trust Exploitation | ◐ Partial (RAI HAX over-reliance guidance) |
| ASI10: Rogue Agents | ● Specific controls |

## Strengths

- A clear, published responsible-AI goals standard — seventeen goals across six principles, durable since 2022
- Strong governance and transparency content (Accountability and Transparency goal series) that grounds the CMM D1 domain
- [OWASP ASI](owasp-asi) mapping demonstrates practical operationalization with Copilot Studio-specific guidance, connecting the goals to product mitigations
- The goals carry across the Microsoft stack: ZT4AI controls and Agent 365 plane operationalize the RAI outcomes

## Gaps and Shortcomings

- RAI v2 specifies no technical security controls — the seventeen goals state outcomes, deferring enforcement to the SDL, the Privacy Standard, and (since 2026) ZT4AI / Agent 365
- No agent-identity, least-agency, egress, or runtime goal — A5 governs human control over a system, not machine identity or agent agency
- The 2022 v2 text predates Microsoft's agent-identity work (Entra Agent ID, 2026), so it carries no agent-specific goal
- The control-richness and product-plane content lives in [[microsoft-zt4ai|ZT4AI]] and [[microsoft-entra-agent-id|Agent 365]], which carry their own platform-lock and licensing constraints

## See Also

- [[microsoft-zt4ai|Microsoft Zero Trust for AI (ZT4AI)]] — the control catalogue that operationalizes the RAI goals
- [[microsoft-entra-agent-id|Microsoft Entra Agent ID / Agent 365]] — the agent-management control plane over those controls
- [[standards-review-microsoft-rai-agent-365-2026-Q2|RAI and Agent 365 Standards Review]] — goal- and capability-level coverage against the CMM
- [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]] — ASI Top 10 that Microsoft maps to its product suite
- [[google-saif|Google SAIF — Secure AI Framework]] — comparable agentic security framework from a hyperscaler
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — RAI grounds the governance goals under D1 (Governance & Accountability); ZT4AI Pillar 1 (Agent governance, Entra Agent ID, Agent 365) → **D2**; Pillar 2 (Data + Purview) → **D6**; Pillar 3 (Prompt Shields, FIDES) → **D4**; Sentinel + Defender for Cloud Apps → **D7**
- [[cosai|CoSAI — Coalition for Secure AI]] — collaborative multi-vendor alternative to Microsoft's proprietary stack

<!-- sources:auto -->
## Sources

- [Microsoft Responsible AI Standard (RAI)](https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/microsoft/final/en-us/microsoft-brand/documents/Microsoft-Responsible-AI-Standard-General-Requirements.pdf)
<!-- /sources -->
