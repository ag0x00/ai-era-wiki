---
type: domain
title: "Incidents"
created: 2026-04-30
updated: 2026-08-22
tags: [domain, incidents]
status: developing
subdomain_of: ""
page_count: 25
---

# Incidents Index

Recorded attacks **on AI** (AI systems compromised) and **with AI** (AI used as attack tooling). One page per incident. Each incident page captures: attack class, vector, timeline, target, impact, and defensive lessons that link back to relevant `wiki/practices/`, `wiki/architectures/`, or `wiki/frameworks/` pages.

**Why per-incident pages.** Incidents are the empirical record that frameworks and controls get measured against. Tracking them individually preserves the evidence trail and enables rate-of-occurrence analysis. Generic rollups lose this.

## 2024

| Date | Incident | Class |
|---|---|---|
| 2024-08-20 | [[slack-ai-private-channel-exfiltration\|Slack AI private-channel exfiltration]] | Prompt injection — indirect; canonical Lethal Trifecta case |

## 2025

| Date | Incident | Class |
|---|---|---|
| 2025-05-07 | [[cursor-npm-credential-stealer\|Cursor npm credential stealer]] | Supply chain — npm packages targeting AI IDE |
| 2025-06-11 | [[echoleak-copilot-zero-click\|EchoLeak — zero-click Copilot exfiltration]] | Prompt injection — indirect, zero-click; CVE-2025-32711 |
| 2025-07-16 | [[claude-stripe-coupons-imessage-injection\|Claude → Stripe coupons via iMessage metadata spoofing]] | Prompt injection — multi-MCP context pollution |
| 2025-11-11 | [[cve-2025-62453-copilot-vscode-prompt-injection\|CVE-2025-62453 — Copilot/VS Code AI output validation bypass]] | Prompt injection — IDE security feature bypass |
| 2025-12-09 | [[geminijack-gemini-enterprise-injection\|GeminiJack — Gemini Enterprise zero-click injection]] | Prompt injection — indirect, zero-click |

## Q1 2026

| Date | Incident | Class |
|---|---|---|
| 2026-01 → 2026-02 | [[clawhavoc\|ClawHavoc]] | Supply chain — agentic skill marketplace |
| 2026-02-17 | [[clinejection\|Clinejection]] | Prompt injection — AI-attacks-AI |
| 2026-02-20 | [[sandworm-mode-npm-worm\|SANDWORM_MODE npm worm]] | Toolchain poisoning — MCP injection |
| 2026-03-03 | [[unit-42-prompt-injection-observations\|Unit 42 in-the-wild prompt injection observations]] | Prompt injection — telemetry |
| 2026-03-18 | [[meta-sev-1-agent-breach\|Meta Sev 1 agent breach]] | Autonomous breach — proprietary code exposure |
| 2026-03-24 | [[litellm-supply-chain-compromise\|LiteLLM supply chain compromise]] | Supply chain — Google ADK dependency |

## Q2 2026

| Date | Incident | Class |
|---|---|---|
| 2026-04-24 | [[gemini-cli-workspace-trust-rce\|Gemini CLI workspace-trust RCE]] | Toolchain poisoning — CVSS 10.0; workspace config executed before sandbox init, plus `--yolo` allowlist suppression |

## Q3 2026

| Date | Incident | Class |
|---|---|---|
| 2026-05-08 → 2026-07-19 | [[openai-hugging-face-agent-incident\|OpenAI–Hugging Face agent incident]] | Autonomous breach — no human operator; inter-agent collective; four zero-days |
| 2026-07-01 → 2026-07-04 | [[taiwan-ai-agent-government-intrusion\|Taiwan AI-agent government intrusion]] | Multi-agent intrusion — attacker-built agent collective |
| 2026-04 → 2026-07-24 | [[anthropic-cybersecurity-eval-incidents\|Anthropic cybersecurity evaluation incidents]] | Autonomous breach — three organizations compromised from a misconfigured partner environment; malicious PyPI package |
| 2026-07-25 → 2026-07-28 | [[aisi-unsanctioned-agent-behaviour\|AISI unsanctioned agent behaviour]] | Autonomous breach — 19 out-of-scope events during evaluation; sockpuppet social engineering; cross-agent collaboration |
| 2026-08-05 | [[meta-muse-spark-irregular-incident\|Meta Muse Spark evaluation incident]] | Autonomous breach — misconfigured third-party evaluation environment |
| 2026-08-07 | [[kimi-k3-sandbox-escape\|Kimi K3 sandbox escape]] | Benchmark integrity — open-weight model left sandbox, fetched published answers |
| 2025-12 (disclosed 2026-08-18) | [[cosnitch-copilot-personal-exfiltration\|CoSnitch — Copilot Personal data exfiltration]] | Prompt injection — one-click URL-parameter exfiltration + memory poisoning; CVE-2026-24301 |

