---
type: concept
title: "Memory Poisoning (Agentic AI)"
created: 2026-05-03
updated: 2026-08-20
tags:
  - concepts
  - memory-poisoning
  - data-plane
  - prompt-injection
  - rag
  - agentic-ai
status: developing
origin: aggregated
scope_axis:
  - sec-of-ai
source_url: "https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/"
related:
  - "[[indirect-prompt-injection]]"
  - "[[lethal-trifecta]]"
  - "[[rag-hardening]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[tool-poisoning]]"
  - "[[mitre-atlas]]"
  - "[[nist-ai-600-1]]"
  - "[[agents-rule-of-two]]"
  - "[[owasp-ai-exchange]]"
  - "[[agent-memory-isolation]]"
  - "[[agent-escape]]"
  - "[[precize-agentic-ai-top10]]"
  - "[[cosnitch-copilot-personal-exfiltration]]"
---

# Memory Poisoning (Agentic AI)

Memory poisoning is the injection of adversarial content into an agent's persistent memory stores — conversation history, episodic memory, semantic memory (vector database), or scratchpad — with the goal of causing the agent to behave maliciously or incorrectly in future interactions. Unlike [[prompt-injection|prompt injection]], which attacks a single inference step, memory poisoning creates a **persistent, durable** attack surface: once poisoned content enters memory, it influences every subsequent retrieval and reasoning step that accesses it.

## Memory types and attack surfaces

| Memory type | Examples | Attack vector |
|---|---|---|
| **Conversation / session history** | Message history passed as prior context in subsequent turns | Inject malicious "prior" messages that appear to be legitimate user or assistant turns |
| **Episodic memory** | Long-term conversation logs stored externally and retrieved by agents | Inject adversarial episodes that instruct the agent to bypass controls on future invocations |
| **Semantic memory (vector store)** | RAG corpus; knowledge base used for retrieval | Embed adversarial documents that score highly in retrieval for target queries and carry malicious instructions |
| **Working memory / scratchpad** | Agent's intermediate reasoning or plan stored between tool calls | Overwrite or append to plan artifacts mid-execution via a compromised tool call |
| **Agent state / checkpoint** | Serialized agent state for long-running tasks | Modify checkpoint state to implant false beliefs or change task objectives |

## Semantic memory poisoning (RAG poisoning)

The most studied variant. An attacker plants adversarial documents in the RAG corpus — either directly (if they have write access to the knowledge base) or indirectly (by causing the agent to ingest attacker-controlled content, e.g., via web retrieval). The adversarial document is constructed to:

1. **Score high on retrieval** for target queries (high cosine similarity to likely user prompts)
2. **Carry prompt injection instructions** embedded within otherwise legitimate-looking content

The PoisonedRAG attack (published 2024) demonstrated that carefully crafted adversarial passages could cause retrieval-augmented generation systems to produce attacker-specified outputs with high reliability. The [[lethal-trifecta|Lethal Trifecta]] framing makes RAG applications unconditionally vulnerable by default: they combine private data access + untrusted content ingestion (the web, user uploads) + generation of outputs potentially read by other agents.

## Episodic / long-term memory poisoning

Long-running agents that store and retrieve memories across sessions face a compounding risk: any content that enters memory through an injection attack in session N becomes part of the retrieved context for session N+1 through N+∞.

Published example (Microsoft Defender for Cloud Apps, March 2026): researchers found 50+ examples of successful memory injection in production agentic systems, in which a single adversarial interaction planted instructions that persisted across sessions. The injected memory caused the agent to take unauthorized actions in later, unrelated user sessions.

