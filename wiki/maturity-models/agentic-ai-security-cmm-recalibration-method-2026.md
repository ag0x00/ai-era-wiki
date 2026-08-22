---
type: maturity-model
title: "CMM: Recalibration Method (Cadence and Cost)"
address: c-000140
created: 2026-05-23
updated: 2026-08-21
tags:
  - maturity-models
  - cmm
  - method
  - recalibration
  - sec-of-ai
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[cmm-calibration-stress-test-2026]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]]"
  - "[[aiuc-1]]"
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
---

# Agentic AI Security CMM — Recalibration Method (Cadence, Capability-Decoupling, Cost)

This note governs the D1–D9 recalibration of the [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]. It exists because a representative-customer stress test ([[agentic-cmm-regulated-fi-stress-test|regulated-FI adoption stress test]]) found the CMM well-structured but mis-calibrated in four ways that recur across every domain: levels anchor to named products rather than capabilities; the bar moves whenever a product reaches general availability, which regulated buyers cannot track; cost is under-represented; and right-sizing guidance sits buried under the L5/L5+ tiers. Each domain's deep research applies the rules below, so the nine rewrites stay consistent.

> [!gap] Single-source grounding
> This page is wiki-internal method, not an externally defined standard. It synthesizes the recalibration approach from one representative-customer source — the [[agentic-cmm-regulated-fi-stress-test|regulated-FI adoption stress test]] — plus internal D1/D6 research. The per-domain D1–D9 rewrites test and firm it against additional deployment shapes.

## On this page

