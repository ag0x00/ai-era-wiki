---
type: framework
title: "Continuous Threat Exposure Management (CTEM)"
address: c-000102
created: 2026-05-23
updated: 2026-05-23
tags:
  - frameworks
  - exposure-management
  - ai-in-sec-defense
  - vulnerability-management
status: developing
scope_axis:
  - ai-in-sec-defense
adoption_signal: active
aliases:
  - "CTEM"
coined_by:
  - "[[gartner]]"
related:
  - "[[vulnops]]"
  - "[[vulnops-implementation-roadmap]]"
  - "[[deloitte]]"
  - "[[zero-day-clock]]"
sources:
  - "https://www.vectra.ai/topics/ctem"
  - "https://www.gartner.com/en/documents/4016760"
---

# Continuous Threat Exposure Management (CTEM)

CTEM is Gartner's five-stage program for managing exposure as a continuous, business-prioritized cycle rather than a periodic scan-and-patch chore. Gartner introduced it in 2022; by 2026 it is the dominant enterprise scaffold for AI-native discovery and remediation. (Confidence: **high** on the stages; **low** on the headline prediction below.)

## The Five Stages

1. **Scoping** — define the program's boundaries by business impact, not technology silos.
2. **Discovery** — find exposures across the scope: vulnerabilities, misconfigurations, identity risks, shadow IT, credential leaks, excessive permissions.
3. **Prioritization** — rank by business context, asset criticality, and attack-path analysis rather than CVSS alone.
4. **Validation** — test whether prioritized exposures are actually exploitable, via breach-and-attack simulation, red teaming, and penetration testing.
5. **Mobilization** — drive cross-functional remediation by connecting security, IT operations, cloud, and governance through structured workflows.

CTEM orchestrates vulnerability management, attack-surface management, and validation tools into one continuous, event-driven loop. Traditional vulnerability management, by contrast, scans for CVEs on a fixed schedule.

## Relevance to This Wiki

CTEM is the enterprise operating frame that [[vulnops|VulnOps]] and the [[vulnops-implementation-roadmap|VulnOps implementation roadmap]] build on: the roadmap maps its AI-native discovery pipeline onto CTEM's Discovery→Prioritization→Validation→Mobilization spine. [[deloitte|Deloitte]] productized this in its Opus-powered CTEM-on-Ascend offering. CTEM's continuous cadence is the structural answer to the [[zero-day-clock|Zero Day Clock]]'s collapsing time-to-exploit.

> [!gap] The 3x prediction is unvalidated
> Gartner predicted organizations running a CTEM program would be "three times less likely to suffer a breach by 2026." As of 2026 this remains directionally supported but not empirically validated — no independent study has compared breach rates of adopters versus non-adopters. Treat the figure as a vendor projection, not a measured result.

<!-- sources:auto -->
## Sources

- [vectra.ai](https://www.vectra.ai/topics/ctem)
- [gartner.com](https://www.gartner.com/en/documents/4016760)
<!-- /sources -->
