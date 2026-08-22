---
type: paper
title: "Secure AI Framework Approach"
address: c-000017
created: 2026-05-07
updated: 2026-06-22
tags:
  - papers
  - google
  - saif
  - implementation-guide
  - ai-security
  - lifecycle
status: summarized
publisher: "Google (Safer with Google)"
authoring_organization: "[[google|Google]]"
publication_date: "2024 (no precise date in document)"
source_url: "https://safety.google/cybersecurity-advancements/saif/"
related:
  - "[[google-saif|Google SAIF]]"
  - "[[google|Google]]"
  - "[[cosai|CoSAI]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]"
  - "[[standards-review-saif-cosai-2026-Q2|Google SAIF and CoSAI standards review]]"
sources:
  - "[[.raw/papers/saif-implementation-guide.pdf]]"
---

# Secure AI Framework Approach — Implementation Guide (Source Summary)

Google's 11-page practitioner-facing implementation guide for [[google-saif|SAIF]] (the Secure AI Framework). Published under the "Safer with Google" imprint as a quick-start companion to the broader SAIF framework documentation. The guide is methodology-heavy: rather than enumerating threats or controls, it gives a four-step adoption pathway and breaks SAIF down into six core elements.

The six core elements summarized below are the *how* of SAIF; they pair with the SAIF Map's *what* — the 15 risks, 24 controls across six categories, and 14 components verified in [[standards-review-saif-cosai-2026-Q2|the Google SAIF and CoSAI standards review]] (2026-06-22) and catalogued on [[google-saif|the SAIF framework page]].

## Document structure

The guide organizes into two halves: a four-step methodology for putting SAIF into practice, then per-element guidance for the six core SAIF elements.

### Four-step methodology

1. **Understand the use** — characterize the specific business problem AI will solve, the data needed, the user-interaction model, and whether you are building / fine-tuning / using a third-party model. Different combinations imply different security requirements (e.g., consumer-finance models face different obligations than internal analyst-summary models).
2. **Assemble the team** — multidisciplinary stakeholder list: Business use case owners; Security; Cloud Engineering; Risk and Audit; Privacy; Legal; Data Science; Development; Responsible AI and Ethics.
3. **Level set with an AI primer** — bring all stakeholders to a baseline understanding of AI / ML / Deep Learning / Gen AI / LLMs, the model development lifecycle, and the relevant capabilities, merits, and limitations.
4. **Apply the six core elements of SAIF** — the framework's substantive content; not chronological, treat as collectively-applied levers.

### Six core elements

These are the SAIF "approach elements" — six practitioner-facing levers, distinct from (and complementary to) the SAIF Risk Map's data / infrastructure / model / application **layers** that the [[google-saif|framework page]] previously documented.

| # | Element | Substantive guidance |
|---|---|---|
| **1** | **Expand strong security foundations to the AI ecosystem** | Review what existing security controls apply; evaluate fit for AI; analyze gaps; prepare to track supply-chain assets, code, training data; ensure data governance and lifecycle management scale to AI; **retain and retrain** existing talent rather than only hire externally. |
| **2** | **Extend detection and response to bring AI into an organization's threat universe** | Develop understanding of threats specific to your AI usage; prepare to respond to attacks against AI **and** issues raised by AI output; for Gen AI, prepare to enforce **content safety policies**; adjust abuse policy + IR processes for AI-specific incident types (malicious content creation, AI privacy violations, AI bias). |
| **3** | **Automate defenses to keep pace with existing and new threats** | Identify AI security capabilities (training-data protection, model protection); use AI defenses to counter AI threats while keeping humans in the loop for important decisions; use AI to automate time-consuming defensive tasks (e.g., AI-assisted YARA rule generation from malware reverse-engineering). |
| **4** | **Harmonize platform-level controls to ensure consistent security across the organization** | Periodic review of AI usage and lifecycle; **prevent fragmentation of controls** by standardizing on tooling and frameworks; right-fit existing control frameworks to AI rather than create new parallel ones. |
| **5** | **Adapt controls to adjust mitigations and create faster feedback loops for AI deployment** | Conduct Red Team exercises; stay current on **prompt injection, data poisoning, evasion attacks**; apply ML to improve detection accuracy / speed; **create a feedback loop** so Red Team findings and discovered attack vectors flow back into training data + protections. |
| **6** | **Contextualize AI system risks in surrounding business processes** | Establish a **model risk management framework**; build inventory of AI models with risk profiles; implement data-privacy / cyber-risk / third-party-risk policies through the ML model lifecycle; risk assessment that considers organizational AI use; **shared responsibility for securing AI** (developer / deployer / user splits); **match AI use cases to risk tolerances** (healthcare or finance vs marketing or customer service). |

