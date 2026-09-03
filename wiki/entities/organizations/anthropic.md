---
type: entity
title: "Anthropic"
created: 2026-04-30
updated: 2026-09-01
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
role: "AI lab; producer of Claude models (Opus, Sonnet, Haiku) and preview-stage Mythos frontier model; CoSAI Premier Sponsor; publisher of Project Glasswing, Claude Code Security, and the open-source defending-code-reference-harness"
related:
  - "[[defending-code-harness|defending-code-harness]]"
  - "[[semgrep|Semgrep]]"
  - "[[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]]"
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
  - ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
---

# Anthropic

**Sources:** [Anthropic (homepage)](https://www.anthropic.com) · [Project Glasswing](https://www.anthropic.com/glasswing) · [Claude Code Security](https://www.anthropic.com/news/claude-code-security) · [Frontier Red Team — zero days](https://red.anthropic.com/2026/zero-days/)

AI lab; producer of Claude foundation models (Opus, Sonnet, Haiku) and preview-stage [[mythos|Claude Mythos Preview]] frontier model. CoSAI Premier Sponsor, ISO 42001 certified.

## Notable Output (security-relevant)

- **[[anthropic-glasswing-announcement|Project Glasswing]]** (announced May 12, 2026): 12-partner coalition initiative with \$100M in usage credits + \$4M in OSS-security donations applying Mythos to defensive vulnerability discovery on critical software. Partners: AWS, Apple, Broadcom, Cisco, CrowdStrike, Google, JPMorganChase, the Linux Foundation, Microsoft, NVIDIA, Palo Alto Networks, plus 40+ extended-access organizations. Mythos is also deployed offensively (non-Glasswing-partner) via [[xbow|XBOW]] (see [[xbow-mythos-evaluation|XBOW's evaluation]]). **Mythos is NOT planned for general availability**: preview-only at \$25/\$125 per M tokens via Claude API, Amazon Bedrock, Google Cloud Vertex AI, and Microsoft Foundry. Anthropic commits to 90-day public reporting on Glasswing findings.
- **[[claude-code-security|Claude Code Security]]** (announced Feb 20, 2026): defender-first vulnerability-discovery capability built into Claude Code on the web. Limited research preview for Enterprise + Team customers with expedited free access for OSS maintainers. Read-and-reason analysis + multi-stage self-critique verification (*"Claude attempts to prove or disprove its own findings"*) + severity + confidence ratings + dashboard review with human-approval-gated patches. Capability anchor: Anthropic FRT found **500+ vulnerabilities in production OSS codebases** using [[mythos|Claude Opus 4.6]] (cited via [red.anthropic.com/2026/zero-days/](https://red.anthropic.com/2026/zero-days/)). See [[claude-code-security-announcement|the paper page]].
- **[[anthropic-2026-agentic-coding-trends|2026 Agentic Coding Trends Report]]** (early 2026): vendor strategic forecast with Trend 8 ("agentic coding improves security defenses — but also offensive uses") and Priority 4 ("embedding security architecture as a part of agentic system design from the earliest stages").
- **[Frontier Red Team blog](https://red.anthropic.com/)** (red.anthropic.com): technical detail layer for the FRT's CTF evaluations, PNNL critical-infrastructure partnership, and zero-days discovery work.
- **[[defending-code-harness|defending-code-harness]]** (`anthropics/defending-code-reference-harness`, Apache 2.0): agentic harness scoped to C/C++ memory-safety bugs; a find counts once a crafted input crashes the target under AddressSanitizer, and a generated patch is verified against re-attack. Runs under a gVisor sandbox with an egress allowlist and outputs SARIF plus patches, per Semgrep's July 2026 LLM-generated repository summary.[^semgrep] Semgrep reports the repository unmaintained and maintains a fork, `semgrep/defending-code-harness`.
- **[[anthropic-threat-intelligence-reports|Threat Intelligence report series]]** (from August 2025): periodic case-study disclosures from a dedicated Threat Intelligence team inside the Safeguards organization, covering misuse of Claude by real actors. The August 2025 edition carries [[gtg-2002-vibe-hacking-extortion|GTG-2002]] and [[gtg-5004-no-code-ransomware|GTG-5004]]; the November 2025 [[gtg-1002-ai-orchestrated-espionage|GTG-1002]] disclosure and the June 2026 [[llm-attack-navigator|LLM ATT&CK Navigator]] continue the line. The series sits outside the vulnerability-discovery slots below and reports on adversaries rather than on capability: 832 banned accounts mapped to MITRE ATT&CK, plus the ARiES risk-scoring methodology and an argued gap in the ATT&CK taxonomy.

## Relevance to This Wiki

Anthropic occupies four distinct slots in AI vulnerability discovery. The first three sit on the `ai-in-sec-defense` axis: **(a) coalition organizer** ([[glasswing|Project Glasswing]], the May 2026 capability-distribution mechanism across 52+ organizations); **(b) commercial-preview product vendor** ([[claude-code-security|Claude Code Security]], the Feb 2026 defender-first productization on Claude Code on the web); **(c) model substrate** ([[mythos|Mythos]] + Claude Opus 4.6, the underlying capability that the Anthropic FRT used to find 500+ OSS vulnerabilities). The fourth distributes method rather than access: **(d) open-source reference implementation** — [[defending-code-harness|`anthropics/defending-code-reference-harness`]], an Apache-2.0 agentic harness for C/C++ memory-safety discovery with execution-verified patching, which [[semgrep|Semgrep]]'s July 2026 survey reports tied for most-starred of the nine harnesses it compares, at ~6K stars alongside Trail of Bits' skills marketplace.[^semgrep] The first three slots distribute access under terms Anthropic sets — partner restrictions, the no-GA stance, an enterprise entitlement. The fourth distributes method under a permissive licence and retains nothing: Semgrep reports the repository unmaintained, with a fork maintained at `semgrep/defending-code-harness`. The defender-first framing holds across the three controlled channels and is carried by a third party in the fourth.

Methodologically, Anthropic is convergent with [[openai|OpenAI]] ([[codex-security|Codex Security]]) on rejecting rule-based SAST as the prior generation and adopting the human-security-researcher metaphor. See [[adversarial-reflexion|Adversarial Reflexion]] for the cross-product FP-control discipline.

> [!contradiction] Contradicts [[moak|MOAK]] empirical results
> The Anthropic Red Team publicly stated: *"Opus 4.6 is currently far better at identifying and fixing vulnerabilities than at exploiting them. This gives defenders the advantage."* (cited in [[moak-mother-of-all-kevs|MOAK origin story]], Apr 9 2026).
>
> [[moak|MOAK]]'s published results demonstrate **98% autonomous exploitation of CISA KEVs using Claude Opus 4.6** as a primary model inside a five-agent agentic pipeline — directly contradicting the claim that Opus 4.6 does not give attackers exploitation capability. The Anthropic statement may accurately characterize bare-model capability; it does not account for agentic orchestration, which MOAK demonstrates closes the gap. See [[moak-mother-of-all-kevs|MOAK origin story]] for the full analysis.

## Notes

[^semgrep]: [Semgrep — Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses), July 2026 (no day-level date exposed; author not named). The licence, category description and unmaintained status are human-written; the star count is human-written but point-in-time; the AddressSanitizer/gVisor/SARIF mechanism detail is from Semgrep's LLM-generated repository summary. Summarized at [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].
