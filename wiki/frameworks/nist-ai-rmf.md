---
type: framework
title: "NIST AI Risk Management Framework (AI RMF)"
created: 2026-04-30
updated: 2026-09-14
tags:
  - frameworks
  - nist
  - risk-management
  - ai-governance
status: developing
source_url: "https://www.nist.gov/itl/ai-risk-management-framework"
scope_axis:
  - sec-of-ai
adoption_signal: maintained
last_substantive_update: 2024-07-26
published_by: "[[nist|NIST]]"
current_version: "1.0 (January 2023); AI 600-1 GenAI Profile (July 2024)"
first_published: "2023-01-26"
doi: 10.6028/NIST.AI.100-1
primary_documents:
  - title: "NIST AI 100-1 — Artificial Intelligence Risk Management Framework (AI RMF 1.0)"
    url: "https://doi.org/10.6028/NIST.AI.100-1"
    version: "1.0"
    published: "2023-01-26"
    retrieved: "2026-06-21"
    archived_copy: ".raw/papers/nist-ai-100-1-rmf-2023-01-26.pdf"
    scope_in_wiki: "Core functions GOVERN/MAP/MEASURE/MANAGE (Tables 1–4); foundational §3 trustworthiness characteristics"
scope: "Voluntary U.S. standard for managing AI risks across the full AI lifecycle"
audience: "Enterprise AI developers, deployers, operators; federal agencies"
aliases:
  - "AI RMF"
  - "NIST AI RMF 1.0"
related:
  - "[[shadow-automation|Shadow Automation]]"
  - "[[agent-catalog|AI Agent Catalog]]"
  - "[[decision-rights]]"
  - "[[nist-ai-600-1]]"
  - "[[nist-sp-800-218a]]"
  - "[[mitre-atlas]]"
  - "[[iso-iec-42001]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[nist-ai-800-4]]"
  - "[[nist-ir-8596-cyber-ai-profile]]"
  - "[[standards-review-nist-ai-rmf-2026-Q2]]"
  - "[[standards-review-iso-42001-27090-2026-Q2]]"
  - "[[stride-ai-2026]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
coined_by:
  - "[[nist]]"
---

# NIST AI Risk Management Framework (AI RMF)

The **NIST AI RMF** is the de facto voluntary U.S. standard for AI risk management, structured around four core functions: **Govern, Map, Measure, and Manage**. NIST published version 1.0 on 26 January 2023 and has not revised it.[^rmf] Two enacted state statutes give it force in U.S. law as of September 2026. The Texas Responsible Artificial Intelligence Governance Act (TRAIGA, HB 149), effective 1 January 2026, makes substantial compliance with the AI RMF or a comparable recognized standard an affirmative defense in an enforcement action.[^traiga] Montana's Right to Compute Act requires the deployer of an AI system that controls critical infrastructure to write a risk management policy that considers the latest AI RMF, the ISO/IEC artificial intelligence standard, or another recognized framework.[^mt] Two further state paths to an affirmative defense have closed. Virginia's HB 2094, which would have granted a rebuttable presumption of conformity to systems aligned with the AI RMF or ISO/IEC 42001, was vetoed on 24 March 2025.[^va] Colorado repealed SB 24-205 and reenacted it as SB 26-189 on 14 May 2026, dropping both the NIST-aligned risk-management program and the affirmative defense attached to it.[^co]

## Structure

- **Govern**: Establish organizational accountability, culture, and processes for AI risk
- **Map**: Identify and categorize AI risks in context
- **Measure**: Analyze and assess identified risks
- **Manage**: Prioritize and respond to identified AI risks

The **Generative AI Profile (NIST AI 600-1)**, published 26 July 2024, adds twelve GenAI risk categories and roughly 200 suggested actions, and names prompt injection, data poisoning, and model extraction among the risks in scope. It frames each as something to assess or red-team. [[standards-review-nist-ai-rmf-2026-Q2|The 2026-Q2 standards review]] read the full action set and found prompt injection in one place, the red-teaming action `MS-2.7-007`, with no action prescribing a mitigation for it.

## Status as of September 2026

AI RMF 1.0 remains the published version. NIST lists it as in revision under the White House AI Action Plan and names neither a version number nor a date.[^rmf] The work that has moved since sits in profiles and control overlays alongside 1.0, and none of it has reached an initial public draft.

### AI RMF Profile for Trustworthy AI in Critical Infrastructure

