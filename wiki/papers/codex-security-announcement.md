---
type: paper
title: "Aardvark / Codex Security Announcement"
address: c-000063
created: 2026-05-15
updated: 2026-08-21
tags:
  - papers
  - vuln-discovery
  - openai
  - codex
  - private-preview
status: summarized
scope_axis: [ai-in-sec-defense]
year: 2026
authors: []
venue: "OpenAI blog / Codex announcements"
source_url: https://openai.com/index/introducing-aardvark/
archived_copy: ".raw/articles/openai-aardvark-codex-security-2026-05-15.md"
key_claim: "An LLM-reasoning + tool-use agentic pipeline — not fuzzing, not SCA — can continuously analyze repositories, monitor commits against a whole-repo threat model, validate exploitability in an isolated sandbox, and emit Codex-generated patches at a recall level (92% on golden repos) competitive with the strongest sourced peer instruments the wiki tracks for AI-assisted vulnerability discovery."
methodology: "Four-stage pipeline. (1) Analysis — produces a whole-repository threat model reflecting project security objectives and design. (2) Commit scanning — inspects each commit-level change against the whole repository + threat model; on initial connect, scans history for existing issues. (3) Validation — attempts to trigger the candidate vulnerability in an isolated sandboxed environment to confirm exploitability; describes the steps taken. (4) Patching — integrates with OpenAI Codex to generate a patch for each finding; Aardvark scans the patch; human review remains the gate. Model: GPT-5 at the original announcement."
related:
  - "[[codex-security|Codex Security (formerly Aardvark)]]"
  - "[[openai|OpenAI]]"
  - "[[openant-announcement|OpenAnt]]"
  - "[[claude-code-security-announcement|Claude Code Security]]"
  - "[[adversarial-reflexion|Adversarial Reflexion]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[mdash|MDASH]]"
  - "[[big-sleep|Big Sleep]]"
  - "[[codemender|CodeMender]]"
sources:
  - "[[.raw/articles/openai-aardvark-codex-security-2026-05-15.md]]"
  - https://openai.com/index/introducing-aardvark/
  - https://openai.com/index/codex-security-now-in-research-preview/
  - https://openai.com/policies/outbound-coordinated-disclosure-policy/
aliases:
  - papers/openai-aardvark-codex-security-2026-05-15
  - openai-aardvark-codex-security-2026-05-15

---

# Introducing Aardvark — Agentic Security Researcher

