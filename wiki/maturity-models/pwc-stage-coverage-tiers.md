---
type: maturity-model
title: "PwC Stage-Coverage Tiers (GenAI-in-SDLC Adoption Maturity)"
address: c-000040
created: 2026-05-13
updated: 2026-07-30
tags:
  - maturity-models
  - pwc
  - agentic-sdlc
  - genai-adoption
  - sdlc-maturity
status: developing
scope_axis:
  - sec-of-ai
adoption_signal: active
last_substantive_update: 2026
coined_by:
  - "[[pwc]]"
related:
  - "[[pwc]]"
  - "[[pwc-agentic-sdlc-in-practice]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[anthropic-2026-agentic-coding-trends]]"
  - "[[collaboration-paradox]]"
  - "[[vibe-coding]]"
  - "[[microsoft-cli-coding-agent-adoption-study]]"
  - "[[generative-coding-deployment-shape-2026]]"
sources:
  - ".raw/papers/pwc-future-of-solutions-dev-gen-ai-2026.pdf"
---

# PwC Stage-Coverage Tiers (GenAI-in-SDLC Adoption Maturity)

PwC Middle East's **4-archetype Stage-Coverage Tiers** is a maturity-model framework introduced in [[pwc-agentic-sdlc-in-practice|the 2026 Agentic SDLC report]] that classifies organizations by **breadth of GenAI integration across the seven SDLC stages** (Ideation / Design / Coding / Testing / CI-CD / Monitoring / Maintenance). It is *not* a security maturity model — adjacent to but orthogonal from the wiki's [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]. An organization can be CMM L4 (security mature) but PwC Observer (no GenAI SDLC adoption), and vice versa.

## Scope and Construction

**Survey basis**: 377 respondents in GCC + Jordan + Egypt (May-June 2025), management roles (CTO/CIO/Director/Manager). Stage-coverage is self-reported (yes/no per stage), so each respondent has an integer score 0-7 representing how many SDLC stages GenAI augments.

**Tier thresholds** (equal-quartile bands):

| Tier | Stages Augmented | Distribution |
|---|---|---|
| **Observer** | 0-1 | 32.4% (n=122) |
| **Experimenter** | 2-3 | 13.3% (n=50) |
| **Integrator** | 4-5 | 16.2% (n=61) |
| **Pioneer** | 6-7 | 38.2% (n=144) |

Distribution is **polarized** — 70.6% are at the extremes (Observer + Pioneer); only 29.5% in middle tiers. PwC's interpretation: organizations leapfrog from minimal to near-full automation; capability-building programs should bifurcate tracks ("getting started" for Observers vs "scaling architecture" for near-Pioneers).

## Tier Profiles

### Tier 1 — Observer (0-1 stages, 32.4%)

- Teams slow to adopt and careful in exposing their SDLC to GenAI.
- Median team size: 8 FTE; tight 1-10 range.
- Skills maturity self-rating: 33.6% "High or Very High" (mean score 3.1/5).
- Closed-source LLM share: 32.8% (lower than other tiers).
- 20.5% still "exploring" tools without committing.
- **Top barrier**: security (47% — 9pp over whole sample). Security paralysis is the defining pain point.
- Cadence: 31 releases/year (median).
- Defect-rate reduction (among bug-trackers): 53.8% report improvement (n=200 cohort).
- Cost-goal uptake: 28.7%.

### Tier 2 — Experimenter (2-3 stages, 13.3%)

- Teams that experiment GenAI on specific tasks but maybe not in a sustained way.
- Median team size: 8 FTE (similar to Observers).
- Skills: 66% High+ (mean 3.86/5).
- Cadence: 47 releases/year.
- Defect-rate reduction: 87.5%.
- Cost-goal uptake: 34%.

### Tier 3 — Integrator (4-5 stages, 16.2%)

- Teams that have adopted GenAI into their SDLC workflows and are focusing on specific tasks in a sustained way.
- Median team size: 8 FTE.
- Skills: 70.5% High+ (mean 3.84/5).
- Cadence: 59 releases/year.
- Defect-rate reduction: 89.2%.
- Cost-goal uptake: 42.6%. **Inflection point** — defect-rate reduction jumps 26pp vs Experimenter; cost goal rises 9pp; efficiency measures begin paying off once 4-5 stages are automated.
- **Compliance pain spikes** (regulatory complaints +5pp over whole sample) — once breadth scales, governance becomes acute.

### Tier 4 — Pioneer (6-7 stages, 38.2%)

