---
type: concept
title: "Cyber Poverty Line"
address: c-000071
created: 2026-05-15
updated: 2026-08-21
tags:
  - concepts
  - cyber-poverty-line
  - collective-defense
  - capacity-asymmetry
status: seed
no_public_url: "Term coined by Wendy Nather; spread via talks and blog commentary with no single canonical primary document"
scope_axis: [sec-against-ai, ai-in-sec-defense]
related:
  - "[[wendy-nather|Wendy Nather]]"
  - "[[mythos-ready-security-program|Mythos-ready Security Program]]"
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - "[[supply-chain-security-for-agents|Supply Chain Security for Agents]]"
  - "[[agentic-soc-ra-investigation-case-management|Agentic SOC Investigation Surface]]"
  - "[[agentic-soc-ra-incident-response|Agentic SOC Incident Response Surface]]"
sources:
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
---

# Cyber Poverty Line

The **Cyber Poverty Line**, introduced by **[[wendy-nather|Wendy Nather]]**, is the threshold below which an organization cannot field the resources (people, tooling, expertise, time) needed to defend itself adequately against current threats. The concept predates the Mythos era and has been cited across the cybersecurity-economics literature for years; it is surfaced in the [[mythos-ready-briefing|Mythos-ready briefing]] as a load-bearing reason to invest in **collective defense** — ISACs, CERTs, sector coordinating groups, and standards bodies — *especially when considering organizations that fall below the line*.

## Consequences in the Mythos era

AI-driven vulnerability discovery widens the offense/defense capacity gap. Organizations above the Cyber Poverty Line can adopt [[vulnops|VulnOps]], deploy [[claude-code-security|Claude Code Security]] / [[codex-security|Codex Security]] / [[openant|OpenAnt]] across their pipelines, build deception capabilities, and stand up automated response. Organizations below the line cannot. The Mythos-era response from the briefing's lead authors is explicit:

> *"Engage now with sector coordinating groups, ISACs, CERTs, and standards bodies to share threat intelligence, coordinate response, and produce sector-specific guidance for this moment. Defenders must do the same and leverage our coordinating groups, especially when considering organizations that fall below the Cyber Poverty Line, as introduced by Wendy Nather."*
> — [[mythos-ready-briefing|Mythos-ready briefing]], §II "Build Collective Defense Now"

The agentic SOC surfaces record where the rise in below-line capability stops. The [[agentic-soc-ra-investigation-case-management|Agentic SOC Investigation Surface]] puts tier-2 reasoning work — enrichment, timeline building, next-pivot suggestions — within reach of a one-person team working over telemetry borrowed from an MDR/MSSP or ISAC, with the human still owning the narrative. The [[agentic-soc-ra-incident-response|Agentic SOC Incident Response Surface]] marks the limit: pre-staged, parameterized containment reaches a small team without the SOAR playbook engineering it once demanded, while delegated response stays unwarranted below the line, because the blast radius of a wrong containment outweighs the speed gained. Capability crosses the line asymmetrically — findings-producing work crosses further than action-taking authority does.

## Adjacent / Open

- Quantitative threshold is not formally defined by Nather; it is a *qualitative* construct describing structural capacity deficit rather than a numeric metric.
- **Mythos-era refinement candidate**: as AI defensive tools become accessible (e.g., free [[openant|OpenAnt]] OSS scan; [[claude-code-security|CCS]] expedited free access for OSS maintainers; OpenAI pro-bono [[codex-security|Codex Security]] scanning for non-commercial OSS), the line itself may move. The briefing implicitly argues that *capability* below the line is rising via free / pro-bono / OSS tooling — but *capacity* (people, time, triage discipline) remains the binding constraint. The capability half is partly settled in §Consequences in the Mythos era, where the agentic SOC surfaces put findings-producing work within reach of a below-line team while delegated action-taking authority stays out of it; the capacity half stays open.

## See Also

- [[wendy-nather|Wendy Nather]] — concept creator.
- [[mythos-ready-security-program|Mythos-ready Security Program]] §5 *How to Adapt* — explicit guidance for below-line organizations.
