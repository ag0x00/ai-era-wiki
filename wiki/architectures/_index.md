---
type: domain
title: "Reference Architectures"
created: 2026-04-30
updated: 2026-09-30
tags: [domain, architectures]
status: seed
subdomain_of: ""
page_count: 11
---

# Reference Architectures Index

Concrete agent and control-plane designs. What goes here: orchestrator/child patterns, tool-annotation systems, egress controls, human-in-the-loop UIs, [[agent-observability|agent observability]] stacks, sandboxing approaches.

## Pages


- [[agent-identity-architecture|AI Agent Identity Architecture]] — The conceptual identity architecture for AI agents comprises three elements: the identity models available, the layers that authenticate...
- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] — This architecture places security decisions and enforcement around one agent run by the organization.
- [[agentic-soc-ra-alert-triage|Agentic SOC Alert Triage Surface]] — Per-function deep-dive for the Alert triage surface of the Agentic SOC Reference Architecture.
- [[agentic-soc-ra-detection-engineering|Agentic SOC Detection Engineering Surface]] — Per-function deep-dive for the Agentic SOC Reference Architecture.
- [[agentic-soc-ra-exposure-vulnops|Agentic SOC Exposure and VulnOps Surface]] — The Exposure & VulnOps row of the Agentic SOC Reference Architecture runs continuous exposure and vulnerability discovery, plus remediati...
- [[agentic-soc-ra-incident-response|Agentic SOC Incident Response Surface]] — Incident response and containment is where the SOC stops acting on the estate.
- [[agentic-soc-ra-investigation-case-management|Agentic SOC Investigation Surface]] — Per-function deep dive for the Investigation & case management function of the Agentic SOC Reference Architecture.
- [[agentic-soc-ra-threat-hunting|Agentic SOC Threat Hunting Surface]] — Per-function deep dive on the threat hunting surface of the Agentic SOC Reference Architecture.
- [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] — This reference architecture is the structural counterpart to the Agentic SOC Capability Maturity Model.
- [[azure-rag-chatbot-security-profile|Azure-Native RAG Chatbot Security Profile (Copilot Studio)]] — This page applies the trust boundaries in the Agentic AI Security Reference Architecture and the nine-domain CMM to one deployment shape:...
- [[system-prompt-architecture|System Prompt Architecture (Boundary Markers + Trust Labels)]] — Boundary markers and trust labels reduce the success rate of indirect prompt injection and leave the Lethal Trifecta intact. This archite...

> [!gap] More needed
> Candidates: Stripe's [[lethal-trifecta|Lethal Trifecta]] containment architecture (elevate from [[stripe|Stripe]] stub), Agentic SOC reference architectures, generic broker / control-plane patterns.

## 