- Teams that have already been adopting GenAI on every aspect or project in their SDLC workflows with full augmentation.
- Median team size: **15.5 FTE** (~2× Observer); upper quartile reaches 35 FTE.
- Skills: 86.6% High+ (mean **4.31/5**).
- Cadence: **74 releases/year** (~+44 vs Observer).
- Defect-rate reduction (among bug-trackers): **96.3%**.
- Cost-goal uptake: **44.4%**.
- Closed-source LLM share: 47.9% (highest); hybrid 31.3%; open-source-only 18.8%.
- 79% maintain multi-role "AI ops squads"; 62% maintain curated prompt libraries.
- 90% are "likely to raise GenAI investment" within 24 months.
- 90% are pursuing agentic AI apps (vs 36% Observers — 2.5× multiplier).

## Correlations and Findings

PwC reports significant correlations between Stage-Coverage tier and downstream outcomes:

| Outcome | Correlation | p-value |
|---|---|---|
| Productivity impact (perceived) | Spearman ρ = **+0.56** | 2 × 10⁻³² |
| Release cadence | Spearman ρ = +0.15 | 0.004 |
| Investment sentiment | Spearman ρ = **+0.52** | < 10⁻²⁰ |
| Agentic-AI app interest | χ²(3) = 83.6 | 4 × 10⁻¹⁷ |
| Team size | Spearman ρ = +0.41 | < 10⁻¹² |
| Defect-rate reduction (track-and-improved) | Spearman ρ = +0.64 | < 10⁻²⁸ |
| Confidence in agentic-SDLC future | Spearman ρ = +0.43 | 2 × 10⁻¹⁸ |

Causality is bidirectional in some places ("Productive teams may simply automate more stages because they can, not solely because GenAI caused the lift" — PwC's own caveat).

## Relationship to Other Wiki Maturity Models

| Model | Axis | Cap structure |
|---|---|---|
| [[agentic-ai-security-cmm-2026\|Agentic AI Security CMM 2026]] | Agentic AI **security** maturity | 5×9 (Levels × Domains) — cumulative |
| PwC Stage-Coverage Tiers (this page) | GenAI-in-SDLC **adoption breadth** | 4 tiers — categorical bins |
| [[red-teaming-capability-framework\|Red Teaming Capability Framework]] | Red-team operational capability | 5 tiers — cumulative |
| [[clasp\|CLASP]] | Capability-centric evaluation rubric | per-dimension scoring |

**Key distinction**: PwC's framework measures **breadth of adoption** (how many stages does GenAI touch), not **depth of capability** (what does GenAI do in each stage) or **maturity of governance** (how well-controlled is the deployment). It is one axis of a multi-axis assessment.

The wiki's existing CMM and PwC's tiers are **orthogonal**:

- An org at **CMM L4 + PwC Observer** has mature security practices but minimal GenAI adoption — common in regulated industries.
- An org at **CMM L1 + PwC Pioneer** has aggressively rolled out GenAI without commensurate security maturity — the "shadow GenAI" problem the wiki's `sec-of-ai` axis tracks.
- An org at **CMM L4 + PwC Pioneer** is the target end-state.

PwC's data shows that **as teams move from Observer to Pioneer, security shifts from being the #1 barrier (47%) to a moderate concern (33%)**. The implication: security-readiness scales *with* adoption, not before it. This is consistent with the wiki's framing of [[agentic-ai-security-cmm-2026|CMM]] progression as a co-evolution of capability and controls.

[[microsoft-cli-coding-agent-adoption-study|Microsoft's 2026 study]] supplies the diffusion mechanism this model's archetypes describe as end states: adoption tracks organizational proximity rather than tool evaluation, with peer and manager use the dominant predictors. An organization does not choose its archetype so much as discover it, which is why coverage breadth tends to be measured after the fact. The security counterpart of a rising coverage tier is a shifting deployment shape, catalogued in [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]].

## Limitations

- **Self-reported breadth**: yes/no per stage answers don't measure depth within stage. A team using GenAI for ad-hoc autocomplete in Coding counts the same as a team using fully autonomous PR agents.
- **Geographic specificity**: GCC + Jordan + Egypt only. Generalization to other regions or industries requires care.
- **Causal direction unclear**: high-velocity teams may automate more stages because they can, not because GenAI caused the velocity.
- **No security dimension**: PwC's tiers do not measure security controls, governance, or risk posture. Use the wiki's [[agentic-ai-security-cmm-2026|CMM]] for that axis.
- **Tier thresholds are PwC's choice**: alternative cut-points would shift counts but not the polarized shape.

## See Also

- [[pwc-agentic-sdlc-in-practice|PwC Agentic SDLC paper]] — primary source.
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — orthogonal-axis wiki maturity model.
- [[anthropic-2026-agentic-coding-trends|Anthropic 2026 Trends Report]] — adjacent vendor-strategic forecast.
- [[collaboration-paradox|Collaboration Paradox]] — concept that bounds even Pioneer-tier full-delegation claims.
- [[vibe-coding|Vibe Coding]] — concept newly named in PwC's report.
- [[metr-rct-2025|METR 2025 RCT]] — productivity-counter-evidence study cited by PwC.
