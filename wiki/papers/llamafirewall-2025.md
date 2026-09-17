---
type: paper
title: "LlamaFirewall Guardrail Paper"
created: 2026-06-23
updated: 2026-09-16
tags:
  - papers
  - guardrails
  - prompt-injection
  - agent-security
  - meta
status: developing
scope_axis:
  - sec-of-ai
origin: aggregated
address: c-000231
source_url: "https://arxiv.org/abs/2505.03574"
related:
  - "[[llamafirewall]]"
  - "[[purple-llama]]"
  - "[[agentdojo]]"
  - "[[prompt-injection]]"
  - "[[prompt-injection-containment]]"
  - "[[joshua-saxe]]"
  - "[[meta]]"
  - "[[chain-of-thought-monitorability]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
sources:
  - "https://arxiv.org/abs/2505.03574"
  - "[[.raw/papers/llamafirewall-arxiv-2505-03574-2026-06-23.md]]"
verified: 2026-09-16
verified_against:
  - ".raw/papers/llamafirewall-arxiv-2505-03574-2026-06-23.md"
verified_findings: 0
---

# LlamaFirewall Guardrail Paper

Source paper for [[llamafirewall|LlamaFirewall]], Meta's open-source agent guardrail framework: *"LlamaFirewall: An open source guardrail system for building secure AI agents"* ([arXiv:2505.03574](https://arxiv.org/abs/2505.03574), submitted 2025-04-29, 19 authors, corresponding author [[joshua-saxe|Joshua Saxe]]). The framework ships as part of Meta's [[purple-llama|Purple Llama]] project.

The paper argues that chatbot-era safeguards leave agentic risk uncovered, because model fine-tuning and content moderation were built for a model that answers rather than one that acts on untrusted inputs — webpages, emails, retrieved documents — and writes code that runs. It proposes a real-time guardrail monitor as a *final* defense layer, enforcing system-level policy written per use case in place of a single universal filter.

## Three guardrails

| Guardrail | Layer | Mechanism | Targets |
|---|---|---|---|
| **PromptGuard 2** | Input | DeBERTa jailbreak classifier (86M and 22M variants) | Explicit jailbreak / [[prompt-injection\|prompt injection]] in inputs |
| **AlignmentCheck** | Reasoning | Few-shot chain-of-thought auditor over the agent's reasoning trace (guardrail LLM: Llama 3.3 70B / Llama 4 Maverick) | Goal hijacking, [[indirect-prompt-injection\|indirect prompt injection]] |
| **CodeShield** | Output | Static analysis (Semgrep + regex) across eight languages | Insecure generated code before execution |

AlignmentCheck inspects *why* the agent decided to act, which catches injections that pass input-layer detection and surface as off-goal reasoning. The paper marks it experimental. [[prompt-injection-containment|Prompt-injection containment]] tracks it as the reasoning-layer tier.

## Reported results

- **PromptGuard 2 (86M):** 98% AUC (English), 97.5% recall at 1% false-positive rate, 92.4 ms latency. The 22M variant trades recall for speed: 88.7% recall at 1% FPR, 19.3 ms.
- **Combined PromptGuard 2 + AlignmentCheck on [[agentdojo|AgentDojo]]:** attack success rate falls from 17.6% to 1.75% (a >90% reduction) at roughly 5% utility cost. AlignmentCheck alone delivers an 83% attack-success reduction.
- **CodeShield:** 96% precision, 79% recall on insecure-code detection, 60–70 ms typical latency.

Meta published every figure above, so each is a self-evaluation. The AgentDojo numbers survive that caveat, because AgentDojo is a peer-reviewed independent comparator (NeurIPS 2024) that third parties run against the same reduction from 17.6% to 1.75%. An external deployment test measures the cost side: Uber's [[adr-agentic-detection-system|ADR]] evaluation runs LlamaFirewall (Llama Guard 3-8B plus heuristics) as a baseline and finds it recall-strong on AgentDojo injection (0.974) and precision-poor on benign enterprise traffic ([[adr-bench|ADR-Bench]] precision 0.167), which is the class-imbalance failure mode of a high-recall guardrail.

## Relevance to this corpus

The paper is the primary citation behind several wiki claims: the two-layer (input + reasoning) detection model in [[prompt-injection-containment|Prompt Injection Containment]], the PromptGuard 2 numbers cited on [[agentdojo|AgentDojo]], and LlamaFirewall's placement at the model layer in [[security-controls-for-ai-stacks|Security Controls for AI Stacks]]. It also anchors the [[agentic-ai-security-cmm-2026|CMM]]'s runtime-guardrail evidence in [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]], where a layered, semantically-aware guardrail failing closed on critical paths is a concrete L4/L5 control instance. AlignmentCheck depends in turn on how much a reasoning trace reveals; see [[chain-of-thought-monitorability|Chain-of-Thought Monitorability]].

## See also

- [[llamafirewall|LlamaFirewall]] — the product page
- [[purple-llama|Purple Llama]] — Meta's umbrella project that ships it
- [[cyberseceval|CyberSecEval]] — the Purple Llama benchmark suite (CyberSecEval3 supplied the insecure-code labels)
- [[agentdojo|AgentDojo]] — the independent benchmark used for the headline numbers
