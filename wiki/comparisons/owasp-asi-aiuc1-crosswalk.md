---
type: comparison
title: "OWASP ASI to AIUC-1 Crosswalk"
address: c-000222
created: 2026-06-22
updated: 2026-06-22
tags:
  - comparisons
  - crosswalk
  - standards-mapping
  - owasp
  - aiuc-1
  - agentic-ai
status: developing
origin: aggregated
scope_axis:
  - sec-of-ai
source_url: "https://genai.owasp.org"
archived_copy: ".raw/papers/owasp-agentic-top10-aiuc1-crosswalk-2026-05.pdf"
related:
  - "[[owasp-agentic-ai-top-10]]"
  - "[[aiuc-1]]"
  - "[[owasp-aivss]]"
  - "[[aiuc-1-critical-evaluation]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[owasp-state-of-agentic-ai-security-governance]]"
  - "[[owasp]]"
  - "[[aiuc]]"
sources:
  - "[[.raw/papers/owasp-agentic-top10-aiuc1-crosswalk-2026-05.pdf]]"
---

# OWASP ASI to AIUC-1 Crosswalk

A bidirectional mapping between the [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]] threat taxonomy and the [[aiuc-1|AIUC-1]] certification requirements, published by the [[owasp|OWASP]] GenAI Security Project's Agentic Security Initiative in collaboration with [[aiuc|AIUC]].[^src] The two artifacts cover different sides of the same problem: ASI names ten agentic threat classes; AIUC-1 specifies certification requirements organized under six principles. This crosswalk connects threats to the controls that address them, in both directions, and records where AIUC-1 has no requirement for a threat the ASI prevention guidelines treat as essential.

**Source.** *AIUC-1: Crosswalks OWASP Top 10 For Agentic Applications*, v1.0, May 2026, OWASP GenAI Security Project (Agentic Security Initiative), CC BY-SA 4.0.[^src] Archived: `.raw/papers/owasp-agentic-top10-aiuc1-crosswalk-2026-05.pdf`.

**The crosswalk uses the published ASI labels.** ASI05 is "Unexpected Code Execution" (RCE) and ASI09 is "Human-Agent Trust Exploitation," matching the published 2026 edition recorded in [[standards-review-owasp-agentic-aivss-2026-Q2|the 2026-Q2 standards review]]. The pre-release draft labels (`ASI05` "Sensitive Data Disclosure", `ASI09` "Missing Guardrails") do not appear in this document.

## Mapping method

Each mapping is **Primary** (directly mitigates the core risk) or **Secondary** (addresses a related consequence or supplies a supporting control).[^src] Primary versus Secondary is set by threat context, not by control type: preventive and scope-constraining controls tend to be Primary; detective and governance controls tend to be Secondary, except where detection is the only way a persistent, cross-session, or multi-agent threat becomes visible (logging is Primary for ASI06 memory poisoning, Secondary for ASI01).

Each mapping also carries a **rationale code** from a controlled taxonomy of eight control functions.[^src]

| Code | Label | Function |
|---|---|---|
| PREV | Prevent | Blocks the core attack mechanism before it succeeds |
| SCOPE | Constrain scope | Limits what a compromised or misbehaving agent can reach |
| GATE | Human gate | Enforces a required human approval, review, or intervention point |
| DETECT | Detect and trace | Runtime detection, behavioral monitoring, or forensic traceability |
| VALID | Validate and test | Tests or audits that other controls function against the threat |
| GOVERN | Policy and governance | Organizational policy, accountability, or response-plan framework |
| ISOLATE | Isolate and contain | Architectural separation that stops cross-agent or cross-tenant propagation |
| DISCLOSE | Disclose and calibrate | Transparency or provenance that lets humans calibrate trust |

The document states the crosswalk identifies relevance, not sufficiency: a Primary mapping means the requirement addresses the core risk, not that it fully mitigates it.[^src]

## ASI threat to AIUC-1 requirement (Part B)

Each ASI category and the AIUC-1 requirements that address it. Counts are Primary / Secondary mappings drawn from the master table.[^src]

