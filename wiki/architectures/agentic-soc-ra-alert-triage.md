---
type: architecture
title: "Agentic SOC Alert Triage Surface"
address: c-000196
created: 2026-06-03
updated: 2026-06-03
tags:
  - architectures
  - agentic-soc
  - alert-triage
  - soc-automation
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
related:
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-cmm-d3-evaluation-ground-truth]]"
  - "[[agentic-soc-cmm-d4-identity-action-authority]]"
  - "[[agentic-soc-cmm-d1-telemetry-data-readiness]]"
  - "[[beyond-the-chatbot-talk]]"
  - "[[1-8m-prompts-30-alerts-talk]]"
  - "[[prompt-volume-to-alert-ratio]]"
  - "[[vulnops-l1-soc-extinction]]"
  - "[[google-agentic-soc]]"
  - "[[microsoft-security-copilot]]"
  - "[[llm-as-a-judge]]"
  - "[[cyber-poverty-line]]"
sources:
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-soc-cmm]]"
  - "[[1-8m-prompts-30-alerts-talk]]"
  - "[[beyond-the-chatbot-talk]]"
---

# Agentic SOC Alert Triage Surface

Per-function deep-dive for the **Alert triage** surface of the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]]. Triage is the function that assesses, validates, and prioritizes incoming alerts, separates true positives from the noise, and decides what reaches a human. It sits between detection and investigation in the SOC lifecycle, and it carries the highest event volume of any function. The agent surface is a set of triage / verdict agents that read an alert, gather context, and return a disposition with a rationale.

This is the function most visibly being absorbed by automation — the "L1-SOC absorption" or **monitor mode** shift, where the first-pass triage that defined the Tier-1 analyst role moves to agents and the human steps up to supervision ([[vulnops-l1-soc-extinction|the VulnOps / L1-SOC-extinction account]]). Delegation pays off earliest here because volume is the constraint, but auto-dismissal is consequential: a wrongly closed alert is a missed true positive that no later function sees.

