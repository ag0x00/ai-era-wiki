---
type: concept
title: "Google Agentic SOC"
address: c-000166
created: 2026-06-01
updated: 2026-06-23
tags:
  - concepts
  - agentic-soc
  - google
  - secops
  - gemini
  - ai-in-sec-defense
status: developing
scope_axis:
  - ai-in-sec-defense
origin: aggregated
complexity: intermediate
domain: "agentic SOC / security operations"
aliases:
  - "Google SecOps agentic SOC"
  - "Gemini agentic defense"
related:
  - "[[agentic-soc-state-of-the-field]]"
  - "[[microsoft-security-copilot]]"
  - "[[google]]"
  - "[[a2a-protocol]]"
  - "[[google-saif]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[oversight-layer]]"
  - "[[llm-as-a-judge]]"
  - "[[agent-identity-architecture]]"
  - "[[mcp-security]]"
sources:
  - "https://cloud.google.com/blog/products/identity-security/the-dawn-of-agentic-ai-in-security-operations-at-rsac-2025"
  - "https://cloud.google.com/blog/topics/google-cloud-next/google-cloud-next-2026-wrap-up"
---

# Google Agentic SOC

Google's agentic SOC is a set of Gemini-powered agents embedded in **Google Security Operations (SecOps)** and **Google Threat Intelligence** that run SOC workflows — triage, investigation, detection engineering, threat hunting, and malware analysis — semi-autonomously under human supervision. Google frames the agents as teammates that take routine, high-volume work so analysts focus on complex investigation and response, summarized as "AI empowers defenders, it does not replace them."[^rsac]

**The offering packages SOC work as a roster of role-specialized agents on one platform, governed by per-agent cryptographic identity and an explicit human-authority boundary.** That framing places Google alongside [[microsoft-security-copilot|Microsoft Security Copilot]] in the copilot-plus-specialized-agents pattern of the [[agentic-soc-state-of-the-field|agentic SOC]], rather than a single assistant bolted onto a SIEM.

## The substrate: Google SecOps and Google Unified Security

The agents run on Google SecOps — the platform combining the Chronicle SIEM, Siemplify-derived SOAR, Mandiant threat intelligence, and Gemini. **Google Unified Security (GUS)** converges SecOps with Google Threat Intelligence, Security Command Center, Mandiant, and Chrome Enterprise into one platform, and is the integration surface the agents operate across.[^gus] Google states that Mandiant analyst practice is the training signal: agents are built to "observe and act like an elite human analyst" using investigation principles derived from Mandiant.[^triage]

## The agents

| Agent | SOC workflow | Product | Status |
|---|---|---|---|
| Alert Triage and Investigation Agent | Tier-1/2 triage and investigation | Google SecOps | Public preview[^triageprev] |
| Malware Analysis Agent | Reverse engineering, malware triage | Google Threat Intelligence | Preview[^rsac] |
| Detection Engineering Agent | Detection authoring and validation | Google SecOps | Preview[^next26] |
| Threat Hunting Agent | Proactive hunting over log data | Google SecOps | Preview[^next26] |
| Third-Party Context Agent | Entity enrichment from external systems | Google SecOps | Preview[^next26] |

The **Alert Triage and Investigation Agent** investigates an alert end to end: it gathers evidence, runs analyses such as decoding obfuscated scripts, correlates signals, and returns a true- or false-positive verdict with a transparent audit log of its evidence and reasoning.[^triage] The **Detection Engineering Agent** examines a deployment, decides whether existing rules would catch a newly identified threat, generates and installs rules where gaps exist, and produces synthetic logs to confirm the rules fire.[^next26] The **Threat Hunting Agent** turns threat intelligence into a hunt plan and runs the searches across customer logs for signs of existing compromise.[^next26]

## Orchestration: A2A and MCP

Google's [[a2a-protocol|Agent2Agent (A2A) protocol]] provides cross-agent interoperability, and the Model Context Protocol ([[mcp-security|MCP]]) provides a common interface for agent access to security tools and data; Google open-sourced MCP servers for Google Unified Security.[^rsac] The published agent roster the orchestration is meant to cover spans data management, triage, investigation, response, threat research, threat hunting, malware analysis, exposure management, and detection engineering.[^rsac]

## Governance: identity, anomaly detection, posture

At Google Cloud Next 2026 Google paired the SOC agents with agent-governance primitives that map onto the [[agentic-ai-security-reference-architecture|reference architecture]]'s Identity and Observability planes:[^next26]

