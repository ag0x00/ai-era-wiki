---
type: concept
title: "METR RCT: AI Productivity Counter-Evidence"
address: c-000042
created: 2026-05-13
updated: 2026-08-22
tags:
  - concepts
  - metr
  - productivity-research
  - rct
  - counter-evidence
  - ai-productivity
status: developing
scope_axis:
  - sec-of-ai
aliases:
  - "METR RCT"
  - "METR 2025 study"
  - "19% slower finding"
source_url: "https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/"
related:
  - "[[pwc-agentic-sdlc-in-practice]]"
  - "[[collaboration-paradox]]"
  - "[[anthropic-2026-agentic-coding-trends]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[microsoft-cli-coding-agent-adoption-study]]"
  - "[[vulnerability-research-agentic-age-keynote]]"
sources:
  - ".raw/papers/pwc-future-of-solutions-dev-gen-ai-2026.pdf"
---

# METR 2025 RCT — AI Productivity Counter-Evidence

In **July 2025**, **METR** (Model Evaluation and Threat Research) published a **randomized controlled trial** showing that enabling **early-2025 AI tools** for **16 experienced open-source maintainers working on their own repositories** made them **~19% slower** on real tasks. The finding is the cleanest single counter-evidence anchor to AI-productivity claims from vendors and consultancies, and it is now widely cited — including by [[pwc-agentic-sdlc-in-practice|PwC's 2026 Agentic SDLC report]] (Indicator 14 reference) where it is positioned as the cautionary counterweight to the survey-based productivity findings.

> [!gap] Direct METR primary source
> This concept page summarizes the METR 2025 RCT as cited and characterized by PwC. The original METR study has not yet been independently ingested. Direct ingest from [metr.org](https://metr.org) is a high-priority candidate for the next ingest pass to verify methodology, full results, and the exact claim wording.

## The Study

| Field | Value |
|---|---|
| Publisher | METR (Model Evaluation and Threat Research) |
| Date | July 2025 |
| Design | Randomized controlled trial |
| Population | 16 experienced open-source maintainers |
| Task substrate | Real tasks on the maintainers' own repositories (in-domain) |
| AI tools tested | "Early-2025 AI tools" (specific model names not in PwC's summary) |
| Headline result | Maintainers were **~19% slower** when AI tools were enabled vs disallowed |

The study's design is significant in two ways: (a) **own-repository in-domain context** — the maintainers were experts on the codebases being tested, so AI assistance was operating in a context where the human's prior knowledge was very high; (b) **randomized**, so participant-level confounds are controlled.

## Forecast vs Observed

PwC's reproduction of METR's data (Indicator 14):

| Condition | Forecast time | Observed time |
|---|---|---|
| AI-disallowed (n=246 forecast, n=110 observed) | High | Moderate |
| AI-allowed (n=246 forecast, n=136 observed) | Moderate (faster) | High (slower) |

**The forecast was that AI-allowed runs would be faster than AI-disallowed**. The observed pattern is the opposite: AI-allowed runs took *longer*. Expectation and reality diverged in a counterintuitive direction.

## Three Mechanisms

PwC's summary suggests three drivers behind the slowdown:

1. **Velocity variance**: AI isn't "free speed"; time shifts into prompt iteration, verification, and fixing half-right code. Plan for peaks and dips, not linear gains.
2. **Cost & quality**: review/cleanup time is real. Governance and test automation must evolve before autonomy pays off.
3. **Talent mix**: the most value may come from AI-literate reviewers/architects ("AI overseers") rather than pure code generation.

## Significance for the Wiki

The METR RCT is the **load-bearing counter-evidence anchor** for productivity claims on the wiki. Multiple sources — [[anthropic-2026-agentic-coding-trends|Anthropic's Trends Report]], [[pwc-agentic-sdlc-in-practice|PwC's Agentic SDLC report]], [[xbow-mythos-evaluation|XBOW's Mythos evaluation]], [[mdash-defense-at-ai-speed|Microsoft's MDASH announcement]] — make productivity claims of varying scales. METR provides the experimental counterweight that prevents the wiki's position from drifting toward uncritical adoption.

**Important caveats** (per PwC and the wiki's own framing):

1. **Early-2025 tools**: the model generation tested is now ~12+ months old. Subsequent capability improvements (Opus 4.6, Mythos Preview, Sonnet 4.6) may shift the result substantially. The 19% slowdown should not be projected forward without evidence.
2. **In-domain expert population**: when the human has high prior knowledge, AI assistance has lower marginal value than when the human is learning a new codebase. METR's design selects for the worst case for AI benefits.
3. **Sample size (n=16)**: small. Effect-size estimates have wide error bars.
4. **Productivity definition**: time-to-complete is one metric. Output volume, defect rate, and downstream rework costs are not captured. The "27% novel tasks" finding from [[anthropic-2026-agentic-coding-trends|Anthropic's Trends Report]] (work that wouldn't be done at all without AI) is invisible to a time-to-complete RCT.

## Relationship to the Collaboration Paradox

METR's 19% slowdown is consistent with [[collaboration-paradox|the collaboration paradox]] — both findings suggest that effective AI collaboration requires active human engagement (verification, prompt iteration, judgment) and is therefore not pure-speed augmentation. Where vendor reports emphasize the upside ("60% of work uses AI"), METR captures the downside ("but doing so well takes time"). Both are real; the wiki holds both.

**Important note**: PwC's reproduction characterizes the finding as *"Speed gains are not guaranteed"* and frames the response as "optimize for verification loops" — not as evidence that AI provides no productivity value. The slowdown is a *yes-but* signal, not a refutation. Vendors that claim universal speedup are overstating; vendors that frame AI as "constant collaborator requiring active supervision" (Anthropic's framing) are consistent with both METR and PwC.

## Application to Wiki Scope Axes

- **`sec-of-ai`**: METR informs how the wiki should frame agent autonomy claims. An agent that *could* operate autonomously may still be slower than a human reviewer for verification-heavy work; deployment decisions should account for this.
- **Vulnerability discovery** ([[frontier-ai-for-vuln-discovery|thesis]]): the [[xbow-mythos-evaluation|XBOW]] / [[mdash-defense-at-ai-speed|MDASH]] / [[google-big-sleep-projectzero|Big Sleep]] claims of capability gains should be read alongside METR — the gains are real but situation-specific, and the verification cost is non-trivial.
- **`sec-against-ai`**: METR also bounds attacker productivity claims — if AI doesn't make experts faster on familiar code, the "attackers will move at machine speed" framing may be more conditional than the [[anthropic-glasswing-announcement|Glasswing announcement]]'s CrowdStrike quote suggests.

## CMM / RA Maps-to

- **[[agentic-ai-security-cmm-d9-operations|CMM D9 (Operations & Human Factors)]] L3+** — METR's findings are the load-bearing evidence for "verification-loop-aware deployment" as a D9 maturity practice.
- **[[agentic-ai-security-cmm-d7-observability|CMM D7 (Observability & Detection)]] L4** — the verification-cost framing argues for closer telemetry on agent-assisted work to measure actual productivity vs assumed productivity.

## Open Questions

- **Replication with 2026-vintage tools**: the METR RCT used early-2025 tools. Would the result replicate with Mythos / Opus 4.6 / GPT 5.5 in 2026? Likely candidate for follow-up studies. **Partially addressed, in the opposite direction.** [[microsoft-cli-coding-agent-adoption-study|Microsoft's 2026 study]] of tens of thousands of engineers on command-line coding agents reports a +24.0% merged-pull-request lift that persists without decay over four months ([arXiv:2607.01418](https://arxiv.org/html/2607.01418v1)). The two results are not directly comparable — METR randomizes, Microsoft observes within-person; METR measures task completion time on the developer's own repositories, Microsoft counts merged pull requests across an organization; METR's population is experienced open-source maintainers, Microsoft's is a general engineering workforce. Neither settles the question, and merged-PR counts do not measure quality or complexity. Treat the pair as bounding the plausible range rather than as one refuting the other.
- **Domain dependency**: the in-domain expert finding doesn't generalize directly to cross-domain or novice users. AI may yield large productivity gains in those settings (as the vendor reports claim). The relevant question is the *distribution* of productivity outcomes, not the headline mean.
- **Task-type breakdown**: PwC's summary doesn't reveal whether the 19% slowdown is uniform across task types or concentrated in particular categories (e.g., bug fixes vs feature work).
- **METR's broader research agenda**: Model Evaluation and Threat Research is the same organization producing several frontier-AI capability/safety evaluations. Their methodology and how it scales is worth wiki tracking.

## See Also

- [[pwc-agentic-sdlc-in-practice|PwC Agentic SDLC paper]] — citing source for this concept page.
- [[collaboration-paradox|Collaboration Paradox]] — adjacent concept that the same data supports.
- [[anthropic-2026-agentic-coding-trends|Anthropic 2026 Trends Report]] — vendor-strategic-forecast counterpoint emphasizing productivity upside.
- [[xbow-mythos-evaluation|XBOW's Mythos Evaluation]] — capability-side evaluation that bounded productivity claims similarly (XBOW: "AI isn't 'free speed'... plan for peaks and dips" parallel framing).
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — wiki thesis that should reference METR as the counter-evidence anchor.
- [[vulnerability-research-agentic-age-keynote|Vulnerability Research in the Agentic Age]] — its own `[!contradiction]` callout bounds the keynote's throughput claims against this RCT's finding.

<!-- sources:auto -->
## Sources

- [METR RCT: AI Productivity Counter-Evidence](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/)
<!-- /sources -->
