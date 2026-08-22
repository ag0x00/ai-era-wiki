---
type: framework
title: "EU AI Act"
created: 2026-04-30
updated: 2026-08-17
tags:
  - frameworks
  - regulation
  - eu
  - compliance
  - high-risk-ai
status: developing
source_url: "https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng"
scope_axis:
  - sec-of-ai
adoption_signal: active
last_substantive_update: 2024-07-12
published_by: "European Commission / EU"
current_version: "Regulation (EU) 2024/1689; enforcement phased through 2026-2027"
first_published: "2024-07-12"
scope: "Binding EU regulation establishing risk-based obligations for AI systems; primary compliance pressure point for enterprise AI in Europe"
audience: "Enterprises deploying AI in the EU, compliance teams, vendors selling into EU markets"
aliases:
  - "AI Act"
  - "EU AI Regulation"
related:
  - "[[owasp-ai-exchange]]"
  - "[[iso-iec-42001]]"
  - "[[nist-ai-rmf]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[owasp-state-of-agentic-ai-security-governance]]"
  - "[[standards-review-eu-ai-act-2026-Q2]]"
  - "[[standards-review-iso-42001-27090-2026-Q2]]"
primary_documents:
  - "Regulation (EU) 2024/1689 — consolidated text: https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng"
  - "Article-by-article + Annex IV rendering: https://artificialintelligenceact.eu/"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# EU AI Act

The **EU AI Act** (Regulation (EU) 2024/1689, published July 12, 2024) is the world's first comprehensive binding legal framework for AI. It establishes risk-based obligations: prohibited AI systems, high-risk AI system requirements, and transparency obligations for general-purpose AI models.

## Enforcement Timeline

| Date | Obligation |
|---|---|
| **August 2, 2025** | GPAI model transparency obligations; prohibited AI bans |
| **August 2, 2026** | High-risk AI system obligations (Article 11, Annex IV documentation, etc.) — **the primary enterprise deadline** |
| **Potentially December 2027** | If Digital Omnibus delay passes (see below) |

## Q1 2026 Status

The **August 2, 2026 deadline** for high-risk AI system obligations is approaching, but the situation is complex:

**Digital Omnibus Delay Proposal:** The European Commission's Digital Omnibus package (November 2025) proposes delaying enforcement of high-risk AI obligations by up to **16 months**. If passed:
- Stand-alone AI systems: enforcement shifts from August 2026 to December 2027
- Embedded AI systems: enforcement shifts to August 2028

**Legislative progress** (as of Q1 2026):
- Council agreed negotiating mandate: March 13, 2026
- IMCO/LIBE committees adopted joint position: March 18, 2026 (101-9-8 vote)
- Trilogue negotiations pending

**Readiness gap:** Only **8 of 27 EU Member States** are assessed as ready for full enforcement.

**Harmonized standards:** CEN and CENELEC missed their 2025 deadline for harmonized technical standards (required for AI Act compliance demonstration). Now targeting end of 2026. The relevant standards being developed are prEN 18282 and ETSI prEN 304 223; these do not directly map to ISO 42001.

One contributor to prEN 18282 is identifiable and quantified. The [[owasp-ai-exchange|OWASP AI Exchange]] states it contributed 70 pages to prEN 18282 through official liaison partnership, and that its founder [[rob-van-der-veer|Rob van der Veer]] works on the AI Act security standard at CEN/CENELEC, where the EU member states elected him co-editor.[^aix-liaison] The same body states a 70-page contribution to ISO/IEC 27090, so the two instruments share content despite being formally separate.

## Key Requirements (High-Risk Systems)

- **Article 11 + Annex IV**: Technical documentation requirements effectively mandate AI-BOM-like disclosures: training data, validation data, model architecture, intended purpose, performance metrics, risk management system documentation
- **Article 9**: Risk management system throughout lifecycle
- **Article 10**: Data governance requirements
- **Article 12**: Logging and record-keeping
- **Article 14**: Human oversight measures
- **Article 50**: Transparency obligations for GPAI models (applies August 2026 regardless of Omnibus delay)

## AI-BOM Implications

EU AI Act Article 11 and Annex IV effectively mandate AI-BOM-like documentation for high-risk systems. However, without a ratified AI-BOM standard, compliance interpretations will vary. This creates near-term demand for AI-BOM tooling even in the absence of a formal standard.

