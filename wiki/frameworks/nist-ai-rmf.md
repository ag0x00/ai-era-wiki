---
type: framework
title: "NIST AI Risk Management Framework (AI RMF)"
created: 2026-04-30
updated: 2026-08-21
tags:
  - frameworks
  - nist
  - risk-management
  - ai-governance
status: developing
source_url: "https://www.nist.gov/itl/ai-risk-management-framework"
scope_axis:
  - sec-of-ai
adoption_signal: maintained
last_substantive_update: 2024-07-01
published_by: "[[nist|NIST]]"
current_version: "1.0 (January 2023); AI 600-1 GenAI Profile (July 2024)"
first_published: "2023-01-26"
doi: 10.6028/NIST.AI.100-1
primary_documents:
  - title: "NIST AI 100-1 — Artificial Intelligence Risk Management Framework (AI RMF 1.0)"
    url: "https://doi.org/10.6028/NIST.AI.100-1"
    version: "1.0"
    published: "2023-01-26"
    retrieved: "2026-06-21"
    archived_copy: ".raw/papers/nist-ai-100-1-rmf-2023-01-26.pdf"
    scope_in_wiki: "Core functions GOVERN/MAP/MEASURE/MANAGE (Tables 1–4); foundational §3 trustworthiness characteristics"
scope: "Voluntary U.S. standard for managing AI risks across the full AI lifecycle"
audience: "Enterprise AI developers, deployers, operators; federal agencies"
aliases:
  - "AI RMF"
  - "NIST AI RMF 1.0"
related:
  - "[[shadow-automation|Shadow Automation]]"
  - "[[agent-catalog|AI Agent Catalog]]"
  - "[[decision-rights]]"
  - "[[nist-ai-600-1]]"
  - "[[nist-sp-800-218a]]"
  - "[[mitre-atlas]]"
  - "[[iso-iec-42001]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[nist-ai-800-4]]"
  - "[[standards-review-nist-ai-rmf-2026-Q2]]"
  - "[[standards-review-iso-42001-27090-2026-Q2]]"
  - "[[stride-ai-2026]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
coined_by:
  - "[[nist]]"
---

# NIST AI Risk Management Framework (AI RMF)

The **NIST AI RMF** is the de facto voluntary U.S. standard for AI risk management, structured around four core functions: **Govern, Map, Measure, and Manage**. Published January 2023, it serves as a governance touchstone cited by 78% of large enterprises implementing AI solutions. State-level safe harbor provisions in Colorado, Texas, and Virginia reference AI RMF compliance.

## Structure

- **Govern**: Establish organizational accountability, culture, and processes for AI risk
- **Map**: Identify and categorize AI risks in context
- **Measure**: Analyze and assess identified risks
- **Manage**: Prioritize and respond to identified AI risks

The **Generative AI Profile (NIST AI 600-1)**, published July 2024, extended coverage to prompt injection, data poisoning, and model extraction for GenAI systems.

## Q1 2026 Developments

**Status:** AI RMF 1.0 remains the published version. Confirmed "in revision" per the AI Action Plan, but no RMF 1.1 or 2.0 draft has materialized as of April 2026.

**CAISI AI Agent Standards Initiative** (February 17, 2026) is the most consequential development: the first U.S. government program explicitly targeting agentic AI interoperability and security standards. Three pillars:
1. Facilitating industry-led agent standards
2. Conducting research and guidelines
3. Stakeholder engagement

An RFI on AI agent security (January 8, 2026) received responses from the OpenID Foundation, Perplexity, and others. A companion **ITL AI Agent Identity and Authorization Concept Paper** (comments due April 2, 2026) signals NIST views agent identity as a critical near-term gap.

Additional Q1 2026 publications:
- **IR 8605A** (COSAiS annotated outline for predictive AI control overlays, January 8): adapts SP 800-53 controls for AI; future overlays for generative and agentic AI planned but unscheduled
- **Cyber AI Profile (NISTIR 8596)**: completed public comment period January 30; applies CSF 2.0 across three focus areas; will progress to initial public draft in 2026
- **NIST AI 800-4** (March 6): first federal-level report mapping gaps in post-deployment AI monitoring; identifies human factors monitoring as the biggest blind spot across six monitoring categories

## Strengths

