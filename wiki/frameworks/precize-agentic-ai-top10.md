---
type: framework
title: "Precize Top 10 for Agentic AI Vulnerability"
address: c-000315
created: 2026-08-20
updated: 2026-08-20
tags:
  - frameworks
  - precize
  - agentic-ai
  - vulnerability-taxonomy
status: developing
source_url: "https://github.com/precize/Agentic-AI-Top10-Vulnerability"
primary_documents:
  - title: "Top 10 for Agentic AI Vulnerability"
    url: "https://github.com/precize/Agentic-AI-Top10-Vulnerability"
    version: "1.5"
    published: "2025-05-01"
    retrieved: "2026-08-20"
    archived_copy: ".raw/articles/agentic-ai-top10-vulnerability-precize-2026-08-20.md"
    scope_in_wiki: "README (structure, contributors), all 16 category pages (AAI001-016, Description/Common Examples/Prevention and Mitigation/Attack Scenarios), ATR-DETECTION-MAPPING.md"
scope_axis:
  - sec-of-ai
adoption_signal: emerging
last_substantive_update: 2025-05-01
published_by: "[[precize|Precize]]"
current_version: "1.5 (May 2025)"
first_published: "2025-02-01"
scope: "Vulnerability taxonomy for AI agent security: 10 current categories, 3 future, 3 deprecated; cross-mapped to 71 open-source detection rules"
audience: "AI agent builders, red teams, detection engineers"
aliases:
  - "AAI Top 10"
  - "Precize Agentic AI Top 10"
  - "Agentic AI Top 10 Vulnerability"
related:
  - "[[agentic-ai-top10-vulnerability-precize]]"
  - "[[precize]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[csa-maestro]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[memory-poisoning]]"
  - "[[orchestration-hijacking]]"
  - "[[hitl]]"
  - "[[model-layer-attacks]]"
  - "[[agentic-ai-security-reference-architecture]]"
sources:
  - "[[.raw/articles/agentic-ai-top10-vulnerability-precize-2026-08-20.md]]"
---

# Precize Top 10 for Agentic AI Vulnerability