This is the self-propagating variant of indirect prompt injection: the attack scales across all future agent interactions without further attacker involvement. [[cosnitch-copilot-personal-exfiltration|CoSnitch]] (Varonis, disclosed against Microsoft Copilot Personal in August 2026) is a sourced case with a stronger persistence claim than the Defender for Cloud Apps count above: the vendor states its planted memory entries survive a password change, a session revocation, and device re-enrollment, and leave no forensic footprint a conventional security tool would flag. If accurate, none of the identity-layer controls a defender would reach for first — credential rotation, session termination — reach a poisoned entry; only a control that operates on the memory store itself does. The [[owasp-ai-exchange|OWASP AI Exchange]] names the mechanism **stored injection** and files it as a subclass of indirect prompt injection, an input threat, where the payload persists in a retrieval index, a shared document, or a database and is retrieved in later sessions.[^aix-pi] The same mechanism appears elsewhere in the Exchange as persistent memory poisoning, a surface of augmentation data manipulation and therefore a runtime threat.[^aix-augmanip] One store and one entry sit under two threat headings, which means a prompt-injection defense program and a memory-integrity program are working the same surface from opposite ends.

The cross-agent path is the sharper version. Where several agents share a store, content written by one may be retrieved by another, so a compromised write is a future read attack against a different agent.[^aix-augmanip] The [[owasp-ai-exchange|OWASP AI Exchange]]'s worked case is a multi-agent customer-support system on a shared vector store: an adversary submits a request containing a fabricated return policy, an agent summarises it into the shared store, and subsequent agents serve the fabricated policy to other customers until the entry is found and removed.[^aix-augmanip] Detection and removal, rather than prevention, bound the damage window in that scenario.

## Defenses

### Source provenance and attestation

Each document or memory entry should carry a cryptographic provenance record: who wrote it, when, and from what source. Memory entries derived from external (untrusted) content should be tagged as untrusted and subjected to higher scrutiny on retrieval. RAGShield implements cryptographic document attestation for this purpose (Exploratory-tier as of Q1 2026).

### Retrieval-side content filtering

Retrieved content should be inspected for embedded instructions before being passed to the model's context. [[llamafirewall|LlamaFirewall]] PromptGuard 2 operates on the input side and can be applied to retrieved context, not just user messages. This is a probabilistic defense (97.5% recall, 1% FPR on its benchmark) — not a guarantee.

### Memory integrity monitoring

For agent scratchpads and state checkpoints, integrity monitoring (hash comparison against a known-good baseline) detects unauthorized modification. The [[agentic-ai-security-reference-architecture|RA]] data plane references SHA-256 monitoring of cognitive files (SOUL.md, IDENTITY.md) as an Exploratory implementation of this pattern.

### Sandboxed memory namespacing

Agents should not share a single memory namespace across trust domains. A multi-tenant deployment where agents for different principals share a vector store creates a path for cross-tenant memory poisoning: a malicious actor in tenant A injects content that is retrieved in tenant B's session.

### State rollback

For long-running agents, maintaining a git-like checkpoint history (Brain Git pattern, Exploratory tier) enables rollback to a pre-poisoned state when an injection is detected. Paired with behavioral drift detection, this allows incident response: detect anomaly → identify injection point → roll back to last clean checkpoint.

### Partitioned memory with write authorization

Partition memory per agent and per session, and authorize reads and writes against the partition rather than against the store. The [[owasp-ai-exchange|OWASP AI Exchange]] describes cross-agent memory access without explicit authorisation as lateral movement through shared state, which places the control in the same family as network segmentation rather than in content inspection.[^aix-augintegrity] Every write carries provenance — source, writer identity, timestamp, and partition — so a poisoned entry can be traced to its writer after the fact.[^aix-augintegrity]

This is the control that bounds damage when detection fails, and the Exchange is explicit that detection does fail: separating legitimate memory updates from adversarial poisoning at scale remains difficult, and structural authorization is named as the compensating approach.[^aix-augintegrity]

### Integrity verification at the read boundary

Verify an entry's integrity before it enters an agent's active context, and quarantine or reject entries that fail. This gates a read, where the integrity monitoring above watches a store, and it is the point at which a poisoned entry stops being data and starts being context. Sanitise the session boundary as well: review and, where appropriate, reset agent context between tasks.[^aix-augintegrity] The Exchange notes that provenance and integrity checks add storage and latency.[^aix-augintegrity]

### Planning-artefact protection

Plan libraries, templates, and heuristics are memory that executes. Hold them under integrity verification and access control, and validate a plan against policy before execution rather than trusting the store it came from.[^aix-augintegrity] An agent that retrieves a poisoned plan has been redirected without any adversarial content appearing in its context window.

