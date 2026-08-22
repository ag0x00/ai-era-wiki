---
type: paper
title: "Continuous Detection and Continuous Response"
address: c-000142
created: 2026-05-25
updated: 2026-05-25
tags:
  - papers
  - agentic-soc
  - soc-automation
  - detection-engineering
  - ai-in-sec-defense
status: summarized
scope_axis: [ai-in-sec-defense]
year: 2026
authors:
  - "Asaf Wiener"
venue: "Mate Security (vendor blog)"
source_url: https://mate.security/blog/continuous-detection-continuous-response---a-new-security-operations-framework
key_claim: "Mate proposes CD/CR (Continuous Detection / Continuous Response): an agentic-SOC architecture in which detection, investigation, and response run as one continuous loop on a single reasoning plane, where closed investigations compress automatically into new context-specific detections — built on a 'Security Context Graph' that models the organization from the investigation side first."
related:
  - "[[mate-security|Mate Security]]"
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[vulnops-l1-soc-extinction|From Threat Intel to VulnOps]]"
  - "[[agent-observability|Agent Observability]]"
sources:
  - https://mate.security/blog/continuous-detection-continuous-response---a-new-security-operations-framework
---

# Continuous Detection / Continuous Response — A New Security Operations Framework (Mate)

A vendor framing piece by Asaf Wiener (co-founder and CEO, Mate Security), May 2026, introducing an architectural framework the company calls **CD/CR — Continuous Detection / Continuous Response**. It is directional rather than empirical: the post argues an architecture and reports qualitative customer outcomes, with no measured benchmarks. Its value to this wiki is as a named, vendor-articulated model of the converged agentic SOC.

## The Argument

The premise is that the classical SOC was built for a different era: AI-operated attacks iterate in seconds, move through activity that looks legitimate, and defeat rules written for known patterns, while data sprawls across SIEMs, lakes, and point tools and analyst headcount scales with the alert queue. The post accepts the emerging re-architecture (decouple compute from storage, move data out of the SIEM into open data lakes, let AI agents operate on top) but argues a layer is still missing: detection and investigation run in separate silos, on separate reasoning planes.

CD/CR converges them. Its central claim is an identity between the two functions: **a detection is an investigation that has been run often enough to automate, and an investigation is a detection that has not been compressed yet.** In the framework, detection, investigation, and response run as one continuous loop on a single reasoning plane — investigations compress into new detections, detections feed the next investigation, confirmed noise closes itself, and containment executes continuously and at scale.

## The Enabling Layer — Security Context Graph

Mate attributes CD/CR to a foundational layer it calls the **Security Context Graph**: a living model of organizational knowledge that aggregates distributed sources — data lakes, telemetry, standard operating procedures, architecture, Slack messages, threat intelligence, and MITRE ATT&CK — and queries data where it lives. The stated design choice is to build it "from the investigation side first," on the argument that an AI investigation observes ground truth (what happened, in this environment, against this threat model) and is therefore the best source from which to generate adaptive, context-specific detections. The contrast drawn: "Vendor rule libraries guess at your environment. Investigations know it." Every closed case becomes a "compression candidate" for a new detection shaped by the organization's own assets and history.

## Claimed Outcomes

The post lists six qualitative effects for organizations running CD/CR, none quantified: faster (continuous detection creation and near-instant containment, no manual release cycle); precise (time-bound, auto-reversing exceptions; blast-radius prioritization); cost-effective (lower SIEM bills by identifying which data earns ingestion); compounding (coverage grows as a byproduct of investigation work); resilient and adaptive (detections update as context changes); and focused (noisy detections filtered before reaching the queue).

## Placement

CD/CR is a third vendor framing of the converged agentic SOC direction tracked in [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]], alongside the Microsoft copilot-plus-specialized-agents pattern and CrowdStrike's AIDR. It is closest to the [[vulnops-l1-soc-extinction|Mallory "threads, not cases" framing]]: both replace static rule libraries and case queues with a continuously-reasoning, context-driven loop, and both are single-vendor-coined, so the model is directional rather than independently established. The "investigation as ground truth that compresses into detections" idea is the substantive contribution; the Security Context Graph is Mate's specific implementation of the shared-context substrate that any converged-SOC claim requires.

> [!gap] No independent evaluation
> CD/CR's outcome claims are vendor-reported and qualitative. There is no benchmark, no disclosed customer data, and no neutral comparison to rule-library or copilot-plus-agents SOCs. Treat the framework as an architectural proposal to test, not a measured result.

## See Also

- [[mate-security|Mate Security]] — the vendor and the Security Context Graph
- [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] — the synthesis page this framing feeds
- [[vulnops-l1-soc-extinction|From Threat Intel to VulnOps]] — the adjacent single-vendor "threads, not cases" framing
