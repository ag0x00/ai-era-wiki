---
type: concept
title: "Collaboration Paradox (60% usage, 0-20% delegation)"
address: c-000038
created: 2026-05-13
updated: 2026-08-21
tags:
  - concepts
  - agentic-coding
  - hitl
  - delegation
  - human-oversight
  - anthropic
status: developing
no_public_url: "wiki-named pattern read out of Anthropic/PwC reports (held as internal sources); no single canonical external page"
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
aliases:
  - "Collaboration Paradox"
  - "60/20 paradox"
  - "Agentic-AI delegation gap"
related:
  - "[[anthropic-2026-agentic-coding-trends]]"
  - "[[anthropic]]"
  - "[[plan-validate-execute]]"
  - "[[hitl]]"
  - "[[least-agency-principle]]"
  - "[[agentic-soc-state-of-the-field]]"
  - "[[pwc-agentic-sdlc-in-practice]]"
  - "[[metr-rct-2025]]"
  - "[[microsoft-cli-coding-agent-adoption-study]]"
  - "[[vulnerability-research-agentic-age-keynote]]"
sources:
  - ".raw/papers/anthropic-2026-agentic-coding-trends-report.pdf"
  - ".raw/papers/pwc-future-of-solutions-dev-gen-ai-2026.pdf"
---

# Collaboration Paradox

The **collaboration paradox** names a quantitative observation from Anthropic's Societal Impacts research (cited in [[anthropic-2026-agentic-coding-trends|the 2026 Agentic Coding Trends Report]]): **developers use AI in roughly 60% of their work, but report being able to "fully delegate" only 0-20% of tasks**. The apparent contradiction resolves because effective AI collaboration requires "thoughtful set-up and prompting, active supervision, validation, and human judgment — especially for high-stakes work." AI is "a constant collaborator," not an autonomous worker.

## Definition

| Term | Value | Source |
|---|---|---|
| Share of developer work where AI is used | ~60% | Anthropic Societal Impacts research |
| Share of tasks that can be "fully delegated" to AI | 0-20% | Anthropic Societal Impacts research |
| Implied gap | ~40-60% of work | derived |

The "implied gap" — the range of work where AI is in use but the developer is not in fully-delegated mode — represents the **active-collaboration band**. The collaboration paradox observes that this band is the *largest single category* of agentic AI usage in software engineering, not an edge case.

## Significance

The framing has direct architectural and governance implications:

1. **HITL is not optional**, even at scale. The [[plan-validate-execute|Plan-Validate-Execute]] pattern, [[hitl|HITL for agentic AI]], and the [[least-agency-principle|least-agency principle]] are not edge-case guardrails — they are the operating mode for the majority of agent usage.
2. **"Fully autonomous agent" is the wrong product target.** A coding agent positioned for 100% autonomous task completion is overpromising on 80%+ of tasks. The actual frontier is *collaboration quality*: how the agent surfaces its reasoning, accepts intermediate corrections, knows when to ask for help.
3. **Productivity gain attribution differs**. Productivity gains in the active-collaboration band come from **time saved per task** + **increased task volume** + **lower setup friction** — not from autonomy displacement of human work. This matches Trend 6's "productivity through output volume, not just speed" framing.
4. **Calibration matters more than capability**. Engineers describe "developing intuitions for AI delegation over time" — when to trust, when to verify, when to take over. As [[anthropic-2026-agentic-coding-trends|the trends report]] quotes: *"I'm primarily using AI in cases where I know what the answer should be or should look like. I developed that ability by doing software engineering 'the hard way.'"*

## Architectural Implication

The collaboration paradox is the strongest single-source argument for the wiki's [[plan-validate-execute|Plan-Validate-Execute]] pattern as the default operating mode for agentic AI — not just for irreversible actions, but for the routine majority of agent-assisted work. The pattern's three stages (plan, validate, execute) are exactly the active-collaboration mode the data implies.

It is also the strongest argument against autonomy-maximizing designs that treat HITL as a fallback rather than a primary mode. The [[agentic-ai-security-cmm-2026|CMM D9 (Operations & Human Factors)]] should anchor its L3+ progression in the collaboration-paradox data rather than in "minimize human review" as the implicit progression target.

## Application to Security Domains

The collaboration paradox has specific implications for the wiki's security-axis pages:

