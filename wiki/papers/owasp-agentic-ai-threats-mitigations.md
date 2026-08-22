---
type: paper
title: "OWASP Agentic AI Threats and Mitigations"
address: c-000219
created: 2026-06-22
updated: 2026-08-17
tags:
  - papers
  - owasp
  - agentic-ai
  - threat-modeling
  - vulnerability-taxonomy
  - playbooks
status: summarized
scope_axis:
  - sec-of-ai
origin: aggregated
year: 2025
authors:
  - "OWASP GenAI Security Project (Agentic Security Initiative)"
publisher: "OWASP GenAI Security Project"
venue: "OWASP Top 10 for LLM Apps & Gen AI — Agentic Security Initiative"
publication_date: 2025-12
source_url: "https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/"
archived_copy: ".raw/papers/owasp-agentic-ai-threats-mitigations-v1.1.pdf"
no_public_url: ""
key_claim: "Agentic AI introduces seventeen threats — some new, some agentic variations of existing LLM risks — that map onto a single-agent reference architecture and resolve into six defensive playbooks."
methodology: "Threat-model-based reference over a single-agent and multi-agent reference architecture, drawing on prior NIST, CSA, academic, and vendor (Precize) work; threats organized by a decision-tree taxonomy navigator and worked through four example scenarios."
related:
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
  - "[[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]]"
  - "[[owasp-state-of-agentic-ai-security-governance|State of Agentic AI Security and Governance]]"
  - "[[owasp-aivss|OWASP AIVSS]]"
  - "[[owasp-llm-top-10|OWASP Top 10 for LLM Applications]]"
  - "[[owasp|OWASP]]"
  - "[[memory-poisoning|Memory Poisoning]]"
  - "[[tool-abuse-chains|Tool-Abuse Chains]]"
  - "[[least-agency-principle|Least Agency Principle]]"
  - "[[indirect-prompt-injection|Indirect Prompt Injection]]"
  - "[[mcp-security|MCP Security]]"
  - "[[non-human-identity|Non-Human Identity]]"
  - "[[csa-maestro|CSA MAESTRO]]"
  - "[[threat-modeling-for-ai|Threat Modeling for AI]]"
  - "[[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]]"
  - "[[hitl|Human-in-the-Loop]]"
sources:
  - ".raw/papers/owasp-agentic-ai-threats-mitigations-v1.1.pdf"
---

# OWASP Agentic AI Threats and Mitigations

**Source:** [OWASP GenAI Security Project — Agentic AI: Threats and Mitigations](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/) (v1.1, December 2025). Local copy: `.raw/papers/owasp-agentic-ai-threats-mitigations-v1.1.pdf` (md5 `c6615bfe74f00ec60b019332a8814a82`).

## Key Claim

Agentic AI introduces a bounded set of seventeen threats — some entirely new, some agentic variations of existing LLM risks — that can be mapped onto a reference agent architecture and resolved into six defensive playbooks of proactive, reactive, and detective controls.[^doc]

## Methodology

The document is a threat-model-based reference, not a control standard. It is the first guide from the OWASP Agentic Security Initiative (ASI), the same working group behind the [[owasp-agentic-ai-top-10|ASI Top 10]].[^doc] The method:

- Defines agent capabilities (planning/reasoning, memory/statefulness, action/tool use) and a single-agent plus multi-agent reference architecture used as the canvas for threat placement.
- Catalogs threats as a 17-row reference threat model (T1–T17), then re-organizes them through an "Agentic Threats Taxonomy Navigator" — a six-step decision tree keyed to agent properties (reasoning, memory, tools, authentication, human dependency, multi-agent).[^nav]
- Pairs each threat group with worked attack scenarios and maps the seventeen threats onto six mitigation playbooks.
- States it does not follow one methodology (STRIDE, PASTA, MAESTRO); it notes the layered [[csa-maestro|MAESTRO]] extension to STRIDE as the agentic option practitioners may adopt.[^maestro]
- Draws explicitly on prior work from NIST, CSA, academic research, and vendor efforts such as Precize.[^prior]