- **Agent Identity** — every agent gets a unique cryptographic identity, creating an auditable trail mapped to authorization policies (the [[agent-identity-architecture|agent identity]] control).
- **Agent Anomaly Detection** — real-time detection of suspicious agent behavior using statistical models and an [[llm-as-a-judge|LLM-as-a-judge]] framework to flag unusual reasoning.
- **Agent Security dashboard** — Security Command Center-powered discovery that maps agent-to-model relationships and scans for vulnerabilities.

Wiz integration feeds Wiz Defend detections into Google SecOps and Mandiant Threat Defense, adds an AI Bill of Materials to inventory [[shadow-ai|shadow AI]], and injects inline security checks into IDE and agent workflows.[^next26]

## Human-authority boundary

Google distinguishes assistive AI (aids a human action) from agentic AI (independently identifies, reasons, and executes a task) and keeps a human in the loop over high-impact actions.[^rsac] The triage agent's transparent audit log and source references exist so analysts can review and accept or reject its verdict, which is the [[oversight-layer|oversight-layer]] pattern applied to defense.[^triage]

## Integration with current SecOps

The agents are native to the SecOps console rather than a separate product: the triage agent acts on the existing alert and case queue, SOAR exposes "agentic automation" inside playbooks, and the detection agent writes into the live rule set. Access is packaged through SecOps tiers and Google Unified Security.[^gus] New partner integrations announced alongside the agents (Darktrace, Gigamon, SAP) widen the data and action surface the agents reason over.[^next26]

## Reported outcomes

Google's figures are vendor-reported and not independently benchmarked. A named customer reports regular-expression authoring dropping from 30–60 minutes to seconds with Gemini.[^rsac] For triage, Google reports reducing a roughly 30-minute manual analysis to about 60 seconds and a 50% faster mean time to respond in early deployments.[^triageprev]

> [!gap] Open items
> The 2026 agents (detection engineering, threat hunting, third-party context) are in preview with general availability stated but not dated. The performance figures are Google-reported; no public multi-task benchmark scores Google's defender agents (see the [[agentic-soc-state-of-the-field|agentic SOC thesis]] on the missing comparator).

## Notes

[^rsac]: [Google Cloud — The dawn of agentic AI in security operations at RSAC 2025](https://cloud.google.com/blog/products/identity-security/the-dawn-of-agentic-ai-in-security-operations-at-rsac-2025), April 28, 2025. Agentic SOC vision; alert triage agent (Google SecOps) and malware analysis agent (Google Threat Intelligence); Agent2Agent (A2A) + MCP; planned multi-agent roster; SecOps Labs; the Apex Fintech regex testimonial.
[^next26]: [Google Cloud — Google Cloud Next 2026 wrap-up](https://cloud.google.com/blog/topics/google-cloud-next/google-cloud-next-2026-wrap-up), April 25, 2026. Detection Engineering, Threat Hunting, and Third-Party Context agents; Agent Identity, Agent Anomaly Detection (LLM-as-a-judge), and Agent Security dashboard; Wiz Defend integration and AI-BOM; Darktrace/Gigamon/SAP partner integrations.
[^triage]: [Google Cloud — Use the Triage and Investigation Agent to investigate alerts](https://docs.cloud.google.com/chronicle/docs/secops/triage-investigation-agent), 2026. Autonomous evidence gathering, deobfuscation, true/false-positive verdict, transparent audit log; Mandiant-derived analysis principles.
[^triageprev]: [Google Cloud Community — Alert Triage and Investigation Agent now in public preview](https://security.googlecloudcommunity.com/news-announcements-9/be-one-step-ahead-with-the-google-secops-alert-triage-and-investigation-agent-now-in-public-preview-6244), 2026. Public-preview status; Google-reported triage time (~30 min to ~60 sec) and 50% faster MTTR.
[^gus]: [Google Cloud — Google Unified Security](https://cloud.google.com/security/google-unified-security), 2026. Converged platform spanning SecOps, Google Threat Intelligence, Security Command Center, Mandiant, and Chrome Enterprise.

<!-- sources:auto -->
## Sources

- [cloud.google.com](https://cloud.google.com/blog/products/identity-security/the-dawn-of-agentic-ai-in-security-operations-at-rsac-2025)
- [cloud.google.com](https://cloud.google.com/blog/topics/google-cloud-next/google-cloud-next-2026-wrap-up)
<!-- /sources -->