- **`ai-in-sec-defense`**: [[agentic-soc-state-of-the-field|Agentic SOC]] systems should be designed for *analyst collaboration*, not analyst replacement. The 60%/0-20% data applies symmetrically: an analyst's usage of a defender agent is in collaboration mode for most cases.
- **`sec-of-ai`**: red-team campaigns against AI applications using AI orchestration ([[pyrit|PyRIT]], [[garak|garak]], etc.) operate in collaboration mode by default — engineer-with-AI rather than autonomous campaigns.
- **Vulnerability discovery**: [[xbow-mythos-evaluation|XBOW]], [[mdash|MDASH]], and [[big-sleep|Big Sleep]] all explicitly retain human-in-the-loop validation — confirming the collaboration paradox at the most autonomy-aggressive end of agentic coding, and qualifying [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]].

## CMM / RA Maps-to

- **[[agentic-ai-security-cmm-2026|CMM]] D9 (Operations & Human Factors)** — collaboration-paradox data should anchor L3+ progression targets toward calibrated-collaboration metrics, not autonomy-maximization.
- **[[agentic-ai-security-cmm-2026|CMM]] D4 (Runtime & Guardrails)** — the active-collaboration band needs guardrails calibrated for "ask for help" patterns, not just for unauthorized-action blocking.
- **[[agentic-ai-security-reference-architecture|RA]] Control Plane** — the architecture should expose collaboration-mode controls (intermediate review, delegation thresholds, escalation triggers) as first-class primitives.

## Independent Corroboration

- [[pwc-agentic-sdlc-in-practice|PwC's 2026 Agentic SDLC report]] independently cites the **Anthropic Economic Impact Index** ("AI in Software Development—Primarily Augmentation, Not Automation") finding that 36% of dev roles use AI for ≥25% of tasks, only 4% extensively, **57% augmentative not replacement**. This is a second Anthropic-published study supporting the same paradox argument from a different methodology.
- The same PwC report cites the **[[metr-rct-2025|METR 2025 RCT]]** (16 experienced devs, 19% slower with AI) as the experimental counter-evidence showing that even when AI *is* used, it does not automatically save time — verification, prompt iteration, and reviewing partially-correct output consume the apparent gains. This is the experimental shape of the paradox.
- [[microsoft-cli-coding-agent-adoption-study|Microsoft's 2026 CLI coding-agent study]] adds a **persistence** result the earlier sources could not supply: the throughput lift held across a four-month window with no statistically significant decay, so the paradox is not a novelty effect that resolves as teams acclimatize. It also reports a signal that cuts against a simple adoption ladder — prior IDE-completion-tool use predicts *starting* with an agentic CLI (+83%) but predicts *lower* retention (−12% to −15%) ([arXiv:2607.01418](https://arxiv.org/html/2607.01418v1)), which suggests the completion-style and agentic modes are different working practices rather than adjacent rungs.
- The convergence: vendor data (Anthropic) reports the *distribution* (60% usage / 20% delegation), advisory data (PwC) reports the *cross-validation* (Anthropic Index + Stack Overflow + JetBrains), and experimental data (METR) reports the *causal direction* (AI in expert hands often slows things down). All three are consistent.

## Limitations and Open Questions

- **Single-source numbers.** The 60%/0-20% figures come from Anthropic Societal Impacts internal research as cited; the underlying study has not been independently ingested or reproduced.
- **Selection bias.** "Developers" in the cited research is plausibly biased toward Anthropic Claude users — early adopters with above-baseline AI tool familiarity. [[vulnerability-research-agentic-age-keynote|Shoshitaishvili's Black Hat USA 2026 keynote]] states the same structural claim from outside coding-productivity research and outside the vendor set: the step from unstructured prompting to a property-aware agentic workflow in his lab's offensive-security pipeline came from human judgement about threat models, and the properties it produced roughly doubled the pipeline's yield again on top of the doubling from workflow.[^asu-keynote] That is the speaker's account of his own lab rather than a study of developers, so it narrows the selection-bias concern by a little and does not retire it.
- **Model-version dependence.** "Fully delegate" capacity scales with model capability; the band may shift with Mythos / next-Opus generations.
- **Task-type dependence.** The paradox averages across task types; the data band on "well-defined, verifiable, low-stakes" tasks is presumably much higher than 0-20%.

## See Also

- [[anthropic-2026-agentic-coding-trends|2026 Agentic Coding Trends Report]] — primary source.
- [[plan-validate-execute|Plan-Validate-Execute]] — the canonical wiki pattern this concept reinforces.
- [[hitl|HITL for Agentic AI]] — adjacent concept; the action-risk tiers and the oversight-requirement axis they compose with.
- [[least-agency-principle|Least Agency Principle]] — architectural correlate.
- [[agentic-soc-state-of-the-field|Agentic SOC]] — application to defender-side agentic systems.
- [[ai-coding-agent-governance|AI Coding Agent Governance]] (Knostic) — adjacent practitioner framework with HITL-as-default assumption.

[^asu-keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06). See [[vulnerability-research-agentic-age-keynote|the talk summary]].