| ASI | Threat | Primary AIUC-1 requirements | Secondary AIUC-1 requirements |
|---|---|---|---|
| ASI01 | Agent Goal Hijack | B001, B002, B005, B006, C009, D003 | A004, C002, C003, C004, C006, C011, E008, E010, E015 |
| ASI02 | Tool Misuse and Exploitation | A003, B006, B007, D003, D004, E009 | A004, A007, B008, C009, E015 |
| ASI03 | Identity and Privilege Abuse | B006, B007, B008, D003, E009 | A004, A007, B003, D004, E010, E015 |
| ASI04 | Agentic Supply Chain Vulnerabilities | B008, E006, E009 | A004, A007, E005, E007, E015 |
| ASI05 | Unexpected Code Execution (RCE) | B006, B008, C006, D003, D004 | B001, B002, B005, B009, C002, C004, C005, E015, F001 |
| ASI06 | Memory & Context Poisoning | A003, A005, B001, B002, B005, E015 | A006, B006, B009, D003 |
| ASI07 | Insecure Inter-Agent Communication | B006, B008, E009, E015 | D003 |
| ASI08 | Cascading Failures | D001, D002, D003, E001, E002, E003, E015 | B005, B006, C003, C007, C009 |
| ASI09 | Human-Agent Trust Exploitation | C003, C007, C009, C010, D001, D002, E016 | A003, B004, B007, B009, C005, C006, E004, E009, E015, E017 |
| ASI10 | Rogue Agents | B006, B008, D003, D004, E001, E015 | B003, B007, B009, C007, C008, C009, D002, E016, F001, F002 |

ASI05 and ASI09 carry the largest secondary footprints. E015 (Log model activity) maps to all ten threats, Primary for the four that are persistent, cross-session, or multi-agent (ASI06, ASI07, ASI08, ASI10).[^src]

## Rationale by ASI category

The control function each ASI category's mappings lean on, from the master table.[^src]

| ASI | Threat | Dominant rationale codes |
|---|---|---|
| ASI01 | Agent Goal Hijack | VALID, DETECT, PREV, SCOPE, GATE, GOVERN |
| ASI02 | Tool Misuse and Exploitation | SCOPE, PREV, VALID, DETECT, ISOLATE |
| ASI03 | Identity and Privilege Abuse | SCOPE, ISOLATE, DETECT, PREV, VALID, GOVERN |
| ASI04 | Agentic Supply Chain Vulnerabilities | ISOLATE, VALID, DETECT, SCOPE, GOVERN |
| ASI05 | Unexpected Code Execution (RCE) | SCOPE, ISOLATE, PREV, VALID, DETECT |
| ASI06 | Memory & Context Poisoning | SCOPE, ISOLATE, VALID, PREV, DETECT |
| ASI07 | Insecure Inter-Agent Communication | SCOPE, ISOLATE, DETECT |
| ASI08 | Cascading Failures | PREV, VALID, SCOPE, GOVERN, DETECT, GATE |
| ASI09 | Human-Agent Trust Exploitation | PREV, GATE, VALID, DISCLOSE, SCOPE, GOVERN, DETECT |
| ASI10 | Rogue Agents | SCOPE, ISOLATE, VALID, GOVERN, DETECT, GATE, DISCLOSE, PREV |

DISCLOSE appears only against ASI09 and ASI10, mapped to E016 (AI disclosure mechanisms) and E017 (transparency policy).[^src]

## Observed AIUC-1 gaps

The crosswalk records eight areas where AIUC-1 has no requirement that the ASI prevention guidelines treat as essential. Four (Gaps 1, 2, 4, 5) describe control surfaces with no dedicated requirement; four (Gaps 3, 6, 7, 8) describe expansions of partial-coverage requirements.[^src]