## Six concern categories (Intro framing)

The guide's introduction organizes SAIF concerns into four meta-categories spanning sixteen concrete sub-concerns:

- **Security** — access management; network/endpoint security; application/product security; supply-chain attacks; data security; AI-specific threats; threat detection and response.
- **AI/ML model risk management** — model transparency and accountability; error-prone manual reviews for detecting anomalies; data poisoning; data lineage, retention, and governance controls.
- **Privacy and compliance** — data privacy and usage of sensitive data; emerging regulations.
- **People and organization** — talent gap; governance / board reporting.

## Six decision domains for data governance

Element 1 ("Expand strong security foundations") names **six decision domains for data governance** — the practitioner taxonomy the guide expects organizations to scale into AI:

1. Data quality
2. Data security
3. Data architecture
4. Metadata
5. Data lifecycle
6. Data storage

The guide expects these domains to be reviewed cross-functionally and adjusted to reflect AI-specific implications.

## Conceptual contributions surfaced (relative to the wiki)

These are angles the implementation guide treats explicitly that the wiki had not previously framed under the SAIF banner:

- **AI shared-responsibility model** — developer / deployer / user split for AI security accountability. Adjacent to the wiki's [[non-human-identity|NHI]] / [[agent-identity-architecture|agent identity architecture]] coverage but framed as a *responsibility* model rather than an *identity* model. Not severe enough to warrant a new concept page; the SAIF-page extension documents it inline.
- **Cross-functional team composition for AI security** — explicit stakeholder list (9 roles). The wiki's [[ai-agent-layered-council|AI Agent Layered Council]] (Gartner) names a similar cross-functional body but at the *governance* level (CIO + CFO + COO + GC + Procurement + CHRO); SAIF's list is at the *implementation team* level (more operational). Worth tracking the parallel between the two.
- **Six decision domains for data governance** — referenced inline above; adjacent to the wiki's [[differential-privacy|differential privacy]] / D6 coverage but at the *governance taxonomy* level rather than the *control mechanism* level.
- **"Retain and retrain" talent strategy** — institutional-knowledge-vs-AI-knowledge tradeoff. Not in the wiki; the guide makes the case that internal cross-training is faster than hiring external AI talent given how long institutional context takes to acquire.

## Relevance to this corpus

- **The six core elements anchor SAIF as a methodology** — the wiki's existing [[google-saif|framework page]] focused on the SAIF *content* (Risk Map layers, Secure Agents principles, A2A Protocol, ADK Go 1.0). This guide adds the *adoption methodology* layer the framework page was missing. The framework page is now extended with both the six elements and the four-step adoption pathway.
- **Practitioner positioning** — SAIF-as-a-set-of-principles vs SAIF-as-an-implementation-program is an important distinction; this guide is squarely in the second category and the wiki's coverage is now bilingual.
- **Cross-walk to wiki frameworks** — the six core elements have natural mappings to the wiki's [[agentic-ai-security-cmm-2026|CMM]] domains and the [[agentic-ai-security-reference-architecture|RA]] planes; the framework-page extension adds the crosswalk table.

## Limitations

- **Pre-Gen-AI framing.** The guide reads as written before agentic AI as a category gained traction (~2023-2024 timing). It mentions Gen AI explicitly but does not address agent-specific concerns (multi-agent orchestration, [[lethal-trifecta|Lethal Trifecta]], [[mcp-security|MCP]] / [[a2a-protocol|A2A]] protocols, [[promptware|promptware]], [[non-human-identity|NHI]] specifics). Subsequent SAIF content (the [[a2a-protocol|A2A Protocol]] and [[google-saif|ADK Go 1.0]]) addresses these surfaces; this guide does not.
- **No threat enumeration.** Unlike [[csa-maestro|CSA MAESTRO]] (which threat-models per layer) or the wiki's [[agentic-ai-security-cmm-2026|CMM]] (which cites specific MITRE ATLAS techniques), this guide names threat *categories* (prompt injection, data poisoning, evasion) without specific technique-level references.
- **No version / publication date.** The PDF carries no internal date; SAIF content evolved through 2024 and was donated to [[cosai|CoSAI]] in 2024. Cite it as "Google SAIF Implementation Guide, ca. 2024" with the donation-to-CoSAI status.
- **No tables or diagrams.** The guide is prose-only; no visual reference architecture or control matrix. Practitioners often want a table-of-controls; this guide leaves that as an exercise for the reader.

## See also

The full structural treatment of SAIF — Risk Map layers, Secure Agents principles, ADK Go 1.0, A2A Protocol, CoSAI continuity — lives at [[google-saif|the SAIF framework page]], now extended with the implementation guide's six core elements and four-step methodology. This source summary is provenance.
