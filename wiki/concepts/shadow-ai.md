---
type: concept
title: "Shadow AI"
created: 2026-05-01
updated: 2026-10-21
tags:
  - concepts
  - shadow-ai
  - byo-ai
  - governance
  - ai-data-security
status: developing
scope_axis:
  - sec-of-ai
complexity: basic
domain: ai-data-security
aliases:
  - "Shadow AI"
  - "BYOAI"
  - "Bring-Your-Own-AI"
  - "Unsanctioned AI"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[ai-data-security]]"
  - "[[oversharing-controls]]"
  - "[[ai-spm]]"
  - "[[non-human-identity]]"
  - "[[mcp-security]]"
  - "[[cyera-agent-guardian-release]]"
  - "[[ai-dfir-toolkit]]"
sources:
  - "[[.raw/articles/knostic-ai-data-security-2026-05-01.md]]"
verified: 2026-10-21
verified_against:
  - ".raw/articles/ai-dfir-toolkit-2026-10-21.md"
  - ".raw/articles/ai-dfir-toolkit-artifact-catalog-2026-10-21.md"
  - ".raw/articles/knostic-ai-data-security-2026-05-01.md"
verified_findings: 0
verified_note: "pass-inserted text verified; Scale figures re-sourced to live deep links and corrected; Knostic grepped for 75/78 only; older sources not reopened"
---

# Shadow AI

**Shadow AI** is the use of unauthorized AI tools in the workplace: the AI-era counterpart of Shadow IT. The same dynamic (unsanctioned tooling adopted by individuals to solve real problems) but with a higher data-leakage blast radius because every interaction puts text or files into a third-party AI system.

## Scale

The 75% and 78% figures that the [[ai-data-security|AI Data Security]] article from Knostic repeats come from Microsoft's [2024 Work Trend Index Annual Report](https://www.microsoft.com/en-us/worklab/work-trend-index/ai-at-work-is-here-now-comes-the-hard-part): 75% of knowledge workers use AI at work, and 78% of AI users bring their own AI tools to work. Independent surveys agree on the direction:

- **[[stanford-hai|Stanford HAI]] AI Index 2025**: 78% of organizations reported using AI in 2024, up from 55% the year before ([AI Index 2025, takeaway 3](https://hai.stanford.edu/ai-index/2025-ai-index-report)).
- **McKinsey, *The state of AI in 2025* (November 2025)**: 88% of respondents report regular AI use in at least one business function, from 1,993 participants in 105 nations ([McKinsey survey](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai-2025)).
- **McKinsey AI high performers**: about 6% of respondents report both an EBIT impact of 5% or more from AI and significant value from it ([McKinsey survey](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai-2025)).
- **BCG, *AI at Work 2025* (June 2025)**: 72% of respondents use AI regularly, and 13% say AI agents are integrated into their workflows ([BCG press release](https://www.bcg.com/press/26june2025-beyond-ai-adoption-full-potential)).
- **IBM Cost of a Data Breach 2025**: one in five organizations (20%) reported a breach due to shadow AI, and organizations with high levels of shadow AI saw average breach costs \$670,000 higher than those with little or none ([IBM press release](https://newsroom.ibm.com/2025-07-30-ibm-report-13-of-organizations-reported-breaches-of-ai-models-or-applications,-97-of-which-reported-lacking-proper-ai-access-controls)).

Use is widespread enough that blocking all unsanctioned AI is impractical. Agent deployment trails general use: 13% of BCG's respondents report agents integrated into their workflows against 72% who use AI regularly ([BCG press release](https://www.bcg.com/press/26june2025-beyond-ai-adoption-full-potential)).

See [[source-triangulation-audit-2026-05-02|Source Triangulation Audit 2026-05-02]] §Claim 2 for full triangulation.

## Canonical Incident

The **Samsung incident** (early 2023): engineers pasted proprietary chip-design code into ChatGPT for debugging help. The data left the corporate boundary and (per OpenAI's then-policy) became eligible for model improvement. Samsung subsequently restricted external GenAI use enterprise-wide. This incident is now the canonical Shadow AI cautionary tale and is referenced across vendor and academic materials.

## Risk Profile

Shadow AI extends standard Shadow IT risks plus:

| Risk | Shadow IT | Shadow AI |
|---|---|---|
| Unauthorized vendor relationship | ✓ | ✓ |
| Unaccounted data egress | ✓ | ✓ + likely enriched (extracted, summarized, transformed) |
| Compliance gap (GDPR, HIPAA, etc.) | ✓ | ✓ + harder to remediate (model training is irreversible) |
| Security review bypass | ✓ | ✓ + AI-specific issues ([[prompt-injection\|prompt injection]], hallucination, vector poisoning) |
| Vendor lock-in / continuity risk | ✓ | ✓ |
| **Inference exposure** ([[inference-exposure\|Inference Exposure (and Retrieval Exposure)]]) | ✗ | ✓ (unique to AI) |
| **Training-data contamination** | ✗ | ✓ (corporate IP enters base models) |
| **Cross-customer leakage** | ✗ | ✓ (early incidents, e.g. ChatGPT outage 2023, showed cross-session exposure) |

## Mitigation

The Knostic article frames mitigation as a triad:

1. **Governance policies**: sanctioned AI list with clear escalation path for adding new tools
2. **Usage monitoring**: DLP / network telemetry / browser-extension visibility into AI tool usage
3. **Employee training**: what is and is not safe to put into an AI tool, with concrete examples

Mature implementations add:

- **Sanctioned alternatives provided proactively.** If users have a sanctioned tool that does what they need, BYOAI rates drop.
- **Discovery via [[ai-spm|AI-SPM]].** Inventory production-grade AI assets, including locally installed MCP servers and IDE extensions. Cyera Endpoint, described in [[cyera-agent-guardian-release|the Cyera Agent Guardian release]], is a vendor example of this discovery extended to desktop agent harnesses, naming Claude Code, Cowork and ChatGPT Desktop among them, and not only IDE extensions. The open [[ai-dfir-toolkit|depalmar/ai-dfir-toolkit]] catalog publishes install, configuration and credential paths for [51 AI tools](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/artifacts/README.md#contents) as a collection-target list, with an osquery pack that answers which hosts run a given tool; its README states that an agent's presence on an endpoint is not an incident.
- **Knowledge-layer controls** ([[oversharing-controls|Oversharing Controls for AI Search]]) to limit damage when sanctioned AI is misused for unsanctioned data.

## Distinction from Sanctioned AI

The bright-line rule: **sanctioned ≠ safe.** Microsoft Copilot, Glean, and Gemini are all *sanctioned* in many enterprises and still drive significant oversharing risk. Shadow AI is the unsanctioned-tool problem; oversharing is the sanctioned-tool problem. Both apply.

## See Also

- [[ai-data-security|AI Data Security (Knostic blog, 2026)]]: primary source
- [[oversharing-controls|Oversharing Controls for AI Search]]: sanctioned-AI counterpart problem
- [[ai-spm|AI Security Posture Management (AI-SPM)]]: inventory discipline that surfaces shadow assets
- [[mcp-security|MCP Security]]: locally installed MCP servers are a Shadow AI surface
- [[non-human-identity|Non-Human Identity (NHI)]]: unsanctioned AI agents are unsanctioned non-human identities
- [[owasp-state-of-agentic-ai-security-governance|State of Agentic AI Security and Governance v2]]: treats Shadow AI as adoption tier AT0, the baseline state every organization must discover before governing deliberate deployments

<!-- sources:auto -->
## Sources

- [knostic.ai](https://www.knostic.ai/blog/ai-data-security)
<!-- /sources -->
