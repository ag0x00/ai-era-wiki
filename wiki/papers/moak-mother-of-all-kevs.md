---
type: paper
title: "Mother of All KEVs (MOAK origin story)"
address: c-000088
created: 2026-05-19
updated: 2026-05-29
tags:
  - papers
  - moak
  - autonomous-exploit-generation
  - kev
  - time-to-exploit
  - zero-day-clock
  - offensive-ai
status: complete
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
authors: ["Niv Hoffman", "Ofir Eisenberg", "Yair Saban"]
published: 2026-04-09
source_url: "https://moak.ai/#moak"
source_file: ".raw/articles/moak-mother-of-all-kevs-2026-04-09.md"
related:
  - "[[moak|MOAK — Mother of All KEVs]]"
  - "[[moak-how-it-works|MOAK How It Works]]"
  - "[[autonomous-exploit-generation|Autonomous Exploit Generation]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[anthropic|Anthropic]]"
  - "[[mythos|Claude Mythos Preview]]"
aliases:
  - papers/moak-mother-of-all-kevs-2026-04-09
  - moak-mother-of-all-kevs-2026-04-09

---

# Mother of All KEVs — MOAK Origin Story

Launch announcement and origin story for the [[moak|MOAK Engine]], published April 9, 2026 by **Niv Hoffman**, **Ofir Eisenberg**, and **Yair Saban** at moak.ai. Companion to [[moak-how-it-works|How Does MOAK Work?]] (technical architecture; authors: Yuval Zadok, Karen Katz, Itay Chechik).

## Discovery Timeline

MOAK was not designed from the start as a KEV-exploitation system. It originated as a workflow for solving CTF challenges. One researcher then tested it against **React2Shell** — described as "the most dangerous vulnerability dropped last year" — to see whether it could handle real-world vulnerabilities.

Initial results failed. After two architectural improvements to reasoning and tooling, the workflow succeeded: **React2Shell exploited in 21 minutes, no human in the loop, no access to public POCs or exploitation details, and with all model training cutoffs confirmed before the vulnerability was disclosed.**

Batch validation: **103 of 122 CISA KEVs** exploited on the first try (84% success rate); 82% completed in less than one hour. After systematic architecture refinement, the system became MOAK.

## Updated Performance Figures

The origin-story post gives updated headline numbers relative to the technical deep-dive:

| Metric | "How It Works" post | This post |
|---|---|---|
| KEVs tested | 178 | 200+ |
| Success rate | 97.8% | 98% |
| Under 1 hour | not stated | 82% |
| No human in loop | yes | yes |
| No external POCs | yes | yes |

**Updated baseline.** The 200+ / 98% figures supersede the 174/178 figures from the technical post. Both posts are dated April 9, 2026; this is the public-facing summary and likely reflects the more current count.

## Time-to-Exploit Data Cited

Two independent data points corroborating the TTE collapse:

- **Google Threat Intelligence Group**: Mean TTE shrunk from **63 days → less than one day** over the last five years.
- **VulnCheck 2025**: nearly **one-third of all KEVs in 2025** were exploited in the wild **before or within 24 hours of disclosure**.

**Zero Day Clock milestone table** (sourced to zerodayclock.com, cited via [[sergej-epp|Sergej Epp]]'s initiative):

| Threshold | Status |
|---|---|
| 1 Year | Reached ~2021 |
| 1 Month | Reached ~2025 |
| 1 Week | Reached ~2026 |
| 1 Day | Projected ~2026 |
| 1 Hour | Projected ~2026 |
| 1 Minute | Projected ~2028 |

## The Anthropic Contradiction

> [!contradiction] Contradicts [[anthropic|Anthropic]] Red Team public statement
> The Anthropic Red Team publicly stated: *"Opus 4.6 is currently far better at identifying and fixing vulnerabilities than at exploiting them. This gives defenders the advantage."*
>
> MOAK demonstrates **98% autonomous exploitation** of CISA KEVs using Claude Opus 4.6 as the primary model inside its agentic pipeline. The Anthropic statement assesses Opus 4.6 as a standalone model; MOAK demonstrates that the same model inside an orchestrated five-agent workflow crosses the exploitation capability threshold. The statement may be narrowly true (model-in-isolation) while operationally misleading (model-in-agentic-framework).
>
> A second Anthropic statement in the same post (re: Mythos) acknowledges the capability threshold: *"Have reached a level of coding capability where they can surpass all but the most skilled humans at finding and exploiting software vulnerabilities. … We do not plan to make Claude Mythos Preview generally available."*
>
> See [[anthropic|Anthropic]] entity page for the complementary callout.

## Argument: "The Future Has Arrived"

The post challenges Anthropic's framing that dangerous exploitation capability is a Mythos-only phenomenon not yet available in public models. MOAK's claim: **Claude Opus 4.6 and GPT 5.4 — both publicly available — already achieve near-universal KEV exploitation in agentic context**. The threat is not a future Mythos GA event but the current deployment of frontier-model APIs in agentic exploit pipelines by any sufficiently motivated actor.

## Relation to Existing Wiki Pages

- [[zero-day-clock|Zero Day Clock]]: Google TIG (63 days → <1 day) and VulnCheck (1/3 KEVs in <24h) are new primary-source data points for the TTE collapse trend, independent of the Zerodayclock.com instrument.
- [[moak|MOAK entity page]]: updated team (six total: three from "How It Works" + three from this post) and updated headline stats.
- [[autonomous-exploit-generation|Autonomous Exploit Generation]]: origin-story arc (CTF → React2Shell → 122-KEV batch → 200+ KEV production) is the founding narrative for AEG at production scale.
