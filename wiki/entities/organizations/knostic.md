---
type: entity
entity_type: organization
org_type: vendor
title: "Knostic"
created: 2026-04-30
updated: 2026-08-20
tags: [entities, organization, vendor, ai-security, coding-agents, ai-search, knowledge-layer]
status: developing
homepage: "https://www.knostic.ai"
website: "https://www.knostic.ai"
related:
  - "[[kirin]]"
  - "[[openant]]"
  - "[[openant-announcement]]"
  - "[[nahum-korda]]"
  - "[[gadi-evron]]"
  - "[[vulnops]]"
  - "[[mythos-ready-briefing]]"
  - "[[mythos-ready-security-program]]"
  - "[[adversarial-reflexion]]"
  - "[[ai-coding-agent-governance]]"
  - "[[ai-data-security]]"
  - "[[oversharing-controls]]"
  - "[[inference-exposure]]"
  - "[[ai-usage-control]]"
  - "[[shadow-ai]]"
  - "[[cyber-defense-matrix]]"
  - "[[ai-trism]]"
  - "[[sounil-yu]]"
  - "[[glean]]"
sources:
  - "https://www.knostic.ai/blog/ai-coding-agent-governance"
  - "https://www.knostic.ai/blog/ai-data-security"
---

# Knostic

**Sources:** [Knostic (homepage)](https://www.knostic.ai) · [AI Coding Agent Governance (blog)](https://www.knostic.ai/blog/ai-coding-agent-governance) · [AI Data Security (blog)](https://www.knostic.ai/blog/ai-data-security)

AI security vendor focused on enterprise AI deployments. Three product surfaces:

1. **Knowledge-layer governance** for enterprise AI search across Microsoft Copilot, Glean, Gemini, and custom LLMs. Detects and remediates [[oversharing-controls|oversharing]]; builds dynamic, need-to-know boundaries that reflect role, context, and actual usage rather than only static labels. Plugs into M365 / Purview / Glean / AWS / ServiceNow / custom LLMs.
2. **Coding-agent governance** via [[kirin|Kirin]], covering Cursor, GitHub Copilot, IDE extensions, MCP servers.
3. **Open-source LLM vulnerability discovery** via [[openant|OpenAnt]]: a six-stage pipeline (Parse → Reachability → Classification → Discovery → Verification → Dynamic) with [[adversarial-reflexion|Adversarial Reflexion]] as the load-bearing constrained-attacker-persona FP-control mechanism. Free; Knostic-hosted OSS-scan program. Research lead: [[nahum-korda|Nahum Korda]]. Announced 2026-05-15; see [[openant-announcement|the OpenAnt announcement page]].

Aligns publicly with [[ai-trism|Gartner AI TRiSM]], [[sounil-yu|Sounil Yu]]'s [[cyber-defense-matrix|Cyber Defense Matrix]], OWASP GenAI / [[owasp-agentic-ai-top-10|ASI Top 10]], [[nist-ai-rmf|NIST AI RMF]], and [[google-saif|Google SAIF]].

> [!note] Gartner Guardian Agents Market Guide inclusion (Feb 2026)
> Knostic is named in the **Agent security and risk specialists** segment of the [[guardian-agents-market-guide|Gartner Market Guide for Guardian Agents (February 2026)]], confirming the wiki's existing positioning of Knostic as a [[guardian-agent|guardian-agent]] vendor. Co-listed with Aiceberg, Apiiro, NeuralTrust, Pillar, Zenity, [[varonis|Varonis]], Noma Security, and others in the same segment.

## Notable Output

- **Blog: AI Data Security** (2026-05, ingested): see [[ai-data-security|AI Data Security (Knostic blog, 2026)]]. Vendor-content survey of AI data security with strong standards anchoring; introduces the [[inference-exposure|Inference Exposure (and Retrieval Exposure)]] / retrieval-exposure framing, the [[ai-usage-control|AI-UC]] layer beyond access control, and the [[ai-spm|AI Security Posture Management (AI-SPM)]] / [[dspm|Data Security Posture Management (DSPM) for AI]] posture pair.
- **Blog: AI Coding Agent Governance** (2026, ingested): see [[ai-coding-agent-governance|AI Coding Agent Governance (Knostic, 2025–2026)]]. Argues governance is structurally distinct from security; introduces the "shadow automation" framing and a four-component / three-phase model.
- **Cyber Defense Matrix** ebook (Sounil Yu collaboration): "Rethinking Cyber Defense for the Age of AI."
- **Kirin**: coding-agent security product targeting Cursor, GitHub Copilot, IDE extensions, MCP servers. See [[kirin|Kirin (Knostic)]].
- **OpenAnt**: open-source LLM-based vulnerability discovery tool (Feb-2026 cost benchmarks, 2026-05-15 blog announcement). Six-stage pipeline; constrained-attacker-persona [[adversarial-reflexion|Adversarial Reflexion]] for FP-control; concrete cross-project filter ratios across OpenSSL (C) / WordPress (PHP) / LangChain (Python) / Rails (Ruby) / Grafana (TypeScript+Go). Free OSS-scan program for maintainers. See [[openant-announcement|the OpenAnt paper page]] and [[openant|the product page]].
- **Blog: Introducing OpenAnt** (2026-05-15, ingested): see [[openant-announcement|the paper page]].

## Capabilities (per published material)

- **Prompt simulation**: synthetic-employee-prompt testing to surface oversharing paths before users hit them
- **Continuous monitoring** at the knowledge layer: flags AI-specific exposure that file-centric DLP misses
- **Audit trail** of who accessed what knowledge and how, including AI-inferred answers from multiple documents
- **Remediation playbooks** scoped by project, department, or data type
- **Sensitivity-label optimization**: reads and tunes M365 sensitivity labels and policies

## Relations

- Produces: [[kirin|Kirin (Knostic)]], [[openant|OpenAnt]]
- Authored: [[ai-coding-agent-governance|AI Coding Agent Governance (Knostic, 2025–2026)]], [[ai-data-security|AI Data Security (Knostic blog, 2026)]], [[openant-announcement|Introducing OpenAnt (Knostic blog, 2026-05-15)]]
- Co-authored: [[mythos-ready-briefing|*The "AI Vulnerability Storm": Building a "Mythos-ready" Security Program* (CSA / SANS / Unprompted / OWASP, 2026-04-12)]]; Gadi Evron is lead author; Sounil Yu is contributing author.
- Personnel: [[gadi-evron|Gadi Evron]] (CEO, [[csa|CSA]] CISO-in-Residence for AI, [[vulnops|VulnOps]] co-introducer, lead author of the Mythos-ready briefing), [[sounil-yu|Sounil Yu]] (CTO, contributing author), [[nahum-korda|Nahum Korda]] (research lead, OpenAnt).
- Aligned with: [[ai-trism|AI TRiSM]], [[cyber-defense-matrix|Cyber Defense Matrix]], [[ai-spm|AI Security Posture Management (AI-SPM)]], [[dspm|Data Security Posture Management (DSPM) for AI]]
- Adjacent vendors named in published material: [[glean|Glean]], Microsoft Copilot (see [[microsoft-rai|Microsoft Responsible AI Standard (RAI)]]), Gemini (Google)
