---
type: paper
title: "Mythos for Offensive Security"
address: c-000025
created: 2026-05-13
updated: 2026-08-21
tags:
  - papers
  - xbow
  - mythos
  - anthropic
  - frontier-models
  - vuln-discovery
  - offensive-security
  - ai-vuln-discovery
status: summarized
scope_axis:
  - ai-in-sec-offense
publication_date: 2026-05-12
authors: []
publisher: "XBOW Blog"
source_url: https://xbow.com/blog/mythos-offensive-security-xbow-evaluation
archived_copy: ".raw/articles/xbow-mythos-evaluation-2026-05-13.md"
no_public_url: ""
related:
  - "[[xbow]]"
  - "[[mythos]]"
  - "[[anthropic]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[offensive-ai-state-of-the-field]]"
sources:
  - "[[.raw/articles/xbow-mythos-evaluation-2026-05-13.md]]"
---

# Mythos for Offensive Security — XBOW's Evaluation

**Source:** [XBOW Blog — Mythos for Offensive Security: XBOW's Evaluation](https://xbow.com/blog/mythos-offensive-security-xbow-evaluation) (2026-05-12). Local copy: `.raw/articles/xbow-mythos-evaluation-2026-05-13.md`.

## Source Summary

[[xbow|XBOW]]'s public evaluation of [[mythos|Mythos Preview]] — an Anthropic frontier model granted early access roughly two months before the post — focused on offensive-security use cases: web-app vulnerability discovery, native-code analysis, reverse engineering, and judgment-quality benchmarks. A ten-expert XBOW team ran Mythos through XBOW's internal benchmark suite (the same harness previously applied to [Opus 4.7](https://xbow.com/blog/anthropic-opus4-7-first-look) and GPT 5.5) plus expanded coverage of threat-modeling judgment, source-code vs. live-site comparison, and native/embedded-firmware tasks.

The headline finding: **Mythos Preview is the strongest frontier model XBOW has evaluated at vulnerability candidate discovery, especially from source code, but it is not self-sufficient as a security tool.** It requires "the right harness and the right tools" (XBOW's framing for tool orchestration plus live-site interaction) to convert candidate findings into validated exploits.

## Key Contributions

### Quantitative results

- **Web exploit benchmark**: vs Opus 4.6, Mythos Preview cut false negatives by **42%** (no source), **55%** (with source). Counted only when PoC-or-GTFO validated after ≤80 actions in XBOW's harness.
- **Source-code reading > source-code writing**: Mythos's discovery advantage was larger when given source than when constrained to live-site-only. The recurring theme: "impressive at writing code, but even more impressive at reading it."
- **Token-for-token efficiency**: at high-accuracy operating points Mythos beats Opus 4.6 / GPT 5.5 on output-token budget. At lower accuracy budgets a longer GPT 5.5 run is often more cost-efficient.
- **Anthropic-disclosed pricing** (as cited by XBOW): Mythos Preview is to be **5× Opus pricing** at GA, making cost-normalized comparisons a genuine procurement consideration rather than a benchmark footnote.

> [!contradiction] Pricing claim contradicted by Anthropic-direct source
> XBOW's "5× Opus at GA" claim above is **contradicted by [[anthropic-glasswing-announcement|Anthropic's Project Glasswing landing page]]** (May 12, 2026, same day as XBOW's post). Anthropic states: (a) **Mythos is not planned for general availability**; (b) Glasswing-participant pricing is **\$25 / \$125 per million input / output tokens** — approximately **1.67× Opus 4.6** (\$15 / \$75), not 5×. XBOW may have been quoting verbal communication from Anthropic that was subsequently revised or referred to a different pricing model. Treat the Glasswing landing page as authoritative; the XBOW citation is preserved here for traceability.

### Qualitative results

- **Native-code and reverse engineering**: strong performance on Chromium-related testing and V8 sandbox work — true positives in subtle threat models where prior baselines produced findings without successful validations. Reverse-engineering across unusual firmware/embedded-system architectures was among the most striking outcomes.
- **Browser interaction and visual acuity**: matches Sonnet 4.6, dramatically outperforms Opus 4.6 at UI-element identification. Practically effective for live-site workflows, though not pixel-accurate for exact coordinates.
- **Judgment is mixed**: command-safety benchmark accuracy 77.8% — below both Opus 4.6 (81.2%) and Haiku 4.5 (90.1%). Mythos tends to prioritize the *letter* of safety rules over the *spirit*; literal-conservative bias produces false positives in edge cases.

### Methodological notes

- XBOW disentangles "Mythos the raw model" (used via API as an engine for XBOW's agents) from "Mythos inside Claude Code" — the orchestration, tooling, prompting, and live-site access materially change outcomes.
- XBOW's benchmark harvest design lets a vulnerability be found from code alone, which lets XBOW measure the source-vs-live-site asymmetry cleanly. Even on a benchmark where vulnerability is purely in code, removing live-site access hurts performance *more* than removing source-code access — XBOW's commercial wedge.
- The McGraw quote — "you won't find the majority of defects by staring at code alone" — is cited as motivation for combining static and dynamic analysis under model orchestration.

## CMM / RA Maps-to

- **[[agentic-ai-security-cmm-2026|CMM]] D7 (Observability & Detection) L4–L5** — adversarial coverage primitives; XBOW's harness is an instance of continuous adversarial testing applied to AI app stacks (the [[red-teaming-for-ai-synthesis|four-quadrant red-team grid]]'s "continuous" quadrant for offensive workflows).
- **[[agentic-ai-security-cmm-2026|CMM]] D9 (Operations & Human Factors)** — pricing and operational-cost framing matters; Mythos's 5×-Opus pricing is a procurement consideration relevant to security-operations budgeting.
- **[[agentic-ai-security-reference-architecture|RA]] Observability Plane** — vulnerability-discovery agents (XBOW orchestrating Mythos) sit on the defender side; the analogous attacker-side capability (autonomous offensive agents using the same models) is the offensive-axis mirror.

## Cross-Axis Implications

- **`ai-in-sec-offense`**: XBOW is the canonical autonomous-pentest tooling reference; this is the first sourced page describing how XBOW orchestrates a frontier model. Reduces the gap noted in [[offensive-ai-state-of-the-field|the offensive-AI thesis]]. It is also the first sourced anchor for [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]], establishing that frontier models materially advance vulnerability discovery while still depending on orchestration and tooling to convert candidate findings into validated exploits; that thesis moved from `seed` to `developing` on it.
- **`sec-against-ai`**: not directly addressed, but the implication is sharp — if XBOW + Mythos produces a 42–55% reduction in false negatives on web vulnerability discovery, an analogous offensive deployment by adversaries (without responsible-disclosure pipelines) collapses defender response times. The [[sdlc-in-the-ai-attacker-era|SDLC thesis]] should annex this finding.

## Limitations

- **No raw numbers for absolute pass rates** in the source — the post reports *relative* improvements (42%, 55%) and qualitative ratings, not the underlying baseline pass rates. Independent reproduction will require XBOW (or AISI / Point Estimate) to publish full benchmark numbers.
- **Mythos availability**: not yet on public APIs at the time of writing. The evaluation is on a preview build under direct Anthropic partnership; production behavior may differ.
- **Vendor evaluation by partner**: XBOW received early access from Anthropic and is a commercial beneficiary of Mythos's strengths (its product orchestrates the model). The post is honest about its position but is not independent.

## Open Questions Surfaced

- Where does [[glasswing|Project Glasswing]] (Anthropic's source-code-audit application of Mythos) sit relative to XBOW's offensive-orientation? Is Glasswing the *defender* application that maps to XBOW's offensive one?
- AI Security Institute (AISI) benchmarked Mythos vs GPT 5.5 (referenced via Point Estimate's analysis). What is the AISI canonical methodology and how does it compare to XBOW's?
- What is Anthropic's stance on offensive-security use of Mythos at GA? The Glasswing partnership signals *defender* framing; commercial offensive use (XBOW) appears to be tolerated under existing usage policies but the policy boundary is not addressed in the post.

## See Also

- [[xbow|XBOW]] — the company / autonomous-pentest platform doing this evaluation.
- [[mythos|Mythos]] — the model being evaluated (Anthropic frontier model, preview-stage).
- [[anthropic|Anthropic]] — model vendor.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — the wiki thesis this source anchors.
- [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]] — adjacent thesis.
- [[red-teaming-for-ai-synthesis|Red Teaming for AI: Synthesis]] — for the four-quadrant red-team grid.
