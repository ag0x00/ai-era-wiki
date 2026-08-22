---
type: framework
title: "Red Teaming Capability Framework"
address: c-000211
created: 2026-04-30
updated: 2026-08-21
tags:
  - frameworks
  - red-teaming
  - capability-framework
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
origin: produced
no_public_url: "Wiki-produced internal proposal (2026); no external publication exists"
adoption_signal: active
last_substantive_update: 2026-06-11
published_by: ""
current_version: "internal proposal 2026"
first_published: 2026
scope: "Layered red-teaming capability for first-party agentic AI"
audience: "Red team leads, security architects"
related:
  - "[[cybersecurity-cmms-exemplars|Cybersecurity CMM Exemplars]]"
  - "[[clasp]]"
  - "[[ai-security-larsen-effect-talk]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-reference-architecture]]"
sources:
  - "[[.raw/reports/red-teaming-project/00-readme.md]]"
  - "[[.raw/reports/red-teaming-project/01-gap-analysis.md]]"
  - "[[.raw/reports/red-teaming-project/03-summary-opinion.md]]"
---

# Red Teaming Capability Framework

The Red Teaming Capability Framework is an internal proposal for structuring a red-teaming services capability for first-party agentic AI. It organizes the work into five tiers, each answering one operational question, from the governance constraints that authorize a test down to the evaluation of outside vendors. Its governing principle is that a capability is what a team can demonstrate, not the set of standards it can name.

**A standard named is not a capability owned.** Each tier below is defined by a question it must answer and by the evidence that answers it; the named standards are reference inputs to that work, not substitutes for it. A team that can cite every framework in Tier 2 but cannot produce an attack transcript, a coverage matrix, or a deterministic replay holds no Tier 2 capability.

## The five tiers

Each tier states the question it answers, the capability it requires, and the standards that inform it. The standards are inputs; the capability is the deliverable.

### Tier 1 — Foundational standards (governance and risk)

*Why, and under what constraints, do we test?* Governance, authorization, scope boundaries, safety norms, and business-impact framing.

Reference inputs: [[nist-ai-rmf|NIST AI RMF 1.0]], [[nist-ai-600-1|NIST AI 600-1]], and [[nist-sp-800-218a|NIST SP 800-218A]]; [[iso-iec-42001|ISO/IEC 42001]] and ISO/IEC 23894; the [[eu-ai-act|EU AI Act]], whose high-risk obligations apply from August 2, 2026 (subject to a possible Digital Omnibus delay); DORA for the financial sector; the NIST/CAISI RFI on securing AI agent systems.

### Tier 2 — Threat taxonomies and attack libraries (what to test)

*What do we test?* Threat coverage across the model, agent, skill, and protocol layers.

Reference inputs by layer:

- **Model layer** — [[owasp-llm-top-10|OWASP LLM Top 10 (2025)]].
- **Agent reasoning and orchestration** — [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]].
- **Skill and behavior execution** — OWASP Agentic Skills Top 10 (AST01–AST10). This is an OWASP project in active development, not a ratified Top 10; treat its coverage as provisional.[^ast]
- **Protocol and infrastructure** — an MCP Top 10. No standalone OWASP list is ratified; the most concrete instantiation is Microsoft's Azure-scoped "OWASP MCP Top 10 for Azure" mapping.[^mcp] See [[mcp-security|MCP security]].
- **Cross-cutting techniques** — [[mitre-atlas|MITRE ATLAS]].
- **Cross-layer model** — [[csa-maestro|CSA MAESTRO]] for tracing a threat across the layers above.

### Tier 3 — Methodology and scoring (how to test)

*How do we test, and how do we judge the result?* Scenario design, non-determinism handling, evidence standards, metrics, severity models, and human validation.

