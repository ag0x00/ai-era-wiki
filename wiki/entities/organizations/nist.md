---
type: entity
title: "NIST"
created: 2026-04-30
updated: 2026-09-14
tags:
  - entities
  - organizations
  - standards-body
  - us-government
status: active
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
  - sec-against-ai
entity_type: organization
org_type: advisory
homepage: "https://www.nist.gov"
role: "U.S. federal standards body; primary author of AI RMF and AI 600-1; leads CAISI agentic AI standards initiative"
related:
  - "[[nist-ai-rmf]]"
  - "[[nist-ai-600-1]]"
  - "[[nist-ssdf]]"
  - "[[nist-sp-800-218a]]"
  - "[[nist-sp-800-162]]"
  - "[[nist-ai-800-4]]"
  - "[[nist-ir-8596-cyber-ai-profile]]"
  - "[[apostol-vassilev]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - "[[.raw/papers/nist-sp-800-218.pdf]]"
  - "[[.raw/papers/nist-sp-800-218A.pdf]]"
verified: 2026-09-14
verified_against:
  - ".raw/papers/ai-security-standards-in-q1-2026.md"
verified_findings: 1
verified_note: "1 finding: the AI Security Role paragraph framed the AI RMF's statutory footing as affirmative defenses only, so Montana's Right to Compute Act (MCA 2-10-205) was absent; corrected to name both statutes. Publications table, CAISI pillars, and all 2026 dates check out against nist.gov and csrc.nist.gov. SP 800-218 / 218A PDFs not read this pass."
---

# NIST — National Institute of Standards and Technology

**Sources:** [NIST (homepage)](https://www.nist.gov) · [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) · [CAISI](https://www.nist.gov/caisi)

**NIST** (National Institute of Standards and Technology) is the U.S. federal agency responsible for technology standards, housed within the Department of Commerce. In AI security, NIST is the primary author of the [[nist-ai-rmf|AI Risk Management Framework]] (AI RMF 1.0, 2023) and its GenAI companion [[nist-ai-600-1|NIST AI 600-1]] (July 2024).

## AI Security Role

NIST's voluntary frameworks carry de facto authority because federal agency compliance is expected and state legislatures draft against them. Two state statutes name the AI RMF: Texas TRAIGA grants an affirmative defense for substantial compliance with it, and Montana's Right to Compute Act requires a risk management policy that considers it wherever an AI system controls critical infrastructure. The affirmative-defense channel narrowed during 2026, as Colorado dropped its equivalent defense in the May 2026 rewrite of its AI act and Virginia's 2025 bill was vetoed. NIST publishes "what" rather than "how": descriptive guidance rather than implementation prescriptions.

## AI Activity Through 2026

- **CAISI (Center for AI Standards and Innovation) AI Agent Standards Initiative** (February 17, 2026): first U.S. government program explicitly targeting agentic AI interoperability and security standards. Three pillars: facilitating industry-led standards, fostering community-led protocols, and investing in research on agent authentication and identity infrastructure. The RFI on AI agent security closed March 9, 2026 and comments on the NCCoE concept paper on software and AI agent identity and authorization closed April 2, 2026; no output from either has been published as of September 2026
- **COSAiS control overlays** (January 8, 2026): annotated outline for the predictive-AI SP 800-53 overlay, feedback due February 13, 2026. Overlays for generative AI, single-agent systems, multi-agent systems, and AI developers remain unreleased
- **NIST AI 800-4** (March 6, 2026): post-deployment AI monitoring gap analysis
- **AI RMF Profile for Trustworthy AI in Critical Infrastructure** (April 2026): concept note; community of interest forming, no draft
- **NIST IR 8596 (Cyber AI Profile)**: initial preliminary draft of December 16, 2025; comments closed January 30, 2026 and the initial public draft has not appeared. Workshop summary reports IR 8578 and IR 8607 published August 3, 2026

Dates and statuses above are cited on [[nist-ai-rmf|NIST AI Risk Management Framework (AI RMF)]].

## Key Publications

| Publication | Date | Description |
|---|---|---|
| AI RMF 1.0 | January 2023 | Core risk management framework |
| NIST AI 600-1 | July 2024 | Generative AI profile |
| NIST IR 8596 (iprd) | December 2025 | Cyber AI Profile, CSF 2.0 for AI |
| COSAiS annotated outline | January 2026 | Predictive AI SP 800-53 control overlay |
| NIST AI 800-3 | February 2026 | Statistical models for AI evaluation |
| NIST AI 800-4 | March 2026 | Post-deployment monitoring gaps |
| NIST IR 8578 / 8607 | August 2026 | Cyber AI Profile workshop summary reports |

## Frameworks Published

- [[nist-ai-rmf|NIST AI RMF]]: de facto voluntary U.S. AI security standard
- [[nist-ai-600-1|NIST AI 600-1]]: GenAI profile of the AI RMF
- [[nist-ssdf|NIST SSDF (SP 800-218 v1.1)]]: Secure Software Development Framework (Feb 2022); federal regulatory anchor under EO 14028 and OMB M-22-18
- [[nist-sp-800-218a|NIST SP 800-218A]]: SSDF Community Profile for Generative AI and Dual-Use Foundation Models (July 2024); federal AI-specific extension of SSDF authorized by EO 14110 § 4.1.a
- [[nist-ai-800-4|NIST AI 800-4]]: descriptive CAISI report on the gaps in monitoring deployed AI systems (March 2026); no normative clauses
- [[nist-ir-8596-cyber-ai-profile|NIST IR 8596]]: Cybersecurity Framework Profile for AI, at initial preliminary draft; Secure / Defend / Thwart focus areas on the CSF 2.0 Core
- [[nist-sp-800-162|NIST SP 800-162]]: Guide to Attribute Based Access Control (ABAC); the wiki's preferred living-standard citation for the four-role (PEP / PDP / PIP / PAP) vocabulary

## Personnel surfaced on the wiki

- [[apostol-vassilev|Apostol Vassilev]]: Computer Security Division; co-author of [[nist-sp-800-218a|SP 800-218A]] and lead author of NIST AI 100-2e2023 (Adversarial ML Taxonomy)