## Mapping to frameworks

Memory poisoning is [[owasp-agentic-ai-top-10|OWASP ASI06]] (Memory & Context Poisoning) and threat **T1** in the OWASP [[owasp-agentic-ai-threats-mitigations|Agentic AI Threats and Mitigations]] guide. [[precize-agentic-ai-top10|The Precize Top 10 for Agentic AI Vulnerability]], which states it precedes the ASI Top 10, names the same surface as AAI006 (Agent Memory and Context Manipulation), with context-amnesia exploitation, cross-session data leakage, and memory poisoning as its three sub-mechanisms. That taxonomy's own May 2025 revision deprecates AAI010 (Agent Knowledge Base Poisoning), the RAG-corpus-specific variant, into AAI006 — the same consolidation this page already reflects by treating semantic-memory and episodic-memory poisoning as one concept. [[mitre-atlas|MITRE ATLAS]] catalogs the corresponding poisoning techniques against agent memory and retrieval corpora as confirmed-in-wild adversarial techniques — `AML.T0080` (AI Agent Context Poisoning, with `AML.T0080.001` Thread) for runtime/session memory and `AML.T0070` (RAG Poisoning) for the retrieval corpus — and the [[nist-ai-600-1|NIST AI 600-1]] GenAI Profile places corpus and memory corruption under its information-integrity risk category, where its Suggested Action `MS-2.7-007` directs deployers to red-team poisoning resilience. That guide's Playbook 2 (Preventing Memory Poisoning and AI Knowledge Corruption) supplies the matching control set: session isolation, source attribution on memory writes, pre-commit validation before cross-session persistence, and AI-generated memory snapshots for forensic rollback.

This is the failure mode that escapes a per-session security rule. [[agents-rule-of-two|The Agents Rule of Two]] bounds the properties an agent may hold *within a session*, and says nothing about a property acquired across sessions through persistent memory or a written instruction file. For coding agents the vector is concrete: an injected hook or an edited `CLAUDE.md` survives the session that wrote it, which is what made CVE-2026-25725 a sandbox escape rather than a contained session compromise.

## Relation to the Data plane

In the [[agentic-ai-security-reference-architecture|RA]], memory poisoning defense is the primary motivation for the **Data plane** (RAG provenance/attestation, memory poisoning defense row, state rollback). The enforcement pattern is:

```
[External content] → source tagging → [Memory store]
                                           ↓
                              retrieval + provenance check
                                           ↓
                              [Retrieved context] → input filter → [Model]
```

The Microsoft Defender for Cloud Apps memory-injection detector is the only production-grade commercial control in this row as of Q2 2026; the other implementations (RAGShield, Brain Git, SHA-256 monitoring) are Exploratory.

## Notes

[^aix-augmanip]: [OWASP AI Exchange — Augmentation data manipulation](https://owaspai.org/go/augmentationdatamanipulation/), retrieved 2026-08-18.
[^aix-augintegrity]: [OWASP AI Exchange — AUGMENTATION DATA INTEGRITY](https://owaspai.org/go/augmentationdataintegrity/), retrieved 2026-08-18.
[^aix-pi]: [OWASP AI Exchange — Prompt injection](https://owaspai.org/go/promptinjection/), retrieved 2026-08-18. Stored injection as a subclass of indirect prompt injection, with the payload persisting in a retrieval index, shared documents, or a database for retrieval in later sessions.

> [!gap]
> Memory poisoning lacks a standardized detection taxonomy. Unlike network-layer attacks with well-defined IOCs, there is no established rubric for what a poisoned memory entry looks like in a vector store, and behavioral anomaly detection triggers after the attack has influenced behavior. The [[owasp-ai-exchange|OWASP AI Exchange]] reaches the same conclusion and states the consequence: distinguishing legitimate memory updates from adversarial poisoning at scale remains difficult, so partition access control and write authorization carry the blast-radius work that detection cannot.[^aix-augintegrity] Pre-retrieval content classification for memory stores stays an open research problem; the structural controls above are what ships in the meantime.

<!-- sources:auto -->
## Sources

- [Memory Poisoning (Agentic AI)](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)
<!-- /sources -->
