---
type: framework
title: "Microsoft Secure Development Lifecycle (SDL)"
address: c-000045
created: 2026-05-14
updated: 2026-07-30
tags:
  - framework
  - secure-sdlc
  - microsoft
  - secure-by-design
  - threat-modeling
  - sdl
status: developing
scope_axis:
  - sec-of-ai
  - sec-against-ai
framework_class: secure-sdlc
issuer: "[[microsoft|Microsoft]]"
homepage: https://www.microsoft.com/en-us/securityengineering/sdl/
related:
  - "[[microsoft]]"
  - "[[microsoft-sdl-evolving-security-practices]]"
  - "[[yonatan-zunger]]"
  - "[[microsoft-zt4ai]]"
  - "[[microsoft-rai]]"
  - "[[microsoft-secure-agentic-ai-end-to-end]]"
  - "[[nist-ssdf]]"
  - "[[owasp-samm]]"
  - "[[google-saif|Google SAIF — Secure AI Framework]]"
  - "[[secure-sdlc-framework-stack-2026]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[threat-modeling-for-ai]]"
  - "[[agent-memory-isolation]]"
  - "[[agent-identity-architecture]]"
  - "[[distributed-kill-switch]]"
  - "[[supply-chain-security-for-agents]]"
  - "[[securing-agentic-coding]]"
  - "[[generative-coding-deployment-shape-2026]]"
sources:
  - "https://www.microsoft.com/en-us/securityengineering/sdl/"
  - "[[.raw/articles/microsoft-sdl-evolving-security-practices-2026-02-03.md]]"
---

# Microsoft Secure Development Lifecycle (SDL)

**Microsoft SDL**, Microsoft's secure-by-design software development framework, codifies a set of security activities applied across the software lifecycle (training, requirements, design, implementation, verification, release, response). Originally published in 2004 as a response to the 2002 Trustworthy Computing memo, SDL is one of the foundational influences on the modern secure-SDLC field and is referenced as a precursor by [[nist-ssdf|NIST SSDF (SP 800-218)]], [[owasp-samm|OWASP SAMM]], and ISO/IEC 27034. The framework is published publicly at `microsoft.com/en-us/securityengineering/sdl/` as a set of practice descriptions, training material, and reference tooling (including the Microsoft Threat Modeling Tool).

On **2026-02-03**, Microsoft published [[microsoft-sdl-evolving-security-practices|*Microsoft SDL: Evolving Security Practices for an AI-Powered World*]] (Yonatan Zunger), announcing the explicit extension of SDL to AI workloads. This page tracks both the classical SDL framework and its 2026 AI extension as a single anchor; the per-area substantive AI guidance is promised "in the coming months."

> [!gap] Pre-2026 SDL coverage on this page is summary-level
> The dedicated SDL practice areas — Training, Requirements, Design, Implementation, Verification, Release, Response — are documented on Microsoft's SDL homepage but not fully transcribed here. The wiki's coverage focuses on the 2026 AI extension and the framework's role in the broader secure-SDLC ecosystem. Track the per-area follow-up posts as they land.

## Position in the secure-SDLC ecosystem

Microsoft SDL is **vendor-authored, prescriptive, free, and pre-AI by inheritance**. Read its strengths and limits relative to the three frameworks that share secure-SDLC scope:

| Framework | Class | Issuer | Position in stack |
|---|---|---|---|
| **Microsoft SDL** | Secure-SDLC framework (vendor-authored) | [[microsoft\|Microsoft]] | Practice-oriented; concrete tooling; explicit named source for SSDF (cited as `MSSDL` in NIST SP 800-218 Table 1) |
| [[nist-ssdf\|NIST SSDF (SP 800-218)]] | Secure-SDLC framework (standard) | [[nist\|NIST]] | Outcomes-oriented; federal regulatory anchor (EO 14028); synthesizes Microsoft SDL + BSAFSS + OWASP SAMM + others |
| [[nist-sp-800-218a\|NIST SP 800-218A]] | SSDF Community Profile for AI | [[nist\|NIST]] | Federal AI extension of SSDF (EO 14110); ~6 months ahead of the Microsoft SDL-for-AI announcement |
| [[owasp-samm\|OWASP SAMM v2]] | Secure-SDLC maturity model | OWASP | 5 functions × 3 practices × 3 levels; vendor-neutral assessment |
| BSIMM | Secure-SDLC industry benchmark | Synopsys / Black Duck | Descriptive industry benchmark; survey of observed practices |

