---
type: concept
title: "Memory Poisoning (Agentic AI)"
created: 2026-05-03
updated: 2026-09-29
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
sources:
  - "https://owaspai.org/docs/4_runtime_application_security_threats/#47-augmentation-data-manipulation"
  - "https://www.microsoft.com/en-us/security/blog/2026/02/10/ai-recommendation-poisoning/"
  - "https://www.varonis.com/blog/cosnitch"
  - "https://arxiv.org/abs/2604.00387v2"
  - "https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Current OWASP, NIST, Microsoft, Varonis, MITRE, and cited research checked 2026-09-29."
---

# Memory Poisoning (Agentic AI)

Memory poisoning occurs when attacker-influenced content enters state that an agent later retrieves and treats as evidence, context, or instruction. The decisive path is **writer → store → retrieval → context → answer or action**. The entry may be a false fact, a forged instruction, or an altered plan. Persistence and reuse across tasks or agents distinguish it from a one-turn injection.

## Attack path

The assessor should identify every store the agent can write or read, including conversation summaries, durable preferences, vector indexes, plan files, and checkpoints. For each store, determine who can write, what source a write can claim, which agents can retrieve it, and whether retrieval crosses a principal or task boundary. A poisoned entry matters when it reaches a consequential answer or action; an untrusted document sitting unread in a corpus has not yet done so. The [OWASP AI Exchange](https://owaspai.org/docs/4_runtime_application_security_threats/#47-augmentation-data-manipulation) describes persistent memory poisoning as a future read attack and gives a shared-store example in which one request plants a false policy later served to other customers.

There are two useful variants. **Corpus poisoning** alters retrieved reference material, including a RAG index. **Agent-memory poisoning** alters state the system writes for later use. Both can carry indirect [[indirect-prompt-injection|Indirect Prompt Injection]], but false factual content may redirect an answer without an explicit instruction. [PoisonedRAG](https://arxiv.org/abs/2402.07867) demonstrates answer corruption from attacker-controlled retrieval passages.

## Evidence and examples

[Microsoft's February 2026 report](https://www.microsoft.com/en-us/security/blog/2026/02/10/ai-recommendation-poisoning/) found attempts to induce assistants to remember a commercial preference through prefilled links. It reports attempted planting and variable effectiveness, not a general success rate for production memory defenses. [Varonis's CoSnitch disclosure](https://www.varonis.com/blog/cosnitch) describes a tested Copilot Personal path from a crafted page to persistent memory; Varonis states it had no evidence of exploitation in the wild and that a fix shipped in August 2026. Both cases identify a write path that a password or session reset alone would not necessarily clear.

Research prototypes answer narrower questions. The current [RAGShield v2](https://arxiv.org/abs/2604.00387v2) evaluates numerical-claim manipulation in government RAG passages with value extraction and cross-source comparison. Its results do not establish general memory-poison detection or cryptographic provenance enforcement. The earlier [v1](https://arxiv.org/abs/2604.00387v1) described a different provenance-oriented design; the current version is the relevant source for a RAGShield claim.

## Controls and tests

[[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]] defines the applicable assessment outcomes. For a deployment with persistent memory, test the complete path:

- **Partition and authorize:** Bind entries to a principal, agent, session, or approved shared scope. Test a denied cross-partition read and write, including through caches or service identities.
- **Record provenance:** Bind each write to its source, distinct writer, time, and partition outside model control. A generic service-account name cannot identify the agent that wrote an entry.
- **Verify integrity at retrieval:** Compare the returned entry with a protected write-time record and refuse a tampered entry before it enters context. Source labels generated by the model are not evidence.
- **Inspect and detect:** Plant both a targeted instruction and a false fact through the actual write path; verify that the configured hold or alert reaches triage. Test benign writes to expose wrongful rejection. A detector's product label is not route evidence.
- **Reset and recover:** Show that an unauthorized carried memory is removed between tasks and that an earlier store state can be restored. Retaining a snapshot without a successful restore test leaves the recovery claim unproved.

[[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]] tests applicable indirect-injection screening before untrusted content enters the model. D6 remains the owner of store integrity, memory access, and rollback. If a poisoned entry causes an unauthorized tool call, the action gate also needs its own test.

## Framework scope

[[mitre-atlas|MITRE ATLAS]] distinguishes AI agent context poisoning (AML.T0080) from RAG poisoning (AML.T0070). [NIST AI 600-1, MS-2.7-007](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf) recommends red teaming against data poisoning and other AI attacks. It does not prescribe agent-memory partitions, memory snapshots, or the D6 tests above. Applying its broad red-team recommendation to persistent agent memory is this wiki's assessment choice.

<!-- sources:auto -->
## Sources

- [OWASP Top 10 for Agentic Applications 2026](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)
- [OWASP augmentation data manipulation](https://owaspai.org/docs/4_runtime_application_security_threats/#47-augmentation-data-manipulation)
- [Microsoft AI recommendation poisoning report](https://www.microsoft.com/en-us/security/blog/2026/02/10/ai-recommendation-poisoning/)
- [Varonis CoSnitch disclosure](https://www.varonis.com/blog/cosnitch)
- [RAGShield v2](https://arxiv.org/abs/2604.00387v2)
- [NIST AI 600-1](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf)
<!-- /sources -->
