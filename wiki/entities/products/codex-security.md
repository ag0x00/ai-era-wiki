---
type: entity
title: "Codex Security"
address: c-000064
created: 2026-05-15
updated: 2026-08-31
tags:
  - entities
  - product
  - tool
  - vuln-discovery
  - openai
  - codex
  - research-preview
status: seed
scope_axis: [ai-in-sec-defense]
entity_type: product
aliases:
  - "Aardvark"
role: "OpenAI's agentic-security-researcher product. Originally announced 2025 as 'Aardvark, an agentic security researcher powered by GPT-5'; renamed Codex Security and built into Codex on 2026-03-06; available as a research preview to ChatGPT Enterprise, Business, and Edu customers via Codex web."
homepage: https://openai.com/index/codex-security-now-in-research-preview/
maintainer: "[[openai|OpenAI]]"
first_mentioned: "[[codex-security-announcement|Introducing Aardvark]]"
related:
  - "[[openai|OpenAI]]"
  - "[[codex-security-announcement|Aardvark / Codex Security announcement]]"
  - "[[claude-code-security-announcement|Claude Code Security]]"
  - "[[openant|OpenAnt]]"
  - "[[adversarial-reflexion|Adversarial Reflexion]]"
  - "[[mdash|MDASH]]"
  - "[[big-sleep|Big Sleep]]"
  - "[[codemender|CodeMender]]"
  - "[[google-cloud-codemender-preview]]"
  - "[[aisle-openssl-12-of-12|AISLE OpenSSL 12 of 12]]"
sources:
  - https://openai.com/index/introducing-aardvark/
  - https://openai.com/index/codex-security-now-in-research-preview/
  - https://openai.com/policies/outbound-coordinated-disclosure-policy/
  - "[[.raw/articles/openai-aardvark-codex-security-2026-05-15.md]]"
---

# Codex Security (formerly Aardvark)

**Sources:** [Original Aardvark announcement](https://openai.com/index/introducing-aardvark/) · [Codex Security research-preview announcement (2026-03-06)](https://openai.com/index/codex-security-now-in-research-preview/) · [Outbound coordinated disclosure policy](https://openai.com/policies/outbound-coordinated-disclosure-policy/)

## Description

OpenAI's agentic-security-researcher product. Originally announced as **Aardvark** — *"an agentic security researcher powered by GPT-5"* — and run in private beta across OpenAI's internal codebases and external alpha partners for several months before the public announcement. On **2026-03-06** the product was renamed **Codex Security** and built directly into Codex; it is now available as a research preview to ChatGPT Enterprise, Business, and Edu customers via Codex web, with free usage for the initial rollout month.

Four-stage pipeline: **Analysis** (whole-repo threat model) → **Commit scanning** (each new commit against repo + threat model; historical back-scan on first connect) → **Validation** (isolated sandboxed exploit trigger to confirm exploitability) → **Patching** (OpenAI Codex generates patches, Aardvark scans the patch, human review gates application). Methodological frame explicitly rejects rule-based SAST primitives — *"does not rely on traditional program analysis techniques like fuzzing or software composition analysis"* — and adopts the human-security-researcher metaphor of reading code, writing tests, and using tools.

## Relevance to This Wiki

Fifth sourced AI vulnerability-discovery production path as of 2026-05-15, alongside [[big-sleep|Big Sleep]] + [[codemender|CodeMender]] (Google), [[mdash|MDASH]] (Microsoft), [[xbow-mythos-evaluation|XBOW × Mythos]] / [[anthropic-glasswing-announcement|Glasswing]] (Anthropic + partners), [[openant|OpenAnt]] (Knostic OSS), and [[claude-code-security-announcement|Claude Code Security]] (Anthropic commercial preview). Adds the **OpenAI-side commercial-preview** entry — adjacent to and structurally parallel with Claude Code Security on the Anthropic side. Both reject rule-based SAST framing in identical language. Both are commercial closed-source private-preview offerings integrated with the vendor's existing developer-product surface (Codex web vs Claude Code on the web).

## Outputs / Numbers

- **92% recall on "golden" repositories** (internal benchmark with known + synthetically-introduced vulnerabilities). Not directly comparable to MDASH's 88.45% / raw Mythos's 83.1% on the public [[cybergym|CyberGym]] leaderboard because the benchmark sets are different. [[aisle-openssl-12-of-12|AISLE's OpenSSL cohort]] cites this figure as one side of that non-comparison.
- **Ten CVE IDs** assigned from OSS responsibly-disclosed Aardvark findings as of the original announcement.
- **Pro-bono OSS scanning** committed for select non-commercial open-source repositories.
- **Updated outbound coordinated-disclosure policy** released in tandem — explicit shift away from rigid timelines toward collaboration to absorb the discovery-rate increase the tool enables.
- **Base-rate framing**: 40,000+ CVEs reported in 2024; ~1.2% of commits introduce bugs.

## Notable Design Choices

- **Continuous commit-level scanning against a stable whole-repo threat model.** The threat model is the durable artifact; subsequent commit scans are deltas evaluated against it. This is a different structural choice from OpenAnt's per-unit static-then-agentic phasing — Aardvark treats the repository as a stable referent and the commit history as the event stream.
- **Validation by sandboxed exploit trigger.** Each candidate vulnerability is *attempted in an isolated, sandboxed environment* to confirm exploitability. The sandbox primitive (Docker, gVisor, Firecracker, custom) is not disclosed. This is the dynamic-execution form of the same FP-control discipline that [[openant|OpenAnt]] formalizes as Adversarial Reflexion, [[claude-code-security-announcement|Claude Code Security]] formalizes as Claude proving-or-disproving its own findings, and [[mdash|MDASH]] formalizes as a prover stage.
- **Codex-integrated patch generation with one-click human approval.** Patches are generated by OpenAI Codex (separate product), scanned by Aardvark, and surfaced for human review. Nothing applied without approval.
- **Explicit rejection of fuzzing + SCA.** *"Aardvark does not rely on traditional program analysis techniques like fuzzing or software composition analysis. Instead, it uses LLM-powered reasoning and tool-use."* This is the same methodological frame as [[claude-code-security-announcement|Claude Code Security]] — both reject the SAST product category as the prior generation.

## Competitive position (mid-2026)

Google's [[codemender|CodeMender]] entered [[google-cloud-codemender-preview|managed preview]] in July 2026 with the same three-part shape: reason-over-code discovery, sandboxed exploit validation, patch generation under human approval, and the same rejection of pattern matching as the prior generation. Sandboxed validation, which Aardvark could claim as a differentiator at its 2025 launch, is now the shared baseline across all three vendor previews.

Two differences remain. Aardvark's stable whole-repo threat model with commit deltas as the event stream has no counterpart in Google's published description. CodeMender composes with a CNAPP asset graph and an offensive agent on the same platform, which neither OpenAI nor Anthropic offers. Aardvark also still holds the only published recall figure of the three.

## Adjacent Gaps

- **No public benchmark.** Golden repositories not disclosed; recall not comparable across vendors.
- **No published FP rate.** "Low false-positive" asserted without quantification.
- **No published cost shape.** Subscription-bundled for Enterprise / Business / Edu; not bounded for non-subscription OSS use.
- **Sandbox primitive not disclosed.** The validation-stage exploit-trigger harness is asserted but not described.
- **Aardvark internal codename retained**; the wiki's canonical name for the product is *Codex Security* with *Aardvark* as alias.