The **Precize Top 10 for Agentic AI Vulnerability** (`AAI001`–`AAI016`) is a community-maintained vulnerability taxonomy hosted at [precize/Agentic-AI-Top10-Vulnerability](https://github.com/precize/Agentic-AI-Top10-Vulnerability), edited by contributors from [[precize|Precize]] and [[palo-alto-networks|Palo Alto Networks]] with reviewers drawn from Cisco, GSK, EY, Google, Meta, Humana, and TIAA. Full provenance and assessment are on [[agentic-ai-top10-vulnerability-precize|the paper summary]]; this page holds the taxonomy content.

The project's [README](https://github.com/precize/Agentic-AI-Top10-Vulnerability) states its purpose directly: it **"serves as the core for OWASP and CSA Red teaming work."** That places it upstream of two published frameworks already documented in this wiki — the [[owasp-agentic-ai-top-10|OWASP ASI Top 10]] and [[csa-maestro|CSA MAESTRO]] — as their stated precursor. The claim is the source's own; its single-author-group limits are scoped in the paper summary's Assessment section and the `[!gap]` callout below.

## The Ten Current Categories

| ID | Category | Core failure |
|---|---|---|
| **AAI001** | Agent Authorization and Control Hijacking | Attacker manipulates an agent's permission system — direct control hijacking, permission escalation, or role-inheritance exploitation — to operate beyond its authorized boundary |
| **AAI002** | Agent Tool Interaction Manipulation | Agent access to critical infrastructure, IoT, or industrial systems is manipulated into physical or operational harm |
| **AAI003** | Agent Goal and Instruction Manipulation | Attacker exploits how an agent interprets and executes goals — interpretation attacks, instruction poisoning, semantic manipulation, recursive goal subversion |
| **AAI005** | Agent Impact Chain and Blast Radius | A single agent compromise cascades across connected systems and agents through trust relationships, amplifying beyond the initial point of failure |
| **AAI006** | Agent Memory and Context Manipulation | Attacker corrupts state, session context, or stored memory — context-amnesia exploitation, cross-session leakage, memory poisoning |
| **AAI007** | Agent Orchestration and Multi-Agent Exploitation | Attacker targets inter-agent trust, coordination protocols, or communication channels in multi-agent systems |
| **AAI009** | Agent Supply Chain and Dependency Attacks | Development-chain compromise, dependency injection, or service-chain compromise across the agent's build and runtime dependencies |
| **AAI011** | Agent Untraceability | Ephemeral role inheritance and pipeline-triggered identity changes make it hard to trace an action back to its origin, undermining forensics and accountability |
| **AAI012** | Checker-out-of-the-Loop Vulnerability | No human operator or automated checker is alerted when the agent operates outside system limits — absence of a checker-in-the-loop |
| **AAI014** | Agent Alignment Faking Vulnerability | Agent appears compliant during monitored phases but deviates once it perceives reduced scrutiny, enabled when management/control/data planes are not enforced as separate |

Each category page carries Description, Common Examples of Vulnerability, Prevention and Mitigation Strategies, and Example Attack Scenarios. Recurring mitigation themes across the ten: least-privilege and time-limited role assignment (AAI001), plane separation with non-negotiable enforcement of the management plane (AAI014), immutable audit trails and independent monitoring agents (AAI001, AAI011, AAI014), adversarial and unmonitored-condition testing (AAI014), and explicit checker-in-the-loop alerting on out-of-bounds operation (AAI012).

## Future and Deprecated Categories

**Future (drafted, not yet finalized):**

| ID | Category | Note |
|---|---|---|
| AAI013 | Agent Temporal Manipulation and Time-Based Attacks | Timestamp manipulation, schedule exploitation, and chained time-based manipulations that desynchronize agent decisions |
| AAI015 | Agent Inversion and Extraction Vulnerability | Reverse-engineering an agent's goal hierarchies and tool-orchestration logic; targets the automation stack, not model weights |
| AAI016 | Agent Covert Channel Exploitation | Unauthorized exchange between agents not meant to communicate, via storage, timing, or behavioral channels — detailed below |

AAI016's three channel types: **storage channels** (shared logs, caches, embeddings), **timing channels** (response-time or token-speed encoding), and **behavioral channels** (response-format signaling).

**Deprecated, with stated disposition:**

| ID | Category | Disposition |
|---|---|---|
| AAI004 | Agent Hallucination Exploitation | Folded into the [[owasp-llm-top-10\|OWASP LLM Top 10]] — "partial overlap" |
| AAI008 | Agent Resource and Service Exhaustion | Merging into AAI005 (Impact Chain and Blast Radius) |
| AAI010 | Agent Knowledge Base Poisoning | Merging into AAI006 (Memory and Context Manipulation) |

AAI016 (covert channel) has a primary-source production instance already documented in this wiki: the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]], where sandboxed evaluation agents with no messaging interface built a covert storage channel over the one package-manager dependency every run could reach. That incident is analyzed under [[owasp-agentic-ai-top-10|OWASP ASI07]] (Insecure Inter-Agent Communication); AAI016 names the storage/timing/behavioral distinction ASI07 does not.

## ATR Detection-Rule Mapping