- [Basis for recalibration](#basis-for-recalibration)
- [The four recalibration rules](#the-four-recalibration-rules)
- [Per-domain deep-dive page template](#per-domain-deep-dive-page-template)
- [AIUC-1 handling (D1)](#aiuc-1-handling-d1)
- [Sequence](#sequence)
- [Reach of the rules, as applied](#reach-of-the-rules-as-applied)

## Basis for recalibration

The customer is treated as a representative buyer persona rather than a one-off. **The persona** is that buyer, and the D1–D9 deep dives use the term for it throughout: the mid-size credit union of the [[agentic-cmm-regulated-fi-stress-test|regulated-FI adoption stress test]], an all-Microsoft E5 shop with 1–2 FTE of AI-security capacity, one production deployment — a member-facing RAG support bot over a knowledge base and account FAQs — NCUA / FFIEC / GLBA examination exposure, and a procurement cycle of 12–18 months. Where a deep dive scores "the persona," it scores that deployment. The recurring findings:

- **L5 moves on product cadence, not capability.** Several L5 criteria point at products that reached GA within weeks of the CMM revision (Agent 365, Okta for AI Agents). A buyer with a 12–18-month procurement cycle (vendor risk assessment, SOC 2 review, board sign-off) cannot adopt a just-GA'd product, so L5 is unreachable on cadence rather than on capability. Penalizing prudent third-party-risk discipline is a calibration error, not a maturity signal.
- **Capability and product are conflated.** When a level says "Miggo DeepTracing or equivalent," the level definition changes every time the market changes. The capability ("real-time AI-BOM reconciliation") is stable; the product list is not.
- **Cost is under-told.** For an incumbent that already owns its platform licenses, the dominant costs are data-governance labor, log-ingestion run-rate, and headcount, not tool purchase. The CMM measures control coverage and is largely silent on what each level costs to operate.
- **Over-scoping for common shapes.** Some L4/L5 requirements (a four-tool quarterly red team, a full AI-BOM for a model *consumer*, a mesh gateway sidecar for a single bot) are sound for high-exposure deployments and controls-for-controls'-sake for a low-risk RAG chatbot.

## The four recalibration rules

Applied to every domain rewrite.

### 1. Capability-decoupled level criteria

Each level criterion states a **capability**, not a product. Capabilities are durable; products are dated. The mapping from capability to current product lives in a separate, dated tooling map (the existing [[agentic-ai-security-cmm-2026|CMM]] §Tooling map and the RA §Recommended stacks), refreshed on its own cadence. A level definition should read "per-task scoped authorization with cryptographic binding," not "Tenuo Warrants." The tooling map then says which products implement that capability today and at what maturity.

### 2. Production-maturity (cadence) qualifier

A control counts toward a level when it is **operating in the org's production environment**, regardless of how recently the underlying product reached GA. To prevent the bar from tracking bleeding-edge releases, level criteria avoid naming a capability whose *only* implementations are less than roughly two quarters past GA; such capabilities belong in L5+ (leading edge) until a production-hardened implementation path exists. This makes the ladder reachable for buyers whose adoption cadence is externally constrained, without lowering the security bar: the capability requirement is unchanged, only its dependence on just-shipped tooling is removed. A conservative-adoption reading is explicit: "implemented via a product in your approved-vendor pipeline with a documented production date" satisfies the criterion.

### 3. Cost dimension per level

Each domain records, per level, the dominant cost drivers in three buckets: **licensing** (often near-zero for incumbents), **operational labor** (FTE and recurring effort, the usual true bottleneck), and **run-rate** (consumption costs such as log ingestion, which scales with agent count at roughly 10–20× human log volume). The buckets carry a defensible signal of where the spend lands rather than a price, so a buyer is not surprised by the data-governance project or the SIEM bill that the control coverage implies.

### 4. Right-sizing surfaced, not buried

Each domain states the realistic target per deployment shape up front (a no-tool RAG chatbot is not held to multi-agent-mesh criteria), and the [[lethal-trifecta|lethal-trifecta]] test is named as the primary instrument for *removing* capability to lower the required level rather than buying controls to meet it. A sound, contained design that legitimately scores lower on a downstream domain is recorded as an intentional trade-off via the existing [[agentic-ai-security-cmm-dependency-rules|effective-score]] strategic-rationale field, not as a deficiency.

## Per-domain deep-dive page template

Each domain (D1–D9) gets a companion deep-dive page, and the CMM domain section is recalibrated in place to a tightened summary that links to it. Every deep-dive page follows this structure:

1. **Control landscape (dated).** What ships today across Standards / OSS / COTS, with GA-vs-preview status and the platform-native option for the major stacks (Microsoft / AWS / GCP), so single-stack buyers are not taxed with an integration exercise.
2. **Capability-decoupled levels.** The L1–L5 (+L5+) criteria for this domain, stated as capabilities per rule 1, with the production-maturity qualifier per rule 2.
3. **Right-sizing by deployment shape.** Realistic targets per shape; where the trifecta test lowers the bar.
4. **Cost model.** Licensing / labor / run-rate per level, per rule 3.
5. **Customer critiques folded in.** The specific stress-test objections that touch this domain, addressed or acknowledged.
6. **Open questions.** What the deep research could not resolve.

## AIUC-1 handling (D1)

The [[aiuc-1|AIUC-1]] certification is currently a hard L5 criterion in D1 (Governance). The D1 deep research includes a critical evaluation of whether mandating one certification is defensible, given that few organizations hold the accreditation and a single-certifier mandate raises a concentration concern. The expected outcome is to **demote AIUC-1 from a mandate to one option among several** (ISO/IEC 42001, AIUC-1, or a documented internal-equivalent governance attestation), with the capability (independent third-party governance assurance) stated per rule 1. The critical evaluation lands as its own page and drives the D1-L5 amendment.

## Sequence

D1–D9 recalibration runs first (per the program decision), in waves grouped by leverage: the data/RAG and governance domains lead (highest customer-flagged risk and the AIUC-1 dependency), then the platform-enforcement core (identity, control, runtime, egress), then the right-sizing-heavy domains (observability, supply chain, operations). The Azure-native RAG profile and the FFIEC/GLBA and Canadian-finance crosswalks are built afterward, against the recalibrated model.

## Reach of the rules, as applied

[[agentic-ai-security-cmm-d6-data-rag|D6 Data, Memory and RAG]] shows how far these rules reach on a domain the stress test flagged. Applying them moved D6's organizing risk from corpus poisoning to answer-time oversharing, made entitlement enforcement the load-bearing L3 capability, and put data-governance labor on the cost line in place of tool purchase. A recalibration can therefore change what a domain measures as well as how its criteria are worded.

[[agentic-ai-security-cmm-d9-operations|D9 Operations and Human Factors]] marks the outer bound of rule 1. About half of that domain — HITL-fatigue and oversight-quality measurement, IR-runbook authoring, decommission drills, bus-factor continuity — has no product on any platform, so the dated tooling map that rule 1 defers to is empty for those rungs and the criteria stand on operating-model evidence alone. The D9 products that do exist are platform-native primitives repurposed from D2 identity lifecycle and D7 observability. Rule 3's licensing bucket therefore reads near-zero for a platform incumbent, and the domain's spend lands in operational labor and run-rate.
