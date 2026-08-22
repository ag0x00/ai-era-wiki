---
type: paper
title: "CodeMender: AI Agent for Code Security"
address: c-000034
created: 2026-05-13
updated: 2026-08-21
tags:
  - papers
  - google
  - deepmind
  - codemender
  - gemini-deep-think
  - vuln-patching
  - ai-vuln-discovery
  - ai-in-sec-defense
status: summarized
scope_axis:
  - ai-in-sec-defense
publication_date: 2025-10-06
authors:
  - Alex Rebert
  - Arman Hasanzadeh
  - Carlo Lemos
  - Charles Sutton
  - Dongge Liu
  - Gogul Balakrishnan
  - Hiep Chu
  - James Zern
  - Koushik Sen
  - Lihao Liang
  - Max Shavrick
  - Oliver Chang
  - Petros Maniatis
publisher: "Google DeepMind"
source_url: https://deepmind.google/blog/introducing-codemender-an-ai-agent-for-code-security/
archived_copy: ".raw/articles/google-codemender-deepmind-2025-10-06.md"
no_public_url: ""
related:
  - "[[codemender]]"
  - "[[google-cloud-codemender-preview|CodeMender Preview on Google Cloud]]"
  - "[[google]]"
  - "[[big-sleep]]"
  - "[[google-big-sleep-projectzero]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[frontier-ai-for-vuln-discovery]]"
sources:
  - "[[.raw/articles/google-codemender-deepmind-2025-10-06.md]]"
---

# Introducing CodeMender — an AI Agent for Code Security