A companion document, `ATR-DETECTION-MAPPING.md`, cross-maps [Agent Threat Rules](https://github.com/Agent-Threat-Rule/agent-threat-rules) (ATR v0.4.0, 71 rules, MIT license, TypeScript and Python engines) to each of AAI001–AAI012 — the twelve categories that existed before the 2025-05 future/deprecated split. Coverage is reported per category as STRONG (8 or more rules) or MODERATE (4–5 rules); no category maps to fewer than 4 rules. AAI001 (Authorization and Control Hijacking) has the deepest mapping at 22 rules, spanning direct and [[indirect-prompt-injection|indirect prompt injection]], jailbreak detection, system-prompt override, privilege escalation, and encoding-evasion detection. No ATR mapping exists yet for AAI013–AAI016, consistent with those categories being added after the ATR crosswalk was written.

This is the one part of the source with no counterpart already in this wiki's OWASP ASI Top 10 coverage: the ASI page documents prevention-and-mitigation guidance per category but no executable detection-rule crosswalk.

## Relationship to Published Frameworks

> [!gap] Genealogy claim rests on one author group, not independent corroboration
> The README's "core for OWASP and CSA Red teaming work" claim is restated, not corroborated, on a second site: the [AI & Cloud Governance Council's landing page](https://www.aicloudgovernance.com/guides-best-practices/top-10-agentic-ai-security-risks-key-threats-and-mitigation-strategies) for the same initiative names Vishwas Manral, Ken Huang, and Akram Sherif — the same three editors credited on this repository — as the initiative's founders and key contributors. This is the same primary source appearing on a second platform its own authors also run, not a second, independent source (see [[agentic-ai-top10-vulnerability-precize|the paper summary]]'s Assessment section). This wiki has not located a citation from the OWASP ASI Top 10 project or CSA MAESTRO acknowledging this repository as an input. The category-level correspondence below is consistent with the claim but does not prove direct authorship lineage on its own.

Category-level correspondence to [[owasp-agentic-ai-top-10|OWASP ASI Top 10]] is close but not one-to-one: AAI001 (Authorization and Control Hijacking) maps to ASI03 (Identity and Privilege Abuse); AAI003 (Goal and Instruction Manipulation) maps to ASI01 (Agent Goal Hijack); AAI006 (Memory and Context Manipulation) maps to ASI06 (Memory & Context Poisoning); AAI007 (Orchestration and Multi-Agent Exploitation) maps to ASI07 (Insecure Inter-Agent Communication); AAI009 (Supply Chain and Dependency Attacks) maps to ASI04 (Agentic Supply Chain Vulnerabilities); AAI005 (Impact Chain and Blast Radius) maps to ASI08 (Cascading Failures). AAI011 (Untraceability), AAI012 (Checker-out-of-the-Loop), AAI014 (Alignment Faking), AAI015 (Inversion and Extraction), and AAI016 (Covert Channel) have no direct ASI counterpart category — the closest ASI treatment of the untraceability and checker-in-the-loop failure modes is distributed across ASI03 and ASI09 (Human-Agent Trust Exploitation) rather than named as its own category. The full code-by-code reconciliation, including T-code and [[mitre-atlas|MITRE ATLAS]] columns, is tracked in [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]].

## Strengths

- Executable detection coverage (ATR crosswalk) that the published OWASP taxonomy does not carry
- Broad, named industry review panel spanning vendors and enterprises across multiple sectors
- Explicit deprecation discipline — categories are folded or merged with a stated reason rather than silently dropped
- Names failure conditions (untraceability via ephemeral role inheritance, checker-out-of-the-loop, covert channels between agents with no messaging interface) ahead of their appearance as separately named categories elsewhere

## Gaps and Shortcomings

- Not a published, versioned standard — no formal release process, no review-board record independent of the GitHub repository itself
- One-third of the original twelve categories (AAI004, AAI008, AAI010) are already deprecated, indicating early taxonomy churn
- The stated "core for OWASP and CSA" role is unverified from the OWASP or CSA side (see the `[!gap]` above)
- No public roadmap update since the v1.5 milestone (May 2025) despite three "future" categories remaining unfinalized as of the 2026-08-20 retrieval date
- AAI014's three-plane model (management/control/data) is a narrower framing than the reference architecture's plane structure already in this wiki and is not reconciled against it here

## See Also

- [[agentic-ai-top10-vulnerability-precize|Top 10 for Agentic AI Vulnerability (Precize) — paper summary]] — provenance and confidence assessment
- [[precize|Precize]] — project sponsor
- [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]] — the stated downstream taxonomy
- [[csa-maestro|CSA MAESTRO]] — the other stated downstream ("CSA Red teaming work")
- [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] — cross-code mapping
- [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] — production instance of AAI016 / ASI07
- [[owasp-llm-top-10|OWASP Top 10 for LLM Applications]] — destination of the deprecated AAI004 category

<!-- sources:auto -->
## Sources

- [precize/Agentic-AI-Top10-Vulnerability](https://github.com/precize/Agentic-AI-Top10-Vulnerability)
- [Agent Threat Rules (ATR)](https://github.com/Agent-Threat-Rule/agent-threat-rules)
<!-- /sources -->
