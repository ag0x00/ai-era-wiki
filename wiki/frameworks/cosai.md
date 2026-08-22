---
type: framework
title: "CoSAI: Coalition for Secure AI"
created: 2026-04-30
updated: 2026-06-23
tags:
  - frameworks
  - cosai
  - collaborative-standards
  - mcp-security
  - agentic-ai
status: developing
source_url: "https://www.coalitionforsecureai.org/"
scope_axis:
  - sec-of-ai
adoption_signal: active
last_substantive_update: 2026-02-09
published_by: "[[cosai-org|CoSAI]]"
current_version: "Living framework / workstream outputs; no formal version numbering"
first_published: "2024"
scope: "Industry collaborative producing operational AI security guidance; MCP security, secure-by-design agentic systems, AI incident response"
audience: "Enterprise AI security teams, platform architects, vendors"
aliases:
  - "Coalition for Secure AI"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[google-saif]]"
  - "[[google|Google]]"
  - "[[meta|Meta]]"
  - "[[cosai-org]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[standards-review-saif-cosai-2026-Q2]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# CoSAI — Coalition for Secure AI

**CoSAI** (Coalition for Secure AI) is a OASIS-hosted industry consortium producing collaborative AI security guidance. As of Q1 2026, it has **40+ industry partners** with 10 Premier Sponsors including Amazon, Microsoft, IBM, Intel, NVIDIA, PayPal, Anthropic, OpenAI, and (as of February 2026) Meta. Google donated the SAIF Risk Map and Risk Assessment to CoSAI in 2024, making CoSAI the primary successor to SAIF content.

CoSAI operates through four structured workstreams, with WS4 (Secure Design Patterns for Agentic Systems) being the most active. The verbatim workstream names, verified against the CoSAI site on 2026-06-22 in the [[standards-review-saif-cosai-2026-Q2|Google SAIF and CoSAI standards review]], are:

- **WS1** Software Supply Chain Security for AI Systems
- **WS2** Preparing Defenders for a Changing Security Landscape
- **WS3** AI Security Risk Governance
- **WS4** Secure Design Patterns for Agentic Systems

## Published deliverables

The CoSAI resources listing carries eight dated deliverables, verified 2026-06-22 in the [[standards-review-saif-cosai-2026-Q2|standards review]]:

| Deliverable | Date | Workstream |
|---|---|---|
| AI Shared Responsibility Framework | 2026-05-28 | WS3 |
| Agentic Identity and Access Management | 2026-04-17 | WS4 |
| The Future of Agentic Security: From Chatbots to Autonomous Swarms | 2026-03-31 | WS4 |
| Model Context Protocol (MCP) Security | 2026-01-20 | WS4 |
| AI Incident Response Framework | 2025-10-30 | WS2 |
| Signing ML Artifacts: Building towards tamper-proof ML metadata records | 2025-09-29 | WS1 |
| Preparing Defenders of AI Systems | 2025-07-16 | WS2 |
| Establish Risks and Controls for the AI Supply Chain | 2025-06-25 | WS1 |

**MCP Security** (2026-01-20) remains the most comprehensive MCP threat taxonomy of any framework, co-led by IBM and Sarah Novotny. The "nearly 40 threats across 12 categories" figure was not re-verifiable from the homepage/resources listing in the 2026-06-22 review pass and is flagged for a deeper-source check.

**Principles for Secure-by-Design Agentic Systems** (February 9, 2026):
- Defense-in-depth principles with practical implementation strategies
- Covers SLSA-based provenance, comprehensive telemetry, and updated incident response playbooks for agentic challenges

**Project CodeGuard** (February 9, 2026) — Cisco donated this model-agnostic security coding agent skills framework; now governed by CoSAI.

**Meta joined as Premier Sponsor** (February 3, 2026).

## A2A Protocol

The **[[a2a-protocol|Agent-to-Agent (A2A) protocol]]** is a key CoSAI/Google-originated initiative — current state **v1.0.0 (2026-03-12)** under **Linux Foundation governance** since 2025-06-23. Spec covers:
- Transport security (HTTPS / TLS 1.3) and authentication delegated to OpenAPI-style schemes (§7)
- Agent Card signing framework — Canonicalization, Signature Format, Signature Verification (§8.4); algorithm-agnostic
- gRPC, JSON-RPC over HTTP, and Server-Sent Events transports

