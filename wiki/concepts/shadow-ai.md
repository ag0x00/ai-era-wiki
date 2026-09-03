---
type: concept
title: "Shadow AI"
created: 2026-05-01
updated: 2026-08-31
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
sources:
  - "[[.raw/articles/knostic-ai-data-security-2026-05-01.md]]"
---

# Shadow AI

**Shadow AI** is the use of unauthorized AI tools in the workplace: the AI-era counterpart of Shadow IT. The same dynamic (unsanctioned tooling adopted by individuals to solve real problems) but with a higher data-leakage blast radius because every interaction puts text or files into a third-party AI system.

## Scale

The 75% / 78% figures are typically cited via Knostic, but the **primary source is Microsoft's [Work Trend Index 2025](https://www.microsoft.com/en-us/worklab/work-trend-index)**. The wiki should cite Microsoft directly rather than the downstream vendor. Independently corroborated:

- **[[stanford-hai|Stanford HAI AI Index 2025]]**: 78% business AI adoption (up from 55% in 2023); methodology disclosed; cross-year comparable
- **McKinsey State of AI (Nov 2025)**: 88% of orgs use AI in ≥1 function; only 6% are "high performers" (n=1,993, 105 countries)
- **BCG "Build for the Future 2025"**: 72% regular use; **only 13% have AI agents in production** vs 56% experimenting; the agentic-vs-GenAI gap is load-bearing
- **IBM Cost of a Data Breach 2025**: 20% of orgs experienced shadow-AI breaches; cost \$670K above average

Directional agreement: rate is high enough that "block all unsanctioned AI" is impractical. The agentic deployment lag (BCG's 13% in production) is the wiki's CMM "L1 Initial" anchor.

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
- **Discovery via [[ai-spm|AI-SPM]].** Inventory production-grade AI assets, including locally installed MCP servers and IDE extensions. Cyera Endpoint, described in [[cyera-agent-guardian-release|the Cyera Agent Guardian release]], is a vendor example of this discovery extended to desktop agent harnesses, naming Claude Code, Cowork and ChatGPT Desktop among them, and not only IDE extensions.
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