NIST released a concept note in April 2026 and is assembling a community of interest before it drafts. The profile would give critical-infrastructure operators a sector-specific set of risk practices and a vocabulary for stating trustworthiness requirements across an AI supply chain. The project page carries no draft and no completion date.[^ciprofile]

### COSAiS control overlays (IR 8605 series)

COSAiS adapts SP 800-53 controls to five AI use cases: generative AI assistant / LLM, predictive AI use and fine-tuning, single-agent systems, multi-agent systems, and security controls for AI developers.[^cosais-uc] One annotated outline has been released — predictive AI, 8 January 2026, with feedback due 13 February 2026 — and no overlay has reached initial public draft.[^cosais] NIST's own project pages carry no report number; the series is reported as `IR 8605`, with `IR 8605A` the predictive-AI overlay and `IR 8605B` and `IR 8605C` the generative and agentic ones.[^8605] The two agent use cases would be the first NIST control text written against agentic systems.

### Cyber AI Profile (NIST IR 8596)

IR 8596 stands at initial *preliminary* draft, published 16 December 2025 with comments closed 30 January 2026.[^8596] NIST ran virtual working sessions on 28 April, 5 May, and 12 May 2026, and published summary reports for the first two Cyber AI workshops as IR 8578 and IR 8607 on 3 August 2026.[^wkshop] The initial public draft has not appeared. The profile applies CSF 2.0 across three focus areas; detail is on [[nist-ir-8596-cyber-ai-profile|NIST IR 8596 Cyber AI Profile]].

### CAISI AI Agent Standards Initiative

CAISI, NIST's Center for AI Standards and Innovation, launched the AI Agent Standards Initiative on 17 February 2026. It is the first U.S. government program aimed at agentic AI interoperability and security standards. NIST states three pillars: facilitating industry-led standards, fostering community-led protocols with NSF funding open-source development through its Pathways program, and investing in research on agent authentication and identity infrastructure.[^caisi] Both intake channels have since closed — the RFI on AI agent security on 9 March 2026, and comments on the NCCoE (National Cybersecurity Center of Excellence) concept paper on software and AI agent identity and authorization on 2 April 2026 — and NIST has published nothing from either. The initiative page, last updated 14 August 2026, names sector listening sessions for healthcare, finance, and education as the next activity.[^caisi]

### Related NIST reports

**NIST AI 800-4** (6 March 2026) maps the gaps, barriers, and open questions in post-deployment AI monitoring across six monitoring categories. It records a wider range of human-factors challenges than for any other category and reads that as a signal that "human-factors monitoring is relatively underexplored."[^ai8004] It is descriptive and carries no normative clauses — see [[nist-ai-800-4|NIST AI 800-4: Monitoring Challenges]]. **NIST AI 800-3** (February 2026) covers statistical models for AI evaluation.[^ai800]

## Strengths

- The CAISI initiative and the NCCoE identity concept paper scope agent authentication and delegation as federal standards work, which is the gap the wiki's `D2` domain fills from other instruments
- COSAiS is the first attempt to give AI-specific SP 800-53 control overlays, including two scoped to agent systems
- The Cyber AI Profile bridges AI RMF and CSF 2.0, so an organization already on CSF can extend coverage to AI without a second framework
- TRAIGA's affirmative defense and Montana's critical-infrastructure policy requirement give the framework a concrete legal consequence in two U.S. states

## Gaps and Shortcomings

- Describes **"what" rather than "how"**: no testable control requirements with evidence criteria
- Does not distinguish model development from runtime security
- Agentic AI-specific controls are acknowledged as a gap; the COSAiS agent overlays are the pending answer and remain unpublished
- MCP/A2A protocol security, plugin/skill supply chains, agent identity management, and cognitive file integrity are unaddressed in published text
- No AI incident response specificity (IoCs, playbooks, forensic guidance)
- ML-BOM/AI-BOM requirements absent
- Platform-level vs. prompt-level enforcement distinction not articulated
- GOVERN presumes an enumerable inventory. [[shadow-automation|Shadow automation]] is the direct negation of that premise: an agent adopted by an individual developer has no documented owner, no risk assessment, and no place in the register the function is written against. Returning an unenumerated deployment to the register depends on discovery machinery the framework leaves to the implementer, which leaves its first function inapplicable in exactly the environment where agentic risk accumulates fastest. The [[agent-catalog|AI Agent Catalog]] supplies the per-agent form the inventory requirement lacks: an agent card carrying a unique identity, the tools and data in scope, and an attributed owner, which is what makes a GOVERN inventory claim auditable one agent at a time.
- GOVERN calls for clear lines of accountability over AI systems and does not say what an accountability record for an autonomous action contains. [[decision-rights|Decision rights for AI agents]] supplies that artifact — action class, decision right, approver, justification, time bound — the concrete filing the function asks for.