Reference inputs: the OWASP GenAI Red Teaming Guide and the CSA Agentic AI Red Teaming Guide for method; [[owasp-aivss|OWASP AIVSS]] (v0.8, pre-1.0) for severity scoring; [[google-saif|Google SAIF]] and [[cosai|CoSAI]] principles as the secure-by-design anchor. A numeric severity scale answers *how severe* only when paired with calibration of *what each score requires* — see [Critical appraisal](#critical-appraisal).

### Tier 4 — Continuous operations (when and how often to test)

*When, and how often, do we test?* Regression testing, drift detection, observability, and safe re-testing.

Required capability, stated independent of any product: continuous and automated red teaming integrated into CI/CD and MLOps pipelines; behavioral baseline monitoring with runtime anomaly detection; scan-on-change triggers for model updates, tool additions, and prompt modifications; and ingestion of current campaign intelligence, such as Snyk's ToxicSkills audit of the agent-skill registry[^toxic] and the [[clawhavoc|ClawHavoc]] skill-marketplace compromise. Automation augments human adversaries; it does not replace them, and emergent or cross-agent behavior still requires [[hitl|human review]].

### Tier 5 — Vendor evaluation and compliance reporting

*How do we evaluate others, and how do we demonstrate compliance?* RFIs, proof-of-concept gates, evidence standards, and compliance mapping.

Reference inputs: the OWASP Vendor Evaluation Criteria for AI Red Teaming Providers and Tooling v1.0; compliance mapping to the EU AI Act, DORA, HIPAA, and NIST AI RMF; and AI-BOM / SBOM requirements per CycloneDX. The buyer-side counterpart to this tier is treated in [[ai-security-larsen-effect-talk|The AI Security Larsen Effect]].

## Criteria for a capability tier

The tiers describe coverage; the following rules are what convert coverage into a defensible capability, and they are the part the standards lists alone do not supply.

- **Stable capability IDs.** Every capability carries a fixed identifier (for example, `T3.MET.04`). Assets — questionnaires, response tables, decks, training material — map to capabilities; an asset that references no capability is out of scope.
- **Evidence is mandatory for any capability claimed as primary.** Acceptable evidence is a threat model, a coverage matrix, an attack transcript or trace, a deterministic replay artifact, a scoring output, a proof-of-concept result, or an authorization artifact. Marketing language, context-free screenshots, "we can build it" statements, and one-off demos without logs do not count. A capability that cannot be evidenced must not be claimed as primary.
- **Named anti-patterns are prohibited.** Claiming full coverage without evidence, conflating jailbreak demos with systemic red teaming, and selling capabilities that cannot be staffed or evidenced are explicit failure modes the framework exists to prevent.

## Critical appraisal

The framework's weaknesses are structural, not in its choice of standards. The standards selection is current and well-targeted; the risks lie in how the framework is read and in what it asserts without showing.

**Inventory read as capability.** Presented as five lists of standards, the framework invites the exact failure its own governing rules prohibit: treating an enumeration of frameworks as proof of capability. The evidence rules above are the corrective, and they belong at the front of the framework rather than as an appendix to the lists.

> [!contradiction] Coverage list vs. evidence rule
> A tier rendered as a bullet list of standards reads as "we cover this." The framework's own rule is that coverage is unproven until a threat model, transcript, replay, or score exists. The lists and the evidence requirement must be read together, or the framework contradicts itself.

**Maturity is not flat.** The tiers place ratified, load-bearing standards (NIST AI RMF 1.0, ISO/IEC 42001, MITRE ATLAS, OWASP LLM Top 10) alongside pre-1.0 and in-flight artifacts (the AST proposal, AIVSS v0.8, the MCP list as a vendor-scoped mapping). Citing all of them at equal authority overstates what can currently anchor an evidence-defensible claim. The skill-layer and protocol-layer taxonomies will move as they ratify, and any coverage claim built on them moves with them.

**Automation does not replace adversaries.** Tier 4's continuous-platform language risks the documented evaluation pitfall of treating automation as a substitute for experienced red teamers. The framework's own operating rule is augmentation: automation supplies scale, repeatability, and regression coverage, while human testers supply novelty and the discovery of emergent and cross-agent behavior. Tier 4 should name the human-to-automation division explicitly rather than leave it implied.

**A scale without calibration is false precision.** A 0–5 rubric or an AIVSS score produces consistent-looking numbers only when each level is calibrated — when the evidence that distinguishes an exemplary finding from an adequate one is written down per item. Without that calibration, inter-rater reliability is low and a precise-looking score conceals subjective judgment. This is the gap an evaluator most easily misses, because the number appears objective.

> [!gap] Grounding is argued, not measured
> The framework asserts the five-tier partition as what a 2026 capability "should be founded on" without comparative evidence that this partition outperforms alternatives, and without adoption data. It is a defensible reasoned proposal; its authority is argumentative, not empirical. A later revision should cite engagement outcomes or external adoption to move it from proposal to evidenced standard.

## Mapping to the Agentic AI Security CMM and Reference Architecture

The five tiers align to the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] domains and the [[agentic-ai-security-reference-architecture|reference architecture]] as follows. See the [[agentic-ai-security-cmm-crosswalk|CMM crosswalk]] for control-level detail.

- **Tier 1** maps to CMM D1 (Governance & Accountability).
- **Tier 2** maps to D4 (Runtime & Guardrails) and D6 (Data, Memory & RAG), and to the threat surface of the reference architecture.
- **Tier 3** maps to D7 (Observability & Detection) and the CMM measurement protocol.
- **Tier 4** maps to D9 (Operations & Human Factors).
- **Tier 5** maps to D8 (Supply Chain & AI-BOM) and vendor procurement.

## Open questions and caveats

> [!gap] Empirical basis for the partition
> No evidence yet establishes that the five-tier split is better than a three-phase or domain-based alternative, nor that all five tiers are load-bearing for every engagement. The partition is reasoned, not measured.

> [!gap] Immature lower-layer taxonomies
> The skill-layer (AST) and protocol-layer (MCP) taxonomies are pre-ratification. Coverage claims that depend on them are provisional and should be re-checked against the published lists when they land.

## See also

- [[ai-security-larsen-effect-talk|The AI Security Larsen Effect]] — the buyer-side counterpart: a capability-based configure/buy/build vendor framework.
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] — the maturity model the tiers map to.
- [[mcp-security|MCP security]] — the protocol-layer threat surface named in Tier 2.

- [[cybersecurity-cmms-exemplars|Cybersecurity CMM exemplars]] — places this framework alongside [[clasp|CLASP]] as one of the narrower maturity models targeting a specific agentic-AI capability rather than a whole program.

## Notes

[^ast]: [OWASP — Agentic Skills Top 10 Project Proposal](https://owasp.org/www-project-agentic-skills-top-10/proposal.html), 2026. OWASP project in active development (v1.0, 2026 edition), covering agent-skill supply-chain risks (AST01–AST10). Not a ratified OWASP Top 10 at time of writing.
[^mcp]: [Microsoft — OWASP MCP Top 10 for Azure, MCP03 Tool Poisoning](https://microsoft.github.io/mcp-azure-security-guide/mcp/mcp03-tool-poisoning/), 2026. The most concrete instantiation of an MCP Top 10 is Microsoft's Azure-scoped mapping, not a standalone ratified OWASP list.
[^toxic]: [Snyk — ToxicSkills: Malicious AI Agent Skills in ClawHub](https://snyk.io/blog/toxicskills-malicious-ai-agent-skills-clawhub/), 2026. First broad security audit of the agent-skill ecosystem (ClawHub and skills.sh); coined "ToxicSkills" for skills that read as benign but act maliciously when executed by a capable agent.
