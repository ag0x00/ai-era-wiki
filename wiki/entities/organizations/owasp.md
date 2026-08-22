---
type: entity
title: "OWASP"
created: 2026-04-30
updated: 2026-08-18
tags:
  - entities
  - organizations
  - standards-body
  - open-source
status: active
entity_type: organization
org_type: advisory
homepage: "https://owasp.org"
role: "Non-profit producing open-source security guidance; most prolific AI security taxonomy producer in Q1 2026"
related:
  - "[[owasp-ai-exchange]]"
  - "[[owasp-llm-top-10]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[owasp-aivss]]"
  - "[[owasp-asi-aiuc1-crosswalk]]"
  - "[[standards-review-owasp-llm-top-10-2026-Q2]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# OWASP — Open Worldwide Application Security Project

**Sources:** [OWASP (homepage)](https://owasp.org) · [OWASP GenAI Security Project](https://genai.owasp.org) · [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/)

**OWASP** (Open Worldwide Application Security Project) is the leading open-source, community-driven security organization. In AI security, OWASP has become the de facto reference taxonomy producer — having published more substantive AI security content in Q1 2026 than any other framework organization.

## AI Security Role

OWASP publishes awareness frameworks and risk taxonomies rather than certifiable compliance standards, with one qualification that has grown material. The [[owasp-ai-exchange|OWASP AI Exchange]] states that it feeds normative standards directly through official liaison partnership, contributing 70 pages to prEN 18282 (cybersecurity for the EU AI Act), 70 pages to ISO/IEC 27090 (AI security), and further material to ISO/IEC 27091 (AI privacy).[^aix-liaison] The output remains non-certifiable in OWASP's own hands; the route to enforceability runs through the standards bodies OWASP contributes to. OWASP's strength beyond that liaison route is community development (100+ experts for the ASI Top 10), vendor adoption, and the speed with which it can codify emerging threat patterns.

OWASP runs **two flagship AI projects**. The [[owasp-ai-exchange|AI Exchange]] is the comprehensive threat-and-control framework covering all AI — agentic, analytical, discriminative, generative, and heuristic — and privacy alongside security; it was awarded flagship status in March 2025. The GenAI Security Project is the umbrella for generative-AI deliverables including the LLM Top 10, the ASI Top 10, and AIVSS. A third project, the OWASP AISVS (AI Security Verification Standard), supplies a three-level verification checklist aligned to ASVS and is the artifact the Exchange directs auditors to ([`/go/aiatowasp/`](https://owaspai.org/go/aiatowasp/)).

## Q1 2026 Activity

OWASP had the most productive AI security quarter of any framework organization:

- **OWASP Top 10 for Agentic Applications** (ASI Top 10) — published December 9, 2025 at the Agentic AI Security Summit, London; adopted by Microsoft, Palo Alto Networks, Auth0, Gravitee in Q1 2026
- **AIVSS v0.8** (March 19, 2026) — first AI-specific vulnerability scoring system extending CVSS 4.0
- **Practical Guide for Secure MCP Server Development** (February 16, 2026)
- **GenAI Security Project announcement** at RSAC 2026 (March 19) — updated Landscape Guide, GenAI Data Security Risks report, Agentic AI Security Solutions Landscape
- **OWASP AIBOM Generator** — CycloneDX-format AI bills of materials for Hugging Face-hosted models

**GenAI Security Project now has 25,000+ members** with new sponsors including F5, Fujitsu, and Apiiro.

## Notable Sponsor M&A Activity (Q1 2026)

Five OWASP sponsor alumni were acquired by major security vendors:
- Pangea → CrowdStrike
- Lakera → Check Point
- Prompt Security → SentinelOne
- Calypso AI → F5
- SPLX → Zscaler

## AI Security Frameworks Published

| Framework | Status |
|---|---|
| [[owasp-llm-top-10\|LLM Top 10 2025]] | Active; unchanged Q1 2026; codes verified by [[standards-review-owasp-llm-top-10-2026-Q2\|the LLM Top 10 review]] |
| [[owasp-agentic-ai-top-10\|Agentic Applications Top 10]] | Active; published Dec 2025 |
| ML Security Top 10 v0.3 | Dormant |
| [[owasp-aivss\|AIVSS v0.8]] | Active; community review |
| [[owasp-state-of-agentic-ai-security-governance\|State of Agentic AI Security and Governance v2]] | Active; published Jun 2026 |
| AIBOM Generator | Active |
| MCP Security Guide | Active (Feb 2026) |
| [[owasp-ai-exchange\|AI Exchange]] | Active; OWASP Flagship since March 2025; 170+ authors; feeds prEN 18282 and ISO/IEC 27090 per the Exchange's own liaison claim, unverified[^aix-liaison] |
| OWASP AISVS | Active; three verification levels aligned to ASVS |

## Cross-Org Strategic Briefings

- [[mythos-ready-briefing|*The "AI Vulnerability Storm": Building a "Mythos-ready" Security Program* (v1.0, 2026-04-12)]] — OWASP **Gen AI Security Project** co-published with [[csa|CSA CISO Community]], [[sans-institute|SANS]], and [[unprompted-conference-march-2026|Unprompted]]. Risk Register cross-walked to OWASP LLM Top 10 2025 + OWASP Agentic Top 10 2026 + MITRE ATLAS + NIST CSF 2.0 + CSA AI Control Matrix v1.0.3. Operational instrument: [[mythos-ready-security-program|Mythos-ready Security Program (Playbook)]].

## Notes

[^aix-liaison]: OWASP AI Exchange, ["About the AI Exchange"](https://owaspai.org/go/about/), retrieved 2026-08-17. The Exchange states 70 pages contributed to prEN 18282 and 70 pages to ISO/IEC 27090 through official liaison partnership, plus contribution to ISO/IEC 27091. These are the source's own claims and are not independently verified here.
