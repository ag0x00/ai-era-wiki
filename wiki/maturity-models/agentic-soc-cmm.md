---
type: maturity-model
title: "Agentic SOC Capability Maturity Model"
address: c-000184
created: 2026-06-03
updated: 2026-08-21
tags:
  - maturity-models
  - agentic-soc
  - cmm
  - levels-of-autonomy
  - practical
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
adoption_signal: proposed
last_substantive_update: 2026-06-03
published_by: "Claude Research wiki"
tier_count: 5
audience: "SOC leaders, detection engineers, security operations architects, MSSP/MDR buyers"
scoring_approach: "Two coupled axes — a per-function autonomy ladder (L0–L4) and eight maturity domains (L1–L5, CMMI shape) that gate it — right-sized by an org-profile axis; crosswalked to SOC-CMM, NIST IR 8596, MITRE ATT&CK + D3FEND + ATLAS"
related:
  - "[[agentic-soc-state-of-the-field]]"
  - "[[agentic-soc-autonomy-ladders]]"
  - "[[ai-augmented-soc-survey]]"
  - "[[nist-ir-8596-cyber-ai-profile]]"
  - "[[security-data-pipeline-architecture]]"
  - "[[agentic-soc-ra-threat-hunting|Agentic SOC Threat Hunting Surface]]"
  - "[[agentic-soc-ra-investigation-case-management|Agentic SOC Investigation Surface]]"
  - "[[soc-cmm]]"
  - "[[cybersecurity-cmms-exemplars]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[evaluating-ai-soc-agents]]"
  - "[[mythos-ready-security-program]]"
sources:
  - "[[ai-augmented-soc-survey]]"
  - "[[agentic-soc-autonomy-ladders]]"
---

# Agentic SOC Capability Maturity Model

A capability maturity model for the agentic Security Operations Center, distinct from the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] that secures agentic-AI *applications*. Where that model measures whether an agentic application is secured, this one measures whether an agentic SOC's *operations* run safely, and specifically whether the SOC has **earned** the autonomy it grants its agents. It is the maturity half of the dedicated Agentic SOC pair; the reference architecture is its structural counterpart.

The model rests on one idea that separates it from the published autonomy ladders surveyed in [[agentic-soc-autonomy-ladders|Agentic SOC Autonomy Ladders]]:

> Autonomy and maturity are different things, and maturity gates autonomy. How much a SOC delegates to its agents (autonomy) is not the same as how well it is prepared to supervise, measure, and contain them (maturity). A reckless team can switch on high autonomy with no evaluation or governance behind it; a small, careful team can run at low autonomy and be fully mature for its scale. The model's central question is not "how autonomous are you?" but "have you earned the autonomy you are running?"

The model's durable subject is **trusted-autonomy operations**: running automation you can hold accountable at machine speed without losing control of it. AI is the mechanism that makes high delegation reachable today, recorded as the *mechanism attribute* rather than the organizing principle. The ladder, the gating rule, and the domains are written to survive the normalization of AI into ordinary technology, the way "cloud security" became "security." The "AI" and "agentic" framing is era-appropriate for findability; the AI-specific particulars (MITRE ATLAS coverage, AI incident-response playbooks, prompt-injection telemetry) live in the dated control-landscape layer of the per-domain deep dives, expected to fold into routine operations over time. One effect of that convergence is already visible: when agentic defenders detect AI-driven attacks on in-house AI applications, the SOC is at once the defender and a defended surface.

## On this page

