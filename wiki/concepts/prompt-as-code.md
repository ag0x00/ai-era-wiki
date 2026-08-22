---
type: concept
title: "Prompt as Code"
created: 2026-05-03
updated: 2026-05-03
tags:
  - concepts
  - prompt-injection
  - threat-model
  - llm-architecture
status: developing
no_public_url: "Named formulation from an ingested Google/Unprompted talk; no stable primary-publisher URL (third-party recaps only)."
attributed_to: "Nicolas Lidzborski (Google), Unprompted March 2026"
related:
  - "[[indirect-prompt-injection]]"
  - "[[lethal-trifecta]]"
  - "[[camel-pattern]]"
  - "[[system-prompt-architecture]]"
  - "[[securing-workspace-genai-at-google-talk]]"
coined_by:
  - "[[google]]"
---

# Prompt as Code

A structural framing for why LLM security cannot rely on syntactic filtering: in an LLM, every token in the input stream is a potential instruction, and the natural-language grammar is fuzzy. Therefore prompts are *code* — executable in semantic terms — yet they are processed through the same channel as data. The result is a structural collapse of the code/data boundary that all of computing has otherwise enforced for half a century.

The framing was articulated in this form by [[nicolas-lidzborski|Nicolas Lidzborski]] (Google Workspace security) at [[unprompted-conference-march-2026|Unprompted March 2026]] as the spine of his three-year GenAI security retrospective. It generalizes [[indirect-prompt-injection|indirect prompt injection]] into a *structural* claim about how LLMs process input rather than treating each injection class as a separate vulnerability.

## The two synergistic problems

### Semantic shift

In traditional appsec, parsers are deterministic. A SQL injection has a recognizable grammar; an XSS payload has a recognizable shape. Filters operate on syntax: block lists, regex, parser-defined token classes.

In GenAI, the grammar is **natural language**, which is inherently fuzzy. The "attack" is no longer breaking syntax — it is **shifting context**. Persuasion, role-playing, linguistic obfuscation, and adversarial prefixes all work because the model interprets *intent* rather than executing rigid commands.

The cat-and-mouse implication: a defender's filter is always one obfuscation layer behind. Base64, hex, ROT13, leetspeak, low-resource-language translation, semantic-equivalent paraphrase — the LLM understands them all. The pattern the filter looks for breaks the moment the data is encoded; the malicious instructions pass through unexamined.

### Collapsed control plane

In classical computing, code and data live in separate memory regions. The CPU enforces this with the **NX bit** (No-eXecute) — a hardware-level marker that some memory pages contain data, not instructions, and any attempt to execute them faults.

In an LLM, system instructions, user input, retrieved documents, and tool output are processed as a **single contiguous token stream**. There is no out-of-band channel to tell the model "these 500 tokens are just data, do not execute them." There is no NX bit for the prompt window. Every token is a potential instruction.

This is the structural reason indirect prompt injection works. The LLM cannot distinguish between an authoritative system instruction and a user-injected fragment that *looks* like an instruction. They share the same channel.

## Significance of the framing

"Prompt as code" reframes the security problem in three useful ways:

1. **It explains why filtering loses.** If prompt is code, then "filter the bad prompts" is equivalent to "filter the bad code" — a problem the security community has spent four decades learning to avoid in favor of structural mitigations (sandboxing, separation, memory protection, capability-based access control).
2. **It points at the right defense category.** Structural mitigations from classical computing have analogues in agentic AI:
   - **Memory protection / NX bit** → channel separation between privileged and quarantined LLMs ([[camel-pattern|CaMeL pattern]])
   - **Sandboxing** → per-task agent sandboxes ([[firecracker|Firecracker]], [[gvisor|gVisor]])
   - **Capability-based access control** → [[capability-based-authorization|Capability-based authorization]] / [[tenuo-warrant|Tenuo Warrants]]
   - **Privilege separation** → [[system-prompt-architecture|System-prompt architecture]] (boundary markers)
3. **It justifies the layered-defense posture in this wiki's RA.** Every plane in the [[agentic-ai-security-reference-architecture|RA]] is, in some form, a structural mitigation of the prompt-as-code problem rather than a syntactic filter.

## Relation to the von Neumann analogy

In Q&A at Unprompted, an audience member observed that LLMs have repeated the **von Neumann mistake** — combining code and data in a shared address space, which historically enabled buffer-overflow exploits — by combining instructions and data in a shared prompt window. The proposed remedy: structurally separate channels for "commands" and "data" rather than expecting the model to discriminate.

This is the audience-side restatement of "prompt as code" + the architectural remedy. The [[camel-pattern|CaMeL pattern]] (Google DeepMind, March 2025) operationalizes the channel separation: a privileged LLM never sees untrusted content; a quarantined LLM processes untrusted content but cannot issue tool calls. The structured output between them is the only crossing point, and it is constrained to data shapes that cannot carry executable instructions.

Lidzborski's framing and CaMeL are complementary: prompt-as-code is the *diagnosis*, channel-separation primitives are the *prescription*.

## Cross-references

- [[indirect-prompt-injection|Indirect prompt injection]] is the most prevalent attack class enabled by prompt-as-code
- [[lethal-trifecta|Lethal Trifecta]] is the operational convergence at which prompt-as-code becomes catastrophic (private data + untrusted content + external comms)
- [[recursive-prompt-injection|Recursive prompt injection]] shows that prompt-as-code applies to LLM judges as much as primary models — they share the same semantic interface
- [[sentinel-tokens|Sentinel tokens]] are a partial structural mitigation, not a complete one — they reduce but do not eliminate the ambiguity
- [[camel-pattern|CaMeL pattern]] is the most architecturally pure response (channel separation)

**"Prompt as code" is a *structural* framing, not a vulnerability class.** It explains why a category of defenses (syntactic filtering, ML classifiers, LLM-as-judge) cannot succeed regardless of implementation quality, and why a different category of defenses (channel separation, capability tokens, deterministic orchestration, sandboxing) is required.
