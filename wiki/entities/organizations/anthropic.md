---
type: entity
title: "Anthropic"
created: 2026-04-30
updated: 2026-08-21
tags:
  - entities
  - organizations
  - frontier-models
  - mythos
status: developing
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
entity_type: organization
org_type: vendor
homepage: "https://www.anthropic.com"
role: "AI lab; producer of Claude models (Opus, Sonnet, Haiku) and preview-stage Mythos frontier model; CoSAI Premier Sponsor; publisher of Project Glasswing and Claude Code Security"
related:
  - "[[glasswing]]"
  - "[[mythos]]"
  - "[[xbow]]"
  - "[[xbow-mythos-evaluation]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[anthropic-2026-agentic-coding-trends]]"
  - "[[claude-code-security]]"
  - "[[claude-code-security-announcement]]"
  - "[[collaboration-paradox]]"
  - "[[cosai-org]]"
  - "[[anthropic-sandbox-runtime]]"
  - "[[security-guidance-plugin]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[anthropic-threat-intelligence-reports]]"
  - "[[llm-attack-navigator]]"
  - "[[gtg-2002-vibe-hacking-extortion]]"
  - "[[gtg-5004-no-code-ransomware]]"
  - "[[gtg-1002-ai-orchestrated-espionage]]"
  - "[[aisi-unsanctioned-agent-behaviour|AISI Unsanctioned Agent Behaviour]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - "https://www.anthropic.com/glasswing"
  - "https://www.anthropic.com/news/claude-code-security"
  - "https://red.anthropic.com/2026/zero-days/"
  - "[[.raw/papers/anthropic-2026-agentic-coding-trends-report.pdf]]"
---

# Anthropic

