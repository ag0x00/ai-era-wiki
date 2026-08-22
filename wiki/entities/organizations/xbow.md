---
type: entity
entity_type: organization
org_type: vendor
title: "XBOW"
address: c-000026
created: 2026-05-13
updated: 2026-08-21
tags:
  - entities
  - organizations
  - vendor
  - autonomous-pentest
  - offensive-security
  - red-teaming
  - ai-in-sec-offense
status: developing
scope_axis:
  - ai-in-sec-offense
homepage: "https://xbow.com"
role: "Autonomous offensive-security platform — orchestrates frontier AI models against live web targets to discover and validate exploitable vulnerabilities"
related:
  - "[[mythos]]"
  - "[[anthropic]]"
  - "[[xbow-mythos-evaluation]]"
  - "[[offensive-ai-state-of-the-field]]"
  - "[[red-teaming-for-ai-synthesis]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[general-analysis]]"
sources:
  - "https://xbow.com"
  - "https://xbow.com/blog/mythos-offensive-security-xbow-evaluation"
  - "https://xbow.com/blog/anthropic-opus4-7-first-look"
  - "https://xbow.com/blog/mythos-like-hacking-open-to-all"
---

# XBOW

**Sources:** [Homepage](https://xbow.com) · [Mythos Evaluation (May 2026)](https://xbow.com/blog/mythos-offensive-security-xbow-evaluation) · [Opus 4.7 First Look](https://xbow.com/blog/anthropic-opus4-7-first-look)

XBOW is an autonomous offensive-security platform that orchestrates frontier AI models — Anthropic's Opus, Sonnet, Haiku, and preview-stage [[mythos|Mythos]]; OpenAI's GPT — against live web targets to discover and validate exploitable vulnerabilities. The product is framed as a *pentest* surface (targeting live deployments as an attacker sees them) rather than a static analysis tool, and the company's commercial wedge is the orchestration plus tooling layer that converts a frontier model's vulnerability *candidates* into validated, reproducible exploits.

## Capability Surface

- **Live-site exploitation harness**: agents execute up to ~80 "actions" per case (shell commands, Python scripts, attack tools) to attempt validated exploitation. Cases are counted as passed only on PoC-or-GTFO outcomes.
- **Web exploit benchmark**: XBOW's internal scoring substrate, harvested from open-source applications frozen at known-vulnerable versions. Reused across multiple Anthropic and OpenAI model evaluations.
- **Multi-model orchestration**: XBOW explicitly maintains "a cadre of models" rather than committing to a single frontier vendor. The choice between Mythos-briefly vs GPT-5.5-longer is framed as a per-task cost-accuracy tradeoff.
- **Source + live-site combined detection**: the canonical XBOW pattern is to analyze source code for leads, probe live-site behavior to understand deployment-specific exposure, then craft validated exploit. Removing live-site access hurts performance more than removing source-code access — even on benchmarks where the vulnerability is purely in code.
- **Visual acuity / browser UI workflows**: XBOW's agents drive browser interaction; visual-acuity QA is part of the internal benchmark suite.
- **Native-code and reverse-engineering**: explicit capability area in the Mythos evaluation; XBOW tests against Chromium, V8, and embedded firmware contexts.
- **Internal benchmarks named in public posts**: web exploit benchmark, command-safety benchmark, visual-acuity QA. Not all of XBOW's evaluation harness is public.

## Position in the Wiki

XBOW closes a major gap on the **`ai-in-sec-offense`** scope axis — see [[scope-expansion-2026-05|Scope Expansion Punch-List]] item 3 and the [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]] thesis. It also anchors [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]: the [[xbow-mythos-evaluation|Mythos evaluation paper]] is the first sourced data point on the wiki for frontier-model-driven vulnerability discovery in operational use.

Relative to other entries on the wiki:

- [[general-analysis|General Analysis]] (CART for agentic AI) is XBOW's nearest neighbor in the scope-axis matrix. General Analysis tests *AI applications*; XBOW tests *web applications using AI as the attacker*. The methodology surface (adversarial testing, validated outcomes, continuous campaigns) is similar; the targets diverge.
- [[mindgard-cart|Mindgard CART]] is closer to General Analysis (AI-application red-teaming) than to XBOW.
- XBOW's public partnership with Anthropic on Mythos preview is analogous to Anthropic's [[glasswing|Project Glasswing]] (mentioned below) — XBOW is the offensive-orientation collaborator; Glasswing is the defender-orientation collaborator.

## Mythos Evaluation (May 2026) Highlights

See [[xbow-mythos-evaluation|XBOW's Mythos Evaluation paper]] for the full source summary. Key claims from XBOW about Mythos Preview, with XBOW's framing:

- 42% false-negative reduction (no source) and 55% (with source) vs Opus 4.6 on web exploit benchmark.
- Source-code reading is Mythos's strongest mode; live-site validation remains "the hard part" and is XBOW's commercial wedge.
- Token-for-token efficient at high-accuracy operating points; not best-in-class cost-normalized for lower-accuracy budgets.
- Strong on native-code and reverse engineering; mixed on judgment (77.8% command-safety accuracy, behind Opus 4.6 at 81.2% and Haiku 4.5 at 90.1%).

## Open Questions

- **Funding and commercial stage**: not addressed in the Mythos evaluation. To be sourced from XBOW press releases, investor disclosures, or analyst coverage.
- **Customer base / case studies**: XBOW's public materials emphasize self-evaluation against frontier models; customer-side case studies are pending ingest.
- **Methodology transparency**: XBOW's benchmark harvest method is described in outline but not in reproducible detail. AI Security Institute (AISI) and Point Estimate analyses (referenced in the Mythos post) are candidate independent comparators.
- **Disclosure pipeline**: when XBOW + Mythos finds zero-days during evaluation, how does coordinated disclosure proceed? Not addressed in the post.

> [!gap] Founders / leadership team
> Not captured in the May 2026 source. Next ingest pass should add founders, exec team, and any public board members.

## See Also

- [[mythos|Mythos]] — Anthropic frontier model XBOW orchestrates.
- [[anthropic|Anthropic]] — model vendor / partner for the Mythos evaluation.
- [[xbow-mythos-evaluation|XBOW's Mythos Evaluation paper]] — the source for this entity page.
- [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]] — the wiki thesis XBOW anchors.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — the adjacent thesis Mythos anchors.
- [[red-teaming-capability-framework|Red Teaming Capability Framework]] — tier-5 vendor evaluation criteria; XBOW fits.
