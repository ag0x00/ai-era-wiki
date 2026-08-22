---
type: entity
entity_type: organization
title: "Uber"
address: c-000214
created: 2026-06-17
updated: 2026-06-17
tags:
  - entities
  - organizations
  - enterprise-deployment
  - agentic-soc
  - ai-in-sec-defense
status: developing
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
org_type: vendor
homepage: "https://www.uber.com"
role: "Enterprise operator; built and deployed the ADR agentic-detection system and released the ADR-Bench benchmark from its production telemetry."
related:
  - "[[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]]"
  - "[[adr-bench|ADR-Bench]]"
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
sources:
  - "https://arxiv.org/abs/2605.17380"
  - "[[.raw/papers/adr-agentic-detection-system-2026-05-17.md]]"
---

# Uber

On this wiki, [Uber](https://www.uber.com) appears as an **enterprise operator and detection-engineering publisher** rather than a security vendor. Its security and ML teams (with academic collaborators at MIT and the University of Oxford) built and deployed [[adr-agentic-detection-system|ADR]], an agentic detection-and-response system for AI agents running over the [[mcp-security|Model Context Protocol]], and released the [[adr-bench|ADR-Bench]] benchmark distilled from that deployment.[^paper]

`org_type` is recorded as `vendor` for the closed-vocabulary lint, but the more accurate framing is **enterprise practitioner**: the value of Uber's contribution is the production deployment evidence, not a product sold to others.

## Basis for citation

- **Production deployment data.** Ten months of ADR in production across 7,200+ unique hosts processing 10,000+ agent sessions daily on corporate macOS endpoints (Intel and ARM).[^paper] First session 2024-12-15; ≥100 sessions/day by ~April 2025; ≥10,000/day by ~October 2025.[^deploy]
- **Credential-exposure findings.** Hundreds of high-severity credential exposures across 26 categories, and a Hooks-based shift-left prevention layer reported at 97.2% precision (206 true positives, 6 false positives across 212 unique credentials).[^deploy]
- **ADR-Bench.** A 302-task, 133-MCP-server benchmark derived from Uber SOC telemetry, released open-source.[^paper]

This makes Uber a peer production-scale data point to Salesforce's [[1-8m-prompts-30-alerts-talk|Agentforce telemetry]] on the [[agentic-soc-state-of-the-field|Agentic SOC]] axis — both report real volumes, real alert economics, and real false-positive rates rather than benchmark-only results.

## See Also

- [[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]]
- [[adr-bench|ADR-Bench]]
- [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]

[^paper]: [arXiv:2605.17380](https://arxiv.org/abs/2605.17380), authorship and §1, §4: Uber/MIT/Oxford authors, the ADR system, and ADR-Bench derived from enterprise telemetry.
[^deploy]: §6 *Real-World Deployment*, [arXiv:2605.17380](https://arxiv.org/abs/2605.17380): host and session counts, the deployment timeline (Figure 8), 26 credential categories, and the prevention-layer precision figures.
</content>