- [Scope and boundaries](#scope-and-boundaries)
- [The two coupled axes](#the-two-coupled-axes)
- [Axis 1 — the autonomy ladder (per function)](#axis-1--the-autonomy-ladder-per-function)
- [The eight SOC functions](#the-eight-soc-functions)
- [Axis 2 — the eight maturity domains](#axis-2--the-eight-maturity-domains)
- [The gating rule](#the-gating-rule)
- [Org-profile right-sizing](#org-profile-right-sizing)
- [Crosswalks](#crosswalks)
- [Per-domain deep dives](#per-domain-deep-dives)
- [Open questions and gaps](#open-questions-and-gaps)
- [Relations](#relations)

## Scope and boundaries

**Is:**
- A self-assessment instrument for SOC leaders, detection engineers, and operations architects, scored as a matrix — an autonomy level per function and a maturity level per domain.
- A domain-specific extension of [[soc-cmm|SOC-CMM]], which as of its 2025 report does not yet model AI or automation. This model occupies that gap and crosswalks back to it.
- A prescriptive instrument: beyond scoring where a SOC is, it names the next domain to mature so a function's autonomy can rise safely.

**Is not:**
- A vendor scorecard or a single autonomy number. A SOC's state is a vector across functions, not one grade.
- A claim that more autonomy is better. The top tier is asymptotic; full unsupervised autonomy is not a goal, and running above earned autonomy is the model's defined failure mode.
- An enterprise-only model. The org-profile axis right-sizes it down to a one-person team; a small SOC running fewer functions at lower autonomy is right-sized, not immature.
- A certification. Vendors and FOSS are named where load-bearing, for concreteness, not endorsement.

## The two coupled axes

The model is a grid. Each **SOC function** (the rows) carries an **autonomy level**, meaning how much it runs without a human in the decision. Eight **maturity domains** (the gates) determine how high each function's autonomy can legitimately climb. An orthogonal **org-profile axis** right-sizes both. The reference architecture realizes the same grid as planes and agent surfaces; the CMM scores it.

## Axis 1 — the autonomy ladder (per function)

Adopted from the converged prior art ([[ai-augmented-soc-survey|the MDPI survey]] ≈ Mohsin et al.; see the [[agentic-soc-autonomy-ladders|ladder comparison]]). Scored **per function**: a SOC's state is an autonomy *vector*.

| Level | Name | Human relationship |
|---|---|---|
| L0 | Manual | The function is performed by hand |
| L1 | Assisted | Decision support; a human acts and verifies (human-in-the-loop) |
| L2 | Semi-Autonomous | Routine sub-tasks execute; every consequential action needs explicit approval |
| L3 | Conditional | Runs within bounds, escalates out-of-bounds; humans monitor and intervene (human-on-the-loop) |
| L4 | Delegated | Owns the lifecycle; humans govern outcomes. Asymptotic — a permanent authority boundary, no unsupervised L5 |

**Mechanism is an attribute, not a rung.** The ladder measures delegation, not how much AI is involved. Deterministic automation occupies rungs on its own: a deterministic alert-prioritization rule is L1, a remediation playbook that gates consequential actions on approval is L2, and a high-confidence auto-containment playbook that escalates exceptions is L3, all without AI. Each function cell therefore records its delegation level *and* how much deterministic enforcement bounds the AI. Deterministic enforcement (policy-as-code such as [[hooking-coding-agents-with-cedar-talk|Cedar]], [[plan-validate-execute|plan-validate-execute]], typed tool contracts) both delivers delegation directly and is the safety substrate that lets AI reach higher delegation; more of it raises the safe AI ceiling. Because hand-built playbooks are expensive engineering, the model treats agentic AI as a **barrier-lowering enabler**: it makes the lower and middle rungs reachable for teams that could never afford to script them, not only the higher ceiling the enterprise pursues.

## The eight SOC functions

| # | Function | Group |
|---|----------|-------|
| 1 | Data management & pipeline | Foundation |
| 2 | Detection engineering | Detection & analysis |
| 3 | Alert triage | Detection & analysis |
| 4 | Investigation & case management | Detection & analysis |
| 5 | Threat hunting | Detection & analysis |
| 6 | Incident response & containment | Detection & analysis |
| 7 | Exposure & VulnOps | Exposure |
| 8 | Reporting & post-incident learning | Learning |

Threat intelligence and continuous evaluation are **cross-cutting capabilities, not rows**: the first is grounding woven through detection, triage, hunting, and VulnOps; the second is the discipline that measures the agents, scored as a maturity domain. Prevention is a lifecycle orientation, not a function: its SOC-owned slices (deception, detection/control validation) live in detection engineering, and broad hardening sits with security engineering. NIST CSF *Protect* and DevSecOps are adjacent lifecycle phases, integrated at named seams and cross-referenced, not absorbed.

In-house AI applications are a **monitored asset class, not a separate framework**: the SOC's detection (2), triage (3), and incident-response (6) functions extend to them, threat-modeled by [[mitre-atlas|MITRE ATLAS]] in D6, with AI-specific incident-response playbooks (CoSAI AI-IR) in function 6 and AI-application telemetry as a D1 readiness requirement. *Securing* those applications by design remains with the [[agentic-ai-security-cmm-2026|application-security pair]] (the `sec-of-ai` surface), cross-referenced. The SOC monitors and responds; the application-security pair secures the build.

## Axis 2 — the eight maturity domains

Domains are scored on a five-level, CMMI-shape scale (**L1 Initial → L2 Developing → L3 Defined → L4 Managed → L5 Optimizing**), cumulative: Level N requires every Level N−1 criterion plus the new ones. They are designed from the SOC's operational reality, not retrofitted from the application-security domains; four overlap the application-security stack only where the SOC's own agents must be secured (the shared layer).

| # | Domain | Role | Shared layer |
|---|--------|------|:---:|
| D1 | Telemetry & Data Readiness | Autonomy gate | |
| D2 | Threat Intelligence & Knowledge | Efficacy gate | |
| D3 | Evaluation & Ground-Truth | Autonomy gate | |
| D4 | Agent Identity & Action-Authority | Autonomy gate | ✓ |
| D5 | Observability & Oversight | Autonomy gate | ✓ |
| D6 | Detection & Response Tradecraft (ATT&CK + D3FEND + ATLAS) | Efficacy gate | |
| D7 | Resilience & Agent Supply Chain | Autonomy gate | ✓ |
| D8 | People & Governance | Autonomy gate | ✓ |

The domains play two roles. **Autonomy gates** (D1, D3, D4, D5, D7, D8) determine whether an agent can be trusted to act with less supervision. **Efficacy gates** (D2, D6) determine whether the output is any good: a SOC can run detection autonomously and still detect nothing useful if its tradecraft and threat-intel are weak. Both are scored; only the first set caps autonomy.

## The gating rule

A function may run at autonomy L_k only if the domains that govern that level are mature enough to support it. The governing set is **cumulative**: higher autonomy brings more domains into the gate.

| To run a function at… | …these autonomy-gate domains must support it |
|---|---|
| **L2** (act with approval) | D1 Data Readiness · D4 Identity & Action-Authority |
| **L3** (autonomous in-bounds) | + D3 Evaluation/Ground-Truth · D5 Observability/Oversight |
| **L4** (delegated) | + D7 Resilience/Supply-Chain · D8 People & Governance |

"Support it" maps to the domain's maturity level. The [[ai-augmented-soc-survey|MDPI survey]] supplies the correspondence: its autonomy levels map onto SOC-CMM maturity roughly as L0–L1 ↔ maturity 1–2, L2–L3 ↔ 3–4, L4 ↔ 5. Applied here, a governing domain must be at about maturity **L_{k+1}** to support function-autonomy **L_k**.

The gate is read under three rules:

- **The weakest governing domain caps the function.** A function's earned autonomy ceiling is set by its least-mature governing domain. You cannot delegate triage (L3) if you cannot measure the triage agent's decisions (D3 immature), even if every other domain is strong.
- **The model is prescriptive.** Each function reports three values: current autonomy, the maturity-justified ceiling, and the gap. Operating *above* the ceiling is the failure mode; operating *below* is headroom. To raise a function's autonomy, the model names the binding domain to mature next.
- **The gate bounds a function's autonomy from one side only.** Two per-function qualifications sit alongside it. [[agentic-soc-ra-threat-hunting|The threat-hunting surface]] carries a second bound the gate does not capture: an **automation boundary**, past which added autonomy raises the rate of plausible-but-wrong hypotheses. A SOC mature enough to earn high hunting autonomy can therefore be right to hold hunting lower. [[agentic-soc-ra-investigation-case-management|The investigation surface]] records that one rung carries different risk in different functions, because investigation produces findings: a confident-but-wrong narrative is correctable, while a reckless containment action is not, so the same autonomy level is safer in investigation than in incident response.

The exact maturity thresholds are calibratable; the correspondence above is the starting anchor, not a fixed constant (see [Open questions](#open-questions-and-gaps)).

## Org-profile right-sizing

The autonomy targets and domain expectations are right-sized across three bands. Maturity is scored against the organization's scale and risk, never an enterprise absolute.

| Band | Reality | Path to maturity |
|---|---|---|
| Solo / small | Near or below the [[cyber-poverty-line\|Cyber Poverty Line]] | Borrow capability (ISACs, MSSP/MDR, sector groups); use AI as a barrier-lowering enabler — agentic automation reachable without the expensive playbook and ETL engineering that gated traditional SOAR/SIEM — plus a few well-gated agents |
| Mid | In-house SOC, constrained | Selective AI delegation on high-volume functions; deterministic enforcement does the heavy lifting |
| Enterprise | Full agent fleet | Formal cross-functional alignment, broad delegation, dedicated evaluation and governance |

The floor is **minimum viable resilience** (the [[mythos-ready-security-program|Mythos-ready]] entry tier: realign cost-of-exploitation, early-detection, and blast-radius measures before chasing maturity). A small team running fewer functions at lower autonomy is right-sized, not immature; this is also why People & Governance (D8) reads as informal at small scale and formal at enterprise scale rather than as two separate domains.

## Crosswalks

- **[[soc-cmm|SOC-CMM]]**: the incumbent SOC maturity standard, which does not yet model AI. This model extends it; the [[ai-augmented-soc-survey|MDPI survey]] supplies a worked mapping of autonomy levels onto SOC-CMM maturity that the gating rule reuses.
- **[[nist-ir-8596-cyber-ai-profile|NIST IR 8596]]**: its "Defend" focus area (AI-enabled cyber defense, on the CSF 2.0 Core) is the normative anchor for this model's scope; the CMM domains map their outcomes against it rather than restating them.
- **MITRE ATT&CK + D3FEND + [[mitre-atlas|ATLAS]]**: D6 Tradecraft scores detection coverage against the offensive (ATT&CK), defensive (D3FEND), and AI-system (ATLAS) technique catalogues — the last covering attacks on the in-house AI-application surface.
- **[[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]**: the shared-layer domains (D4, D5, D7, D8) secure the SOC's own agents like any other non-human identity; this model cross-references that pair rather than duplicating it.
- **[[mythos-ready-security-program|Mythos-ready Security Program]]**: its risk register and Priority-Actions table are the CISO-program-level companion; this CMM scores the SOC capabilities several of those actions build.

## Per-domain deep dives

Each domain has a deep-dive page carrying the **"how"**, the way the application-security CMM did: a dated control landscape, level-by-level criteria, right-sizing by org profile, dated vendor and FOSS tooling maps, and a cost/barrier model. This page fixes the framework they hang from.

- [[agentic-soc-cmm-d1-telemetry-data-readiness|D1 — Telemetry & Data Readiness]] — usable telemetry by classic ETL or agentic schema-on-read; AI-application telemetry as a readiness requirement. Grounded by [[security-data-pipeline-architecture|Security Data Pipeline Architecture]].
- [[agentic-soc-cmm-d2-threat-intel-knowledge|D2 — Threat Intelligence & Knowledge]] — grounding, knowledge-graph currency, novel-threat coverage beyond lagging KEV.
- [[agentic-soc-cmm-d3-evaluation-ground-truth|D3 — Evaluation & Ground-Truth]] — the primary autonomy gate; you cannot delegate what you cannot measure.
- [[agentic-soc-cmm-d4-identity-action-authority|D4 — Agent Identity & Action-Authority]] — per-agent identity, scoped permissions, response-authority and blast-radius gating *(shared layer)*.
- [[agentic-soc-cmm-d5-observability-oversight|D5 — Observability & Oversight]] — agent telemetry, intent attribution, human-on-the-loop oversight *(shared layer)*.
- [[agentic-soc-cmm-d6-detection-response-tradecraft|D6 — Detection & Response Tradecraft]] — coverage against ATT&CK, D3FEND, and ATLAS; detection quality; deception; response-playbook maturity.
- [[agentic-soc-cmm-d7-resilience-agent-supply-chain|D7 — Resilience & Agent Supply Chain]] — securing the SOC's own agents and the fleet's resilience under failure *(shared layer)*.
- [[agentic-soc-cmm-d8-people-governance|D8 — People & Governance]] — workforce shift, the human-authority boundary, and the autonomy-raising decision right *(partial shared layer)*.

## Open questions and gaps

> [!gap] Calibration and forthcoming pieces
> - The gating thresholds (which domain maturity supports which autonomy level) are anchored to the MDPI ↔ SOC-CMM correspondence but not yet empirically calibrated. The [[evaluating-ai-soc-agents|Gartner evaluation criteria]] and [[defensebench|DefenseBench]] are candidate calibration signals.
> - The per-domain level criteria and cost models are wiki-internal calibration, not an externally ratified standard; each deep dive flags this and will firm up as the model is applied.
> - D6 Tradecraft scores coverage against D3FEND, which is itself thin on AI-era defensive techniques (agent supervision, evaluation-gating, intent attribution, deception against AI attackers) — a named contribution opportunity tracked in the [[d3fend-ai-defense-technique-gap|D3FEND AI-defense technique gap]].

## Relations

- The maturity half of the dedicated pair anchored in [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]; the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] is its structural counterpart.
- Builds on the prior-art comparison in [[agentic-soc-autonomy-ladders|Agentic SOC Autonomy Ladders]] and the closest academic analog, [[ai-augmented-soc-survey|the MDPI AI-Augmented SOC survey]].
- Shares its securing-the-agents layer with the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] and [[agentic-ai-security-reference-architecture|Agentic AI Security RA]].