**Sources:** [Anthropic (homepage)](https://www.anthropic.com) · [Project Glasswing](https://www.anthropic.com/glasswing) · [Claude Code Security](https://www.anthropic.com/news/claude-code-security) · [Frontier Red Team — zero days](https://red.anthropic.com/2026/zero-days/)

AI lab; producer of Claude foundation models (Opus, Sonnet, Haiku) and preview-stage [[mythos|Claude Mythos Preview]] frontier model. CoSAI Premier Sponsor, ISO 42001 certified. Referenced 17+ times across the wiki.

## Notable Output (security-relevant)

- **[[anthropic-glasswing-announcement|Project Glasswing]]** (announced May 12, 2026): 12-partner coalition initiative with \$100M in usage credits + \$4M in OSS-security donations applying Mythos to defensive vulnerability discovery on critical software. Partners: AWS, Apple, Broadcom, Cisco, CrowdStrike, Google, JPMorganChase, the Linux Foundation, Microsoft, NVIDIA, Palo Alto Networks, plus 40+ extended-access organizations. Mythos is also deployed offensively (non-Glasswing-partner) via [[xbow|XBOW]] (see [[xbow-mythos-evaluation|XBOW's evaluation]]). **Mythos is NOT planned for general availability**: preview-only at \$25/\$125 per M tokens via Claude API, Amazon Bedrock, Google Cloud Vertex AI, and Microsoft Foundry. Anthropic commits to 90-day public reporting on Glasswing findings.
- **[[claude-code-security|Claude Code Security]]** (announced Feb 20, 2026): defender-first vulnerability-discovery capability built into Claude Code on the web. Limited research preview for Enterprise + Team customers with expedited free access for OSS maintainers. Read-and-reason analysis + multi-stage self-critique verification (*"Claude attempts to prove or disprove its own findings"*) + severity + confidence ratings + dashboard review with human-approval-gated patches. Capability anchor: Anthropic FRT found **500+ vulnerabilities in production OSS codebases** using [[mythos|Claude Opus 4.6]] (cited via [red.anthropic.com/2026/zero-days/](https://red.anthropic.com/2026/zero-days/)). See [[claude-code-security-announcement|the paper page]].
- **[[anthropic-2026-agentic-coding-trends|2026 Agentic Coding Trends Report]]** (early 2026): vendor strategic forecast with Trend 8 ("agentic coding improves security defenses — but also offensive uses") and Priority 4 ("embedding security architecture as a part of agentic system design from the earliest stages").
- **[Frontier Red Team blog](https://red.anthropic.com/)** (red.anthropic.com): technical detail layer for the FRT's CTF evaluations, PNNL critical-infrastructure partnership, and zero-days discovery work.
- **[[anthropic-threat-intelligence-reports|Threat Intelligence report series]]** (from August 2025): periodic case-study disclosures from a dedicated Threat Intelligence team inside the Safeguards organization, covering misuse of Claude by real actors. The August 2025 edition carries [[gtg-2002-vibe-hacking-extortion|GTG-2002]] and [[gtg-5004-no-code-ransomware|GTG-5004]]; the November 2025 [[gtg-1002-ai-orchestrated-espionage|GTG-1002]] disclosure and the June 2026 [[llm-attack-navigator|LLM ATT&CK Navigator]] continue the line. The series sits outside the vulnerability-discovery slots below and reports on adversaries rather than on capability: 832 banned accounts mapped to MITRE ATT&CK, plus the ARiES risk-scoring methodology and an argued gap in the ATT&CK taxonomy.

## Relevance to This Wiki

Anthropic occupies three distinct slots in AI vulnerability discovery, all of them on the `ai-in-sec-defense` axis: **(a) coalition organizer** ([[glasswing|Project Glasswing]], the May 2026 capability-distribution mechanism across 52+ organizations); **(b) commercial-preview product vendor** ([[claude-code-security|Claude Code Security]], the Feb 2026 defender-first productization on Claude Code on the web); **(c) model substrate** ([[mythos|Mythos]] + Claude Opus 4.6, the underlying capability that the Anthropic FRT used to find 500+ OSS vulnerabilities). Across all three slots, the strategic framing is *defender-first*: capabilities are distributed to defenders ahead of offensive exposure via Glasswing partner restrictions, Mythos's no-GA stance, and CCS's expedited-OSS-maintainer access.

Methodologically, Anthropic is convergent with [[openai|OpenAI]] ([[codex-security|Codex Security]]) on rejecting rule-based SAST as the prior generation and adopting the human-security-researcher metaphor. See [[adversarial-reflexion|Adversarial Reflexion]] for the cross-product FP-control discipline.

> [!contradiction] Contradicts [[moak|MOAK]] empirical results
> The Anthropic Red Team publicly stated: *"Opus 4.6 is currently far better at identifying and fixing vulnerabilities than at exploiting them. This gives defenders the advantage."* (cited in [[moak-mother-of-all-kevs|MOAK origin story]], Apr 9 2026).
>
> [[moak|MOAK]]'s published results demonstrate **98% autonomous exploitation of CISA KEVs using Claude Opus 4.6** as a primary model inside a five-agent agentic pipeline — directly contradicting the claim that Opus 4.6 does not give attackers exploitation capability. The Anthropic statement may accurately characterize bare-model capability; it does not account for agentic orchestration, which MOAK demonstrates closes the gap. See [[moak-mother-of-all-kevs|MOAK origin story]] for the full analysis.

## Adjacent Gaps

- **FRT zero-days post** ([red.anthropic.com/2026/zero-days/](https://red.anthropic.com/2026/zero-days/)) cited as the load-bearing capability anchor for Claude Code Security; not yet ingested.
- **FRT critical-infrastructure post** ([red.anthropic.com/2026/critical-infrastructure-defense/](https://red.anthropic.com/2026/critical-infrastructure-defense/)): PNNL partnership cited inline; not yet ingested.
- **FRT CTF post** ([red.anthropic.com/2025/ai-for-cyber-defenders/](https://red.anthropic.com/2025/ai-for-cyber-defenders/)): Claude entered in competitive Capture-the-Flag events; not yet ingested.
- Full product line and SKU map; ISO 42001 + AIUC-1 posture; complete personnel list; FRT team breakdown; relationship to US-government AI Safety Institute (AISI); the proposed "independent third-party body" governance structure floated in the Glasswing post.