The document defers general (non-agentic) LLM threats to the existing OWASP guides and cross-references each agentic threat to the relevant [[owasp-llm-top-10|LLM Top 10 2025]] entry (e.g. LLM01 Prompt Injection, LLM03 Supply Chain, LLM06 Excessive Agency, LLM08 Vector and Embedding Weaknesses).[^llmmap] The [[owasp-ai-exchange|OWASP AI Exchange]] defers to this document in the other direction: its agentic section directs readers wanting agentic threat detail to "Agentic AI threats and mitigations, from the GenAI security project" ([`/go/agenticaioverview/`](https://owaspai.org/go/agenticaioverview/)), which establishes the T1–T17 catalog as the agentic detail layer beneath the Exchange's general asset-and-impact matrix.

## Reference Threat Model (T1–T17)

The T-code taxonomy is the catalog the published [[owasp-agentic-ai-top-10|ASI Top 10]] cross-maps each ASI category to (ASI Appendix; see the [[standards-review-owasp-agentic-aivss-2026-Q2\|2026-Q2 standards review]]).[^doc]

| TID | Threat | Core risk |
|---|---|---|
| T1 | Memory Poisoning | Corrupting short- or long-term memory to alter decisions; see [[memory-poisoning\|Memory Poisoning]] |
| T2 | Tool Misuse | Deceiving an agent into abusing authorized tools, incl. Agent Hijacking; see [[tool-abuse-chains\|Tool-Abuse Chains]] |
| T3 | Privilege Compromise | Exploiting permission management, dynamic role inheritance, or misconfiguration |
| T4 | Resource Overload | Exhausting compute, memory, or service quotas to degrade or fail the system |
| T5 | Cascading Hallucination Attacks | Plausible-but-false output propagating through memory, tools, or multi-agent loops |
| T6 | Intent Breaking and Goal Manipulation | Subverting planning, reasoning, and self-evaluation to redirect objectives |
| T7 | Misaligned and Deceptive Behaviors | Agent evades safety constraints via deceptive reasoning to reach a goal |
| T8 | Repudiation and Untraceability | Actions cannot be traced or attributed due to insufficient logging |
| T9 | Identity Spoofing and Impersonation | Impersonating agents, users, or services; theft of persistent agent identity |
| T10 | Overwhelming Human-in-the-Loop | Inducing decision fatigue or compromising the oversight interface; see [[hitl\|HITL]] |
| T11 | Unexpected RCE and Code Attacks | Abusing agent code-execution environments to run unauthorized code |
| T12 | Agent Communication Poisoning | Manipulating inter-agent channels to spread false information |
| T13 | Rogue Agents in Multi-Agent Systems | Compromised agents operating outside monitoring boundaries; "infectious backdoors" |
| T14 | Human Attacks on Multi-Agent Systems | Exploiting inter-agent delegation and trust to escalate privileges |
| T15 | Human Manipulation | Exploiting user trust in the agent to drive harmful human actions |
| T16 | Insecure Inter-Agent Protocol Abuse | Flaws in MCP / A2A — consent bypass, context hijacking; see [[mcp-security\|MCP Security]] |
| T17 | Supply Chain Compromise | Tampered models, libraries, tools, or build environments enter the agent |

[^tcodes]

> T16 and T17 are present in the T-code reference model and the per-threat navigator but are absent from the page-16 "Detailed Threat Model" diagram, which enumerates T1–T15. The two are added in the taxonomy navigator and the example threat models.

## Taxonomy Navigator (decision tree)

| Step | Question | Threat group |
|---|---|---|
| 1 | Does the agent independently determine its steps? | Agency/reasoning: T6, T7, T8 |
| 2 | Does the agent rely on stored memory? | Memory-based: T1, T5 |
| 3 | Does the agent execute via tools / commands / integrations? | Tool, execution, supply chain: T2, T3, T4, T11, T16, T17 |
| 4 | Does the system rely on authentication? | Authentication/spoofing: T9 |
| 5 | Does the agent require human engagement? | Human-related: T10, T15 |
| 6 | Does the system rely on multiple interacting agents? | Multi-agent: T12, T14, T13 |

[^nav]

## Mitigation Playbooks

Six playbooks group the controls; each carries proactive (prevention), reactive (response), and detective (monitoring) steps.[^playbooks]

| # | Playbook | Threats covered |
|---|---|---|
| 1 | Preventing AI Agent Reasoning Manipulation | T6, T8, T7 |
| 2 | Preventing Memory Poisoning and AI Knowledge Corruption | T1, T5 |
| 3 | Securing AI Tool Execution and Preventing Unauthorized Actions Across Supply Chains | T2, T3, T11, T4, T16, T17 |
| 4 | Strengthening Authentication, Identity and Privilege Controls | T3, T9, T16 |
| 5 | Protecting HITL and Preventing Decision-Fatigue Exploits | T10, T15 |
| 6 | Securing Multi-Agent Communication and Trust Mechanisms | T12, T14, T13 |

Recurring control primitives across playbooks: memory content validation and session isolation; least-privilege / RBAC-ABAC and just-in-time tool access; execution sandboxing with per-call reset; cryptographic and immutable logging; mutual authentication and short-lived credentials for agent identities; signed agent cards, prompt templates, and model definitions with verifiable SBOMs (AI SBOM / AIBOM / Agent SBOM); and message authentication for inter-agent channels.[^playbooks]

## Notable Findings

- The guide is the **source of the T1–T17 codes** that the published [[owasp-agentic-ai-top-10|ASI Top 10]] cross-maps to. The two documents are companions: the ASI Top 10 is the ranked awareness list, this guide is the underlying threat-and-mitigation reference.
- It is the first OWASP document to name **Insecure Inter-Agent Protocol Abuse (T16)** against MCP and A2A — consent-flow manipulation, MCP response injection, and tool-description exploitation — as a distinct threat. See [[mcp-security|MCP Security]].
- It anchors **Non-Human Identity** risk explicitly: agents operate under NHIs (machine accounts, service identities, agent API keys) that lack session-based oversight, raising privilege-misuse and token-abuse risk.[^nhi] See [[non-human-identity|Non-Human Identity]].
- It cites named real-world incidents as scenario anchors: the Amazon Q for VS Code destructive-prompt supply-chain incident (v1.84.0) and the Replit autonomous-agent database-deletion incident.[^incidents]
- It treats **Misaligned and Deceptive Behaviors (T7)** as distinct from hallucination — emergent from advanced reasoning, not random error — and cites Anthropic and OpenAI work on the area.[^deceptive]
- Four example threat models work the T-codes through concrete settings: Enterprise Co-Pilots, Agentic IoT smart-home cameras, and agent-driven RPA in an expense-reimbursement workflow.[^examples]

## Strengths and Weaknesses

Strengths:

- Architecture-anchored: every threat is placed on a reference diagram, which supports threat-modeling conversations with consistent language.
- The decision-tree navigator gives a repeatable path from agent properties to applicable threats — usable by builders, not only security specialists.
- Playbooks separate proactive/reactive/detective steps, which maps cleanly onto control-catalog and detection-engineering work.

Weaknesses:

- Awareness-and-guidance document, not a compliance standard: no `shall`-level requirements, no audit or evidence criteria, no maturity tiers. The same enforceability limit applies as to the [[owasp-agentic-ai-top-10|ASI Top 10]].
- Internal numbering is inconsistent: the page-16 diagram shows T1–T15 while the navigator and example models use T1–T17.
- Mitigations are control candidates rather than testable baselines; several ("probabilistic truth-checking", "AI-driven behavioral profiling") are stated without implementation detail.
- The Gartner "33% of enterprise software by 2028 / 15% of day-to-day decisions" figure is cited as an adoption claim without a primary link in the document.[^gartner]

## Relations

- Companion to [[owasp-agentic-ai-top-10|the ASI Top 10]]: this guide supplies the T1–T17 codes the ranked list maps each ASI category to.
- Supports [[memory-poisoning|Memory Poisoning]] (T1), [[tool-abuse-chains|Tool-Abuse Chains]] (T2), and [[mcp-security|MCP Security]] (T16) with worked scenarios and playbook controls.
- Reinforces [[least-agency-principle|Least Agency]] — the playbooks' least-privilege, JIT-tool-access, and scoped-memory controls are the operational form of the principle the ASI Top 10 names.
- Uses but does not adopt [[csa-maestro|CSA MAESTRO]]; the T-codes are the reference catalog of the [[threat-modeling-for-ai|threat-modeling spine]] and the row source for the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix (each T-code mapped to its ASI category, RA plane, and CMM domain).
- Scored and reconciled in [[standards-review-owasp-agentic-aivss-2026-Q2|the OWASP Agentic AI / AIVSS standards review (2026-Q2)]], which already treats the T-codes as the ASI cross-map target.
- Sibling deliverable from the same ASI working group: [[owasp-state-of-agentic-ai-security-governance|State of Agentic AI Security and Governance v2]], which applies this threat taxonomy to a governance maturity model and a documented 2025–2026 incident base.
- Referenced as a "see also" companion throughout [[owasp-asi-aiuc1-crosswalk|the OWASP ASI to AIUC-1 crosswalk]], whose ASI threat descriptions cite this guide for deeper technical context.
- The [[owasp-ai-exchange|OWASP AI Exchange]], OWASP's other flagship AI project, defers to this document for agentic threat detail rather than duplicating it, positioning the T1–T17 catalog beneath its own general asset-and-impact matrix.

[^doc]: OWASP GenAI Security Project, *Agentic AI – Threats and Mitigations*, v1.1, December 2025 — Introduction (p. 3) and Agentic AI Threat Model (pp. 12–15). `.raw/papers/owasp-agentic-ai-threats-mitigations-v1.1.pdf`.
[^nav]: Same source, "Agentic Threats Taxonomy Navigator" (pp. 21–32), six-step decision path.
[^maestro]: Same source, "Threat modeling approach" (p. 12): MAESTRO described as a layered STRIDE extension for agentic threats; the document declines to adopt a single methodology.
[^prior]: Same source, taxonomy provenance note (p. 19): "Our taxonomy draws from a wide range of prior work including work from NIST, CSA, academic research, industry work, and taxonomies developed by vendor-led efforts, such as Precize."
[^llmmap]: Same source, per-threat LLM Top 10 cross-references throughout the navigator (pp. 21–32).
[^tcodes]: Same source, "Detailed Threat Model" table (pp. 16–19) for T1–T15; T16 and T17 added on pp. 18–19.
[^playbooks]: Same source, "Mitigation Strategies" — playbook/threat mapping overview (pp. 34–35) and Playbooks 1–6 (pp. 35–42).
[^nhi]: Same source, Reference Threat Model — Non-Human Identities discussion (p. 14).
[^incidents]: Same source, Supply Chain Attacks scenarios (pp. 28–29): Amazon Q for VS Code v1.84.0 destructive prompt and the Replit vibe-coding database-deletion incident.
[^deceptive]: Same source, T7 Misaligned & Deceptive Behaviors (p. 17), citing published Anthropic and OpenAI work.
[^examples]: Same source, "Example Threat Models" (pp. 43–49): Enterprise Co-Pilots, Agentic IoT smart-home cameras, agent-driven RPA expense reimbursement.
[^gartner]: Same source, AI Agents section (p. 4): Gartner forecast that by 2028 33% of enterprise software applications will use agentic AI, enabling 15% of day-to-day work decisions to be made autonomously — cited without a primary-source link.
