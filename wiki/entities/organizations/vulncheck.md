---
type: entity
entity_type: organization
org_type: vendor
title: "VulnCheck"
address: c-000123
created: 2026-05-25
updated: 2026-06-21
tags:
  - entities
  - organization
  - vendor
  - vulnerability-intelligence
  - kev
  - sec-against-ai
status: developing
scope_axis: [sec-against-ai, ai-in-sec-defense]
homepage: https://www.vulncheck.com
related:
  - "[[vulncheck-exploitation-trends-q1-2025|2025 Q1 Trends in Vulnerability Exploitation]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[continuous-threat-exposure-management|Continuous Threat Exposure Management]]"
sources:
  - https://www.vulncheck.com
  - https://www.vulncheck.com/blog/exploitation-trends-q1-2025
---

# VulnCheck

**Sources:** [VulnCheck (homepage)](https://www.vulncheck.com) · [2025 Q1 exploitation-trends report](https://www.vulncheck.com/blog/exploitation-trends-q1-2025)

VulnCheck is an exploit- and vulnerability-intelligence vendor. Its widely-cited data products are the VulnCheck KEV (a known-exploited-vulnerabilities catalog broader and often earlier than the CISA KEV) and NVD++ (an enriched mirror of the National Vulnerability Database).

On this wiki VulnCheck appears mainly as a measurement source for exploitation speed. Its VulnCheck KEV feed is one of the three datasets behind the [[zero-day-clock|Zero Day Clock]] (alongside CISA KEV and XDB), and its quarterly [[vulncheck-exploitation-trends-q1-2025|exploitation-trends reports]] are the first-party origin of the wiki's Q1-2025 figure that more than a quarter of newly-exploited vulnerabilities had exploitation evidence within a day of disclosure. The wiki cites VulnCheck exploitation evidence as a prioritization input ahead of score-based triage, consistent with the company's own finding that EPSS is a trailing indicator for emerging threats.
