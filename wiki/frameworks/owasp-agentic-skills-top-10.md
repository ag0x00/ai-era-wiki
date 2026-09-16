---
type: framework
title: "OWASP Agentic Skills Top 10"
address: c-049634
created: 2026-09-16
updated: 2026-09-16
tags:
  - frameworks
  - owasp
  - agentic-ai
  - skills
  - mcp
status: developing
scope_axis:
  - sec-of-ai
origin: aggregated
published_by: "[[owasp|OWASP]]"
current_version: "2026"
first_published: 2026
scope: "Ten risks on the agent skill, the packaged instruction-and-script artifact an agent loads, positioned as the behavior layer between the model and the MCP tool layer"
audience: "Agent platform builders, skill marketplace operators, security engineers assessing agent tool ecosystems"
adoption_signal: unknown
last_substantive_update: "2026-09-16"
primary_documents:
  - title: "OWASP Agentic Skills Top 10 (AST10) project page"
    url: "https://owasp.org/www-project-agentic-skills-top-10/"
    retrieved: "2026-09-16"
    archived_copy: ".raw/articles/owasp-agentic-skills-top-10-project-2026-09-16.md"
  - title: "GenAI Crosswalk data layer and README (AST entry names, severities, MAESTRO mappings)"
    url: "https://genai-security-project.github.io/crosswalk/"
    retrieved: "2026-09-16"
    archived_copy: ".raw/articles/owasp-genai-crosswalk-2026-09-16.md"
related:
  - "[[mcp-security]]"
  - "[[csa-maestro]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[supply-chain-security-for-agents]]"
  - "[[owasp-genai-crosswalk]]"
  - "[[ken-huang]]"
  - "[[owasp]]"
  - "[[standards-review-backlog]]"
sources:
  - "https://owasp.org/www-project-agentic-skills-top-10/"
  - "[[.raw/articles/owasp-genai-crosswalk-2026-09-16.md]]"
  - "[[.raw/articles/owasp-agentic-skills-top-10-project-2026-09-16.md]]"
---

# OWASP Agentic Skills Top 10

## Scope

The OWASP Agentic Skills Top 10 (AST10) ranks ten risks on the agent skill: the packaged instruction-and-script artifact an agent loads and runs, distinct from the model that reasons and the tool the skill calls over [[mcp-security|MCP]]. The project states the boundary directly: "MCP = how the model talks to tools; AST10 = what those tools actually do."[^ast10] It places the skill as a behavior layer between the model and the MCP tool layer.[^ast10]

## Structure

| ID | Name | Severity |
|---|---|---|
| AST01 | Malicious Skills | Critical |
| AST02 | Supply Chain Compromise | Critical |
| AST03 | Over-Privileged Skills | High |
| AST04 | Insecure Metadata | High |
| AST05 | Untrusted External Instructions | High |
| AST06 | Weak Isolation | High |
| AST07 | Update Drift | Medium |
| AST08 | Poor Scanning | Medium |
| AST09 | No Governance | Medium |
| AST10 | Cross-Platform Reuse | Medium |

The ten names and severities above are computed from the GenAI Crosswalk's entry inventory.[^crosswalk]

## Leadership

The project page names Ken Huang as project lead and Fabio Cerullo as project co-leader.[^ast10] Huang is also named on the [[owasp-aivss|OWASP AIVSS]] leadership team; see [[ken-huang|Ken Huang]].

## Mapping to CSA MAESTRO

The project states that each AST10 risk is mapped to CSA MAESTRO's seven layers.[^ast10] The [[owasp-genai-crosswalk|GenAI Crosswalk]] corroborates the target framework: all 36 AST mappings recorded in that dataset target [[csa-maestro|CSA MAESTRO]] and no other framework.[^crosswalk] It does not corroborate a one-layer assignment per risk — its AST rows carry three to five MAESTRO control ids per entry.[^crosswalk]

## Maturity

This is a registered list with a published risk set and an unreviewed mapping layer. All 36 mappings against MAESTRO carry the note "DRAFT — SME review required."[^crosswalk] The crosswalk's classifier, which proposes candidate mappings for every other source list in the dataset, has produced no predictions for AST10 at all.[^crosswalk] The crosswalk's own README records the list's framework coverage as "registered; mappings pending."[^crosswalk]

The wiki carries no dated standards review of this list, and the [[standards-review-backlog|Standards Review Backlog]] records that as deliberate rather than outstanding: no wiki gap claim, maturity rung or reference-architecture control currently rests on an AST code.

## See Also

- [[mcp-security|MCP Security]] — the tool layer AST10 sits above.
- [[csa-maestro|CSA MAESTRO]] — the only framework AST10 currently maps to, and only in draft.
- [[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10]] — the agent-level sibling taxonomy; AST10 ranks the skill artifact rather than the agent.
- [[supply-chain-security-for-agents|Supply Chain Security for Agents]] — AST02 and AST08 anchor its pre-install scanning layer.
- [[owasp-genai-crosswalk|GenAI Crosswalk]] — the dataset carrying AST10's only published mappings.
- [[ken-huang|Ken Huang]] — project lead.
- [[owasp|OWASP]] — publishing organization.

## Notes

[^ast10]: OWASP, [Agentic Skills Top 10 (AST10) project page](https://owasp.org/www-project-agentic-skills-top-10/), retrieved 2026-09-16. States: "The OWASP Agentic Skills Top 10 (AST10) is the first comprehensive security framework for AI agent skills"; names project leads Ken Huang and Fabio Cerullo; states each AST10 risk maps to CSA MAESTRO's seven layers. Not independently corroborated by a second source.
[^crosswalk]: OWASP GenAI Security Project, *GenAI Crosswalk*, v4.0.0, data layer and README, captured 2026-09-16. `.raw/articles/owasp-genai-crosswalk-2026-09-16.md`, Parts A and C.

<!-- sources:auto -->
## Sources

- [OWASP Agentic Skills Top 10 (AST10) project page](https://owasp.org/www-project-agentic-skills-top-10/)
- [GenAI Crosswalk data layer and README (AST entry names, severities, MAESTRO mappings)](https://genai-security-project.github.io/crosswalk/)
<!-- /sources -->
