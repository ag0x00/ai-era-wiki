---
type: entity
title: "Claude Code Security"
address: c-000066
created: 2026-05-15
updated: 2026-08-21
tags:
  - entities
  - product
  - tool
  - vuln-discovery
  - anthropic
  - claude-code
  - research-preview
status: seed
scope_axis: [ai-in-sec-defense]
entity_type: product
role: "Anthropic's defender-first vulnerability-discovery capability built into Claude Code on the web. Reads and reasons about codebases the way a human security researcher would; multi-stage self-critique verification ('Claude attempts to prove or disprove its own findings'); severity + confidence ratings; dashboard review with suggested patches gated by human approval. Limited research preview for Enterprise + Team customers; expedited free access for OSS maintainers. Announced 2026-02-20."
homepage: https://claude.com/solutions/claude-code-security
maintainer: "[[anthropic|Anthropic]]"
first_mentioned: "[[claude-code-security-announcement|Making frontier cybersecurity capabilities available to defenders]]"
related:
  - "[[anthropic|Anthropic]]"
  - "[[claude-code-security-announcement|Claude Code Security announcement]]"
  - "[[codex-security|Codex Security (formerly Aardvark)]]"
  - "[[openant|OpenAnt]]"
  - "[[adversarial-reflexion|Adversarial Reflexion]]"
  - "[[mythos|Mythos]]"
  - "[[anthropic-glasswing-announcement|Project Glasswing]]"
  - "[[codemender]]"
  - "[[google-cloud-codemender-preview]]"
  - "[[security-guidance-plugin|Security Guidance Plugin]]"
  - "[[anthropic-sandbox-runtime|Anthropic Sandbox Runtime]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]"
sources:
  - https://www.anthropic.com/news/claude-code-security
  - https://claude.com/solutions/claude-code-security
  - https://red.anthropic.com/2026/zero-days/
  - https://www.anthropic.com/news/claude-opus-4-6
  - "[[.raw/articles/anthropic-claude-code-security-2026-05-15.md]]"
---

# Claude Code Security

**Sources:** [Announcement (Feb 20, 2026)](https://www.anthropic.com/news/claude-code-security) · [Product page](https://claude.com/solutions/claude-code-security) · [Application access](https://claude.com/contact-sales/security)

## Description

Anthropic's defender-first vulnerability-discovery capability, built into Claude Code on the web. The capability reads and reasons about codebases the way a human security researcher would — *"understanding how components interact, tracing how data moves through your application, and catching complex vulnerabilities that rule-based tools miss"* — and runs a multi-stage self-critique verification step in which Claude attempts to prove or disprove its own findings to filter false positives. Validated findings appear in the Claude Code Security dashboard with severity and confidence ratings and suggested patches; nothing is applied without human approval.

Limited **research preview** for Anthropic Enterprise and Team customers; **expedited free access** for open-source maintainers explicitly committed. Underlying model: [Claude Opus 4.6](https://www.anthropic.com/news/claude-opus-4-6) (released February 2026).

## Relevance to This Wiki

Sixth sourced AI vulnerability-discovery production path as of 2026-05-15 — the Anthropic-side commercial private-preview entry, paired with [[codex-security|Codex Security]] (OpenAI commercial preview) and [[openant|OpenAnt]] (Knostic OSS). The product's load-bearing FP-control mechanism — *"Claude re-examines each result, attempting to prove or disprove its own findings and filter out false positives"* — is the **self-critique** instance of the same discipline that [[openant|OpenAnt]] implements as constrained-attacker-persona, that [[codex-security|Aardvark]] implements as sandboxed exploit-trigger validation, and that [[mdash|MDASH]] implements as a prover stage. Four mechanism instances, same architectural commitment. The product is on the wiki primarily as second-Anthropic posture on the axis (alongside [[anthropic-glasswing-announcement|Project Glasswing]] coalition organizing and [[mythos|Mythos]] frontier model preview).

## Outputs / Numbers

- **500+ vulnerabilities in production OSS codebases** found by Anthropic's Frontier Red Team using Claude Opus 4.6 — *"bugs that had gone undetected for decades, despite years of expert review."* This is the capability anchor for Claude Code Security as productized capability; the 500+ count is the FRT research-side result, not the Claude-Code-Security-product-side recall.
- **Severity + confidence rating per finding.** The confidence rating is a triage primitive recognizing that *"these issues often involve nuances that are difficult to assess from source code alone."*
- **Multi-stage self-critique verification** before any finding reaches an analyst.
- **No public benchmark recall number.**

## Notable Design Choices

- **Self-critique as the verification primitive.** *"Claude re-examines each result, attempting to prove or disprove its own findings."* Mechanism: model-vs-itself adversarial loop. This is structurally distinct from constrained-persona (OpenAnt), sandbox-execution-trigger (Aardvark), and ensemble-prover (MDASH) — same discipline, fourth distinct mechanism.
- **Built on Claude Code on the web.** Inherits dev-workflow integration without a new platform. Teams review findings and iterate on fixes in the tools they already use.
- **Confidence rating as a first-class output.** Severity is the obvious dimension; confidence is the more novel one — explicit recognition that AI-finding quality varies and analyst triage benefits from per-finding confidence signal.
- **OSS maintainer expedited-access track.** Explicit commitment to make this capability available to OSS maintainers at no cost via an expedited application path. Aligned with the [[anthropic-glasswing-announcement|Glasswing]] \$4M OSS donations posture but applied at the product level rather than at the coalition level.
- **Methodological frame rejects rule-based SAST.** *"Rather than scanning for known patterns, Claude Code Security reads and reasons about your code the way a human security researcher would."* Convergent with [[codex-security|Aardvark]]'s and [[openant|OpenAnt]]'s framing.
- **Human approval gating.** *"Nothing is applied without human approval: Claude Code Security identifies problems and suggests solutions, but developers always make the call."*

## Adjacent Gaps

- **No public benchmark.** Recall not quantified; comparisons across vendors (MDASH 88.45% CyberGym, Aardvark 92% internal golden, OpenAnt no published recall) require a common third-party benchmark that does not yet exist.
- **No sandboxed dynamic execution disclosed.** The verification is described as self-critique; whether sandbox-execution validation is in scope is unclear. OpenAnt (Stage 6), Aardvark (Validation stage), and [[codemender|CodeMender]] (verify stage) all describe sandboxed exploit triggering; CCS does not. CodeMender has run proof-of-concept exploits in a customer-managed sandbox since its [[google-cloud-codemender-preview|July 2026 preview]], leaving self-critique the outlier mechanism among the three commercial previews.
- **Cost shape bundled into Enterprise + Team subscription**; not bounded for OSS use.
- **FRT zero-days post not yet ingested.** [red.anthropic.com/2026/zero-days/](https://red.anthropic.com/2026/zero-days/) is the load-bearing capability anchor; already on the wiki's gap list.
