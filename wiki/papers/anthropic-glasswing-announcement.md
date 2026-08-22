---
type: paper
title: "Project Glasswing: Securing Critical Software"
address: c-000032
created: 2026-05-13
updated: 2026-08-21
tags:
  - papers
  - anthropic
  - glasswing
  - claude-mythos
  - vuln-discovery
  - ai-vuln-discovery
  - ai-in-sec-defense
  - coalition
  - critical-infrastructure
status: summarized
scope_axis:
  - ai-in-sec-defense
  - sec-against-ai
publication_date: 2026-05-12
authors: []
publisher: "Anthropic"
source_url: https://www.anthropic.com/glasswing
archived_copy: ".raw/articles/anthropic-glasswing-2026-05-13.md"
no_public_url: ""
related:
  - "[[glasswing]]"
  - "[[mythos]]"
  - "[[anthropic]]"
  - "[[anthropic-glasswing-initial-update]]"
  - "[[xbow-mythos-evaluation]]"
  - "[[mdash-defense-at-ai-speed]]"
  - "[[cybergym]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
sources:
  - "[[.raw/articles/anthropic-glasswing-2026-05-13.md]]"
---

# Project Glasswing: Securing Critical Software for the AI Era

**Source:** [Anthropic, Project Glasswing](https://www.anthropic.com/glasswing) (May 12, 2026). Local copy: `.raw/articles/anthropic-glasswing-2026-05-13.md`.

> [!note] One-month update available
> Anthropic published a [[anthropic-glasswing-initial-update|first progress report (2026-05-22)]] one month after this launch: 10,000+ high/critical vulnerabilities found across ~50 partners, the discovery-vs-remediation bottleneck inversion, the open-source scanning funnel, and generally available defender tooling. See that page for the current state.

## Source Summary

Anthropic announced **Project Glasswing**, a coalition initiative bringing together twelve named partners (**Amazon Web Services, Anthropic, Apple, Broadcom, Cisco, CrowdStrike, Google, JPMorganChase, the Linux Foundation, Microsoft, NVIDIA, and Palo Alto Networks**) plus 40+ additional organizations that build or maintain critical software infrastructure, to apply **Claude Mythos Preview** for defensive cybersecurity work on the world's most-critical software. Anthropic committed **up to \$100M in usage credits** for Mythos Preview across these efforts, plus **\$4M in direct donations** to open-source security organizations (\$2.5M to Alpha-Omega and OpenSSF through the Linux Foundation; \$1.5M to the Apache Software Foundation).

The strategic frame: **"AI models have reached a level of coding capability where they can surpass all but the most skilled humans at finding and exploiting software vulnerabilities."** Glasswing is positioned as "an urgent attempt to put these capabilities to work for defensive purposes" before "such capabilities proliferate, potentially beyond actors who are committed to deploying them safely."

## Key Contributions

### Scope and boundaries of Claude Mythos Preview

- **Claude Mythos Preview** is the correct full name (the wiki had been using "Mythos" / "Mythos Preview" loosely).
- An **unreleased frontier model** trained by Anthropic. General-purpose; not security-specific.
- **Not planned for general availability.** Anthropic's quote: *"We do not plan to make Claude Mythos Preview generally available."* The model is preview-only and access is restricted to Project Glasswing participants and approved organizations.
- **Pricing for Glasswing participants** (post-credit-burndown): **\$25 / \$125 per million input/output tokens**. Available on Claude API, Amazon Bedrock, Google Cloud Vertex AI, and Microsoft Foundry.
- Anthropic plans to **launch new safeguards with an upcoming Claude Opus model** that allows safeguards to be refined "with a model that does not pose the same level of risk as Mythos Preview." General-availability path goes through that safer Opus successor, not through Mythos itself.

### Concrete vulnerability findings

- "Thousands of high-severity vulnerabilities" found by Mythos Preview, including some in **every major operating system and every major web browser**.
- Three named examples:
  - **27-year-old OpenBSD vulnerability**: remote crash via network connection; one of the most security-hardened OSes in the world.
  - **16-year-old FFmpeg vulnerability**: in a line of code that automated testing tools had hit **5 million times** without ever catching the problem.
  - **Linux kernel privilege escalation**: autonomously found and chained several vulnerabilities to escalate from ordinary user to complete machine control.
- "Nearly all of these vulnerabilities (and develop many related exploits) entirely autonomously, without any human steering."
- All cited examples have been reported and patched; remaining undisclosed vulnerabilities are tracked via cryptographic hashes published on the [Anthropic Frontier Red Team blog](https://red.anthropic.com/2026/mythos-preview).

### Public benchmark scores (Mythos Preview vs Opus 4.6)

| Benchmark | Mythos Preview | Opus 4.6 | Notes |
|---|---|---|---|
| **CyberGym** (1,507 real-world vuln-repro tasks) | **83.1%** | 66.6% | This is the unnamed "second entry" cited by [[mdash-defense-at-ai-speed\|MDASH]] (88.45% leader); MDASH's harness adds ~5 points over raw Mythos. |
| SWE-bench Pro | 77.8% | 53.4% | Software-engineering tasks |
| SWE-bench Multilingual | 87.3% | 77.8% | |
| SWE-bench Verified | 93.9% | 80.8% | (memorization screens applied) |
| Terminal-Bench 2.0 | 82.0% | 65.4% | Terminus-2 harness, 1M token budget; Mythos hits 92.1% on Terminal-Bench 2.1 with 4-hour timeout |
| GPQA Diamond | 94.6% | 91.3% | Reasoning |
| BrowseComp | 86.9% | 83.7% | Web browsing; Mythos uses 4.9× fewer tokens |
| OSWorld-Verified | 79.6% | 72.7% | OS-level tasks |
| Humanity's Last Exam (no tools) | 56.8% | 40.0% | Possible memorization caveat |
| Humanity's Last Exam (with tools) | 64.7% | 53.1% | |

CTI-REALM (Microsoft's open-source security benchmark, mentioned in Microsoft's Glasswing quote) is a candidate next-ingest concept page, not yet documented on the wiki.

### Partner deployment posture

Eight named executives provided quotes, each describing how their organization is using Mythos:

- **Cisco** (Anthony Grieco, SVP & CSTO): "We can identify and fix security vulnerabilities across hardware and software at a pace and scale previously impossible."
- **AWS** (Amy Herzog, VP and CISO): testing Mythos in AWS security operations; applying to critical codebases; helping harden Mythos for broader use.
- **Microsoft** (Igor Tsyganskiy, EVP Cybersecurity and Microsoft Research): tested Mythos against CTI-REALM (Microsoft's open-source security benchmark) with substantial improvements; uses include "augment our security and development solutions."
- **CrowdStrike** (Elia Zaitsev, CTO): *"The window between a vulnerability being discovered and being exploited by an adversary has collapsed; what once took months now happens in minutes with AI."*
- **The Linux Foundation** (Jim Zemlin, CEO): focuses on OSS maintainers without large security teams; "trusted sidekick for every maintainer."
- **JPMorganChase** (Pat Opet, CISO): financial-system framing; "rigorous, independent approach to determining how to proceed."
- **Google** (Heather Adkins, VP Security Engineering): Mythos available to Glasswing participants via Vertex AI; alongside Google's own Big Sleep and CodeMender.
- **Palo Alto Networks** (Lee Klarich, CPTO): *"There will be more attacks, faster attacks, and more sophisticated attacks. Now is the time to modernize cybersecurity stacks everywhere."*

### Anthropic commitments

- **Usage credits**: up to \$100M for Glasswing partners + extended-access organizations.
- **Direct donations**: \$2.5M to Alpha-Omega and OpenSSF (via Linux Foundation); \$1.5M to Apache Software Foundation.
- **Public reporting**: 90-day cadence. Anthropic commits to publicly reporting findings and improvements within 90 days.
- **Standards contribution**: to "collaborate with leading security organizations to produce a set of practical recommendations for how security practices should evolve in the AI era." Specifically named areas: vulnerability disclosure, software update processes, OSS and supply-chain security, SDLC and secure-by-design, standards for regulated industries, triage scaling and automation, patching automation.
- **Government engagement**: "Anthropic has also been in ongoing discussions with US government officials about Claude Mythos Preview and its offensive and defensive cyber capabilities." National-security framing ("US and its allies must maintain a decisive lead").
- **Long-term structure**: "In the medium term, an independent, third-party body — one that can bring together private- and public-sector organizations — might be the ideal home for continued work on these large-scale cybersecurity projects."

## Contradictions Resolved / Surfaced

> [!contradiction] Mythos pricing — XBOW vs Anthropic
> [[xbow-mythos-evaluation|XBOW's blog]] cited Anthropic as saying Mythos would be "5× as expensive as an Opus model" at GA. **Anthropic's direct framing**: Mythos is **not planned for general availability**; preview pricing for Glasswing participants is **\$25 / \$125 per million input/output tokens** — approximately 1.67× Opus 4.6 (\$15 / \$75), not 5×. XBOW's source may have been a verbal description, a different model variant, or based on a prior pricing model. The Glasswing landing page is authoritative.

**MDASH's unnamed #2 on CyberGym = raw Claude Mythos Preview.** [[mdash-defense-at-ai-speed|MDASH's announcement]] reported MDASH at 88.45% on CyberGym, ~5 points above an unnamed #2 entry at 83.1%. Anthropic's Glasswing page now confirms Mythos Preview scored **83.1% on CyberGym** — the same number. The MDASH #2 is raw Mythos Preview, which means **MDASH's multi-model agentic harness adds ~5 percentage points over the raw model**. This is the clearest quantitative measurement we have on the "harness over model" architectural argument from both [[xbow|XBOW]] and Microsoft.

**MDASH's model-stack silence is explained.** [[mdash-defense-at-ai-speed|MDASH's announcement]] said only "generally available AI models" without naming SOTA-reasoner candidates. The Glasswing announcement confirms Microsoft is a Glasswing participant with Mythos access. Mythos is almost certainly one of MDASH's orchestrated models; Microsoft's silence reflects coordinated-announcement constraint, not model-stack mystery.

## CMM / RA Maps-to

- **[[agentic-ai-security-cmm-2026|CMM]] D7 (Observability & Detection) L5+**: Glasswing is the canonical industrial-scale example of frontier-AI-driven defensive vulnerability research. Should be cited as the load-bearing reference for L5+ "research-stage primitives" once the 90-day public report lands.
- **[[agentic-ai-security-cmm-2026|CMM]] D8 (Supply Chain & AI-BOM)**: Glasswing's focus on OSS infrastructure (the \$4M direct donations, Linux Foundation participation) is a supply-chain-security primitive at L5+. The Apache + OpenSSF + Alpha-Omega donations are programmatic.
- **[[agentic-ai-security-reference-architecture|RA]] Observability Plane**: defender-side vulnerability discovery agents (Mythos orchestrated via partner systems) is a candidate L5+ reference primitive.

## Cross-Axis Implications

- **`ai-in-sec-defense`** (primary): Glasswing reframes the wiki's defender-AI axis from "vendor-by-vendor productized capability" to "coalition-backed industrial-scale initiative with committed capital and public reporting commitment." It is the third sourced anchor for [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]: XBOW's offensive evaluation, Microsoft's MDASH defensive evaluation, and Anthropic's coalition framing landed in the same week (May 12, 2026), taking that thesis from the wiki's thinnest evidence base to its strongest inside two days.
- **`sec-against-ai`** (primary): CrowdStrike's quote ("the window between a vulnerability being discovered and being exploited by an adversary has collapsed; what once took months now happens in minutes with AI") and Palo Alto's ("there will be more attacks, faster attacks, and more sophisticated attacks") directly support the [[sdlc-in-the-ai-attacker-era|SDLC thesis]]'s core argument. Anthropic's own framing of the asymmetry, the same capabilities for offense and defense, is the cleanest articulation on the wiki.

## Limitations

- **Vendor-published numbers.** All benchmark scores are Anthropic-self-reported. CyberGym is independently verifiable (public leaderboard); other benchmarks involve memorization caveats and harness-configuration choices.
- **Coalition diversity caveat.** 12 named partners is broad, but the named coalition is heavily US-based and US-aligned. National-security framing ("US and its allies must maintain a decisive lead") is explicit.
- **Capability disclosures.** Many vulnerabilities are noted as cryptographically hashed pending patch; the published examples (OpenBSD, FFmpeg, Linux kernel) are selected for impact.
- **No technical methodology detail.** The post is announcement-grade; technical details for cited vulnerabilities live in the [Frontier Red Team blog](https://red.anthropic.com/2026/mythos-preview) which has not yet been ingested.
- **No model-architecture detail.** Mythos's training, scale, or technical positioning is not disclosed beyond "general-purpose frontier model trained by Anthropic."

## Open Questions Surfaced

- **Frontier Red Team blog ingest**: [red.anthropic.com/2026/mythos-preview](https://red.anthropic.com/2026/mythos-preview), [red.anthropic.com/2026/firefox/](https://red.anthropic.com/2026/firefox/), [red.anthropic.com/2026/exploit/](https://red.anthropic.com/2026/exploit/): three primary technical sources adjacent to Glasswing that should be ingested next.
- **CTI-REALM benchmark**: Microsoft's open-source security benchmark, mentioned in passing by Igor Tsyganskiy. Not currently on the wiki; concept-page candidate.
- **The 40+ additional organizations**: the Glasswing post mentions "over 40 additional organizations that build or maintain critical software infrastructure" but does not list them. The 90-day public report may reveal them.
- **Claude Mythos Preview system card** ([anthropic.com/claude-mythos-preview-system-card](https://anthropic.com/claude-mythos-preview-system-card)): the canonical technical reference; ingest candidate.
- **Big Sleep and CodeMender** (Google): mentioned in Heather Adkins's quote as Google's parallel AI-powered cybersecurity tools. Not on the wiki; concept/product pages plausible.
- **Independent third-party body**: Anthropic floats this as a long-term structure. Worth tracking governance evolution.

## See Also

- [[glasswing|Project Glasswing (entity page)]]: the coalition.
- [[mythos|Claude Mythos Preview]]: the model.
- [[anthropic|Anthropic]]: vendor.
- [[xbow-mythos-evaluation|XBOW's Mythos Evaluation]]: offensive-side companion source.
- [[mdash-defense-at-ai-speed|Microsoft MDASH announcement]]: defender-side companion source from a Glasswing partner.
- [[cybergym|CyberGym]]: public benchmark.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]: the wiki thesis Glasswing reframes.
- [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]: adjacent thesis directly supported by the coalition's framing.