Per the [[agentic-soc-reference-architecture#per-function-agent-surfaces|RA's per-function table]], triage runs on the **Orchestration** and **Observability & Evaluation** planes, and its autonomy is gated by **D1 (Telemetry & Data Readiness)**, **D4 (Agent Identity & Action-Authority)**, and **D3 (Evaluation & Ground-Truth)**. The [[agentic-soc-cmm|Agentic SOC CMM]] scores whether the SOC has earned each rung; this page applies its [[agentic-soc-cmm#axis-1--the-autonomy-ladder-per-function|autonomy ladder]] and [[agentic-soc-cmm#the-gating-rule|gating rule]] to triage specifically.

## The agent surface

On the supervisor-worker topology, triage is a worker pattern the orchestrator invokes per alert: an alert lands, the orchestrator routes it to a triage agent, and the agent works it to a verdict. The agent gathers context — enrichment from threat intelligence, asset and identity lookups, related-alert correlation, prior dispositions — applies a judgment, and returns one of a small set of outcomes: escalate to investigation, suppress as a false positive, or hold for human review. In Salesforce's production [[beyond-the-chatbot-talk|Polyphonic SOC]], one agent maps to one workload, and the orchestrator binds it through an **agent card** that declares its persona, allowed tool capabilities, and a **max-iteration** ceiling; reaching that ceiling exits the session gracefully rather than letting an open-ended chain run away.

The mechanism mix is hybrid. Deterministic prioritization (alert severity, asset criticality, suppression lists, deduplication) has run in SIEM/SOAR for years and occupies the lower rungs on its own; it is exact and cheap. The AI layer adds the judgment that a static rule cannot encode: reading an alert in context, reasoning about whether the observed behavior fits the asset's baseline, and explaining the verdict. The two compose, with deterministic filters cutting the volume and the AI agent reasoning over what survives.

The tools and data the agent calls sit on the Data & Knowledge plane: the alert queue (SIEM/XDR, SOAR cases), enrichment sources (threat-intel graph, asset inventory, identity directory), and the prior-disposition history that tells the agent how comparable alerts were handled. Behavioral-baseline triage is a distinct data dependency: where the alert is itself a deviation score against a learned norm — as in Salesforce's three-level user/agent/org ensemble that reduces roughly 1.8 million daily prompts to fewer than 30 actionable alerts ([[1-8m-prompts-30-alerts-talk|"1.8M Prompts, 30 Alerts"]]) — the agent triages an anomaly, not a signature match, and the [[prompt-volume-to-alert-ratio|prompt-volume-to-alert ratio]] is the signal-to-noise measure of how well that upstream layer has already thinned the queue.

The human-authority boundary for triage sits at the **disposition**, and specifically at the irreversible one. Escalation is low-stakes — the worst case is a human's time spent on a false positive. Auto-suppression is the consequential action, because a closed alert is, in practice, an unreviewed one. The boundary is therefore drawn around auto-close: which alert classes an agent may dismiss on its own authority, at what confidence, with what audit trail, and which it must hand up. That authority is what D4 scopes and what the autonomy ladder raises one rung at a time.

## Autonomy progression

The ladder below walks the CMM's L0–L4 for triage. The load-bearing column is the gating one: a function may run at autonomy L_k only if the domains that govern that level are mature enough to support it, and the weakest governing domain sets the ceiling ([[agentic-soc-cmm#the-gating-rule|the gating rule]]). Operating above that earned ceiling — auto-closing alerts the SOC cannot measure or bound — is the model's defined failure mode.

| Level | What it looks like for triage | Gating domains |
|---|---|---|
| **L0 — Manual** | An analyst reads each alert in the queue and dispositions it by hand. Prioritization is human judgment or a static severity field. | None |
| **L1 — Assisted** | The agent enriches and summarizes the alert and proposes a disposition with a rationale; the analyst reads, verifies, and acts. Deterministic prioritization (severity, asset criticality, dedup) also lives here. Verdict authority stays with the human. | None to act; D1 to make the enrichment trustworthy |
| **L2 — Semi-autonomous** | Routine, low-stakes dispositions execute under standing approval rules; every consequential close or escalation that crosses a defined line is presented for explicit approval. The agent runs the workflow; the human signs off on the verdicts that matter. | **D1** (the data behind the verdict is real and reasonably complete) · **D4** (the agent holds scoped, revocable authority to propose a disposition, traceable to a human owner) |
| **L3 — Conditional** | The agent dispositions in-bounds alert classes autonomously — closing high-confidence false positives, escalating confirmed positives — and escalates anything outside its bounds. Humans monitor on the loop and intervene. | + **D3** (the SOC can measure whether the agent's verdicts are good against ground truth) · **D5** (the agent's reasoning is observable and overridable) |
| **L4 — Delegated** | The agent owns the triage queue end to end; humans govern outcomes — alert-class scope, the auto-close confidence bar, false-negative budget — rather than individual verdicts. Asymptotic: the authority boundary remains. | + **D7** (the triage agents are resilient and supply-chain-sound) · **D8** (people and governance own the autonomy-raising decision) |

D3 is the rung that makes triage delegation legitimate, and it is the one most often skipped. Deterministic prioritization is exhaustively testable, so the deterministic rungs lean on change-control rather than evaluation. AI triage is non-deterministic: the same alert can draw a different verdict across runs, so its decision quality is a *measured* property, not a verified one ([[agentic-soc-cmm-d3-evaluation-ground-truth|D3 Evaluation & Ground-Truth]]). The close decision cannot legitimately be delegated (L3) while the SOC cannot answer whether the triage agent's closes are right, however strong the telemetry or the identity controls.

**A triage agent should auto-close no alert class it cannot both measure and bound.** D3 supplies the measurement — a rubric and ground-truth store that say whether the verdicts are good; D4 supplies the bound — scoped authority that limits which classes the agent may close and forces the rest up to a human. Running auto-close ahead of either is the model's failure mode named for this function: a queue that looks clean because true positives are being dismissed unseen.

## Control landscape (dated)

Real tools for triage span a mature deterministic core and a newer agentic layer. Vendors are dated, swappable examples, not endorsements; GA versus preview is marked and should be re-verified at use.

| Capability | What ships today | Status (mid-2026) |
|---|---|---|
| Deterministic prioritization | SIEM/XDR triage queues and SOAR playbooks: severity scoring, asset-criticality weighting, suppression and deduplication, ticket routing. The incumbent L1–L2 layer. | GA and ubiquitous; the baseline triage automation every band already runs |
| Behavioral-baseline triage | UEBA and anomaly-scoring engines that emit deviation scores rather than signature hits; multi-level (user / entity / org) baselines such as Salesforce's three-level ensemble ([[1-8m-prompts-30-alerts-talk\|"1.8M Prompts, 30 Alerts"]]) | GA as a category (UEBA); production multi-level agent-aware baselines are practitioner-reported at platform scale, not a packaged product |
| Verdict agent (vendor fleet) | Microsoft's [[microsoft-security-copilot\|Security Copilot]] **Alert Triage Agent** (first-pass disposition on SIEM/XDR alerts); Google SecOps **Alert Triage and Investigation Agent** (evidence gathering, deobfuscation, true/false-positive verdict with an audit log); CrowdStrike Falcon agentic triage | Microsoft agents named on the pre-RSAC-2026 roadmap; Google's triage agent is in **public preview** ([[google-agentic-soc\|Google Agentic SOC]]); maturity and independent benchmarks are thin — vendor-reported figures only |
| Verdict explanation | LLM-as-alert-explainer: a secondary agent turns a feature vector or detection into a plain-English rationale an investigator can act on without a data-science background ([[1-8m-prompts-30-alerts-talk\|Salesforce]]) | Pattern-level and GA in practice; an interpretability design choice (a simple distance-based detector kept explainable) rather than a product |
| Verdict evaluation | Eval harnesses scoring triage trajectories against a labelled true/false-positive store; an [[llm-as-a-judge\|LLM-as-a-judge]] grading dispositions at scale, calibrated against a human sample | Harnesses GA outside security; security-specific triage rubrics are bespoke per SOC, and judge calibration is practitioner-reported (see [[agentic-soc-cmm-d3-evaluation-ground-truth\|D3]]) |
| Authority gating | SOAR / response-platform approval workflows realizing auto / propose / approve / block over close and escalate actions, scoped per agent identity | GA in mainstream SOAR; per-action authority tiering over triage agents is configuration, not a shipped agent-governance feature (see [[agentic-soc-cmm-d4-identity-action-authority\|D4]]) |

The packaged vendor triage agents are the newest and least independently measured row. They instantiate the worker pattern this page describes, but each is single-vendor, several are in preview, and no public benchmark scores triage-agent verdict quality across products — the comparator the field still lacks.

## Failure modes and what to watch

- **False-positive collapse (the silent miss).** An over-confident agent auto-closes a class that contains real positives; the queue looks clean while true positives are dismissed unseen. This is the consequential triage failure, because no downstream function reviews a closed alert. Bounded by D3 — a ground-truth store and a rubric that track the false-negative rate of the auto-close classes, not only the false-positive rate — and by D4, which caps which classes may auto-close at all.
- **Operating above the earned ceiling.** Auto-close switched on before D3 can measure verdict quality, or before D4 scopes the agent's close authority. The gating rule names this directly: the weakest of D1/D4 (for L2) and D3/D5 (for L3) caps the rung, and running past it is reckless autonomy.
- **Alert flooding / queue overwhelm.** A noisy detection layer or a baseline warm-up period buries the agent in low-signal alerts, eroding precision. Bounded upstream by detection-in-pipeline filtering and behavioral baselines (the [[prompt-volume-to-alert-ratio|prompt-volume-to-alert ratio]] is the health metric), and by a documented new-agent warm-up window during which elevated noise is expected rather than surfaced raw.
- **Verdict drift.** Offline evaluation passes, but in-production disposition quality degrades as the alert mix shifts. Bounded by D5 online drift monitoring paired with D3's offline golden-dataset runs.
- **Automation bias / trust erosion.** Analysts either rubber-stamp agent verdicts (bias) or stop trusting them after a bad close (erosion). Bounded by D5 observability — a glass-box rationale and audit log the analyst can inspect and override — and by D3 calibration that keeps the published quality honest.
- **Unattributable disposition.** A close cannot be traced to the agent and its human owner, breaking accountability. Bounded by D4 per-agent identity and the non-repudiation log of who approved which consequential close.

> [!gap] The silent-miss rate is the hardest number to measure
> Auto-close is gated on D3's ability to bound the false-negative rate of each class, yet that rate is the one a SOC cannot easily observe: a closed alert is, by construction, the one nobody looks at again, so a true positive the agent dismisses leaves no downstream signal. The control rests on a ground-truth store and periodic re-audit of closed classes rather than on production feedback, and there is no established sampling rate or budget for that re-audit. Until the silent-miss rate is measurable in production rather than only on a golden set, the most consequential triage failure is bounded by an estimate.

## Right-sizing by org profile

The realistic triage autonomy target rises with the volume the SOC faces and the evaluation and authority maturity it can sustain. A small team running triage at low autonomy on borrowed capability is right-sized, not immature.

| Band | Realistic triage autonomy | Why |
|---|---|---|
| Solo / small | L1, often via the provider | Near or below the [[cyber-poverty-line\|cyber poverty line]]. A team borrowing through an MSSP/MDR inherits the *provider's* triage maturity and evaluation; for its own alerts, agent-assisted enrichment and suggested dispositions (L1) cut the load without owing the calibrated-judge machinery that auto-close (L3) requires. AI lowers the barrier here: agentic enrichment and verdict suggestion are reachable without the playbook engineering that gated traditional SOAR, so even the smallest team gets first-pass triage support it could never have scripted. |
| Mid | L2 → L3 on selected classes | An in-house SOC delegating its highest-volume function within bounds must reach D3 L3 on triage (a rubric, a regression suite, a curated true/false-positive store, online drift monitoring) and D4 L3 (auto/propose/approve/block over close and escalate). The path is L2 first — routine dispositions under approval — then in-bounds auto-close on the well-measured classes only. |
| Enterprise | L3 → selective L4 | A full agent fleet handling enterprise alert volume needs calibrated, governed verdict evaluation and measured close-authority across alert classes, with selective L4 (delegated queue, governed by outcome budgets) where volume and ground-truth quality justify it. The vendor fleets (Microsoft, Google, CrowdStrike) target this band. |

Triage is volume-bound, and volume is what a small team cannot staff. Agentic enrichment and suggested verdicts put first-pass triage within reach below the line where a scripted SOAR pipeline never was, which is where AI lowers the floor most. The discipline is to keep the borrowed or in-house agent at L1 — assist, do not auto-close — until the SOC, or its provider, can measure and bound the verdicts.

## Relations

- Hangs off the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] as the **Alert triage** agent surface; runs on its **Orchestration** plane (the supervisor-worker routing and max-iteration governance) and its **Observability & Evaluation** plane (verdict evaluation and intent attribution).
- Applies the [[agentic-soc-cmm|Agentic SOC CMM]]'s autonomy ladder and gating rule to triage. Gating domains, with their deep dives: [[agentic-soc-cmm-d1-telemetry-data-readiness|D1 Telemetry & Data Readiness]] and [[agentic-soc-cmm-d4-identity-action-authority|D4 Agent Identity & Action-Authority]] (the L2 gates), and [[agentic-soc-cmm-d3-evaluation-ground-truth|D3 Evaluation & Ground-Truth]] (with D5, the L3 gate — the rung that makes auto-close legitimate).
- Practitioner evidence: Salesforce's behavioral-baseline triage at scale ([[1-8m-prompts-30-alerts-talk|"1.8M Prompts, 30 Alerts"]] and the [[prompt-volume-to-alert-ratio|prompt-volume-to-alert ratio]]) and its supervisor-worker SOC ([[beyond-the-chatbot-talk|Beyond the Chatbot]]); the L1-SOC-absorption / monitor-mode shift ([[vulnops-l1-soc-extinction|From Threat Intel to VulnOps]]); vendor triage agents in [[google-agentic-soc|Google Agentic SOC]] and [[microsoft-security-copilot|Microsoft Security Copilot]].
