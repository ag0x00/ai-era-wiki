---
type: entity
entity_type: organization
org_type: vendor
title: "Microsoft"
created: 2026-05-04
updated: 2026-08-20
tags:
  - entities
  - organizations
  - hyperscaler
  - ai-platform
  - identity
  - security
  - mdash
status: seed
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
homepage: "https://www.microsoft.com"
related:
  - "[[microsoft-rai]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[microsoft-sdl]]"
  - "[[microsoft-sdl-evolving-security-practices]]"
  - "[[microsoft-secure-agentic-ai-end-to-end]]"
  - "[[microsoft-security-copilot]]"
  - "[[mdash]]"
  - "[[mdash-defense-at-ai-speed]]"
  - "[[glasswing]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[mythos]]"
  - "[[network-layer-prompt-injection-containment]]"
  - "[[binaryshield-ai-fingerprints-talk]]"
  - "[[vasu-jakkal]]"
  - "[[taesoo-kim]]"
  - "[[yonatan-zunger]]"
  - "[[csa]]"
  - "[[cosai-org]]"
  - "[[claude-code-github-action-credential-exposure]]"
  - "[[microsoft-cli-coding-agent-adoption-study]]"
  - "[[cosnitch-copilot-personal-exfiltration]]"
  - "[[echoleak-copilot-zero-click]]"
  - "[[varonis]]"
sources:
  - "https://www.microsoft.com/security"
  - "https://learn.microsoft.com/azure/ai-services"
  - "[[.raw/articles/microsoft-secure-agentic-ai-end-to-end-2026-05-07.md]]"
  - "[[.raw/articles/microsoft-defense-at-ai-speed-2026-05-13.md]]"
  - "[[.raw/articles/microsoft-sdl-evolving-security-practices-2026-02-03.md]]"
---

# Microsoft

