---
type: maturity-model
title: "Agentic SOC CMM D3 Evaluation and Ground Truth"
address: c-000203
created: 2026-06-03
updated: 2026-08-16
tags:
  - maturity-models
  - cmm
  - agentic-soc
  - evaluation
  - ground-truth
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
related:
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-reference-architecture]]"
  - "[[evaluating-ai-soc-agents]]"
  - "[[defensebench]]"
  - "[[measuring-agent-effectiveness-talk]]"
  - "[[rethinking-security-agent-evaluation-talk]]"
  - "[[llm-as-a-judge]]"
  - "[[cyber-poverty-line]]"
  - "[[vulnerability-research-agentic-age-keynote]]"
  - "[[analyzer-ordering-confound]]"
sources:
  - "[[measuring-agent-effectiveness-talk]]"
  - "[[evaluating-ai-soc-agents]]"
  - "[[defensebench]]"
  - "[[llm-as-a-judge]]"
---

# Agentic SOC CMM D3 Evaluation and Ground Truth

Companion deep-dive to [[agentic-soc-cmm|the Agentic SOC CMM]]'s D3 domain. D3 measures whether the SOC can judge the quality of its agents' decisions against ground truth: rubric evaluation of agent reasoning and behavior, calibration of an [[llm-as-a-judge|LLM-as-a-judge]] grader, regression testing of every change, and a ground-truth store the evaluation harness reads. It scores the [[agentic-soc-reference-architecture|reference architecture]]'s Observability & Evaluation plane on the measurement question — whether agent decision quality is known, not assumed — distinct from D5's question of whether the agent can be watched and overridden.

D3 is an **autonomy gate**, and the primary one. With [[agentic-soc-cmm#the-gating-rule|D5 Observability & Oversight]], it caps whether a function can reach **L3 — autonomous within bounds**: an agent cannot be left to act inside its bounds without an approver if the SOC has no way to measure whether its decisions are good. The model's governing discipline rests on D3 above every other domain because of a single asymmetry it makes explicit.

> Evaluation maturity is the rate limiter on safe autonomy. Deterministic automation is exhaustively testable — a playbook either fires correctly on a known input or it does not — so it leans on change-control and identity/authority maturity and needs little evaluation maturity. AI delegation is non-deterministic: the same alert can draw a different trajectory and a different verdict across two runs, so its decision quality is a measured property, not a verified one. The gating rule therefore gates AI delegation hard by D3. You cannot delegate what you cannot measure, and the SOC's gating discipline is only as trustworthy as the evaluation behind it.

> [!gap] Wiki-internal calibration
> The level criteria, cost model, and right-sizing below synthesize the wiki's own design spec against four grounding sources: the [[measuring-agent-effectiveness-talk|Saxe talk on measuring security-agent effectiveness]] (the load-bearing source on the no-oracle problem, rubric dimensions, and judge calibration), [[evaluating-ai-soc-agents|Gartner's seven evaluation questions]] (the buyer-side outcome and explainability criteria), [[defensebench|DefenseBench]] (the first public defender-agent benchmark, honest about its single-dataset limits), and the [[llm-as-a-judge|LLM-as-a-judge]] concept page. They are wiki-internal calibration, not an externally ratified standard, and will firm up as the sibling domains and crosswalks are tested.

A single problem shapes the whole domain.

**Cybersecurity has no clean ground-truth oracle, so evaluation cannot collapse to a single correctness bit.** Whether a patch works or a binary is malicious runs into undecidable questions; where labels come from people, analysts disagree on which alerts were worth investigating at a double-digit rate. Flipping even about 1% of labels in an evaluation set drives measured accuracy into a *noise ceiling* above which real improvement can no longer be seen.[^saxe] An agent may produce a long, well-justified trajectory of retrieval, tool calls, and reasoning, and an outcome-only metric discards all of it against a label no one fully trusts. D3 maturity is therefore the discipline of measuring the *right* thing — the agent's reasoning and behavior across a multi-stage loop — under noisy ground truth, not of chasing a clean accuracy number that the environment cannot supply.

## Control landscape (dated)

Evaluation tooling has matured fast outside security and is only now acquiring a security-specific layer. The general harnesses and judge frameworks are dated and swappable, and the security-specific benchmark is a research preview; both are kept here rather than in the level definitions.

