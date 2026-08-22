---
type: paper
title: "From Threat Intel to VulnOps"
address: c-000079
created: 2026-05-15
updated: 2026-07-30
tags:
  - papers
  - vulnops
  - agentic-soc
  - threat-intel
  - level-1-soc
  - mallory
status: summarized
scope_axis: [ai-in-sec-defense]
year: 2026
authors: []
venue: "CYBR.SEC.Media (trade press)"
source_url: https://www.cybrsecmedia.com/from-threat-intel-to-vulnops-why-level-1-soc-as-we-know-it-is-heading-to-extinction/
archived_copy: ".raw/articles/cybrsecmedia-vulnops-l1-soc-extinction-2026-05-15.md"
featured:
  - "[[jonathan-cran|Jonathan Cran]] (founder, Mallory)"
key_claim: "The conventional CTI → SIEM → ticket-queue → analyst-triage pipeline is at end-of-life. VulnOps — the operational fusion of vulnerability management and threat intelligence into a single agent-augmented function — is the successor model. Level 1 SOC tasks (alert triage, initial investigation, ticket routing) are being absorbed by existing SOC-automation vendors; the higher-level threat-intelligence + contextualization + reasoning work is where the human analyst role evolves. The SOC end state is *monitor mode*; the CISO end state is *router and trusted source of information* rather than incident commander."
methodology: "Trade-press feature article centered on an interview with Jonathan Cran (founder of Mallory). Single-vendor framing tempered by explicit naming of the L1-SOC-automation competitive landscape; not peer-reviewed; not community-consensus. Vendor-strategic-narrative source."
related:
  - "[[agentic-soc-ra-alert-triage|Agentic SOC RA — Alert Triage]]"
  - "[[mallory|Mallory]]"
  - "[[jonathan-cran|Jonathan Cran]]"
  - "[[vulnops|VulnOps]]"
  - "[[agentic-soc-state-of-the-field|Agentic SOC — State of the Field]]"
  - "[[mythos-ready-briefing|Mythos-ready briefing]]"
  - "[[hitl|Human-in-the-Loop for Agentic AI]]"
  - "[[guardian-agent|Guardian Agent]]"
sources:
  - "[[.raw/articles/cybrsecmedia-vulnops-l1-soc-extinction-2026-05-15.md]]"
  - https://www.cybrsecmedia.com/from-threat-intel-to-vulnops-why-level-1-soc-as-we-know-it-is-heading-to-extinction/
aliases:
  - papers/cybrsecmedia-vulnops-l1-soc-extinction-2026-05-15
  - cybrsecmedia-vulnops-l1-soc-extinction-2026-05-15

---

# From Threat Intel to VulnOps — Why Level 1 SOC as We Know It Is Heading to Extinction

