---
type: maturity-model-companion
title: "CMM Vocabulary and Notation"
created: 2026-09-18
updated: 2026-09-29
tags:
  - maturity-models
  - cmm
  - vocabulary
  - notation
status: developing
origin: produced
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
target: "[[agentic-ai-security-cmm-2026]]"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-soc-cmm]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[cybersecurity-cmms-exemplars]]"
  - "[[google-cloud-agentic-security-profile]]"
  - "[[azure-rag-chatbot-security-profile]]"
  - "[[cmm-calibration-stress-test-2026]]"
sources:
  - "https://insights.sei.cmu.edu/documents/853/2010_005_001_15287.pdf"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Whole-page read against current core, Handbook, Reference Architecture and SOC-model distinction; corrected deployment shape, target, L0 and unanswerable terms. No archived raw source opened; CMMI five-level source checked live."
---

# CMM Vocabulary and Notation

The [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] assesses nine domains, and the [[agentic-soc-cmm|Agentic SOC CMM]] assesses eight. Both number from D1, so a citation names its model. [CMMI's five maturity levels](https://insights.sei.cmu.edu/documents/853/2010_005_001_15287.pdf) inform the scale; this wiki defines its own criteria ([[cybersecurity-cmms-exemplars|Cybersecurity CMM Exemplars and Design Lessons]]).

## Assessment terms

| Term | Means |
|---|---|
| domain | One practice area, cited `D` and a number |
| **ladder** | **Retired for Agentic AI CMM domains.** The SOC model still names its separate autonomy ladder. |
| **rung** | **Retired for Agentic AI CMM domains.** A former synonym for level. |
| L0 | **Retired for the Agentic AI Security CMM.** If the L1 state cannot be established, report the domain unanswerable outside the L1–L5 scale. Other instruments can define L0. |
| L5+ | **Retired for the Agentic AI Security CMM.** Former research tier. The Agentic SOC CMM has its own level definitions; check that model before interpreting an older L5+ citation. |
| raw score | **Retired for the Agentic AI Security CMM.** The former level before dependency arithmetic. |
| effective score | **Retired for the Agentic AI Security CMM.** The former result after dependency caps. [[agentic-ai-security-cmm-dependency-rules\|The dependency page]] preserves the historical rule IDs. |
| cap | **Retired as score arithmetic.** A cross-domain prerequisite is checked against the criterion's actual evidence path. |
| **floor** | **Retired.** The lowest domain score was once the program headline. |
| L4-stable | **Retired.** The former label for L5 criteria met without an extra L5 gate. |
| program rating | **Retired for the Agentic AI Security CMM.** Report nine domain results, including applicability decisions and evidence gaps. |
| verdict | One of four per criterion: met, not met, not applicable, unanswerable. Only met establishes the criterion; a documented not-applicable criterion leaves the applicable population. |
| unanswerable | Available evidence cannot establish whether an applicable criterion is met. Record the missing fact and its owner; supplier opacity is one possible cause. |
| assurance class | Tested, inspected, or attested, recorded beside a verdict so the reader can see how the outcome was established. |

## Target terms

| Term | Means |
|---|---|
| deployment shape | The defined agent configuration, workflow, authority, hosting, data, tools, people, and suppliers assessed together. |
| org profile | The SOC model's equivalent axis: solo or small, mid, enterprise |
| right-sizing | Setting a target per shape or profile instead of per organization |
| target range | Historical two-level shorthand such as `L3 → L4`. The current Agentic AI Security CMM reports one risk-selected target per applicable domain. |
| **band** | **Retired for the Agentic AI Security CMM.** The SOC model uses it for a target range. |
| core criteria | The control set a level rests on |
| **spine** | **Retired.** A synonym for core criteria |
| trust boundary | A crossing between operators, authority, or data trust in [[agentic-ai-security-reference-architecture\|the Reference Architecture]]. The current RA names logical components and tests their crossings; older six-plane citations describe its retired layout. |

## The arrow

Its meaning depends on the table or diagram that uses it.

- **A target range**, in an older right-sizing table.
- **Evidenced against needed**, in [[google-cloud-agentic-security-profile|the Google Cloud profile]], whose column head reads `Evidenceable → needed`. [[azure-rag-chatbot-security-profile|The Azure profile]] heads the same column `Realistic target` and means the target range.
- **A verb**, in prose. Retired vault-wide, surviving in the fixed names `L4→L5` and `D2→D5`.

## Ladder-relative levels

An `L` token means nothing without its ladder. The SOC model couples two: eight maturity domains from L1, and a per-function autonomy ladder from L0 to L4. A band in [[agentic-soc-cmm-d1-telemetry-data-readiness|a SOC domain page]] is a maturity target; a band in [[agentic-soc-ra-incident-response|the incident-response surface]] is an autonomy target. A governing domain sits about one maturity level above the autonomy it supports.
