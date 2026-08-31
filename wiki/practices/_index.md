---
type: domain
title: "Emerging Best Practices"
created: 2026-04-30
updated: 2026-04-30
tags: [domain, practices]
status: seed
subdomain_of: ""
page_count: 0
---

# Emerging Best Practices Index

Controls, playbooks, and patterns that are converging across vendors but not yet codified into a published framework. When something here matures into a named framework, promote it to `wiki/frameworks/` and leave a stub redirect.

## Pages


- [[agent-observability|Agent Observability]] — Improving agent observability requires moving from a "black-box" model, where only final outputs are seen, to a "glass-box" security para...
- [[agent-sandboxing|Agent Sandboxing]]
- [[agent-token-chargeback|Agent Token Chargeback]] — The agent token chargeback practice attributes agentic-AI token spend to the consuming business unit and use case via a variable chargeba...
- [[agentic-vulnerability-discovery|Agentic Vulnerability Discovery]] — Agentic vulnerability discovery directs an LLM agent to locate a defect in a codebase and produce an input that proves the defect exists.
- [[ai-bom|AI-BOM: AI Bill of Materials]]
- [[ai-era-supply-chain-hardening|AI-Era Software Supply Chain Hardening]]
- [[ai-spm|AI Security Posture Management (AI-SPM)]] — AI-SPM is the AI analog to CSPM (Cloud Security Posture Management): a continuous discipline of inventorying AI assets, detecting misconf...
- [[anti-patterns-and-failure-modes|RA and CMM Anti-Patterns and Failure Modes]] — Closes peer-review-readiness §4: *"No anti-patterns / failure-modes catalog.
- [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]] — The credential proxy pattern keeps real secrets out of an agent's reach: the agent holds only a short-lived proxy token, and a proxy reso...
- [[distributed-kill-switch|Distributed Kill Switch]] — The distributed kill switch practice extends the authority to halt an agent or agentic workflow to every team member in the loop, beyond...
- [[dspm|Data Security Posture Management (DSPM) for AI]] — DSPM maps where sensitive data lives across cloud repositories and SaaS, classifies it, and ties that map to AI usage.
- [[end-to-end-harness-evaluation|End-to-End Harness Evaluation]] — An end-to-end harness evaluation grades a whole system rather than a model.
- [[evaluating-ai-soc-agents|Evaluating AI SOC Agents: Gartner's Seven Questions]] — A buyer-side evaluation framework for AI SOC agents (Gartner's term for the agentic-SOC category), from the Gartner report Validate the P...
- [[guardian-agent-metagovernance|Guardian Agent Metagovernance (Guards for the Guardians)]]
- [[multi-agent-runtime-security|Multi-Agent Runtime Security: Cascade Detection and IR]] — The depth-companion to the wiki's single-agent observability page, focused on what's specific to multi-agent meshes: cascade-failure dete...
- [[nhi-governance-for-agents|NHI Governance for AI Agents]] — Non-Human Identity (NHI) governance for AI agents is the discipline of managing the lifecycle of every credential, token, certificate, an...
- [[oversharing-controls|Oversharing Controls for AI Search]] — AI oversharing is the failure mode where an AI search tool retrieves and combines content that is technically RBAC-permitted but contextu...
- [[plan-validate-execute|Plan-Validate-Execute Pattern]]
- [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]]
- [[rag-hardening|RAG Hardening]]
- [[securing-agentic-coding|Securing Agentic Coding]]
- [[securing-ai-talking-points|Securing AI: Talking Points]] — A four-point briefing outline for an AI security pitch / customer conversation.
- [[supply-chain-security-for-agents|Supply Chain Security for Agentic AI]]
- [[vulnops-implementation-roadmap|VulnOps Implementation Roadmap (Enterprise)]] — VulnOps names the function; this page is how an enterprise team builds it.

> [!gap] Still needed
> Egress control patterns (dedicated page), tool annotation systems, evaluation loops (offline + online), agent quality measurement, structured I/O for agents, RAG poisoning defenses.

## 