**Source:** [CYBR.SEC.Media — *From Threat Intel to VulnOps: Why Level 1 SOC As We Know It Is Heading to Extinction*](https://www.cybrsecmedia.com/from-threat-intel-to-vulnops-why-level-1-soc-as-we-know-it-is-heading-to-extinction/) (fetched 2026-05-15). Local copy: `.raw/articles/cybrsecmedia-vulnops-l1-soc-extinction-2026-05-15.md`. Featured: [[jonathan-cran|Jonathan Cran]], founder of [[mallory|Mallory]].

## Key Claim

The conventional security operations pipeline — CTI feeds → SIEM → alert queue → analyst triage → case management — *"is running out of road."* The successor model fuses vulnerability management and threat intelligence into a single agent-augmented function the article and its featured vendor call **VulnOps**. The article makes three nested claims:

1. **Information silos are the load-bearing problem**, not analyst capacity. *"The fundamental idea is to un-silo the information so that it can be brought into the context window for the agent to be able to operationalize it."* — Cran. Threat intel, asset inventories, cloud configurations, and code-repository state are kept in separate tools; analysts bridge them mentally; agents cannot.
2. **Level 1 SOC tasks are being absorbed by existing SOC-automation and AI providers** — alert triage, initial investigation, ticket routing. The higher-level work (threat-intelligence contextualization, reasoning, supervised judgement, policy and guardrail design) is where the analyst role evolves.
3. **The end state is "monitor mode"** — SOC teams hand off as much as possible to routines and shift to supervising rather than executing incident response. The **CISO** evolves from incident commander to *"a router and a trusted source of information"* translating between AI-enabled operational teams and business leaders.

## Notable Findings

### 1. Second independent sourced use of "VulnOps" on the wiki — with a different framing

The wiki's existing [[vulnops|VulnOps]] concept page (filed 2026-05-15 from the [[mythos-ready-briefing|CSA / SANS / Unprompted / OWASP Mythos-ready briefing]]) attributes the term to **[[heather-adkins|Heather Adkins]] + [[gadi-evron|Gadi Evron]] + Bruce Schneier (October 2025)**, framed as *a permanent function staffed and automated like DevOps for autonomous **vulnerability research and remediation**.*

This article reports that **[[mallory|Mallory]]'s customers** independently use the term VulnOps to mean *the fusion of **threat intelligence and vulnerability management** into a single discipline.* Both framings center the same structural observation — that previously-separate functions need to be operationalized as one — but they emphasize different sides:

| Framing | Emphasis | Source |
| :--- | :--- | :--- |
| **Mythos-ready / Adkins-Evron-Schneier** | *Vulnerability research + remediation* — discovery-side function staffed like DevOps; PA 11 in the playbook | [[mythos-ready-briefing\|Mythos-ready briefing]] |
| **Mallory customer usage** | *CTI + vulnerability management fusion* — operationalize external intel against your own asset/code/config inventory | This article |

The two framings are compatible — the Mythos-ready briefing's VulnOps owns *continuous discovery of zero-day vulnerabilities across the entire software estate (own code through third-party software) and establishes automated remediation pipelines*, which structurally requires the CTI-to-environment-context fusion Mallory's customers describe. The wiki treats **both as load-bearing sourced framings of the same emerging operational pattern** and updates the VulnOps concept page accordingly rather than treating them as competing claims.

### 2. The Mallory architecture

Per Cran's description, the practical architecture is:

- **Continuous ingestion of ~3,000 sources**: social feeds, ISAC (Information Sharing and Analysis Center) data, vendor advisories, GitHub security disclosures, structured government feeds.
- **Threat graph** the ingested intelligence is enriched and mapped against.
- **Automatic mapping** of global intelligence to the **organization's specific assets, cloud environments, code repositories, and infrastructure-as-code configurations**.
- **Action-on-finding**: when a new vulnerability surfaces, the system *"produces detections, routes tickets to the appropriate teams, and in some cases takes direct action, subject to whatever policy guardrails the organization has configured."* The configurability is in user-provided **skill files** that define *the right thing* per environment.

### 3. "Threads, not cases" — the case-management model is replaced

Conventional case management treats the analyst as the integration layer: artifacts in, reports out. Cran's model replaces the *case* with a *thread* — *"every investigation, every action, is a thread. There's an agent in there, too."* Threads are collaborative analyst-agent exchanges working the same problem simultaneously, rather than the analyst feeding a case from outside.

### 4. Workforce implications — what gets elevated, what fades

Three role classes the article surfaces:

- **Disappearing / shrinking**: L1 SOC alert triage, initial investigation, ticket routing — absorbed by SOC-automation and AI providers.
- **Elevated**: policy and guardrail design (defining what the system is authorized to do autonomously and when it escalates), supervisory tuning of AI-driven routines, understanding when the system's judgment is sound and when it needs correction.
- **Emerging**: collaborative analyst-agent investigation work via threads.

> *"Some roles are going to change or go away, but you have teams of people who understand security context, and these systems are there to maintain the data."* — Cran

### 5. CISO role evolution

End-state framing from Cran: the CISO functions *"less as an incident commander and more as a router and a trusted source of information,"* busy translating between AI-enabled operational teams and business leaders, *"with security context as their primary value add."*

## Strengths and Weaknesses

**Strengths.** Names **L1 SOC absorption** as the specific workforce shift caused by agentic SOC automation. Provides a concrete vendor architecture (~3,000 sources + threat graph + asset/cloud/IaC mapping + policy-driven action) for the *un-silo* claim. Names *threads vs cases* as the structural change to case management. Names *monitor mode* as the SOC end state. Provides direct quotes from a named founder of a startup actively building in this space.

**Weaknesses.**

- **Single-vendor framing.** The article is centered on one Mallory interview; the *VulnOps* term-attribution to "customers" is vendor-reported. Independent corroboration (other vendors converging on the same VulnOps framing on the *CTI + vuln-mgmt fusion* axis) is the natural next-ingest direction.
- **No quantitative anchors.** Unlike the [[mythos-ready-briefing|Mythos-ready briefing]] or the [[zero-day-clock|Zero Day Clock]], this article provides no measurable claims — only architectural narrative.
- **Mallory is pre-GA.** *"Several components, particularly the asset correlation layer and user-customizable skill files are still maturing. Cran is targeting Black Hat for a full demonstration."* The architecture is described prospectively as much as descriptively.
- **L1-SOC absorption claim is partially predictive.** The article asserts L1 SOC tasks are being absorbed by existing SOC-automation vendors — true for some segments but not yet uniformly proven across enterprise sectors. The wiki's [[agentic-soc-state-of-the-field|Agentic SOC thesis]] tracks this trajectory; this article is one of multiple data points rather than an empirical anchor.

## Relations

- **Strengthens** [[vulnops|VulnOps]] concept page — adds a second independent sourced use of the term, with a complementary CTI-fusion framing alongside the Mythos-ready briefing's discovery-and-remediation framing. The VulnOps page is updated accordingly.
- **Strengthens** [[agentic-soc-state-of-the-field|Agentic SOC — State of the Field]] thesis — adds the L1-SOC-extinction / monitor-mode-end-state narrative and the CISO-as-router framing. The thesis is updated with a new evolution entry.
- **Adds** [[mallory|Mallory]] as a sourced product entity and [[jonathan-cran|Jonathan Cran]] as a sourced person.
- **Compatible with** [[hitl|HITL]] — Mallory's *policy and guardrail design* and *human escalation conditions* are the HITL parameters made operational at the SOC platform level.
- **Adjacent to** [[guardian-agent|Guardian Agent]] (Gartner-coined procurement-language for the oversight layer) — Mallory positions itself in a complementary slot, focused on the contextualization-and-reasoning layer rather than the gating layer.

**VulnOps is now sourced from two independent directions.** The Mythos-ready briefing attributes *VulnOps* to Adkins + Evron + Schneier (Oct 2025) and frames it as a vulnerability-research-and-remediation function staffed like DevOps. This article attributes the term to Mallory's customer base and frames it as the fusion of threat intelligence and vulnerability management into a single discipline. Both framings center the same structural observation: previously-separate functions need to be operationalized as one. Two independent sourcings (one strategic-briefing, one vendor-trade-press) in the same six-month window suggest the term is converging into the field's vocabulary rather than being a single-vendor or single-author coinage.

[[agentic-soc-ra-alert-triage|The Agentic SOC RA's alert-triage function]] is where this absorption is most visible in an architecture: first-pass triage moves to agents and the human steps up to supervision, which is the monitor-mode shift described here rendered as a concrete RA function.
