---
type: framework
title: "NIST AI 800-4: Monitoring Challenges"
address: c-000217
created: 2026-06-21
updated: 2026-06-21
tags:
  - frameworks
  - nist
  - caisi
  - post-deployment-monitoring
  - observability
status: developing
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
framework_class: descriptive-research-report
issuer: "[[nist|NIST]]"
homepage: https://doi.org/10.6028/NIST.AI.800-4
doi: 10.6028/NIST.AI.800-4
current_version: "final (March 2026)"
first_published: 2026-03
last_substantive_update: 2026-03
published_by: "[[nist|NIST]]"
authors:
  - "Anita Rao"
  - "Drew Keller"
  - "Neha Kalra"
  - "Ryan Steed"
  - "Kweku Kwegyir-Aggrey"
  - "Kevin Klyman"
  - "Diane Staheli"
  - "Stevie Bergman"
adoption_signal: maintained
scope: "Descriptive landscape of the gaps, barriers, and open questions in monitoring deployed AI systems; not a control catalog"
audience: "AI developers, deployers, monitors, regulators; research community"
aliases:
  - "AI 800-4"
  - "NIST AI 800-4"
related:
  - "[[nist-ai-rmf]]"
  - "[[nist-ai-600-1]]"
  - "[[nist|NIST]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[standards-review-nist-ai-rmf-2026-Q2]]"
sources:
  - "[[.raw/papers/nist-ai-800-4-monitoring-deployed-ai-2026-03.pdf]]"
primary_documents:
  - title: "NIST AI 800-4 — Challenges to the Monitoring of Deployed AI Systems"
    url: "https://doi.org/10.6028/NIST.AI.800-4"
    version: "final"
    published: "2026-03"
    retrieved: "2026-06-21"
    archived_copy: ".raw/papers/nist-ai-800-4-monitoring-deployed-ai-2026-03.pdf"
    scope_in_wiki: "Full report — six monitoring categories (§2); cross-cutting and category-specific challenges (§3); open questions (§3.3); methodology (§1.2, App. B)"
coined_by:
  - "[[nist|NIST]]"
---

# NIST AI 800-4 — Challenges to the Monitoring of Deployed AI Systems

**NIST AI 800-4** (March 2026) is a report from the [[nist|NIST]] Center for AI Standards and Innovation (CAISI) in the Trustworthy and Responsible AI series. It documents the gaps, barriers, and open questions in monitoring AI systems after deployment, with particular relevance to frontier generative AI systems. It is **descriptive, not normative**: it catalogs what the field cannot yet do and the questions it has not resolved, and prescribes no controls, measurable criteria, or maturity tiers.

The report exists because pre-deployment evaluation runs in controlled, limited environments while deployed AI is non-deterministic and exposed to a large attack surface ("cloud servers, GPUs, tools, classifiers"). The field has consensus on the *need* for post-deployment monitoring but lacks "best practices, validated methodologies, and common terminology."

## Defined terms

- **Post-deployment** — the period after a system is put into at least partial working operation (§1.1).
- **Monitoring** — any type of measurement, potentially continuous, of an AI system and its immediate interacting components (§1.1).
- **Gaps** — areas under-explored or lacking sufficient attention; **Barriers** — known obstacles; **Open questions** — unsettled or unanswered questions (§3).
- **Monitorability tax** — the cost developers may bear to keep agents monitorable (Baker et al., quoted §3.1.5).

## Six monitoring categories (§2, Table 1)

Functionality, Operational, Human Factors, Security, Compliance, and Large-Scale Impacts. Figure 1 shows how the coded literature distributes across them; Appendix C is the codebook.

## Challenge taxonomy (§3)

**Cross-cutting (§3.1):** trusted methods and tools (§3.1.1), visibility and transparency (§3.1.2 — including the immature information-sharing ecosystem and the lack of direct visibility into model properties), pace of change (§3.1.3), organizational incentives and culture (§3.1.4), and resource requirements (§3.1.5).

**Category-specific (§3.2):** distribution shift, missing ground-truth datasets, and degradation-threshold gaps (§3.2.1 Functionality); fragmented logging across distributed infrastructure (§3.2.2 Operational); human-AI feedback loops, sycophancy, and telemetry underutilization (§3.2.3 Human Factors); detecting deceptive behavior, scheming, and monitor-evasion (§3.2.4 Security); ToS-violation tracking and policy-landscape complexity (§3.2.5 Compliance); human-flourishing metrics and open-weight downstream effects (§3.2.6 Large-Scale Impacts).

**Open questions (§3.3):** organized as Why / Who / What / When / How.

## Agentic relevance

The report treats agentic systems as a recurring lens rather than its primary frame: agent identifiers as an unstandardized visibility gap (§3.1.2), agent non-determinism (§3.1.1), the cost of "lengthy agentic tasks" (§3.1.5), monitoring multi-agent distributed systems (§3.2.2), longitudinal agent-usage tracking (§3.2.1), and scheming / sandbagging / subverting the monitoring setup (§3.2.4). The lineage DevOps → MLOps → LLMOps/AgentOps is noted (footnote 2).

## Methodology

A 23-paper preliminary literature review grew to 87 papers via citation backchaining, combined with three workshops (April–May 2025, 250+ participants) and inductive thematic coding by two reviewers. The initial taxonomy was seeded in part by LLMs (Claude 3.7 Sonnet, GPT 4.1, Gemini 2.5 Pro via Perplexity; Appendix B).

## Position relative to the AI RMF stack

AI 800-4 is the monitoring-focused companion to the [[nist-ai-rmf|AI RMF]] and the [[nist-ai-600-1|AI 600-1 GenAI Profile]]. For the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] it is a requirements- and gap-source for **D7 Observability & Detection** (its core topic) and the agentic-monitoring open items, not a control standard. See the [[standards-review-nist-ai-rmf-2026-Q2|NIST AI RMF Standards Review (2026-Q2)]] for the clause-level coverage matrix and the bounded finding that AI 800-4 supplies no mappable controls.

## See Also

- [[nist-ai-rmf|NIST AI Risk Management Framework (AI RMF)]] — parent framework
- [[nist-ai-600-1|NIST AI 600-1 — Generative AI Profile]] — companion profile
- [[agentic-ai-security-cmm-d7-observability|Agentic AI Security CMM — D7 Observability]] — the CMM domain this report informs
- [[standards-review-nist-ai-rmf-2026-Q2|NIST AI RMF Standards Review (2026-Q2)]] — clause-level review

<!-- sources:auto -->
## Sources

- [NIST AI 800-4: Monitoring Challenges](https://doi.org/10.6028/NIST.AI.800-4)
<!-- /sources -->
