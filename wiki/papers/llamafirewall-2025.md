---
type: paper
title: "LlamaFirewall Guardrail Paper"
created: 2026-06-23
updated: 2026-06-23
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
sources:
  - "https://arxiv.org/abs/2505.03574"
  - "[[.raw/papers/llamafirewall-arxiv-2505-03574-2026-06-23.md]]"
---

# LlamaFirewall Guardrail Paper

Source paper for [[llamafirewall|LlamaFirewall]], Meta's open-source agent guardrail framework: *"LlamaFirewall: An open source guardrail system for building secure AI agents"* ([arXiv:2505.03574](https://arxiv.org/abs/2505.03574), submitted 2025-04-29, 19 authors, corresponding author [[joshua-saxe|Joshua Saxe]]). The framework ships as part of Meta's [[purple-llama|Purple Llama]] project.

The thesis: chatbot-era safeguards — model fine-tuning, content moderation — do not cover the risks agents take on when they act on untrusted inputs (webpages, emails, retrieved documents) and write code that runs. The paper proposes a real-time guardrail monitor as a *final* defense layer, with system-level, use-case-specific policy enforcement rather than a single universal filter.

## Three guardrails

| Guardrail | Layer | Mechanism | Targets |
|---|---|---|---|
| **PromptGuard 2** | Input | DeBERTa jailbreak classifier (86M and 22M variants) | Explicit jailbreak / [[prompt-injection\|prompt injection]] in inputs |
| **AlignmentCheck** | Reasoning | Few-shot chain-of-thought auditor over the agent's reasoning trace (guardrail LLM: Llama 3.3 70B / Llama 4 Maverick) | Goal hijacking, [[indirect-prompt-injection\|indirect prompt injection]] |
| **CodeShield** | Output | Static analysis (Semgrep + regex) across eight languages | Insecure generated code before execution |

AlignmentCheck is the novel and still-experimental piece: a prospective control that inspects *why* the agent decided to act, catching injections that pass input-layer detection but surface as off-goal reasoning. It is the mechanism the wiki tracks under [[prompt-injection-containment|prompt-injection containment]]'s reasoning-layer tier.

## Reported results

- **PromptGuard 2 (86M):** 98% AUC (English), 97.5% recall at 1% false-positive rate, 92.4 ms latency. The 22M variant trades recall for speed: 88.7% recall at 1% FPR, 19.3 ms.
- **Combined PromptGuard 2 + AlignmentCheck on [[agentdojo|AgentDojo]]:** attack success rate 17.6% → 1.75% (a >90% reduction) at roughly 5% utility cost. AlignmentCheck alone delivers an 83% attack-success reduction.
- **CodeShield:** 96% precision, 79% recall on insecure-code detection, 60–70 ms typical latency.

Self-evaluation caveat: these are vendor-published numbers. AgentDojo is the independent comparator (peer-reviewed, NeurIPS 2024), which is why the same 17.6% → 1.75% figures are defensible across both Meta's paper and third-party use. An external deployment test tells the other half of the story — Uber's [[adr-agentic-detection-system|ADR]] evaluation runs LlamaFirewall (Llama Guard 3-8B plus heuristics) as a baseline and finds it recall-strong on AgentDojo injection (0.974) but precision-poor on benign enterprise traffic ([[adr-bench|ADR-Bench]] precision 0.167), the class-imbalance failure mode of a high-recall guardrail.

## Relevance to this corpus

The paper is the primary citation behind several wiki claims: the two-layer (input + reasoning) detection model in [[prompt-injection-containment|Prompt Injection Containment]], the PromptGuard 2 numbers cited on [[agentdojo|AgentDojo]], and LlamaFirewall's placement at the model layer in [[security-controls-for-ai-stacks|Security Controls for AI Stacks]]. It also anchors the [[agentic-ai-security-cmm-2026|CMM]] D4 runtime-guardrail evidence: a layered, semantically-aware guardrail that fails closed on critical paths is a concrete L4/L5 control instance.

## See also

- [[llamafirewall|LlamaFirewall]] — the product page
- [[purple-llama|Purple Llama]] — Meta's umbrella project that ships it
- [[cyberseceval|CyberSecEval]] — the Purple Llama benchmark suite (CyberSecEval3 supplied the insecure-code labels)
- [[agentdojo|AgentDojo]] — the independent benchmark used for the headline numbers