| Gap | Area | Surfaces at |
|---|---|---|
| 1 | Inter-agent communication security (mutual auth, message integrity, replay protection, signed agent cards) | ASI07, ASI08, ASI10 |
| 2 | Agent identity attestation and containment (per-agent crypto identity, signed behavioral manifests, kill switches) | ASI03, ASI10 |
| 3 | Agentic supply chain attestation (SBOMs/AIBOMs, prompt provenance, code signing for agents and tools) | ASI02, ASI04 |
| 4 | Cascading failure containment (circuit breakers, blast-radius caps, planner-executor isolation) | ASI08 |
| 5 | Agent tool-use infrastructure controls (tool registration, tool authentication, agent-tool-call logging) | ASI02, ASI03, ASI05 |
| 6 | Runtime agent security monitoring (malicious model/container/payload detection inside the agent) | ASI05, ASI10 |
| 7 | Resource and cost abuse controls (AI service entitlement, monetary-budget and token-consumption monitoring) | ASI01, ASI10 |
| 8 | Input/output schema controls and determinism (schematic controls at the agent-model boundary) | ASI01, ASI06, ASI08 |

Two further items are framed as scope-expansion recommendations rather than new mappings: guardrail placement architecture for C003/C004 (enforce pre-AI and post-AI guardrails in agent code, not at the model layer alone) and data sovereignty for agentic operations under E011 (cross-region data use during inference and retrieval, distinct from training-time data governance).[^src]

## Newly validated mappings

Contributor review added five Secondary mappings for four previously unmapped AIUC-1 requirements, each a governance, accountability, or transparency layer that supports technical mitigation rather than implementing it.[^src]

| AIUC-1 requirement | ASI category | Relevance |
|---|---|---|
| E004 Assign accountability | ASI09 | Secondary |
| E008 Review internal processes | ASI01 | Secondary |
| E010 Establish AI acceptable use policy | ASI01 | Secondary |
| E010 Establish AI acceptable use policy | ASI03 | Secondary |
| E017 Document system transparency policy | ASI09 | Secondary |

After validation, eight AIUC-1 requirements remain unmapped to any ASI category (A001, A002, C001, C012, E011, E012, E013, E014), described as policy and process requirements that operate at a governance layer above specific agentic threat scenarios.[^src]

## Asymmetric coverage

The ASI Top 10 supplies threat-level coverage AIUC-1 lacks at the agentic-control layer: per-agent identity, inter-agent authentication, cascading-failure containment, agent-tool-call observability, and runtime behavioral monitoring all surface as AIUC-1 gaps in this document. The ASI prevention guidelines also reference companion artifacts (Multi-Agentic System Threat Modeling Guide, Agent Name Service) that propose mechanisms with no AIUC-1 counterpart.

AIUC-1 supplies the governance, accountability, and certification layer the ASI Top 10 lacks: assigned accountability (E001–E004), failure response plans (E001–E003), acceptable-use and transparency policy (E010, E016, E017), vendor due diligence (E006), and the catastrophic-misuse Society pillar (F001, F002) that the ASI list addresses only through Rogue Agents. The eight unmapped AIUC-1 requirements are governance and process controls that have no single ASI threat to anchor to but remain part of an enterprise certification. The relationship is bidirectional rather than one-to-one: AIUC-1 data-protection controls also constrain the blast radius available to a compromised agent.[^src]

## Relations

- [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]] — the threat taxonomy on the OWASP side
- [[aiuc-1|AIUC-1 AI Agent Certification Standard]] — the certification requirements on the AIUC side
- [[aiuc-1-critical-evaluation|AIUC-1 Critical Evaluation]] — the wiki's standing assessment of AIUC-1 as a CMM D1 assurance scheme
- [[owasp-aivss|OWASP AIVSS]] — severity scoring referenced in the gap analysis (autonomy, self-modification, non-determinism amplification factors)
- [[agentic-ai-security-cmm-crosswalk|CMM: Standards Crosswalk Matrix]] — the wiki's own CMM-to-standards map, which holds an AIUC-1 six-pillars crosswalk
- [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]] — companion ASI artifact (T-codes)
- [[owasp-state-of-agentic-ai-security-governance|OWASP State of Agentic AI Security and Governance]] — companion ASI artifact
- [[owasp|OWASP]] / [[aiuc|AIUC]] — the two publishing organizations

[^src]: *AIUC-1: Crosswalks OWASP Top 10 For Agentic Applications*, v1.0, May 2026, OWASP GenAI Security Project (Agentic Security Initiative), CC BY-SA 4.0. Archived at `.raw/papers/owasp-agentic-top10-aiuc1-crosswalk-2026-05.pdf`.
