---
type: entity
title: "LlamaFirewall"
created: 2026-04-30
updated: 2026-06-23
tags:
  - entities
  - products
  - guardrails
  - open-source
  - meta
status: developing
scope_axis:
  - sec-of-ai
entity_type: product
homepage: "https://github.com/meta-llama/PurpleLlama"
role: "Open-source AI guardrail framework by Meta; three specialized guardrail components for prompt injection, goal hijacking, and generated code"
related:
  - "[[llamafirewall-2025]]"
  - "[[purple-llama]]"
  - "[[cyberseceval]]"
  - "[[agentdojo]]"
  - "[[mcp-security]]"
  - "[[agent-observability]]"
  - "[[prompt-injection-containment]]"
  - "[[security-controls-for-ai-stacks]]"
sources:
  - "https://arxiv.org/abs/2505.03574"
  - "[[.raw/papers/llamafirewall-arxiv-2505-03574-2026-06-23.md]]"
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - "[[.raw/papers/emerging-cybersecurity-practices-for-agentic-ai-applications.md]]"
---

# LlamaFirewall

**Sources:** [LlamaFirewall paper, arXiv:2505.03574](https://arxiv.org/abs/2505.03574) (see [[llamafirewall-2025|the summary page]]) · [Purple Llama repo](https://github.com/meta-llama/PurpleLlama)

Open-source AI guardrail framework published by **Meta AI** ([arXiv:2505.03574](https://arxiv.org/abs/2505.03574), submitted April 2025) and distributed in the [[purple-llama|Purple Llama]] project. Designed for building secure AI agents; provides three specialized guardrail components that operate at different points in the agent execution pipeline. The framework is a *final* runtime defense layer supporting system-level, use-case-specific safety policies rather than a single universal filter.

## Architecture: Three Components

### PromptGuard 2
Input-side DeBERTa classifier for jailbreak and [[prompt-injection|prompt injection]] detection, shipped in two sizes — **86M** (98% AUC, 97.5% recall at 1% FPR, 92.4 ms) and **22M** (88.7% recall at 1% FPR, 19.3 ms) — trading recall for latency. Operates before the LLM processes the input.[^lf]

### AlignmentCheck
Inspects the agent's **chain-of-thought reasoning** before tool execution for signs of goal hijacking, using a guardrail LLM (Llama 3.3 70B / Llama 4 Maverick). This is a prospective control — it fires after the model has reasoned but before it acts, catching [[indirect-prompt-injection|indirect injections]] that pass input-layer detection but manifest as abnormal reasoning. Addresses OWASP ASI01 (Agent Goal Hijack). Still experimental per the paper; alone it cuts attack success ~83% on [[agentdojo|AgentDojo]]. Combined with PromptGuard 2, the framework drops AgentDojo attack success from **17.6% to 1.75%** at roughly 5% utility cost.[^lf]

### CodeShield
Static analysis (Semgrep + regex, eight languages) for **LLM-generated code** before execution. Catches dangerous patterns (shell injection, file deletion, credential access) in code the agent writes and is about to run: 96% precision, 79% recall, 60–70 ms latency, validated against [[cyberseceval|CyberSecEval3]]-labeled completions.[^lf]

## Positioning

LlamaFirewall operates at the **input and reasoning layers** (model layer in the [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] taxonomy). For containment, it is combined with platform-level controls. The key architectural note: LlamaFirewall guardrails should be deployed at the framework/runtime layer, not as prompt instructions, to achieve their effectiveness guarantees.

## Relationship to Traditional Security

LlamaFirewall maps to IPS/WAF at the model layer — pattern-matching and behavioral analysis on inputs and reasoning rather than network packets and HTTP requests. AlignmentCheck is novel: no traditional equivalent exists for prospective chain-of-thought auditing.

## As an external baseline

Uber's [[adr-agentic-detection-system|ADR]] paper uses LlamaFirewall (Llama Guard 3-8B plus heuristic rules, official thresholds) as one of three detection baselines. The results split by benchmark and expose LlamaFirewall's operating point. On the enterprise [[adr-bench|ADR-Bench]] (260 benign / 42 malicious), it fires 40 false positives — precision 0.167, F1 0.178, the lowest of the four detectors and 19x ADR's cost per task — the failure mode that makes a high-false-positive guardrail unusable for production alerting under class imbalance.[^adr] On [[agentdojo|AgentDojo]] (prompt injection), the picture inverts: LlamaFirewall reaches recall 0.974, near-best at catching attacks, but 21 false alarms drag precision to 0.638.[^adr] The takeaway is not that one detector is better but that input/reasoning-layer guardrails optimize for recall on injection and pay for it in precision on benign enterprise traffic.

[^lf]: All component figures from the [[llamafirewall-2025|LlamaFirewall paper]], [arXiv:2505.03574](https://arxiv.org/abs/2505.03574) (Meta, 2025).

[^adr]: §5 *Evaluation*, Table 2, [arXiv:2605.17380](https://arxiv.org/abs/2605.17380): LlamaFirewall scores precision 0.167 / recall 0.190 / F1 0.178 with 40 FPs on ADR-Bench, and precision 0.638 / recall 0.974 / F1 0.771 with 21 FPs on AgentDojo.

## See Also

- [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]] — the practice page covering the two-layer detection + containment model
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] — model layer where LlamaFirewall operates
- [[meta|Meta]] — publisher