**Source:** [Google DeepMind — Introducing CodeMender (October 6, 2025)](https://deepmind.google/blog/introducing-codemender-an-ai-agent-for-code-security/). Local copy: `.raw/articles/google-codemender-deepmind-2025-10-06.md`.

## Source Summary

Foundational announcement of **CodeMender** — Google DeepMind's AI agent that *patches* software vulnerabilities, complementing the discovery-side capability of [[big-sleep|Big Sleep]]. CodeMender operates **reactively** (instantly patching new vulnerabilities) and **proactively** (rewriting existing code to eliminate entire vulnerability classes). Over the six months before announcement, the team had already **upstreamed 72 security fixes to OSS projects** including codebases as large as 4.5 million lines of code. All patches are reviewed by human researchers before submission.

The strategic frame: as AI-powered vulnerability discovery accelerates ("it will become increasingly difficult for humans alone to keep up"), the patching pipeline becomes the bottleneck. CodeMender addresses the bottleneck.

## Key Contributions

### Architecture

CodeMender leverages **Gemini Deep Think** as the reasoner. The system pairs the LLM with a toolbox for reasoning + validation:

- **Advanced program analysis** — static analysis, dynamic analysis, differential testing, fuzzing, **SMT solvers**. Used to systematically scrutinize code patterns, control flow, and data flow; identify root causes of security flaws and architectural weaknesses.
- **Multi-agent systems** — special-purpose sub-agents tackle specific aspects:
  - An **LLM-based critique tool** highlights differences between original and modified code to verify proposed changes do not introduce regressions; the agent self-corrects based on critique feedback.
  - An **LLM-judge for functional equivalence** is used at the validation stage to confirm semantics are preserved across modifications.
- **Automatic validation** — only surfaces high-quality patches for human review. Quality dimensions: fixes the *root cause* (not just the symptom), functionally correct, no regressions, follows style guidelines.

The architecture parallels [[mdash|Microsoft MDASH]]'s five-stage Prepare-Scan-Validate-Dedup-Prove pipeline but oriented to *patching* rather than *discovery*. The shared design pattern — multi-agent specialization with LLM-judge validation — is converging across vendors.

### Two operating modes

1. **Reactive patching** — given a newly-discovered vulnerability, CodeMender debugs root cause and devises a patch. Two examples in the post: (a) a heap buffer overflow where the actual problem was "incorrect stack management of XML elements during parsing"; (b) a non-trivial patch dealing with "complex object lifetime issues" requiring modification of a custom C code generator inside the project.
2. **Proactive rewriting** — applies safer constructs to existing code. Worked example: `-fbounds-safety` annotations on the **libwebp** image compression library. Once applied, the compiler adds bounds checks that would have rendered **CVE-2023-4863** (the libwebp zero-click iOS exploit used in BLASTPASS / NSO Group operations) "unexploitable forever," along with most other buffer overflows in annotated sections.

### Results to date (as of Oct 2025)

- **72 security patches upstreamed** to OSS projects in the six months before announcement.
- Some target codebases as large as **4.5 million lines of code**.
- All patches **human-reviewed before submission**.
- Patches "have already been accepted and upstreamed."

### CVE-2023-4863 / libwebp reference

The post cites [CVE-2023-4863](https://www.cve.org/CVERecord?id=CVE-2023-4863) — a heap buffer overflow in libwebp used in [a zero-click iOS exploit](https://citizenlab.ca/2023/09/blastpass-nso-group-iphone-zero-click-zero-day-exploit-captured-in-the-wild/) (BLASTPASS, attributed to NSO Group) — as the concrete impact context for proactive annotation. The argument: applying `-fbounds-safety` annotations to libwebp would have *prospectively* prevented exploitation of that class of vulnerability across the entire dependency surface.

## CMM / RA Maps-to

- **[[agentic-ai-security-cmm-2026|CMM]] D6 (Data, Memory & RAG) L5+** — proactive rewriting of vulnerable data-handling code (libwebp, XML parsers) is a D6-adjacent primitive.
- **[[agentic-ai-security-cmm-2026|CMM]] D8 (Supply Chain & AI-BOM) L5+** — upstreaming patches to OSS codebases is a supply-chain hardening primitive; the 4.5M-LOC scale suggests dependency-graph-wide reach.
- **[[agentic-ai-security-reference-architecture|RA]] Observability Plane** — patch validation (regression checks, functional equivalence) extends agent-output auditing.
- **[[agentic-ai-security-cmm-2026|CMM]] D9 (Operations & Human Factors)** — human-review-before-submission is the explicit HITL pattern; analogous to [[plan-validate-execute|Plan-Validate-Execute]] but applied to autonomous-patch generation.

## Convergence with Other Wiki Sources

- **Big Sleep → CodeMender as discovery → patching**: Google's two-pronged DeepMind-affiliated stack. Big Sleep finds, CodeMender patches. The integration / handoff architecture is not documented in either post, but the symmetry is structural.
- **Multi-agent + LLM-judge pattern**: shared with [[mdash|MDASH]] (debater + critique stages), [[clasp|CLASP]]-style capability evaluation, and Stripe's [[guardrails-beyond-vibes-talk|Guardrails Beyond Vibes]] LLM-judge usage. The pattern is converging.
- **OSS-Fuzz lineage**: CodeMender's announcement cites OSS-Fuzz and [AI-powered fuzzing](https://security.googleblog.com/2023/08/ai-powered-fuzzing-breaking-bug-hunting.html) as prior Google AI security work. The Google AI-security stack lineage: OSS-Fuzz → AI-powered fuzzing → Naptime → Big Sleep → CodeMender.

## Limitations

- **Research-stage productization.** "We're taking a cautious approach, focusing on reliability." No GA pricing, no public API. Patches arrive through human OSS-maintainer outreach. [[google-cloud-codemender-preview|A managed preview on Google Cloud]] (2026-07-21) superseded this on availability, defining three purchasing paths and an enterprise data-handling posture. It publishes no efficacy figures, so the recall gap below is unchanged nine months on.
- **No raw recall / precision numbers.** "72 patches upstreamed" is a forward-looking activity metric, not a recall-against-ground-truth measurement.
- **All patches human-reviewed**. CodeMender is described as a research/HITL system; the autonomy boundary is conservatively drawn.
- **No model attribution beyond family.** "Gemini Deep Think" is named; specific model size, tuning, and inference characteristics are not.
- **Annotated language**: the announcement emphasizes "early results"; the formal technical papers and reports are promised but not yet published.

## Open Questions

- **CodeMender vs SAST**: at scale, does CodeMender replace traditional static-analysis tools, augment them, or run alongside?
- **Patch acceptance rate by maintainers**: 72 patches upstreamed — what fraction were accepted vs rejected? Maintainer feedback patterns?
- **Big Sleep + CodeMender integration**: do they exchange artifacts directly, or are they independent agents operating on the same codebase?
- **Glasswing role**: [[anthropic-glasswing-announcement|Glasswing]] (May 2026) names Big Sleep and CodeMender as Google's parallel AI-cyber tools but does not describe operational integration with Mythos / Glasswing-partner work.
- **Authorship overlap**: 13 named authors. Several names (e.g., Oliver Chang on OSS-Fuzz) are recognizable across Google AI-security publications. The team continuity is the human-capital signal.

## See Also

- [[codemender|CodeMender (product page)]] — the agent.
- [[big-sleep|Big Sleep]] — discovery-side counterpart.
- [[google-big-sleep-projectzero|Big Sleep foundational paper]] — adjacent source.
- [[google|Google]] — vendor.
- [[anthropic-glasswing-announcement|Glasswing announcement]] — May 2026 coalition that names Big Sleep + CodeMender.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — wiki thesis CodeMender co-anchors.
- [[mdash|MDASH]] — parallel multi-agent discovery system from Microsoft, with similar critique+validation patterns.
