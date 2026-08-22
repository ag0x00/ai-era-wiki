---
type: comparison
title: "Agentic SOC Autonomy Ladders"
created: 2026-06-02
updated: 2026-08-21
tags:
  - comparisons
  - agentic-soc
  - maturity-models
  - levels-of-autonomy
  - prior-art
status: developing
origin: produced
address: c-000180
scope_axis:
  - ai-in-sec-defense
related:
  - "[[agentic-soc-state-of-the-field]]"
  - "[[least-agency-principle]]"
  - "[[emerging-cybersecurity-practices-for-agentic-ai-applications]]"
  - "[[owasp-ai-exchange]]"
  - "[[cybersecurity-cmms-exemplars]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[nist-ir-8596-cyber-ai-profile]]"
sources:
  - "https://arxiv.org/abs/2505.23397"
  - "https://arxiv.org/abs/2506.23592"
  - "https://cloudsecurityalliance.org/blog/2026/01/08/introducing-the-ai-maturity-model-for-cybersecurity"
  - "https://cloudsecurityalliance.org/blog/2026/01/28/levels-of-autonomy"
  - "https://www.mdpi.com/2624-800X/5/4/95"
---

# Agentic SOC Autonomy Ladders

External prior art for the planned [[agentic-soc-state-of-the-field|Agentic SOC]] reference architecture and capability maturity model. Five published frameworks independently propose a graded **levels-of-autonomy ladder** for AI in security operations. Their convergence fixes the spine an agentic-SOC maturity model should adopt; their differences mark the design choices it must make explicitly. This page is the defender-side counterpart to [[cybersecurity-cmms-exemplars|Cybersecurity CMM Exemplars]], which catalogues general-purpose security CMMs.

The ladders below grade a **security-operations program's** standing autonomy. Two other four-value instruments govern individual agent actions, and neither is a ladder of this kind. The four action-risk tiers — auto, notify, confirm, block — sort a single **action** by the harm it can do, a scheme [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]] §3.2 supplies and [[least-agency-principle|Least Agency Principle]] carries in full. The [[owasp-ai-exchange|OWASP AI Exchange]] oversight requirements assign each tier **how a human is involved**, and compose with the tiers. A ladder level here is a property of the deployment; a tier is a property of an individual action, fixed at tool-annotation time, and an oversight requirement attaches to that tier.

## The ladders compared

| Framework | Origin | Levels (bottom → top) | Scope | Second axis |
|---|---|---|---|---|
| Trusted-Autonomy SOC | Mohsin et al., arXiv 2505.23397 (May 2025) | L0 Manual · L1 AI-Assisted · L2 Semi-Autonomous (approval) · L3 Conditional (HITL) · L4 Fully Autonomous | SOC-specific, defensive | Trust calibration + HITL/HOTL/HOoTL |
| Cybersecurity AI | Mayoral-Vilches, arXiv 2506.23592 (Jun 2025) | L0 No Tools · L1 Manual Tools · L2 LLM-Assisted · L3 Semi-Automated · L4 Cybersecurity AIs · L5 Autonomous | Offense / pentest | Plan · Scan · Exploit · Mitigate |
| AI Maturity Model for Cybersecurity | Iddya / Darktrace, via CSA (Jan 2026) | L0 Manual Operations · L1 Automation Rules · L2 AI Assistance · L3 AI Collaboration · L4 AI Delegation | Cybersecurity functions | — |
| Levels of Autonomy | Reavis / CSA (Jan 2026) | L0 No Autonomy · L1 Assisted · L2 Supervised · L3 Conditional · L4 High Autonomy · L5 Full Autonomy | Generic agentic AI | Oversight shift |
| [[ai-augmented-soc-survey\|AI-Augmented SOC survey]] | Srinivas et al., MDPI JCP (Nov 2025) | L0 Manual · L1 AI-Assisted · L2 Semi-Autonomous · L3 Conditionally Autonomous · L4 Fully Autonomous | SOC-specific, eight functions | Explicit SOC-CMM crosswalk |

## Contribution of each ladder

**Trusted-Autonomy SOC (Mohsin et al.)** is the closest direct analog: a SOC-specific, defensive five-level ladder that formally couples autonomy with **trust** and **human-in-the-loop** mode. It models autonomy as a function of task complexity, risk, and trust, with human involvement as the complement, and maps levels onto Tier-1/2/3 analyst roles and onto the HITL → human-on-the-loop → human-out-of-the-loop progression across core SOC functions (monitoring, protection, detection, triage, response). It is the only source that treats trust as a graded, measurable input rather than an implicit assumption.

**Cybersecurity AI (Mayoral-Vilches)** is offense-leaning and adapts the SAE J3016 driving-automation analogy to a six-level (L0–L5) ladder, crossed against four capability columns: **Plan, Scan, Exploit, Mitigate**. Its central argument is that *automation is not autonomy*. Today's "autonomous" pentest systems sit at L3–L4 (they execute complete sequences but need human review for edge cases and strategy), and a true L5 does not exist. The two-axis structure (level × capability) is the most useful idea to carry over: a single scalar level hides which functions an agent can actually run unsupervised.

