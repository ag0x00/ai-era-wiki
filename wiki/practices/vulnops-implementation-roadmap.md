---
type: practice
title: "VulnOps Implementation Roadmap (Enterprise)"
address: c-000101
created: 2026-05-23
updated: 2026-05-29
tags:
  - practices
  - vulnops
  - exposure-management
  - ai-in-sec-defense
  - ai-vuln-discovery
status: developing
scope_axis:
  - ai-in-sec-defense
related:
  - "[[vulnops]]"
  - "[[vulnerability-operations-center]]"
  - "[[mythos-ready-security-program]]"
  - "[[continuous-threat-exposure-management]]"
  - "[[zero-day-clock]]"
  - "[[cyber-poverty-line]]"
  - "[[claude-code-security]]"
  - "[[deloitte]]"
  - "[[verizon-dbir-2026]]"
  - "[[first-vulnerability-forecast-2026]]"
sources:
  - "https://cyberagents.exchange/playbooks/vulnops/"
  - "https://blog.checkpoint.com/exposure-management/the-case-for-a-vulnerability-operations-center"
  - "https://www.vectra.ai/topics/ctem"
  - "https://www.sans.org/posters/key-metrics-cloud-enterprise-vmmm"
  - "https://www.verizon.com/business/resources/reports/2026-dbir-data-breach-investigations-report.pdf"
  - "https://www.first.org/blog/20260211-vulnerability-forecast-2026"
  - "https://thehackernews.com/2026/02/the-ctem-divide-why-84-of-security.html"
---

# VulnOps Implementation Roadmap (Enterprise)

[[vulnops|VulnOps]] names the function; this page is how an enterprise team builds it. The roadmap synthesizes three external frames into a phased implementation: Gartner's [[continuous-threat-exposure-management|CTEM]] program spine, the [[vulnerability-operations-center|Vulnerability Operations Center (VOC)]] operating model, and the [[mythos-ready-security-program|Mythos-ready playbook]]'s priority-action sequence. (Confidence: **medium**. The frameworks are well-documented, but the synthesis is this wiki's, and the tool pipelines and VOC adoption survey come from vendor sources.)

## Timing and problem size

Two non-vendor sources bound the scale a VulnOps function must absorb. The [[first-vulnerability-forecast-2026|FIRST Vulnerability Forecast]] projects CVE publication crossing 50,000 for the first time in 2026, with a median near 59,000 and a credible upper bound past 117,000, and that forecast does not even model AI-accelerated discovery. The [[verizon-dbir-2026|Verizon DBIR 2026]] supplies the other axis: across 22,000 confirmed breaches, vulnerability exploitation (31%) overtook stolen credentials (13%) as the top initial-access vector, while remediation moved backward — the share of CISA KEV criticals fully patched fell from 38% to 26% and the median time to resolution rose from 32 to 43 days. The volume is more than a team can triage by hand, and it arrives faster than any human can patch. This roadmap is built to survive that problem, not to deliver a maturity upgrade.

## Operating model: a control tower, not another team

A [[vulnerability-operations-center|VOC]] coordinates remediation; it does not replace the teams that do the work. It runs alongside the SOC and divides the labor cleanly: the **SOC detects and responds**, the **VOC prevents through remediation**. The VOC owns five missions (detection and collection, qualification and contextualization, prioritization, remediation steering, and reporting and governance) and holds the other teams accountable through a single, tracked model. Check Point reports VOC adoption at roughly 65% of surveyed enterprises, most rating it medium-to-high value; the figure traces to the France-weighted CESIN *Baromètre* (Vague 11, Jan 2026), so treat it as European rather than global. See [[vulnerability-operations-center|VOC]] for the sourcing.

This coordination role makes VulnOps tractable when AI-driven discovery floods the queue: triage and accountability, not scanning, are the scarce resources.

## The AI-native pipeline, mapped to CTEM

The Mythos-ready operating pipeline runs five stages, each mapping onto a CTEM stage:

| Pipeline stage | What it does | CTEM stage |
|---|---|---|
| AI static analysis | reads and reasons about code for logic and access-control flaws ([[claude-code-security\|Claude Security]]-class tools) | Discovery |
| Dynamic testing | catches vulnerabilities that surface only at runtime | Discovery |
| Third-party discovery | finds zero-day exposure in dependencies and the wider estate | Discovery |
| Consolidation and prioritization | correlates findings with asset context, ranks by business risk and attack path | Prioritization + Validation |
| Human checkpoint | analyst approves before any automated remediation deploys | Mobilization |

The human checkpoint is deliberate. Full automation is available upstream of it; the gate keeps a person accountable for what ships. Scoping, CTEM stage one, happens before the pipeline runs: define the estate by business impact.

## Phased roadmap

**Crawl (this week).** Point an agent at one repository or pipeline and ask for a security review (Mythos-ready Priority Action 1). Triage findings manually. The goal is a working loop, not coverage.

**Walk (1–3 months).** Extend discovery across the full software estate: own code, AI-generated code, third-party libraries, container images, MCP servers, IDE extensions, agent skills, and rules-files. Stand up the consolidation-and-prioritization layer. Set remediation SLAs by asset class. Staff a **VulnOps Analyst** to own the triage gate.

**Run (6–12 months).** Operate a standing VulnOps/VOC function: automated remediation within defined guardrails, governance and reporting, and a metrics dashboard the board can read. This is Mythos-ready Priority Action 11, "Stand Up VulnOps."

## Metrics and SLAs

Track execution, not vulnerability counts: mean time to detect, mean time to remediate, validation success rate, exposure-backlog reduction, and SLA adherence across asset classes. The signal that matters is **sustained backlog reduction**: a backlog that stays down, rather than a cleanup sprint that knocks it down once and lets it climb back.

## Watch items

- **Triage discipline is the design center.** Existing CVE/NVD infrastructure was built for dozens of critical CVEs a month, not hundreds; severity scoring, confidence scoring, deduplication, and prioritization queues must be first-class from day one.
- **The gap is execution, not awareness.** The 2026 *CTEM Divide* survey (128 security decision-makers) found 87% rate CTEM important but only 16% have operationalized it. The failure point is Mobilization, the stage that turns findings into tracked remediation. Budget and accountability are scarcer than agreement. (Source: [The Hacker News — *The CTEM Divide*](https://thehackernews.com/2026/02/the-ctem-divide-why-84-of-security.html). Confidence: **low-to-medium**; vendor-published survey, sample size disclosed but methodology thin.)
- **Autonomous patching has a ceiling.** Gartner projects that by 2028 more than half of threat exposures will stem from nontechnical causes that automated patching cannot fix: misconfiguration, identity, and process. A VulnOps function scoped only around code patches will close a shrinking fraction of real exposure; the remediation-steering mission has to reach configuration and identity owners, not just developers. (Source: Gartner, via [Vectra's CTEM summary](https://www.vectra.ai/topics/ctem).)
- **Below the [[cyber-poverty-line|Cyber Poverty Line]],** standing up an independent VulnOps function is not feasible; collective defense through ISACs and sector coordinating groups is the realistic path.
- **The remediation commons is the new bottleneck.** When discovery outruns patching, the inversion the [[zero-day-clock|Zero Day Clock]] tracks, virtual patching and mitigation buy time. The durable fix is remediation throughput: the capacity this roadmap is designed to build.