## Position Among Peer Regimes

[[owasp-state-of-agentic-ai-security-governance|OWASP's State of Agentic AI Security and Governance]] places the EU AI Act inside a multi-jurisdiction survey of agentic-relevant regulation, which sets its obligations against peers an enterprise must satisfy in parallel. For agentic deployments the report reads the Act through three articles: Article 14 human oversight, Article 72 post-market monitoring (the locus for behavioral-drift monitoring of a deployed agent), and Article 25 value-chain liability. It positions these alongside sectoral and extra-EU regimes — DORA and NIS2 incident-reporting clocks, GDPR Article 22 as a floor on autonomous consequential decisions, and U.S. state safety-incident-reporting laws — and argues that the compressed reporting windows across these regimes assume continuous oversight rather than periodic review, which is the operational link to the governance-maturity model the report builds.

The [[owasp-ai-exchange|OWASP AI Exchange]] reads the Act's compliance model as outcome-based, in contrast to the control-focused model of ISO/IEC 27001, and draws the operational consequence that an information security management system extended to AI needs assurance processes demonstrating that risks were sufficiently mitigated rather than an inventory showing controls exist ([`/go/organize/`](https://owaspai.org/go/organize/)). That reading matches the [[standards-review-eu-ai-act-2026-Q2|2026-Q2 standards review]]'s finding that the Act states outcomes rather than technical controls. It also raises a problem for agentic deployments that the article-by-article view does not surface: a regime that assumes every workflow is describable before deployment meets a compositional agentic system — many tools, multi-step chaining, an execution-path space too large to enumerate — that cannot supply the description Annex IV point 2 assumes, and needs an explicit program handling instead of a workflow list ([`/go/agenticaioverview/`](https://owaspai.org/go/agenticaioverview/)).

## Compliance Pathways

- **ISO/IEC 42001** certification: positioned as primary compliance pathway, but harmonized standards (prEN 18282) are distinct from ISO 42001 and still in development
- **NIST AI RMF alignment**: acceptable for non-EU markets but does not satisfy EU Act requirements directly
- **CEN/CENELEC harmonized standards**: when finalized (end 2026), will provide presumption of conformity

## Watch Items (2026)

- **Digital Omnibus trilogue outcome**: determines whether August 2026 or December 2027 deadline applies
- **CEN/CENELEC harmonized standards**: targeting end 2026; critical for certification pathway
- **National AI Authority establishment** across EU Member States
- **AI Sandbox requirements**: applicable August 2, 2026 regardless of Omnibus delay
- **Article 50 transparency obligations**: apply August 2026

## See Also

- [[iso-iec-42001|ISO/IEC 42001 — AI Management Systems]] — primary certifiable compliance pathway for EU AI Act; its governance-only profile is mapped in [[standards-review-iso-42001-27090-2026-Q2|the 2026-Q2 ISO/IEC 42001 + 27090 review]]
- [[nist-ai-rmf|NIST AI Risk Management Framework (AI RMF)]] — U.S. voluntary counterpart; alignment gap with EU requirements
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — Art. 9 Risk Mgmt → **D1**; Art. 10 Data → **D6**; Art. 11 + Annex IV (technical documentation) → **D8** (full Annex IV item-by-item map in [[agentic-ai-security-cmm-crosswalk|Agentic AI Security CMM — Standards Crosswalk Matrix]]); Art. 12 Logging → **D7 + D9**; Art. 14 Human oversight → **D3 + D9**; Art. 15 Cybersecurity → **D4 + D5**; Art. 72 Post-market monitoring → **D9**
- [[standards-review-eu-ai-act-2026-Q2|EU AI Act Standards Review (2026-Q2)]] — verifies every article and Annex IV point this page cites against the primary source; confirms the Act is outcome-stated risk-management and conformity-assessment law, not an agentic technical-control standard

## Notes

[^aix-liaison]: OWASP AI Exchange, ["About the AI Exchange"](https://owaspai.org/go/about/), retrieved 2026-08-17. The Exchange states 70 pages contributed to prEN 18282 and 70 pages to ISO/IEC 27090 through official liaison partnership, plus contribution to ISO/IEC 27091. These are the source's own claims and are not independently verified here.

<!-- sources:auto -->
## Sources

- [EU AI Act](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng)
- [artificialintelligenceact.eu](https://artificialintelligenceact.eu/)
<!-- /sources -->