- CAISI initiative directly addresses the agentic identity gap
- COSAiS project is the first attempt to provide specific, implementable SP 800-53 control overlays for AI
- Cyber AI Profile bridges AI RMF and CSF 2.0, enabling organizations already using CSF to extend coverage to AI
- Broad adoption and state-level regulatory reference creates accountability mechanisms

## Gaps and Shortcomings

- Describes **"what" rather than "how"**: no testable control requirements with evidence criteria
- Does not distinguish model development from runtime security
- Agentic AI-specific controls acknowledged as a gap but no published guidance exists yet
- MCP/A2A protocol security, plugin/skill supply chains, agent identity management, and cognitive file integrity are completely unaddressed
- No AI incident response specificity (IoCs, playbooks, forensic guidance)
- ML-BOM/AI-BOM requirements absent
- Platform-level vs. prompt-level enforcement distinction not articulated
- COSAiS overlays are annotated outlines, not implementation guides
- GOVERN presumes an enumerable inventory. [[shadow-automation|Shadow automation]] is the direct negation of that premise: an agent adopted by an individual developer has no documented owner, no risk assessment, and no place in the register the function is written against. Returning an unenumerated deployment to the register depends on discovery machinery the framework leaves to the implementer, which leaves its first function inapplicable in exactly the environment where agentic risk accumulates fastest. The [[agent-catalog|AI Agent Catalog]] supplies the per-agent form the inventory requirement lacks: an agent card carrying a unique identity, the tools and data in scope, and an attributed owner, which is what makes a GOVERN inventory claim auditable one agent at a time.
- GOVERN calls for clear lines of accountability over AI systems and does not say what an accountability record for an autonomous action contains. [[decision-rights|Decision rights for AI agents]] supplies that artifact — action class, decision right, approver, justification, time bound — the concrete filing the function asks for.

## Coverage Against OWASP ASI Top 10

| ASI Category | Coverage |
|---|---|
| ASI01: Agent Goal Hijack | ○ None |
| ASI02: Tool Misuse | ○ None |
| ASI03: Identity & Privilege | ○ None |
| ASI04: Supply Chain | ◐ Partial |
| ASI05: Unexpected Code Execution (RCE) | ○ None |
| ASI06: Memory Poisoning | ○ None |
| ASI07: Insecure Inter-Agent | ○ None |
| ASI08: Cascading Failures | ○ None |
| ASI09: Human-Agent Trust Exploitation | ● AI 600-1 Human-AI Configuration (over-reliance, automation bias) |
| ASI10: Rogue Agents | ○ None |

## Watch Items (2026)

- **NIST IR 8605B/C**: COSAiS overlays for generative and agentic AI (SP 800-53 control adaptations); unscheduled but expected H2 2026
- **NISTIR 8596 initial public draft**: Cyber AI Profile bridging CSF 2.0 and AI RMF
- RMF revision (1.1 or 2.0): no draft timeline announced

## See Also

- [[nist|NIST]] (publisher)
- [[nist-ai-600-1|NIST AI 600-1]] — GenAI profile
- [[nist-sp-800-218a|NIST SP 800-218A]] — Secure Software Development Framework for AI
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — anchors **D1 Governance** to AI RMF's Govern function; per-domain crosswalk in [[agentic-ai-security-cmm-crosswalk|Agentic AI Security CMM — Standards Crosswalk Matrix]]
- [[iso-iec-42001|ISO/IEC 42001 — AI Management Systems]] — the certifiable complement; its governance-only / no-agentic-technical-control profile is mapped in [[standards-review-iso-42001-27090-2026-Q2|the 2026-Q2 ISO/IEC 42001 + 27090 review]]
- [[mitre-atlas|MITRE ATLAS]] — adversary technique coverage that NIST lacks
- [[stride-ai-2026|STRIDE-AI Threat Modeling Framework]] — academic threat-modeling method that proposes bridging AI RMF governance to the [[owasp-llm-top-10|OWASP LLM Top 10]] technical taxonomy via an AI-adapted STRIDE

<!-- sources:auto -->
## Sources

- [NIST AI Risk Management Framework (AI RMF)](https://www.nist.gov/itl/ai-risk-management-framework)
- [NIST AI 100-1 — Artificial Intelligence Risk Management Framework (AI RMF 1.0)](https://doi.org/10.6028/NIST.AI.100-1)
<!-- /sources -->
