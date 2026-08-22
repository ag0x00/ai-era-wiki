---
type: framework
title: "NIST AI 600-1: Generative AI Profile"
created: 2026-04-30
updated: 2026-04-30
tags:
  - frameworks
  - nist
  - generative-ai
  - risk-profile
status: developing
source_url: "https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-generative-artificial-intelligence"
adoption_signal: maintained
last_substantive_update: 2024-07-01
published_by: "[[nist|NIST]]"
current_version: "1.0 (July 2024)"
first_published: "2024-07-26"
doi: 10.6028/NIST.AI.600-1
primary_documents:
  - title: "NIST AI 600-1 — AI RMF: Generative Artificial Intelligence Profile"
    url: "https://doi.org/10.6028/NIST.AI.600-1"
    version: "1.0"
    published: "2024-07-26"
    retrieved: "2026-06-21"
    archived_copy: ".raw/papers/nist-ai-600-1-genai-profile-2024-07.pdf"
    scope_in_wiki: "§2 twelve GenAI risk categories; §3 ~200 Suggested Actions (GV/MP/MS/MG IDs)"
scope: "Risk profile for generative AI systems; companion to NIST AI RMF covering GenAI-specific risks including prompt injection, data poisoning, and model extraction"
audience: "Enterprises deploying GenAI; federal agencies; AI developers"
aliases:
  - "NIST AI 600-1"
  - "GenAI Profile"
related:
  - "[[nist-ai-rmf]]"
  - "[[nist-ai-800-4]]"
  - "[[nist|NIST]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[standards-review-nist-ai-rmf-2026-Q2]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# NIST AI 600-1 — Generative AI Profile

**NIST AI 600-1** (Generative AI Profile, July 2024) extends the [[nist-ai-rmf|NIST AI RMF]] to address GenAI-specific risks. It maps risk categories to AI RMF Govern/Map/Measure/Manage functions and provides guidance on:

- **Prompt injection**: both direct (user-provided) and indirect (via external content)
- **Data poisoning**: training and fine-tuning data integrity
- **Model extraction**: intellectual property protection

## Status (Q1 2026)

NIST AI 600-1 is **unchanged** as of Q1 2026. The publication remains at its July 2024 state. No updates or agentic AI extension has been announced.

Key gap: AI 600-1 addresses GenAI risks but does not extend to agentic AI. NIST's [[nist-ai-rmf|CAISI initiative]] (February 2026) is the first signal that agentic AI guidance will follow, though no companion "Agentic AI Profile" has been announced or scheduled.

## Companion Publications (Q1 2026)

- **NIST IR 8605A** (January 8, 2026): COSAiS annotated outline for predictive AI control overlays; adapts SP 800-53 controls for AI; generative and agentic AI overlays planned but unscheduled
- **NIST AI 800-4** (March 6, 2026): first federal report mapping gaps in post-deployment AI monitoring; six monitoring categories with human factors as biggest blind spot

## See Also

- [[nist-ai-rmf|NIST AI Risk Management Framework (AI RMF)]] — parent framework
- [[nist|NIST]] — publisher
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — AI 600-1 §2.4 (Data Privacy) → **D6**; §2.1 (CBRN Information) → **D4**; §2.2 (Confabulation) → **D4**; §2.7 (Human-AI Configuration) → **D9**; §2.8 (Information Integrity) → **D6/D7**; §2.9 (Information Security / exfil) → **D5**; §2.12 (Value Chain and Component Integration) → **D8**. Risk-category numbering and per-domain anchors verified clause-level in [[standards-review-nist-ai-rmf-2026-Q2|the NIST AI RMF review (2026-Q2)]].

<!-- sources:auto -->
## Sources

- [NIST AI 600-1: Generative AI Profile](https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-generative-artificial-intelligence)
- [NIST AI 600-1 — AI RMF: Generative Artificial Intelligence Profile](https://doi.org/10.6028/NIST.AI.600-1)
<!-- /sources -->