**Opacity principle**: *"Agents collaborate based on declared capabilities and exchanged information, without needing to share their internal thoughts, plans, or tool implementations."*

**Gap**: A2A v1.0 has security woven through §7 + §8.4 but no standalone security spec; message integrity, replay protection, and cross-agent delegation remain vendor-side ([[multi-agent-runtime-security|Oktsec-class]]) or proposal-side ([Issue #1575](https://github.com/a2aproject/A2A/issues/1575)).

## AI Incident Response Framework

The **AI Incident Response Framework** (2025-10-30, continuously updated) is the first industry-wide AI incident response framework following the NIST lifecycle. It is the closest thing to an authoritative AI IR playbook, but lacks AI-specific IoCs and forensic procedures.

## Strengths

- MCP Security White Paper is the most comprehensive MCP threat taxonomy of any framework
- Secure-by-Design principles bridge conceptual guidance and operational practice
- AI Incident Response Framework v1.0 provides the only multi-stakeholder AI IR structure
- Collaborative model (40+ partners including all major hyperscalers) gives unique convening power
- A2A protocol's "opacity principle" is architecturally sound for multi-agent security
- Google SAIF donation ensures content continuity under collaborative governance

## Gaps and Shortcomings

- Publications remain **principled guidance rather than enforceable specifications**
- MCP Security White Paper catalogs threats but does not provide specific, testable control implementations
- Secure-by-Design principles lack maturity assessment criteria
- [[a2a-protocol|A2A v1.0]] has no standalone security specification — message integrity, replay protection, and cross-agent delegation remain vendor- or proposal-side
- AI Incident Response Framework lacks AI-specific IoCs or forensic procedures
- No AI-BOM requirements
- Cognitive file integrity and proof-of-guardrail concepts unaddressed
- Workstream outputs are not certification-backed

## Coverage Against OWASP ASI Top 10

| ASI Category | Coverage |
|---|---|
| ASI01: Agent Goal Hijack | ◐ Partial (MCP paper) |
| ASI02: Tool Misuse | ◐ Partial |
| ASI03: Identity & Privilege | ◐ Partial (A2A Agent Cards) |
| ASI04: Supply Chain | ● SLSA-based provenance |
| ASI05: Unexpected Code Execution (RCE) | ○ None |
| ASI06: Memory Poisoning | ◐ Partial |
| ASI07: Insecure Inter-Agent | ● A2A protocol + MCP paper |
| ASI08: Cascading Failures | ◐ Partial |
| ASI09: Human-Agent Trust Exploitation | ○ None |
| ASI10: Rogue Agents | ◐ Partial |

## Watch Items (2026)

- **Workstream 4 updates** to MCP Security White Paper as MCP spec evolves
- **A2A protocol security specification** — when formalized authorization schemes ship
- Additional workstream outputs on multi-cloud agentic security patterns

## See Also

- [[cosai-org|CoSAI]] (the organization)
- [[google-saif|Google SAIF — Secure AI Framework]] — original SAIF framework; CoSAI is the institutional successor
- [[meta|Meta]] — Premier Sponsor as of February 2026; LlamaFirewall contributor
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — CoSAI Principles → **D1**; Model Context Protocol (MCP) Security (2026-01-20) → **D4/D5**; Agentic Identity and Access Management (2026-04-17) → **D2**; AI Incident Response Framework (2025-10-30) → **D9 Operations & Human Factors**; CoSAI contribution is **D1 L5** evidence
- [[standards-review-saif-cosai-2026-Q2|Google SAIF and CoSAI standards review]] — verified the four verbatim workstream names and eight dated deliverables; reconciled the MCP date (2026-01-20) and the Agentic IAM title/date (2026-04-17)
- [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]] — ASI Top 10 complements CoSAI's MCP/agentic guidance

<!-- sources:auto -->
## Sources

- [CoSAI: Coalition for Secure AI](https://www.coalitionforsecureai.org/)
<!-- /sources -->
