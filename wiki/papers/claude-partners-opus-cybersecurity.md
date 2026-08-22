---
type: paper
title: "Partners Putting Opus to Work for Cybersecurity"
address: c-000093
created: 2026-05-23
updated: 2026-05-29
tags:
  - papers
  - glasswing
  - claude-mythos
  - ai-in-sec-defense
  - ai-vuln-discovery
  - ai-in-sec-offense
status: summarized
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
year: 2026
authors: ["Anthropic"]
venue: "Anthropic / Claude blog"
source_url: "https://claude.com/blog/how-our-partners-are-putting-opus-to-work-for-cybersecurity"
archived_copy: ".raw/articles/claude-partners-opus-cybersecurity-2026-05-23.md"
key_claim: "Eight technology and services partners now run Claude Opus in production cyber defense, with the find→fix gap — not discovery — as the explicit target."
methodology: "Vendor announcement aggregating partner-reported results (mix of customer-production, internal, and own-infrastructure validation)."
related:
  - "[[claude-code-security-announcement]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[anthropic-glasswing-initial-update]]"
  - "[[vulnops]]"
  - "[[zero-day-clock]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[wiz]]"
  - "[[palo-alto-networks]]"
  - "[[crowdstrike]]"
  - "[[accenture]]"
  - "[[trend-micro]]"
  - "[[deloitte]]"
  - "[[pwc]]"
sources:
  - "[[.raw/articles/claude-partners-opus-cybersecurity-2026-05-23.md]]"
---

# How Our Partners Are Putting Opus to Work for Cybersecurity

**Source:** [Anthropic / Claude blog — How our partners are putting Opus to work for cybersecurity](https://claude.com/blog/how-our-partners-are-putting-opus-to-work-for-cybersecurity) (published 2026-05-21). Local copy: `.raw/articles/claude-partners-opus-cybersecurity-2026-05-23.md`.

## Key Claim

This companion piece to [[claude-code-security-announcement|Claude Security's public beta]] turns the partner roster into shipped product. Eight technology and services partners now run [[mythos|Claude Opus]] in production defense. The post targets one problem: the gap between finding a vulnerability and fixing it, which is "where much of vulnerability exposure lives." This is the defender-productization wave the [[frontier-ai-for-vuln-discovery|frontier-AI-for-vulnerability-discovery thesis]] anticipated, operationalizing the bottleneck inversion the [[anthropic-glasswing-initial-update|Glasswing one-month update]] reported.

## The Three Areas

Anthropic groups the offerings into three jobs, each mapping to an existing wiki thread:

- **Continuous offensive testing at production scale** — attacking your own systems the way an adversary would, continuously rather than in periodic engagements.
- **Closing the find→fix gap** — triage, prioritization, patch testing, and cross-team handoffs compressed from days to hours. This is [[vulnops|VulnOps]] by another name.
- **Getting AI into production, governed** — the controls, audit evidence, and autonomy boundaries that move security AI out of "pilot purgatory."

## Partner Results

| Partner | Offering | Reported result | Validation context |
|---|---|---|---|
| [[wiz\|Wiz]] | Red Agent | 150,000+ production assets/week; thousands of validated high/critical findings; **zero false positives** | customer production |
| [[palo-alto-networks\|Palo Alto Networks]] | Unit 42 Frontier AI Defense | a year's pentesting effort in **under three weeks** | internal testing |
| [[crowdstrike\|CrowdStrike]] | Frontier AI Readiness & Resilience Service | continuous latent-zero-day hunting; platform trusted by >60% of the Fortune 500 | service |
| [[accenture\|Accenture]] | Cyber.AI | coverage **10% → 80%** across 1,600 apps + 500,000+ APIs; scans **3–5 days → under an hour** | own infrastructure |
| [[trend-micro\|Trend Micro]] | TrendAI Vision One | virtual patching across 185 countries; findings into ZDI **up to 96 days before** a vendor patch | service |
| [[deloitte\|Deloitte]] | CTEM on Ascend | discovery→remediation as one workflow; countermeasure design when no patch exists; remediate in hours | service |
| [[pwc\|PwC]] | Claude Native Cybersecurity | sandbox→production in weeks; governed autonomous execution with audit evidence | service |

BCG, Infosys, and SentinelOne are named as building defensive offerings on Opus; none are live yet.

## Notable Findings

- **Wiz's zero-false-positive claim at 150k assets/week** is the strongest production-scale FP-control assertion in the wiki, extending the *FP-control-as-architectural-primary* discipline from research tools into continuous customer-facing pentest. Red Agent reasons over application logic, chains steps, and adapts to live server responses — the logic-driven flaws that scanners miss.
- **The find→fix gap is the unifying frame.** CrowdStrike's Mark Manglicmot calls it "pushing vulnerability management all the way to the left"; Deloitte's Adnan Amjad says "the gap helps determine whether attackers or defenders win the window" — both restate the [[zero-day-clock|Zero Day Clock]] time-to-exploit logic.
- **Virtual patching as the interim remedy.** Trend Micro's 96-day pre-patch protection window answers the maintainer-side bottleneck the Glasswing update surfaced: when discovery outruns patching, mitigation buys time.
- **Common substrate.** Every offering runs on "the same underlying Opus capability: reasoning about code, understanding which exposures translate into real-world risk, and sustaining long agentic workflows."

## Strengths and Weaknesses

The numbers are partner-reported and mix validation contexts — customer production (Wiz), internal testing (Palo Alto), own-infrastructure (Accenture) — so they are directional, not independently benchmarked. No methodology detail backs the "zero false positives" or coverage figures. The announcement is promotional, but the convergence of seven independent firms on the same find→fix framing, in the same month as the Glasswing update, is itself the signal.

## Relations

- Supports: [[vulnops|VulnOps]] — the three-area structure is VulnOps productized; the find→fix gap is the explicit target across all seven partners.
- Supports: [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — the defender-productization wave the thesis projected.
- Supports: [[claude-code-security-announcement|Claude Security]] — names the partner access points promised at public-beta launch.
- Extends: [[anthropic-glasswing-announcement|Glasswing]] — several partners (Wiz, Palo Alto, CrowdStrike) are coalition members now shipping.