Where SDL fits: it is the **vendor reference implementation** of secure-SDLC practice. Organizations rarely adopt SDL wholesale (its tooling and process specifics are Microsoft-centric), but they routinely borrow its threat-modeling discipline, requirements taxonomy, and lifecycle activity model. SDL is the historical anchor of the field; SSDF is the modern outcomes-based standard; SAMM is the maturity assessment instrument.

## The 2026 AI Extension: Six focus areas

Per [[microsoft-sdl-evolving-security-practices|the 2026-02-03 announcement]], Microsoft's SDL for AI introduces specialized guidance and tooling in six areas. Each maps cleanly to the wiki's [[agentic-ai-security-cmm-2026|CMM]] domain structure:

| SDL-for-AI focus area | Wiki concept anchor | CMM domain | RA plane |
|---|---|---|---|
| **Threat modeling for AI** | [[threat-modeling-for-ai\|Threat modeling for AI]] *(gap)* | D4 — Threat Modeling & Adversarial Defense | Control |
| **AI system observability** | [[agent-observability\|Agent observability]] | D7 — Observability & Anomaly Detection | Observability |
| **AI memory protections** | [[agent-memory-isolation\|Agent memory isolation]] | D5 — Data & Memory Governance | Identity / Data |
| **Agent identity and RBAC enforcement** | [[agent-identity-architecture\|Agent identity architecture]] | D3 — Identity & Access Management | Identity |
| **AI model publishing** | [[supply-chain-security-for-agents\|Supply chain security for agents]] | D6 — Supply Chain & Component Governance | Identity / Egress |
| **AI shutdown mechanisms** | [[distributed-kill-switch\|Distributed kill switch]] | D9 — Incident Response & Recovery | Observability / Egress |

The six-area scope is unusually clean: each area maps to a distinct CMM domain with negligible overlap, and three of the six (memory protections, agent identity & RBAC, shutdown mechanisms) had not previously been named as first-order concerns by any major-vendor secure-SDLC framework.

## The 2026 AI Extension: Six operating pillars

Microsoft also names six **pillars** that describe *how* SDL for AI operates, the meta-process scaffolding rather than the per-area content:

- **Research**: staying ahead of evolving cyberthreat landscape (prompt injection, model poisoning).
- **Policy**: living documents that adapt based on research and incidents; integrated from start, revisited throughout the lifecycle.
- **Standards**: technical and operational standards translating policy into actionable practices and design patterns.
- **Enablement**: tools, communications, and training to implement security measures effectively.
- **Cross-functional collaboration**: explicitly broadens SDL ownership beyond the security organization, including Business Process and Application UX teams.
- **Continuous improvement**: real-world feedback loops to refine strategies, update standards, and evolve policies and training.

The pillar set is similar in shape to [[google-saif|Google SAIF]]'s six core elements and OWASP SAMM's five functions. The distinctive Microsoft choice is **Cross-functional collaboration** as a named pillar, explicitly bringing in non-security teams as first-order participants.

## "A way of working, not a checklist"

The 2026 announcement's load-bearing thesis statement: *"Security policy falls short of addressing real-world cyberthreats when it is treated as a list of requirements to be mechanically checked off."* This framing directly addresses the most common organizational anti-pattern in secure-SDLC adoption: treating the framework as a compliance instrument rather than living guidance.

The framing has practical implications:
- Policies must evolve through tight feedback loops with engineering: *"co-creating requirements, threat modeling together, testing mitigations in real workloads, and iterating quickly."*
- AI-specific risks lack decades of hardened best practices; requirements must be adaptable to the specific scenario while preserving necessary security properties.
- Frictionless experiences via automation and templates; guidance positioned as partnership, not policing.

Concept anchor: see [[cybersecurity-cmms-exemplars|Cybersecurity CMMs Exemplars]] §Living guidance for the broader argument that prescriptive maturity models work only as living instruments.

## In the RA / CMM

