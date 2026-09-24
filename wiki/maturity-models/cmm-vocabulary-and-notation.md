---
type: maturity-model-companion
title: "CMM Vocabulary and Notation"
created: 2026-09-18
updated: 2026-09-24
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
  - "https://www.sei.cmu.edu/our-work/cybermaturity/"
verified: 2026-09-19
verified_against: []
verified_findings: 0
verified_note: "Internal-consistency read of the L4→L5 gate split (issue #173) across the core page, protocol, dependency rules, D9 deep dive, crosswalk, vocabulary and the gaps register; no .raw document opened. Fixed a truncated floor cell; L4-stable and program rating checked against the core page and protocol, and no live use of floor as a graded criterion survives."
---

# CMM Vocabulary and Notation

Both maturity models score in terms each defines where it uses them. This page is the lookup. The two are [[agentic-ai-security-cmm-2026|the Agentic AI Security CMM]], nine domains, and [[agentic-soc-cmm|the Agentic SOC CMM]], eight; both number from D1, so a citation names its model first. The five-level shape is CMMI's, from [Carnegie Mellon's Software Engineering Institute](https://www.sei.cmu.edu/our-work/cybermaturity/); the criteria are this wiki's own ([[cybersecurity-cmms-exemplars|Cybersecurity CMM Exemplars and Design Lessons]]).

## What an assessor recorded

| Term | Means |
|---|---|
| domain | One practice area, cited `D` and a number |
| **ladder** | **Retired.** A synonym for a domain's levels, or its level definitions |
| **rung** | **Retired.** A synonym for level |
| L0 | No evidence the L1 baseline exists. Level definitions start at L1, scores at 0 |
| L5+ | The tier above L5: research-stage primitives in production, named contribution to a standard. Every domain in both models carries one |
| raw score | The level a domain evidences |
| effective score | The lesser of a raw score and the raw scores it depends on ([[agentic-ai-security-cmm-dependency-rules\|the dependency rules]]) |
| cap | A dependency rule that fired. The report names the domain that set it |
| **floor** | **Retired.** The lowest domain score, the headline rating until 2026-05-04. No graded criterion uses it; a report dated before 2026-09-19 may still name it |
| L4-stable | A domain that evidences every L5 criterion while the L5 gate's evidence is absent |
| program rating | One level claimed for the whole program. L5 requires L5 in all nine domains, L5+ that rating plus its own tier criteria. Never one domain's score |
| verdict | One of four per criterion: met, not met, not applicable, unanswerable. A score counts met alone |
| unanswerable | The customer can neither test nor read it, and the vendor states nothing that names the control. A finding against the vendor, never met |
| assurance class | tested, inspected or attested, recorded beside a verdict and kept out of the score |

## What an organization should aim at

| Term | Means |
|---|---|
| deployment shape | An archetype of AI application. The core table grades nine. Also called an agent archetype |
| org profile | The SOC model's equivalent axis: solo or small, mid, enterprise |
| right-sizing | Setting a target per shape or profile instead of per organization |
| target range | A two-level target, `L3 → L4`: the level peers reach, then the level exposure justifies. Never a score |
| **band** | **Retired.** A synonym for target range |
| core criteria | The control set a level rests on |
| **spine** | **Retired.** A synonym for core criteria |
| plane | One of the six control layers of [[agentic-ai-security-reference-architecture\|the Reference Architecture]]: Identity, Control, Runtime, Egress, Data, Observability, pairing with D2 through D7 |

## The arrow

It carries three relations, and one page says which it means.

- **A target range**, in a right-sizing table. Its commonest use.
- **Evidenced against needed**, in [[google-cloud-agentic-security-profile|the Google Cloud profile]], whose column head reads `Evidenceable → needed`. [[azure-rag-chatbot-security-profile|The Azure profile]] heads the same column `Realistic target` and means the target range.
- **A verb**, in prose. Retired vault-wide, surviving in the fixed names `L4→L5` and `D2→D5`.

> [!contradiction] The arrow's relation is undeclared
> A right-sizing cell and a Google Cloud profile cell carry the same glyph for different relations, and neither `wiki/meta/conventions.md` nor [[agentic-ai-security-cmm-measurement-protocol|the measurement protocol]] settles which governs. Tracked as [#255](https://github.com/ag0x00/ai-era/issues/255).

## Ladder-relative levels

An `L` token means nothing without its ladder. The SOC model couples two: eight maturity domains from L1, and a per-function autonomy ladder from L0 to L4. A band in [[agentic-soc-cmm-d1-telemetry-data-readiness|a SOC domain page]] is a maturity target; a band in [[agentic-soc-ra-incident-response|the incident-response surface]] is an autonomy target. A governing domain sits about one maturity level above the autonomy it supports.
