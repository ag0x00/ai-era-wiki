---
type: paper
title: "Enterprise Patch and Remediation Benchmark"
address: c-000119
created: 2026-05-25
updated: 2026-05-25
tags:
  - papers
  - patch-management
  - remediation
  - mttr
  - benchmark
  - sec-against-ai
status: summarized
scope_axis: [ai-in-sec-defense, sec-against-ai]
year: 2026
authors: []
venue: "Qualys (vendor benchmark)"
source_url: https://blog.qualys.com/qualys-insights/2026/04/20/enterprise-patch-remediation-benchmark-2026
archived_copy: ".raw/articles/qualys-patch-remediation-benchmark-2026-05-25.md"
key_claim: "Across roughly 150 million patches deployed in twelve months of anonymized enterprise telemetry, the mean time to remediation for complex, hard-to-patch applications (Java, .NET, Citrix) reached 5 months and 10 days, while about 40 million patches were deployed autonomously with no human in the loop — quantifying both the remediation-lag problem and the shift toward autonomous patching."
related:
  - "[[qualys|Qualys]]"
  - "[[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[vulnops|VulnOps]]"
  - "[[continuous-threat-exposure-management|Continuous Threat Exposure Management]]"
sources:
  - https://blog.qualys.com/qualys-insights/2026/04/20/enterprise-patch-remediation-benchmark-2026
---

# Enterprise Patch & Remediation Benchmark 2026 (Qualys)

A vendor benchmark built on anonymized remediation telemetry from global enterprise environments over twelve months. It is operational data, not a capability study: it measures how fast organizations actually deploy patches, where deployment stalls, and how much remediation now runs without human intervention.

## Key Figures

- **Mean time to remediation for complex applications: 5 months and 10 days.** This figure applies to the most-delayed class — applications such as Java, .NET, and Citrix Workspace App, where compatibility testing and uptime requirements turn patching into a controlled, slow process.[^mttr] It is not a global median across all vulnerabilities.
- **Roughly 150 million patches deployed in twelve months, about 40 million autonomously** (no human in the loop), per CEO Sumedh Thakar.[^autonomous]
- **Over 8 million Google Chrome patches deployed** — the single highest-volume patch, ahead of Microsoft Visual C++ Redistributable and Edge.[^chrome]

## Contribution

The benchmark quantifies the **defender-side lag** that the [[zero-day-clock|Zero Day Clock]] pairs with the attacker-side time-to-exploit collapse. When exploitation of the median disclosed vulnerability now lands on or before disclosure, a multi-month mean remediation time for complex applications is the operational gap that [[ai-era-supply-chain-hardening|patching-SLA tightening]] and a standing [[vulnops|VulnOps]] function exist to close.

The autonomous-patching figure is the more forward-looking data point: roughly a quarter of deployed patches already ship with no human in the loop, concentrated on high-volume, low-risk third-party applications under set-and-forget automation. That is the remediation-side analogue of autonomous discovery and exploitation.

## Caveats

A vendor benchmark drawn from one platform's customer base, framed around the vendor's automation product. The figures describe Qualys-managed environments, not the whole market. The headline 5-months-10-days number is a **mean for the hardest-to-patch application class**; quoting it as a general remediation median overstates the typical case and understates the tail.

## Notes

[^mttr]: [Qualys — Enterprise Patch & Remediation Benchmark 2026](https://blog.qualys.com/qualys-insights/2026/04/20/enterprise-patch-remediation-benchmark-2026), April 2026. "Across these applications, the average mean time to remediation (MTTR) was 5 months and 10 days" — referring to the most-commonly-delayed complex applications (Java, .NET, Citrix Workspace App).
[^autonomous]: [Qualys — Enterprise Patch & Remediation Benchmark 2026](https://blog.qualys.com/qualys-insights/2026/04/20/enterprise-patch-remediation-benchmark-2026), April 2026. Sumedh Thakar (President & CEO): about 40 million of roughly 150 million patches deployed in the last twelve months were deployed autonomously, with no human in the loop.
[^chrome]: [Qualys — Enterprise Patch & Remediation Benchmark 2026](https://blog.qualys.com/qualys-insights/2026/04/20/enterprise-patch-remediation-benchmark-2026), April 2026. Google Chrome led deployment volume at over 8 million patches, followed by Microsoft Visual C++ Redistributable and Edge.