## Coverage Against OWASP ASI Top 10

| ASI Category | Coverage |
|---|---|
| ASI01: Agent Goal Hijack | ○ None |
| ASI02: Tool Misuse | ○ None |
| ASI03: Identity & Privilege | ○ None |
| ASI04: Supply Chain | ◐ Partial |
| ASI05: Unexpected Code Execution (RCE) | ○ None |
| ASI06: Memory Poisoning | ○ None |
| ASI07: Insecure Inter-Agent | ○ None |
| ASI08: Cascading Failures | ○ None |
| ASI09: Human-Agent Trust Exploitation | ● AI 600-1 Human-AI Configuration (over-reliance, automation bias) |
| ASI10: Rogue Agents | ○ None |

## Watch items

- **COSAiS single-agent and multi-agent overlays**: the first NIST control text scoped to agentic systems; no public draft as of September 2026
- **COSAiS generative AI overlay**: annotated outline not yet released
- **COSAiS AI-developer security-controls overlay**: annotated outline not yet released
- **NIST IR 8596 initial public draft**: the preliminary draft's comment period closed in January 2026 and the next draft has not appeared
- **Critical Infrastructure profile**: concept note only; community of interest forming
- **RMF revision (1.1 or 2.0)**: confirmed in revision, no draft and no announced timeline

## See Also

- [[nist|NIST]] (publisher)
- [[nist-ai-600-1|NIST AI 600-1]] — GenAI profile
- [[nist-ai-800-4|NIST AI 800-4]] — post-deployment monitoring challenges
- [[nist-ir-8596-cyber-ai-profile|NIST IR 8596 Cyber AI Profile]] — the CSF 2.0 side of the same stack
- [[nist-sp-800-218a|NIST SP 800-218A]] — Secure Software Development Framework for AI
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — anchors **D1 Governance** to AI RMF's Govern function; per-domain crosswalk in [[agentic-ai-security-cmm-crosswalk|Agentic AI Security CMM — Standards Crosswalk Matrix]]
- [[iso-iec-42001|ISO/IEC 42001 — AI Management Systems]] — the certifiable complement; its governance-only / no-agentic-technical-control profile is mapped in [[standards-review-iso-42001-27090-2026-Q2|the 2026-Q2 ISO/IEC 42001 + 27090 review]]
- [[mitre-atlas|MITRE ATLAS]] — adversary technique coverage that NIST lacks
- [[stride-ai-2026|STRIDE-AI Threat Modeling Framework]] — academic threat-modeling method that proposes bridging AI RMF governance to the [[owasp-llm-top-10|OWASP LLM Top 10]] technical taxonomy via an AI-adapted STRIDE

[^rmf]: [NIST — AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework), retrieved 2026-09-14. AI RMF 1.0 (NIST AI 100-1) published 2023-01-26; the page states the framework "is being revised as part of the White House AI Action Plan" and names no version number or completion date.