Microsoft SDL is a **policy-anchor framework** at the program level. It lives above the wiki's [[agentic-ai-security-reference-architecture|RA]] (which describes runtime control planes) and below the [[agentic-ai-security-cmm-2026|CMM]] (which scores maturity). In a 2026 secure-SDLC stack assembled per the [[secure-sdlc-framework-stack-2026|framework-stack thesis]], SDL plays the role that NIST SSDF plays in the canonical recommendation: a vendor-authored, prescriptive secure-SDLC anchor with a tooling and process model engineering teams can directly borrow.

**Microsoft SDL ≈ "vendor-authored SSDF".** For organizations already in the Microsoft developer ecosystem (Visual Studio, GitHub, Azure DevOps, Azure AI Foundry), SDL is functionally a vendor-tooled version of SSDF — the same outcome scope but with concrete Microsoft tooling at every stage. [[nist-ssdf|NIST SSDF v1.1]]'s reference column cites `MSSDL` as a named source for at least 8 SSDF tasks, making the Microsoft SDL → SSDF lineage documented rather than asserted.

> [!check] Federal AI-extension exists in parallel — [[nist-sp-800-218a|SSDF Community Profile (SP 800-218A)]] preceded Microsoft SDL for AI by ~18 months
> Earlier wiki revisions described Microsoft SDL for AI as the "first major-vendor secure-SDLC framework with an explicit AI scope." More precisely: Microsoft SDL for AI (Feb 2026) is the first major-**vendor** classical-SDLC AI extension. NIST SP 800-218A (July 2024) is the federal AI extension, published ~18 months earlier with substantive Table-1-level content. The two are complementary instruments — vendor-tooled engineering practice vs. federal-procurement-attestation surface — not competing claims. See [[nist-sp-800-218a|the 218A page]] for the federal anchor.

## Placement in this wiki

- **First dedicated SDL framework page** on the wiki. Closes a long-standing gap where ZT4AI and Agent 365 were tracked but the underlying SDL framework was not.
- **Vendor-implementation evidence for the [[secure-sdlc-framework-stack-2026|framework-stack thesis]].** Microsoft's anchor + AI overlay pattern is a production-deployed instance of what the thesis prescribes. Updates filed on the thesis page noting the new evidence.
- **Anchors the Microsoft SDL ↔ NIST SSDF lineage** for any future ingest concerning secure-SDLC frameworks.

The 2026 AI extension's six focus areas govern agents an organization *ships*: threat modeling for AI, AI observability, memory protections, agent identity and RBAC, model publishing, and shutdown. The agent running on its own developers' workstations — holding repository credentials, executing shell commands, and reading attacker-reachable repository content — is a different subject that the extension does not reach. See [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack for 2026]] §Gap 4 and [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]].

## See also

- [[microsoft-sdl-evolving-security-practices|Microsoft SDL: Evolving Security Practices for an AI-Powered World (2026-02-03)]] — the announcement paper
- [[microsoft|Microsoft]] — issuing organization
- [[yonatan-zunger|Yonatan Zunger]] — author of the 2026 announcement
- [[nist-ssdf|NIST SSDF — Secure Software Development Framework]] — peer secure-SDLC standard
- [[owasp-samm|OWASP SAMM]] — peer maturity model
- [[google-saif|Google SAIF — Secure AI Framework]] — peer AI-extended secure-development framework
- [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack for 2026]] — thesis on how SDL / SSDF / SAMM / AI overlays compose
- [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] — adjacent thesis
- [[microsoft-zt4ai|Microsoft ZT4AI]] — sibling Microsoft framework, focused on zero-trust for AI
- [[microsoft-rai|Microsoft Responsible AI Standard]] — sibling Microsoft governance framework
- [[microsoft-secure-agentic-ai-end-to-end|Microsoft Secure Agentic AI End-to-End]] — broader 2026 portfolio announcement

<!-- sources:auto -->
## Sources

- [Microsoft Secure Development Lifecycle (SDL)](https://www.microsoft.com/en-us/securityengineering/sdl/)
- [Microsoft SDL: Evolving security practices for an AI-powered world](https://www.microsoft.com/en-us/security/blog/2026/02/03/microsoft-sdl-evolving-security-practices-for-an-ai-powered-world/)
<!-- /sources -->