| Layer | What ships today | Status (mid-2026) |
|---|---|---|
| Eval harness / regression runner | General LLM- and agent-eval platforms (LangSmith, Braintrust, Arize Phoenix, Promptfoo) and open harnesses (OpenAI Evals, DeepEval, Inspect AI from the UK AI Safety Institute), capturing full agent trajectories (steps, tool calls, reasoning), not only final answers | GA across the named platforms; agent-trajectory evaluation is the maturing frontier, and none is security-SOC-specific out of the box |
| LLM-as-a-judge grader | [[llm-as-a-judge\|LLM-as-a-judge]] as the dominant pattern for grading open-ended output; a Bayesian disagreement-weighting model down-weights labels where reviewers disagree, calibrated from a small human sample (~100 in the [[measuring-agent-effectiveness-talk\|Saxe]] account) | Pattern-level and widely adopted; calibration method is practitioner-reported, and the judge's circularity (it may share the agent's failure modes) is a known, partly mitigated risk |
| Rubric / capability-centric evaluation | Multi-dimensional rubrics scoring evidence gathering, policy understanding, first-principles reasoning, auditable logging, and decision accuracy ([[measuring-agent-effectiveness-talk\|Saxe]]); capability-centric, observability-first evaluation of the find→confirm→patch→validate loop ([[rethinking-security-agent-evaluation-talk\|Khurana]]) | Practitioner methodology, not a ratified standard; the rubric criteria are where domain expertise belongs, and they are bespoke per function |
| Security-specific benchmark | [[defensebench\|DefenseBench]] (BOTSv3 over Splunk Boss of the SOC v3) as the first public, reproducible scoreboard for agentic SOC investigation | Research preview; a single Splunk-derived dataset with thin published methodology, currently ranking general coding agents rather than purpose-built defender agents — a comparator the field previously lacked, not a maturity certificate |
| Buyer-side evaluation criteria | [[evaluating-ai-soc-agents\|Gartner's seven questions]]: TDIR outcomes (MTTD/MTTR, with mean-time-to-contain as the end goal) over "alerts processed," explicit autonomy boundaries, and glass-box explainability with human-readable audit trails | Vendor-neutral guidance for procurement; criteria, not scores — it tells a buyer what to ask, not how a product measures |
| Online / production signals | Offline golden-dataset runs paired with online drift monitoring; user-feedback rollouts where no fixed ground truth exists; signal-to-noise ratios such as the [[prompt-volume-to-alert-ratio\|prompt-volume-to-alert ratio]] for the SOC's own AI-application monitoring | Pattern-level; online evaluation is necessary because offline accuracy does not guarantee stable in-production behavior |

The decisive rows in this landscape are **judge and rubric**. The general harnesses solve the plumbing (running variants, storing trajectories, gating CI), but they do not define what a good SOC decision looks like; that is the rubric, and it is bespoke per function. The judge that grades trajectories at scale must itself be calibrated against human reviewers, and trusted only as far as that calibration holds, because an uncalibrated judge can share the failure modes of the agent it scores. DefenseBench shows the shape of a security benchmark but not its sufficiency: a single-dataset scoreboard is necessary and not enough for a multi-stage loop, exactly the gap [[rethinking-security-agent-evaluation-talk|Khurana's capability-centric argument]] names.

## Capability levels

Stated as capabilities specific to evaluation and ground truth; cumulative, so Level N assumes every Level N−1 criterion. The level text is mechanism-agnostic, describing a measurement discipline that would survive AI normalizing into ordinary tooling. AI-specific particulars (judge models, agent-eval harnesses, named benchmarks) sit in the control landscape above; the levels describe the rising rigor of measurement the gating rule reads.

- **L1 — Initial.** Agent decision quality is judged by anecdote. A function is trusted because it looked right in a demo or on an engineer's machine; there is no held-out test set, no rubric, and no ground-truth store. Changes ship without measurement, and a regression is discovered only when an analyst notices a bad call in production.
- **L2 — Developing.** A held-out evaluation set exists — labelled true/false positives, confirmed incidents, or worked cases — and an agent's outputs are scored against it before a change ships. Scoring may be outcome-only and largely manual, but it is repeatable: the same change run twice gives a comparable number, and an obvious regression is caught before release rather than after. This is the measurement floor; it does not yet support delegation, but it ends the demo-driven failure mode. The held-out set is also what makes the comparison honest. A SOC that instead judges a new agent by pointing it at production and counting findings measures the [[analyzer-ordering-confound|ordering confound]] — the incumbent tool has already harvested everything both would have found, so the newcomer's count reflects its position in the sequence more than its quality, and the same comparison run in the other order inverts.[^asu-keynote] A fixed labelled corpus with a known oracle removes that effect, which is a second reason for the L2 requirement beyond repeatability.
- **L3 — Defined.** Evaluation is multi-dimensional and runs as a regression suite on every change. A rubric scores the agent's reasoning and behavior — evidence gathering, policy application, reasoning quality, auditable logging — alongside the final verdict, so a change that fixes output formatting while degrading security content is caught. A ground-truth store is curated and versioned, with label noise acknowledged rather than ignored. Offline evaluation is paired with online drift monitoring in production. This is the readiness floor for **L3 function autonomy (autonomous within bounds)**: with D5 satisfied, D3 at this level lets a function be delegated inside its bounds because the SOC can measure whether those in-bounds decisions are good. A function cannot legitimately cross this gate while its decisions are unmeasured.
- **L4 — Managed.** The grading judge is calibrated and governed. An [[llm-as-a-judge|LLM-as-a-judge]] grades trajectories at scale, calibrated against a human sample with a disagreement-weighting model so noisy labels are down-weighted and the judge's agreement with reviewers is tracked, not assumed. Evaluation gates releases quantitatively: a defined bar across rubric dimensions, aligned with leadership, must be cleared before deployment. Online and offline signals are reconciled, and ground-truth currency is maintained as cases close. The evaluation result feeds the gating rule as a measured input, so a delegated function's autonomy ceiling moves with its measured quality rather than with optimism.
- **L5 — Optimizing.** Evaluation drives an automated improvement loop. With a reliable multi-dimensional measurement in place, agent variants are mutated and selected against the rubric — prompt optimization, configuration search — while guarding against overfitting to the eval set, so the agents improve continuously rather than only on manual tuning. The judge is re-calibrated as the threat surface and case mix drift, and the ground-truth store is a maintained asset with its own freshness and quality metrics. Evaluation is no longer a gate the SOC passes once but a loop it runs continuously.
- **L5+ — Leading Edge.** All of L5, plus a named contribution to shared evaluation practice: a published defender-agent benchmark or dataset, a rubric or judge-calibration method other defenders adopt, or contribution to a security-agent evaluation standard. DefenseBench's existence marks how thin this layer still is; the field lacks the equivalent of a ratified, multi-stage defender-agent benchmark.

Because D3 is the primary autonomy gate, its level appears directly in the [[agentic-soc-cmm#the-gating-rule|gating table]], and the model's whole gating discipline depends on it: a function's earned autonomy is capped at the level its weakest governing domain supports, and for the in-bounds delegation step D3 is the governing domain that makes the cap meaningful. A SOC cannot legitimately delegate triage if it cannot measure the triage agent's decisions, however strong its data, identity, or observability controls. This is the model's central failure mode made concrete: autonomy run ahead of evaluation is reckless autonomy.

## Right-sizing by org profile

The realistic D3 target is scored against the autonomy the SOC actually wants to grant. A small team that runs its agents at low autonomy and evaluates them lightly is right-sized; a team that wants in-bounds delegation must build the measurement to earn it, whatever its size.

| Band | Realistic D3 target | Why |
|---|---|---|
| Solo / small | L1 → L2 | Near or below the [[cyber-poverty-line\|cyber poverty line]], a small team rarely builds a calibrated judge or an automated improvement loop. Where agents are borrowed through an MDR/MSSP, the *provider's* evaluation maturity governs the borrowed function. The team's own bar is a held-out set and a regression check before any change — enough to keep its few well-gated agents off the demo-driven failure mode and at the low autonomy its scale warrants. |
| Mid | L2 → L3 | An in-house SOC delegating high-volume functions (triage, detection) within bounds must reach L3 on those functions: a rubric, a regression suite, a curated ground-truth store, and online drift monitoring. A calibrated judge (L4) is the stretch goal, justified first on the highest-volume delegated function where manual grading does not scale. |
| Enterprise | L4 → selective L5 | A full agent fleet under broad in-bounds delegation needs calibrated, governed judging and quantitative release gates across functions, with selective L5 (the automated improvement loop) where the volume and stakes justify it. L5 earns its cost where the fleet is large enough that continuous, automated evaluation outruns manual tuning. |

A small SOC at L1–L2, running a few agents at low autonomy with a held-out set and a regression check, has right-sized D3: it is not delegating in-bounds, so it does not owe the calibrated-judge machinery that L3+ delegation requires. The asymmetry is the point. Because deterministic automation needs little evaluation maturity, a small team can reach useful L1–L2 *deterministic* delegation through change-control and identity discipline while its D3 stays low, and owes D3 maturity only when it wants *AI* delegation within bounds.

## Cost model

The dominant cost in D3 is evaluation labor — defining rubrics, curating ground truth, calibrating the judge — not eval-harness licensing. The harnesses are inexpensive or free at the licensing line; the spend is the domain expertise to say what a good decision looks like and the recurring labor to keep the ground truth current.

| Level | Tooling / licensing | Operational labor | Run-rate note |
|---|---|---|---|
| L2 | ~0 on an open harness (OpenAI Evals, DeepEval, Inspect AI); commercial platform where bought | ~0.25–0.5 FTE to build a held-out set and a repeatable scoring run | Ground-truth labelling is the load-bearing labor — without labelled cases there is nothing to score against |
| L3 | Add a regression runner and trajectory storage; rubric authoring | ~0.5–1 FTE recurring: rubric definition per function, ground-truth curation and versioning, online drift monitoring | Rubric definition is where domain expertise belongs and cannot be bought off the shelf; it is per-function and bespoke |
| L4 | Judge-model inference cost; release-gating integration | Recurring judge calibration (a human sample plus disagreement-weighting), bar alignment with leadership, online/offline reconciliation | Judge inference is a per-trajectory cost that scales with grading volume; calibration is recurring, not one-off |
| L5 | As L4, plus automated variant search / optimization compute | Heaviest: running and guarding the improvement loop against overfitting, re-calibrating the judge as the case mix drifts | Practitioner accounts put evaluation at roughly half a team's time, repaid by markedly faster, safer iteration — price the loop, not the harness |

D3 is a rubric-and-ground-truth labor cost, not a tooling-licence cost. The expensive failure is the unmeasured regression: a change that scores fine on a single run but degrades decision quality across the distribution. The multi-dimensional regression suite at L3 and the calibrated judge at L4 are the controls that catch it. Price the evaluation rhythm and the ground-truth curation, not the harness.

## Open questions

- The gating model places D3 at the L3 step alongside D5. The exact maturity threshold — how strong D3 must be to *support* a given autonomy level — is anchored to the [[ai-augmented-soc-survey|MDPI ↔ SOC-CMM]] correspondence the [[agentic-soc-cmm#the-gating-rule|main page]] cites, but is not yet empirically calibrated; [[defensebench|DefenseBench]] and the [[evaluating-ai-soc-agents|Gartner criteria]] are candidate calibration signals.
- The no-oracle problem means there is no clean ceiling for "good enough" evaluation. The noise-ceiling and label-disagreement figures are practitioner-reported from one talk;[^saxe] whether the ~1% flip rate and ~100-sample calibration generalize across SOC functions and environments is an open empirical question.
- The judge's circularity is mitigated, not eliminated: a calibrated [[llm-as-a-judge|LLM-as-a-judge]] can still share blind spots with the agent it grades, and the standard human-in-the-loop discovery layer is a backstop, not a proof.
- No public, multi-stage defender-agent benchmark exists. DefenseBench narrows the gap with a single Splunk-derived dataset that currently ranks general coding agents; a ratified, capability-centric benchmark for purpose-built defender agents across the find→confirm→respond→validate loop is the named contribution opportunity at L5+.
- There is no standard metric for evaluation coverage itself — how much of a function's decision space the held-out set and rubric actually exercise — so D3 scoring relies on judgment, not a ratified benchmark.

## Relations

- Companion deep-dive to [[agentic-soc-cmm#axis-2--the-eight-maturity-domains|the Agentic SOC CMM]]'s D3 domain, which classifies D3 as the primary autonomy gate and one of the two that govern the L3 in-bounds delegation step.
- Scores the [[agentic-soc-reference-architecture|reference architecture]]'s Observability & Evaluation plane on the measurement question, distinct from the oversight question that the same plane carries for D5.
- The other L3 gate is D5 Observability & Oversight; the first delegation step is governed by [[agentic-soc-cmm-d1-telemetry-data-readiness|D1 Telemetry & Data Readiness]] and [[agentic-soc-cmm-d4-identity-action-authority|D4 Agent Identity & Action-Authority]], and the L4 step adds D7 Resilience & Agent Supply Chain and D8 People & Governance. The efficacy-gate sibling whose output D3 indirectly measures is [[agentic-soc-cmm-d2-threat-intel-knowledge|D2 Threat Intelligence & Knowledge]].
- The measurement methodology is grounded in [[measuring-agent-effectiveness-talk|Measuring Security-Agent Effectiveness]] (no-oracle problem, rubric, judge calibration) and [[rethinking-security-agent-evaluation-talk|Rethinking Security-Agent Evaluation]] (capability-centric, multi-stage evaluation); the grader pattern is detailed in [[llm-as-a-judge|LLM-as-a-Judge]].
- The buyer-side criteria for the same capability are [[evaluating-ai-soc-agents|Gartner's seven evaluation questions]]; the first public defender-agent scoreboard is [[defensebench|DefenseBench]].

## Notes

[^saxe]: The no-oracle argument, the label-noise simulation (flip rates from ~0.5% to ~3%, severe by ~1%) that defines the noise ceiling, and the ~100-sample judge-calibration figure are from Joshua Saxe's Unprompted talk, summarized at [[measuring-agent-effectiveness-talk|Measuring Security-Agent Effectiveness]] (source: <https://unpromptedcon.org/abstract-march2026/>). Practitioner-reported from a single talk; treat as directional.
[^asu-keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06). See [[vulnerability-research-agentic-age-keynote|the talk summary]].