[^traiga]: Baker Botts, [*Texas Enacts Responsible AI Governance Act: What Companies Need to Know*](https://www.bakerbotts.com/thought-leadership/publications/2025/july/texas-enacts-responsible-ai-governance-act-what-companies-need-to-know), July 2025. HB 149 signed 2025-06-22, effective 2026-01-01; the safe-harbor list includes entities that "substantially comply with the NIST AI Risk Management Framework or other recognized standards."

[^mt]: Montana Code Annotated, [2-10-205 — Infrastructure controlled by critical artificial intelligence system](https://mca.legmt.gov/bills/mca/title_0020/chapter_0100/part_0020/section_0050/0020-0100-0020-0050.html), enacted by SB 212 (Ch. 150, L. 2025), retrieved 2026-09-14. The deployer "shall develop a risk management policy after deploying the system that is reasonable and considers guidance and standards in the latest version of the artificial intelligence risk management framework from the national institute of standards and technology, the ISO/IEC 4200 artificial intelligence standard from the international organization for standardization, or another nationally or internationally recognized risk management framework for artificial intelligence systems."

[^va]: CSIS, [*After the Virginia AI Bill Was Vetoed, What's Next for State-Level AI Legislation?*](https://www.csis.org/analysis/after-virginia-ai-bill-was-vetoed-whats-next-state-level-ai-legislation), 2025. HB 2094 passed 2025-02-19 and was vetoed by Governor Glenn Youngkin on 2025-03-24; it would have presumed conformity for high-risk systems aligned with the NIST framework, ISO/IEC 42001, or an equivalent.

[^co]: Colorado General Assembly, [SB26-189 — Automated Decision-Making Technology](https://leg.colorado.gov/bills/sb26-189): repeals and reenacts the SB 24-205 provisions, signed 2026-05-14, effective 2027-01-01. Buchalter, [*Colorado Rewrites Its AI Law: What Employers Must Know About SB 26-189*](https://www.buchalter.com/insights/colorado-rewrites-its-ai-law-what-employers-must-know-about-sb-26-189/), 2026: "No mandatory risk management program aligned to NIST AI RMF or ISO 42001."

[^ciprofile]: NIST, [Concept Note — AI RMF Profile on Trustworthy AI in Critical Infrastructure](https://www.nist.gov/programs-projects/concept-note-ai-rmf-profile-trustworthy-ai-critical-infrastructure), project created 2026-04-06, page updated 2026-07-17, status ongoing.

[^cosais-uc]: NIST CSRC, [COSAiS Use Cases](https://csrc.nist.gov/Projects/cosais/use-cases), retrieved 2026-09-14.

[^cosais]: NIST CSRC, [Control Overlays for Securing AI Systems (COSAiS)](https://csrc.nist.gov/Projects/cosais), retrieved 2026-09-14. Concept paper 2025-08-14; predictive-AI annotated outline discussion draft 2026-01-08 with feedback due 2026-02-13.

[^8605]: [[ai-security-standards-in-q1-2026|AI Security Standards: Agentic Threats Outpace Frameworks]], which names `IR 8605A` for the January 2026 predictive-AI outline and `IR 8605B` / `IR 8605C` for the planned generative and agentic overlays. The [COSAiS project page](https://csrc.nist.gov/Projects/cosais), its publications tab, and the August 2025 concept paper carry no IR number, all retrieved 2026-09-14. Local copy: `.raw/papers/ai-security-standards-in-q1-2026.md`.

[^8596]: NIST CSRC, [NIST IR 8596 (Initial Preliminary Draft) — Cybersecurity Framework Profile for Artificial Intelligence](https://csrc.nist.gov/pubs/ir/8596/iprd), published 2025-12-16, comments closed 2026-01-30; spring 2026 virtual working sessions listed for 28 April, 5 May, and 12 May.

[^wkshop]: NIST CSRC, [NIST IR 8607 — Workshop Summary Report for "Cyber AI Profile" Hybrid Workshop #2](https://csrc.nist.gov/pubs/ir/8607/final), August 2026; companion IR 8578 covers workshop #1. Both published 2026-08-03.

[^caisi]: NIST, [AI Agent Standards Initiative](https://www.nist.gov/artificial-intelligence/ai-agent-standards-initiative), page created 2026-02-17, last updated 2026-08-14. Carries the three pillars, the RFI and concept-paper deadlines, and the planned sector listening sessions.

[^ai8004]: NIST, [*Challenges to the monitoring of deployed AI systems*](https://doi.org/10.6028/NIST.AI.800-4) (NIST AI 800-4), [published 2026-03-06](https://www.nist.gov/publications/challenges-monitoring-deployed-ai-systems-center-ai-standards-and-innovation), §3: the wider range of human-factors challenges "when coupled with the larger proportion of workshop quotes in comparison to the literature shown in Figure 1, could signal that human-factors monitoring is relatively underexplored (Section 3.2.3)." The report ranks no category and does not use the phrase "blind spot." Local copy: `.raw/papers/nist-ai-800-4-monitoring-deployed-ai-2026-03.pdf`.

[^ai800]: NIST, [*Expanding the AI Evaluation Toolbox with Statistical Models*](https://www.nist.gov/publications/expanding-ai-evaluation-toolbox-statistical-models) (NIST AI 800-3), 17 February 2026.

<!-- sources:auto -->
## Sources

- [NIST AI Risk Management Framework (AI RMF)](https://www.nist.gov/itl/ai-risk-management-framework)
- [NIST AI 100-1 — Artificial Intelligence Risk Management Framework (AI RMF 1.0)](https://doi.org/10.6028/NIST.AI.100-1)
<!-- /sources -->
