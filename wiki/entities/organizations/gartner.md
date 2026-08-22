---
type: entity
entity_type: organization
org_type: advisory
title: "Gartner"
created: 2026-05-01
updated: 2026-07-30
tags: [entities, organization, analyst-firm]
status: developing
homepage: "https://www.gartner.com"
website: "https://www.gartner.com"
related:
  - "[[ai-trism]]"
  - "[[guardian-agents-market-guide]]"
  - "[[evaluating-ai-soc-agents]]"
  - "[[scaling-agentic-ai-cios-talk]]"
  - "[[guardian-agent]]"
  - "[[ai-agent-layered-council]]"
  - "[[human-parity-line]]"
  - "[[avivah-litan]]"
  - "[[daryl-plummer]]"
  - "[[brandon-gummer]]"
  - "[[remy-gulzar]]"
  - "[[gartner-mq-enterprise-ai-coding-agents-2026]]"
sources:
  - "[[.raw/articles/gartner-market-guide-for-guardian-agents-2026-05-01.md]]"
  - "[[.raw/talks/scaling-agentic-ai-cios-2026-05-01.md]]"
---

# Gartner

**Sources:** [Gartner (homepage)](https://www.gartner.com) · [Gartner AI TRiSM topic](https://www.gartner.com/en/information-technology/topics/ai-trust-risk-and-security-management)

The dominant industry-analyst firm covering enterprise IT and security. Published research is paywalled but routinely surfaces in client RFPs, board decks, and procurement guidance. Gartner-named categories (e.g., AI TRiSM, Guardian Agents, AMPs) become procurement-language standards regardless of architectural merit because of Gartner's gravity in the buying process.

## Relevance to this corpus

This wiki's audience is enterprise CISOs, AI security architects, and AI platform engineers — the same audience Gartner serves. Adopting Gartner's terminology is alignment with the language the audience uses, not endorsement of Gartner's analysis.

When Gartner's framing is sharper than the wiki's existing framing (the [[guardian-agent|guardian-agent]] abstraction, [[sentinels-and-operatives|Sentinels/Operatives]] split, [[guardian-agent-metagovernance|metagovernance]]), the wiki adopts it. When the wiki's framing is sharper than Gartner's ([[lethal-trifecta|Lethal Trifecta]], [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]], specific incident anchoring), the wiki retains its position and notes the Gartner gap.

## Frameworks and categories Gartner has defined

| Category | Gartner publication | Wiki page |
|---|---|---|
| AI Trust, Risk, and Security Management (TRiSM) | 2023+ ongoing | [[ai-trism\|Gartner AI TRiSM]] |
| Guardian Agents | Market Guide G00836300 (Feb 24, 2026) | [[guardian-agents-market-guide\|paper]], [[guardian-agent\|concept]] |
| AI Agent Management Platforms (AMPs) | "AI Vendor Race" series | [[ai-agent-management-platform\|concept]] |
| AI Agent Layered Council | Scaling Agentic AI webinar (May 2026) | [[scaling-agentic-ai-cios-talk\|talk]], [[ai-agent-layered-council\|concept]] |
| Human Parity Line | Scaling Agentic AI webinar (May 2026) | [[human-parity-line\|concept]] |
| (Hype Cycle for AI Trust, Risk and Security Management) | Annual | (no dedicated page yet) |

## Notable analysts (named in the Guardian Agents Market Guide)

- [[avivah-litan|Avivah Litan]] — primary author, AI security
- [[daryl-plummer|Daryl Plummer]] — co-author, AI strategy
- Lane Severson, Bart Willemsen, Akif Khan, Jeremy D'Hoinne, Dennis Xu — co-authors

## Notable analysts (CIO-research / agentic AI scaling)

- [[brandon-gummer|Brandon Gummer]] — VP Analyst, CIO operating models, co-presenter of [[scaling-agentic-ai-cios-talk|Scaling Agentic AI (May 2026)]]. *Name spelling unverified.*
- [[remy-gulzar|Remy Gulzar]] — VP Analyst, AI governance & compliance, co-presenter of [[scaling-agentic-ai-cios-talk|Scaling Agentic AI (May 2026)]]. *Name spelling unverified.*

## Gartner's stance on independent guardian agents

Gartner explicitly argues that hyperscaler-embedded guardian agents (Microsoft Agent 365, AWS Bedrock Guardrails, Google Vertex AI guardrails) cannot fully manage cross-vendor or third-party agents. Therefore enterprises need **independent enterprise-owned guardian agents** alongside platform-embedded ones. This is the structural argument that makes a vendor-neutral RA + CMM (this wiki's posture) Gartner-aligned.

## Surveys and evidence Gartner publishes

- **2026 Gartner CIO and Technology Executive Survey** — n=2,501 (May–June 2025); 17% of enterprises had deployed AI agents, 42% planning within 12 months
- Various Hype Cycle reports placing AI TRiSM categories on the maturity curve

## See Also

- [[ai-trism|Gartner AI TRiSM]]
- [[guardian-agents-market-guide|Gartner Market Guide for Guardian Agents (Feb 2026)]]
- [[guardian-agent|Guardian Agent]]