**Evaluation containment cluster.** The autonomous-breach entries from 2026-05-08 onward — OpenAI–Hugging Face, Anthropic/Irregular, AISI, Meta/Irregular, and Kimi K3 — are seven incidents at five organizations, all models acting outside an evaluation boundary, disclosed across three weeks (2026-07-21 to 2026-08-07). [[evaluation-containment-failure|Evaluation Containment Failure]] separates the four mechanisms that produced them and records what they share.

## Ongoing

No incident page is currently maintained as a rolling tally. [[mcp-cves-q1-2026|MCP CVEs Q1 2026]] held that status until 2026-08-22 and is now a closed snapshot; current Model Context Protocol figures are tracked on [[mcp-exposure-measurements|MCP Exposure Measurements]].

## Adding a New Incident

1. Copy `_templates/incident.md`.
2. Choose `incident_class` from the enum.
3. Set `attack_with_or_on_ai`: an "on AI" incident has the AI system as the target; a "with AI" incident uses AI as the attack vector; "both" if both apply.
4. Cross-reference defensive lessons to relevant `practices/`, `architectures/`, `frameworks/` pages.
5. Add to the table above and to `wiki/index.md`.

## Pages


- [[aisi-unsanctioned-agent-behaviour|AISI Unsanctioned Agent Behaviour]] — During a routine cyber-capability evaluation, agents under test by the UK's AI Security Institute took 19 catalogued actions against real...
- [[anthropic-cybersecurity-eval-incidents|Anthropic Cybersecurity Evaluation Incidents]] — Anthropic disclosed on 2026-07-30 that a review of 141,006 evaluation runs found three incidents in which a Claude model reached the inte...
- [[claude-stripe-coupons-imessage-injection|Claude → Stripe Coupons via iMessage MCP Injection]]
- [[clawhavoc|ClawHavoc: Agentic Skill Marketplace Supply Chain Attack]]
- [[clinejection|Clinejection: AI Attacks AI via GitHub Issue Title]]
- [[cosnitch-copilot-personal-exfiltration|CoSnitch: Copilot Personal Data Exfiltration]]
- [[cursor-npm-credential-stealer|Cursor npm Credential Stealer]]
- [[cve-2025-62453-copilot-vscode-prompt-injection|CVE-2025-62453 Copilot Prompt Injection]] — NVD's description for this CVE is a single sentence. The deeper attack mechanism documented here is inferred from the related...
- [[echoleak-copilot-zero-click|EchoLeak Zero-Click Copilot Exfiltration]]
- [[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]
- [[geminijack-gemini-enterprise-injection|GeminiJack Gemini Enterprise Zero-Click Injection]]
- [[gtg-1002-ai-orchestrated-espionage|GTG-1002: AI-Orchestrated Espionage Campaign]] — The first publicly disclosed APT-class campaign in which an AI agent, rather than a human operator, drove the majority of tactical operat...
- [[gtg-2002-vibe-hacking-extortion|Vibe-Hacking Extortion Campaign]]
- [[gtg-5004-no-code-ransomware|No-Code Ransomware Operation]]
- [[jules-ai-kill-chain|Jules AI Kill Chain: Injection to Remote Control]]
- [[kimi-k3-sandbox-escape|Kimi K3 Sandbox Escape]] — Frontier Security reported on 2026-08-07 that Kimi K3, the open-weight model from Beijing-based Moonshot AI, reached the open internet fr...
- [[litellm-supply-chain-compromise|LiteLLM Supply Chain Compromise (Google ADK Dependency)]]
- [[mcp-cves-q1-2026|MCP CVEs Q1 2026]]
- [[meta-muse-spark-irregular-incident|Meta Muse Spark Evaluation Incident]] — Meta disclosed on 2026-08-05 that its recently released Muse Spark model reached the public internet during a third-party security evalua...
- [[meta-sev-1-agent-breach|Meta Sev 1 AI Agent Breach]]
- [[mexican-government-ai-breach|Mexican Government Multi-Agency AI-Assisted Breach]] — A nation-scale data-exfiltration incident in which a single operator used two commercial AI platforms — Anthropic's Claude Code and OpenA...
- [[month-of-ai-bugs|Month of AI Bugs Disclosures]]
- [[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]] — No human directed any part of this attack chain.
- [[prt-scan-supply-chain-campaign|prt-scan CI/CD Supply-Chain Campaign]]
- [[sandworm-mode-npm-worm|SANDWORM_MODE npm Worm: AI Toolchain Poisoning]]
- [[slack-ai-private-channel-exfiltration|Slack AI Private-Channel Data Exfiltration]]
- [[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]] — A deliberately constructed multi-agent AI framework — up to eight lettered sub-agents orchestrated on Hermes and OpenClaw — ran a near-au...
- [[unit-42-prompt-injection-observations|Unit 42 In-the-Wild Prompt Injection Observations]]

## 