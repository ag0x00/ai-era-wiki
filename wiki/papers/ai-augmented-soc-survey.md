---
type: paper
title: "AI-Augmented SOC Survey"
created: 2026-06-02
updated: 2026-06-02
tags:
  - papers
  - agentic-soc
  - maturity-models
  - levels-of-autonomy
  - systematic-review
status: summarized
source_url: "https://www.mdpi.com/2624-800X/5/4/95"
fetched: 2026-06-02
origin: aggregated
address: c-000182
scope_axis:
  - ai-in-sec-defense
related:
  - "[[agentic-soc-autonomy-ladders]]"
  - "[[agentic-soc-state-of-the-field]]"
  - "[[soc-cmm]]"
  - "[[cybersecurity-cmms-exemplars]]"
sources:
  - "[[ai-augmented-soc-survey-mdpi-2025.pdf]]"
  - "https://www.mdpi.com/2624-800X/5/4/95"
---

# AI-Augmented SOC Survey

*AI-Augmented SOC: A Survey of LLMs and Agents for Security Automation* (Srinivas, Kirk, Zendejas, Espino, Boskovich, Bari, Dajani, Alzahrani; California State University, San Bernardino). *Journal of Cybersecurity and Privacy* 2025, 5, 95; published 5 November 2025; CC BY. Source: [mdpi.com/2624-800X/5/4/95](https://www.mdpi.com/2624-800X/5/4/95) (DOI 10.3390/jcp5040095). A PRISMA-guided systematic review (600 records → 105 papers included, 2022–2025; OSF-preregistered). It is the closest academic prior art for an agentic-SOC capability maturity model: it proposes a five-level autonomy ladder and maps it onto the incumbent [[soc-cmm|SOC-CMM]].

## Eight SOC functions

The survey organizes the field around eight SOC functions and catalogues the LLM and AI-agent techniques applied to each: log summarization, alert triage, threat intelligence, ticket handling, incident response, report generation, asset discovery and management, and vulnerability management. These are the same functional rows an agentic-SOC reference architecture must model.

## The five-level capability maturity model

The paper's headline contribution is a five-level autonomy ladder for AI-enabled SOCs (its Section 4, Figure 3):

| Level | Name | Human relationship |
|---|---|---|
| L0 | Manual Operations | Human-only; predefined rule sets, no AI/LLM |
| L1 | AI/LLM-Assisted Operations | Decision support (alert prioritization, initial triage); analysts retain full control and verification — human-in-the-loop |
| L2 | Semi-Autonomous Operations | AI with SOAR/LLMs automates routine tasks; explicit human approval for critical or ambiguous cases |
| L3 | Conditionally Autonomous Operations | AI independently runs complex analysis, attack-path reconstruction, and reporting; analysts review and approve critical actions — human-on-the-loop |
| L4 | Fully Autonomous Operations | AI manages the full incident lifecycle with minimal human involvement; humans shift to governance and strategic oversight |

The progression runs across the human-in-the-loop → human-on-the-loop → human-out-of-the-loop spectrum. The paper notes that fully autonomous (L4) operation remains largely theoretical, and its own classification of current implementations (Table 3) places nearly all of them at L1–L3.

> The level names matter for the wider prior-art picture. An earlier automated scan misattributed the "Automation Rules → AI Assistance → AI Collaboration → AI Delegation" vocabulary to this paper; those are the [[agentic-soc-autonomy-ladders|Darktrace/CSA model]]'s names. This survey's actual ladder — Manual → AI-Assisted → Semi-Autonomous → Conditionally Autonomous → Fully Autonomous — is instead near-identical to the independent [[agentic-soc-autonomy-ladders|Mohsin et al. SOC ladder]], which is strong convergence evidence for that shape.

## Explicit crosswalk to SOC-CMM

Section 4.1 positions the model as a domain-specific extension of SOC-CMM (the Business / People / Process / Technology / Services maturity model), rather than a replacement. It maps its autonomy levels onto SOC-CMM process-maturity levels: L0–L1 to SOC-CMM Maturity 1–2 (Initial/Defined), L2–L3 to Maturity 3–4 (Managed / Quantitatively Managed), and L4 to Maturity 5 (Optimizing), while adding AI-specific criteria SOC-CMM does not carry — model governance, data provenance, explainability, and autonomy-safety controls. The authors frame it as "an AI-focused overlay that quantifies automation maturity and trust calibration." This is a worked template for the standards-crosswalk layer an agentic-SOC maturity model needs.

## Status and limits

The model is proposed, not validated. Section 4.2 describes validation as future work: content validity through alignment with SOC-CMM, SIM3, and NIST CSF; construct validity in simulated environments (Wazuh, Security Onion, Elastic Stack) and operational settings; correlation of maturity scores with mean-time-to-detect, mean-time-to-respond, and false-positive rates; and inter-rater reliability checks. Treat the ladder as a conceptual scaffold with academic grounding but no empirical calibration. A model that ties autonomy advancement to measured outcomes can close that gap.

## Relevance to the build

- Direct prior art the planned agentic-SOC maturity model must crosswalk against, catalogued alongside the other ladders in [[agentic-soc-autonomy-ladders|Agentic SOC Autonomy Ladders]].
- Its near-identity with the Mohsin et al. ladder makes the manual → assisted → approval-gated → conditional → delegated shape the settled SOC-specific convention.
- Its SOC-CMM mapping and its acknowledged lack of validation mark where a new model adds value: an evaluation-in-the-loop discipline that ties autonomy advancement to a measured signal.
