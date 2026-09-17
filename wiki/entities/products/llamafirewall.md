---
type: entity
title: "LlamaFirewall"
created: 2026-04-30
updated: 2026-09-16
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
  - "[[chain-of-thought-monitorability]]"
sources:
  - "https://arxiv.org/abs/2505.03574"
  - "[[.raw/papers/llamafirewall-arxiv-2505-03574-2026-06-23.md]]"
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - "[[.raw/papers/emerging-cybersecurity-practices-for-agentic-ai-applications.md]]"
verified: 2026-09-16
verified_against:
  - ".raw/papers/llamafirewall-arxiv-2505-03574-2026-06-23.md"
verified_findings: 0
---

# LlamaFirewall

**Sources:** [LlamaFirewall paper, arXiv:2505.03574](https://arxiv.org/abs/2505.03574) (see [[llamafirewall-2025|the summary page]]) · [Purple Llama repo](https://github.com/meta-llama/PurpleLlama)

**Meta AI** published LlamaFirewall as an open-source AI guardrail framework ([arXiv:2505.03574](https://arxiv.org/abs/2505.03574), submitted April 2025) and distributes it in the [[purple-llama|Purple Llama]] project. It carries three specialized guardrail components, each at a different point in the agent execution pipeline. The framework runs as a *final* runtime defense layer and supports system-level, use-case-specific safety policies, so a deployment writes one policy per use case in place of a single universal filter.

## Architecture: Three Components

### PromptGuard 2
Input-side DeBERTa classifier for jailbreak and [[prompt-injection|prompt injection]] detection, shipped in two sizes — **86M** (98% AUC, 97.5% recall at 1% FPR, 92.4 ms) and **22M** (88.7% recall at 1% FPR, 19.3 ms) — trading recall for latency. Operates before the LLM processes the input.[^lf]

### AlignmentCheck
Inspects the agent's **chain-of-thought reasoning** before tool execution for signs of goal hijacking, using a guardrail LLM (Llama 3.3 70B / Llama 4 Maverick). This is a prospective control — it fires after the model has reasoned but before it acts, catching [[indirect-prompt-injection|indirect injections]] that pass input-layer detection but manifest as abnormal reasoning. Addresses OWASP ASI01 (Agent Goal Hijack). Still experimental per the paper; alone it cuts attack success ~83% on [[agentdojo|AgentDojo]]. Combined with PromptGuard 2, the framework drops AgentDojo attack success from **17.6% to 1.75%** at roughly 5% utility cost.[^lf]

### CodeShield
Static analysis (Semgrep + regex, eight languages) for **LLM-generated code** before execution. Catches dangerous patterns (shell injection, file deletion, credential access) in code the agent writes and is about to run: 96% precision, 79% recall, 60–70 ms latency, validated against [[cyberseceval|CyberSecEval3]]-labeled completions.[^lf]

## Positioning

LlamaFirewall operates at the **input and reasoning layers** (model layer in the [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] taxonomy), so a deployment pairs it with platform-level controls to get containment. Its guardrails must run at the framework or runtime layer, because the effectiveness figures the paper reports apply to that placement. A guardrail written as a prompt instruction carries no such figure.

AlignmentCheck has not been updated since May 2025: the LlamaFirewall repository is active, with its directory last touched in August 2026, and the last commit reaching AlignmentCheck specifically is 2025-05-13, which leaves it frozen inside a live project with no maturity label and no revised figures on the README or the docs. Its input is also moving, because chain-of-thought monitorability is reported as declining across model generations, so a reasoning-trace auditor carries a dependency on a property the model vendor controls ([[chain-of-thought-monitorability|Chain-of-Thought Monitorability]]).

## Relationship to Traditional Security

LlamaFirewall maps to IPS/WAF at the model layer, applying pattern-matching and behavioral analysis to inputs and reasoning where an IPS applies them to network packets and HTTP requests. AlignmentCheck sits outside that mapping. It reads the agent's reasoning trace before the action, where an IPS or WAF reads traffic the system has already emitted.

## External baseline results

Input and reasoning-layer guardrails optimize for recall on injection and pay for it in precision on benign enterprise traffic. Uber's [[adr-agentic-detection-system|ADR]] paper shows the trade in measurements, using LlamaFirewall (Llama Guard 3-8B plus heuristic rules, official thresholds) as one of three detection baselines.

On the enterprise [[adr-bench|ADR-Bench]] (260 benign / 42 malicious), LlamaFirewall fires 40 false positives, scoring precision 0.167 and F1 0.178, the lowest of the four detectors and 19x ADR's cost per task.[^adr] Class imbalance turns that false-positive rate into an unusable production alerting stream. On [[agentdojo|AgentDojo]] (prompt injection), the picture inverts: LlamaFirewall reaches recall 0.974, near-best at catching attacks, while 21 false alarms drag precision to 0.638.[^adr]

[^lf]: All component figures from the [[llamafirewall-2025|LlamaFirewall paper]], [arXiv:2505.03574](https://arxiv.org/abs/2505.03574) (Meta, 2025).

[^adr]: §5 *Evaluation*, Table 2, [arXiv:2605.17380](https://arxiv.org/abs/2605.17380): LlamaFirewall scores precision 0.167 / recall 0.190 / F1 0.178 with 40 FPs on ADR-Bench, and precision 0.638 / recall 0.974 / F1 0.771 with 21 FPs on AgentDojo.

## See Also

- [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]] — the practice page covering the two-layer detection + containment model
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] — model layer where LlamaFirewall operates
- [[meta|Meta]] — publisher