**AI Maturity Model for Cybersecurity (Darktrace, via CSA)** is a vendor model grounded in roughly ten thousand deployments, with a clean L0–L4 progression (Manual Operations → Automation Rules → AI Assistance → AI Collaboration → AI Delegation) across risk management, threat detection, alert triage, and incident response. Its level vocabulary is the most operationally concrete.

**Levels of Autonomy (CSA, Reavis)** is a general agentic-AI ladder (L0 No Autonomy → L5 Full Autonomy) keyed to how human oversight shifts — individual-action approval, then plan approval, then boundary enforcement, then exception monitoring, then strategic-only. It is not SOC-specific but supplies the governance framing for the autonomy axis.

**[[ai-augmented-soc-survey|AI-Augmented SOC survey]] (Srinivas et al.)** is the closest academic prior art: a PRISMA systematic review whose five-level ladder (Manual → AI-Assisted → Semi-Autonomous → Conditionally Autonomous → Fully Autonomous) is near-identical to the independent Mohsin et al. ladder, across the same human-in/on/out-of-the-loop spectrum. Two independent peer-reviewed papers converging on the same SOC-specific shape is the strongest single convergence signal here. It is also the only source that publishes an explicit crosswalk to [[soc-cmm|SOC-CMM]] (its levels mapped onto SOC-CMM maturity 1–5), and it classifies today's implementations at L1–L3 with L4 still theoretical.

## The convergent shape

All five ladders trace the same progression: **manual → assisted / decision-support → approval-gated execution → bounded or conditional autonomy (human-in-the-loop) → delegated or autonomous operation.** The two SOC-specific defensive ladders (Mohsin et al. and the MDPI survey) arrive at this independently and almost name-for-name. Two properties recur and should be treated as settled:

> The top tier is asymptotic, not terminal. No framework reports a shipped, fully autonomous SOC; the highest level is either explicitly aspirational (Mayoral's L5, "no such system exists") or bounded by a permanent human role (Darktrace's L4 "Delegation," Mohsin's L4 "minimal oversight"). [[gartner|Gartner]]'s position — that there will never be a fully autonomous SOC — is the analyst counterpart to this structural observation. A maturity model should not define a terminal level that asserts unsupervised autonomy.

The second recurring property is that **autonomy is per-function, not global**: Mayoral's capability columns and Mohsin's per-function mapping both reject a single scalar. An agent may be trusted to triage at machine speed while remaining approval-gated for containment.

## Design choices this forces

- **Staged ladder vs. continuous axes.** These ladders are *staged* (discrete levels). The incumbent SOC maturity standard, SOC-CMM, is *continuous*: a maturity axis separated from a capability axis, assessed per domain. An agentic-SOC CMM must decide whether to adopt a staged autonomy ladder, the continuous SOC-CMM style, or a hybrid (autonomy ladder on one axis, coverage/capability on another). The MDPI survey already demonstrates one such hybrid: it maps its staged autonomy levels onto SOC-CMM's continuous maturity scale (L0–L1 to SOC-CMM maturity 1–2, L2–L3 to 3–4, L4 to 5), so the mapping is a workable template rather than an open problem. See [[cybersecurity-cmms-exemplars|Cybersecurity CMM Exemplars]] for the continuous-vs-staged design lesson.
- **Where trust and evaluation enter.** Only Mohsin grades trust as a measurable input; the MDPI survey names trust calibration but leaves validation to future work. An evaluation-in-the-loop pillar that measures agent decision quality against ground truth before granting a higher autonomy level is under-modeled across all five and is a candidate differentiator.
- **Identity and action-authority as a graded dimension.** None of the five ladders grades per-agent identity or scoped action-authority, though vendors already implement it operationally (see [[microsoft-security-copilot|Microsoft Security Copilot]] agent identities and permissions). Fusing the autonomy ladder with a graded identity/authority dimension is open ground the planned model can occupy.

## Open items

The MDPI survey is read from the primary (PDF archived 2026-06-02); its levels are verified above, and the earlier misattribution of the Darktrace/CSA vocabulary to it is corrected. One primary remains unread:

> [!gap] Gartner primary pending
> The Gartner "Predict 2025: There Will Never Be an Autonomous SOC" document (doc 6027635) is subscriber-gated and could not be retrieved. Its position is corroborated through multiple secondary summaries, but the verbatim human-authority clauses should be confirmed against the primary before being quoted. Queued in #116.

## Relations

- Grounds [[agentic-soc-state-of-the-field|Agentic SOC State of the Field]] and the maturity model planned there.
- Defender-side counterpart to [[cybersecurity-cmms-exemplars|Cybersecurity CMM Exemplars and Design Lessons]].
- Normative scaffolding for the "Defend" surface is catalogued in [[nist-ir-8596-cyber-ai-profile|NIST IR 8596 Cyber AI Profile]].