**Sources:** [Microsoft (homepage)](https://www.microsoft.com) · [Microsoft Security](https://www.microsoft.com/security) · [Azure AI Services docs](https://learn.microsoft.com/azure/ai-services)

> [!gap] Stub
> Hyperscaler with deep agentic-AI surface area in the wiki, operating defender-AI at **two distinct layers**: at the SOC layer, [[microsoft-security-copilot|Microsoft Security Copilot]] + [[microsoft-entra-agent-id|Agent 365 + Entra Agent ID]] (GA May 1 2026, M365 E7 The Frontier Suite) + Microsoft Prompt Shields + Microsoft Purview AI + Defender for Cloud + Defender Predictive Shielding + Sentinel agent-aware SIEM playbooks + Sentinel MCP Entity Analyzer. At the AppSec / vulnerability-research layer, [[mdash|MDASH]] (multi-model agentic scanning harness, 100+ specialized agents; [[mdash-defense-at-ai-speed|announced May 12 2026]] by the Microsoft **Autonomous Code Security (ACS)** team in collaboration with **Microsoft Windows Attack Research and Protection (WARP)**; led by [[taesoo-kim|Taesoo Kim]]). **Named launch partner of [[glasswing|Project Glasswing]]** (Anthropic coalition, same-day announcement May 12 2026) — Microsoft makes [[mythos|Mythos Preview]] available to Glasswing participants via **Microsoft Foundry**, and tested Mythos against its own **CTI-REALM** open-source security benchmark with "substantial improvements." Igor Tsyganskiy (EVP of Cybersecurity and Microsoft Research) is the quoted Glasswing executive. The earlier MDASH "generally available AI models" silence is explained by Glasswing coordinated-launch constraint — Mythos is almost certainly one of MDASH's orchestrated SOTA-reasoner models. Plus broader frameworks: Microsoft Responsible AI Standard ([[microsoft-rai|RAI]]), [[microsoft-zt4ai|ZT4AI]] (Zero Trust for AI), M365 memory-injection detector, FIDES (zero-PI on AgentDojo), [[pyrit|PyRIT]] (Microsoft AI Red Team OSS), and [[binaryshield-ai-fingerprints-talk|BinaryShield]] (privacy-preserving cross-service prompt-injection fingerprint sharing; arXiv:2509.05608).
>
> Pending content: company overview, full AI security product portfolio, ISO 42001 + AIUC-1 posture, key personnel beyond Vasu Jakkal, Taesoo Kim, and Igor Tsyganskiy (Jason Clinton, MSFT AI Red Team leadership, ACS / WARP team breakdown). CTI-REALM benchmark needs its own concept page.

## March 2026 RSAC announcement portfolio

Per [[microsoft-secure-agentic-ai-end-to-end|Vasu Jakkal's pre-RSAC 2026 post]] (2026-03-20), Microsoft's three-pillar agentic-AI security framing organizes the portfolio:

**Pillar 1 — Secure agents.** [[microsoft-entra-agent-id|Agent 365]] (launched Ignite 2025 / 2025-11-18, GA 2026-05-01) bundling Defender / Entra / Purview capabilities for agent governance.

Microsoft's three AI-security instruments sit at distinct layers, and the wiki keeps them separate: the [[microsoft-rai|Responsible AI Standard]] is a responsible-AI **goals** standard (seventeen goals, not a control catalogue); [[microsoft-zt4ai|ZT4AI]] is the **control catalogue**; and [[microsoft-entra-agent-id|Agent 365]] is the **management plane** (registry, identity, observability) over those controls. The goals-versus-control-plane distinction is set out in [[standards-review-microsoft-rai-agent-365-2026-Q2|the 2026-Q2 RAI / Agent 365 review]] and [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]].

**Pillar 2 — Secure foundations.** Security Dashboard for AI (GA); Entra Internet Access Shadow AI Detection (GA Mar 31); Enhanced Intune App Inventory (May); Entra Backup & Recovery (preview); Entra Tenant Governance (preview, shadow-tenant detection); Entra Passkey + Windows Hello integration; Entra External MFA (GA); Entra Adaptive Risk Remediation (Apr); Unified Identity Security (preview); expanded Purview DLP for M365 Copilot (GA Mar 31, blocks PII / credit card numbers in prompts); Purview Embedded in Copilot Control System (Apr); Purview Customizable Data Security Reports (preview); **Entra Internet Access [[network-layer-prompt-injection-containment|Prompt Injection Protection]] (GA Mar 31)** — first major-vendor network-layer PI defense; Defender for Cloud Container Security; Defender for Cloud Posture Management (AWS + GCP, Apr); **Defender Predictive Shielding (preview)** — adaptive policy contraction during active attacks.

**Pillar 3 — Defend with agents and experts.** Security Copilot (now in M365 E5 + E7); Security Analyst Agent in Defender (Mar 26); Security Alert Triage Agent in Defender (Apr); Conditional Access Optimization Agent in Entra; Data Security Posture Agent in Purview (with credential scanning); Data Security Triage Agent in Purview; 15+ partner-built agents in the Security Store. Microsoft Sentinel additions: Data Federation via Microsoft Fabric (preview, integrating Databricks / Fabric / ADLS); Playbook Generator with NL Orchestration (preview); Granular Delegated Administrator Privileges + Unified RBAC (preview); Security Store Embedded in Purview + Entra (GA Mar 31); Custom Graphs via Microsoft Fabric (preview); Sentinel MCP Entity Analyzer (GA Apr) — first SIEM with native MCP integration. Microsoft Defender Experts Suite for managed XDR.

**Stats** (per the Mar 2026 post): 80% of Fortune 500 already deploying agents (Copilot Studio + Agent Builder, Nov 2025 baseline); Microsoft Security: 1.6M customers, 1B identities, 24B Copilot interactions, 100T daily signals.

## Secure Development Lifecycle (SDL) — AI extension (2026-02-03)

Microsoft's classical secure-by-design framework — [[microsoft-sdl|Microsoft Secure Development Lifecycle (SDL)]] — published its first explicit AI extension on 2026-02-03 in a Microsoft Security Blog post by [[yonatan-zunger|Yonatan Zunger]]: [[microsoft-sdl-evolving-security-practices|*Evolving Security Practices for an AI-Powered World*]]. The post announces **six SDL-for-AI focus areas** (threat modeling for AI, AI system observability, AI memory protections, agent identity and RBAC enforcement, AI model publishing, AI shutdown mechanisms) and **six operating pillars** (research, policy, standards, enablement, cross-functional collaboration, continuous improvement). Substantive per-area technical guidance is promised "in the coming months." This makes Microsoft SDL the **first major-vendor secure-SDLC framework with an explicit AI scope** (NIST SSDF SP 800-218A remains partial; [[google-saif|Google SAIF]] is AI-first rather than an extension of a classical secure-SDLC anchor).

The 2026 SDL extension complements rather than replaces the broader Microsoft AI-security portfolio: SDL operates at the *develop-and-ship* lifecycle layer (policy + practice + tooling), while [[microsoft-entra-agent-id|Agent 365 + Entra Agent ID]], [[microsoft-zt4ai|ZT4AI]], [[microsoft-secure-agentic-ai-end-to-end|the Mar 2026 RSAC portfolio]], and [[mdash|MDASH]] operate at the *run-and-defend* layer.

## Copilot vulnerability disclosures

Three researcher-disclosed prompt-injection vulnerabilities have their own wiki incident page as of this count, reaching shipping Copilot products across a fourteen-month span: [[echoleak-copilot-zero-click|EchoLeak]] (CVE-2025-32711, June 2025, Microsoft 365 Copilot, zero-click RAG exfiltration), [[cve-2025-62453-copilot-vscode-prompt-injection|CVE-2025-62453]] (November 2025, GitHub Copilot Chat / VS Code, agent-mode security-feature bypass), and [[cosnitch-copilot-personal-exfiltration|CoSnitch]] (CVE-2026-24301, August 2026, Copilot Personal, one-click URL-parameter exfiltration and memory poisoning, [[varonis|Varonis]] Threat Labs). [[varonis|Varonis]] alone reports two further 2026 Copilot findings not yet incident-paged here — Reprompt (Copilot Personal, no disclosed CVE) and SearchLeak (CVE-2026-42824, Microsoft 365 Copilot Enterprise) — so the product family's actual disclosure count exceeds this page's tracked three. The three tracked here land on three distinct Copilot surfaces — email/document RAG, IDE agent mode, and consumer chat — rather than repeating one weakness, which argues against treating any single mitigation as covering the product family. For CoSnitch, Microsoft states Microsoft 365 Copilot Enterprise is unaffected; the CoSnitch incident page records reporting that disputes how long that separation holds given Microsoft's announced "Copilot Fusion" merger of the personal and enterprise product lines.