**Source:** [OpenAI blog — *Introducing Aardvark*](https://openai.com/index/introducing-aardvark/) (fetched 2026-05-15). Banner update dated **March 6, 2026**: *"Aardvark is now Codex Security, and is available as a research preview"* — built into Codex, rolling out to ChatGPT Enterprise, Business, and Edu customers via Codex web with free usage for one month. Local copy: `.raw/articles/openai-aardvark-codex-security-2026-05-15.md`.

## Key Claim

An LLM-reasoning + tool-use agentic pipeline can continuously analyze repositories, monitor commits against a whole-repo threat model, validate exploitability in an isolated sandbox, and emit Codex-generated patches with **92% recall on "golden" repositories** (known + synthetically-introduced vulnerabilities). The framing rejects classical primitives — *"Aardvark does not rely on traditional program analysis techniques like fuzzing or software composition analysis"* — and explicitly adopts the human-security-researcher metaphor: *"reading code, analyzing it, writing and running tests, using tools."*

## Methodology — Four-Stage Pipeline

1. **Analysis.** Whole-repository read; emits a threat model reflecting project security objectives and design. The threat model is the durable artifact subsequent stages consult.
2. **Commit scanning.** Each new commit is inspected against the whole repository and against the threat model; on first connection to a repository, history is back-scanned to find pre-existing issues. Findings annotate the affected code step-by-step for human review.
3. **Validation.** Each candidate vulnerability is attempted in an isolated, sandboxed environment to confirm exploitability. The validation steps are described to support quality assessment. Stated goal: *"high-quality, low false-positive insights."*
4. **Patching.** OpenAI Codex generates a patch for each confirmed finding; Aardvark scans the patch; a one-click human-review-and-apply workflow on the Codex side closes the loop.

## Notable Findings

- **92% recall on "golden" repos.** Benchmark testing on golden repositories (known + synthetically-introduced vulnerabilities). Comparable in shape to [[mdash|MDASH]]'s 88.45% on the public [[cybergym|CyberGym]] leaderboard and [[mythos|raw Mythos]]'s 83.1% on the same benchmark — but on a different (internal, not public) golden-repo set, so directly comparing the numbers across benchmarks is unsafe.
- **In production for "several months" before the announcement** across OpenAI's internal codebases and external alpha partners. The announcement is the productization milestone, not the first deployment.
- **Ten CVE IDs assigned** from OSS responsible-disclosure work conducted by Aardvark. Pro-bono scanning for select non-commercial OSS projects is committed.
- **Updated coordinated-disclosure policy.** [Outbound coordinated disclosure policy](https://openai.com/policies/outbound-coordinated-disclosure-policy/) was revised in conjunction with this launch — explicit shift away from rigid timelines toward collaboration. Anticipates that AI-driven discovery rates will require disclosure-pipeline reform.
- **Cited base rate**: 40,000+ CVEs reported in 2024; ~1.2% of commits introduce bugs. Establishes the operational case for continuous commit-level scanning.
- **March 6 2026 rename and Codex integration.** *"Aardvark is now built directly into Codex as Codex Security"* — research preview to ChatGPT Enterprise / Business / Edu via Codex web, free for the rollout month. The product name stabilizes as **Codex Security**; *Aardvark* remains the recognizable internal codename.
- **Methodological frame rejects classical SAST.** *"Aardvark does not rely on traditional program analysis techniques like fuzzing or software composition analysis"* — explicit positioning against the rule-based SAST product category. This is the same framing [[claude-code-security-announcement|Claude Code Security]] adopts ("Rather than scanning for known patterns, Claude Code Security reads and reasons about your code the way a human security researcher would") in February 2026.
- **Four-stage pipeline convergent with peer products.** Compare:
   - **Aardvark**: Analysis → Commit scanning → Validation (sandbox) → Patching (Codex)
   - **[[openant-announcement|OpenAnt]]**: Parse → Reachability → Classification → Discovery → Verification → Dynamic (sandbox)
   - **[[mdash-defense-at-ai-speed|MDASH]]**: Prepare → Scan → Validate → Dedup → Prove
   - **[[claude-code-security-announcement|Claude Code Security]]**: Read/reason → Multi-stage self-critique verification → Severity + confidence rating → Dashboard review + suggested patches

   Five-stage shape (read → scan → validate → confirm → patch) recurs across four unrelated vendors. The convergence is the strongest architectural signal in [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]].

## Strengths and Weaknesses

**Strengths.** Reports a concrete recall benchmark (92% on golden repos) rather than only vendor anecdote. Closes the loop from finding to fix via Codex patch generation, with explicit human-approval gating. Explicitly updates the coordinated-disclosure policy in tandem with the tooling launch — recognizes that AI discovery pace requires pipeline reform, not just better detection. Convergent four-stage pipeline structure with peer products confirms the disciplinary observation that **validation is the load-bearing stage**, not detection.

**Weaknesses and open scope.**

- **Golden-repo benchmark is not public.** *"Golden"* repositories are not disclosed; the 92% number is not directly comparable to MDASH's 88.45% CyberGym or raw Mythos's 83.1% CyberGym. A common third-party benchmark (CyberGym, AISI, or similar) would be the natural comparison instrument.
- **No false-positive rate disclosed.** Claims *"high-quality, low false-positive insights"* without a quantified FP rate. Peer products do better on this — OpenAnt publishes filter ratios (e.g., 28 → 3 verified for OpenSSL); Anthropic Glasswing publishes concrete OpenBSD / FFmpeg / Linux-kernel anchored disclosures.
- **Closed-source, private preview only.** Initial Aardvark release was private beta; March 2026 rename made it part of the Codex Security research preview for paying ChatGPT tiers. The internals (threat-model schema, validation harness, sandbox configuration) are not disclosed and not auditable.
- **No published cost discipline.** Unlike [[openant-announcement|OpenAnt]] which publishes per-stage cost ranges and total-project costs for five real OSS projects, Aardvark's cost shape is not in the announcement. For ChatGPT Enterprise / Business / Edu customers the cost is bundled into the subscription, but for non-subscription open-source use it is not bounded.
- **Validation-stage details thin.** *"Isolated, sandboxed environment"* is asserted but the sandbox primitive (Docker, gVisor, Firecracker, custom) and the exploit-trigger language are not described. Compare OpenAnt's documented Docker-sandboxed dynamic test stage with explicit security-researcher-persona framing.

## Relations

- **Supports** [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] as the **commercial-vendor private-preview** entry on the axis — the OpenAI-side companion to [[claude-code-security-announcement|Claude Code Security]] (Anthropic), [[mdash-defense-at-ai-speed|MDASH]] (Microsoft), [[openant-announcement|OpenAnt]] (Knostic OSS), and [[google-big-sleep-projectzero|Big Sleep]] / [[google-codemender-deepmind|CodeMender]] (Google production). Six sourced production paths across five vendors as of 2026-05-15.
- **Supports** [[adversarial-reflexion|Adversarial Reflexion]] obliquely — Aardvark's Validation stage *attempts to trigger the candidate vulnerability in an isolated sandboxed environment*, which is the dynamic-execution form of the same FP-control discipline. OpenAnt formalizes this as *constrained-attacker-persona with explicit trace*; Aardvark formalizes it as *sandbox-trigger validation*; CCS formalizes it as *Claude attempting to prove or disprove its own findings*; MDASH formalizes it as a *prover stage*. Four mechanism instances, same disciplinary commitment.
- **Authored at** [[openai|OpenAI]]. Adds to OpenAI's product family alongside Codex (now hosting Codex Security as a built-in capability) and the broader Codex ecosystem.
- **Convergent with** [[claude-code-security-announcement|Claude Code Security]] (Anthropic, Feb 2026) on the methodological frame: both products explicitly reject rule-based pattern-matching SAST and explicitly adopt the human-security-researcher metaphor. Both are commercial closed-source private-preview offerings.

**Methodological frame convergence — rejecting rule-based SAST.** Both Aardvark and [[claude-code-security-announcement|Claude Code Security]] frame themselves *against* the classical SAST product category: *"Aardvark does not rely on traditional program analysis techniques like fuzzing or software composition analysis"* (OpenAI) and *"Rather than scanning for known patterns, Claude Code Security reads and reasons about your code the way a human security researcher would"* (Anthropic). The frame is convergent: rule-based pattern matching is positioned as the prior generation, LLM-reasoning + tool-use as the successor. [[openant-announcement|OpenAnt]] (Knostic) reaches the same conclusion from the OSS side. This is now a sourced framing across three vendors.

> [!gap] FRT zero-days post not yet ingested
> [red.anthropic.com/2026/zero-days/](https://red.anthropic.com/2026/zero-days/) (cited inline by Claude Code Security: *"using Claude Opus 4.6, our team found over 500 vulnerabilities in production open-source codebases"*) is the load-bearing quantitative reference for the Anthropic side. Already on the wiki's gap list for the [[frontier-ai-for-vuln-discovery|frontier-AI thesis]]; reiterated here as next-ingest candidate.

> [!gap] Common third-party benchmark for vuln-discovery harnesses
> Aardvark (92% on internal golden repos), MDASH (88.45% on CyberGym), raw Mythos (83.1% on CyberGym), XBOW × Mythos (42-55% FN reduction vs Opus 4.6), OpenAnt (no published recall) — all use different evaluation surfaces. A common third-party benchmark (CyberGym extension, AISI evaluation, or new) for verified-exploitable findings is the largest measurement gap on this axis.
