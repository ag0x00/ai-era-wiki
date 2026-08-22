---
type: concept
title: "Mechanistic Interpretability for Defense"
created: 2026-05-03
updated: 2026-08-19
tags:
  - concepts
  - mechanistic-interpretability
  - detection-engineering
  - latent-space
  - behavior-based-detection
  - forward-pass
  - agent-observability
status: seed
origin: aggregated
no_public_url: "Defensive framing synthesized from a private conference talk (Carl Hurd, Unprompted 2026); no single external canonical source"
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
related:
  - "[[measuring-agent-effectiveness-talk|Measuring Agent Effectiveness]]"
  - "[[glass-box-security]]"
  - "[[agent-observability]]"
  - "[[glass-box-security-talk]]"
  - "[[inline-gateway-vs-runtime-instrumentation]]"
  - "[[starseer]]"
  - "[[carl-hurd]]"
  - "[[nist-ai-800-4|NIST AI 800-4]]"
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
sources:
  - .raw/talks/2026-03-03_Carl-Hurd_Glass-Box-Security_transcript.md
---

# Mechanistic Interpretability for Defense

The application of mechanistic interpretability techniques — originally developed to understand how neural networks encode knowledge and compute functions — to runtime security monitoring of AI agents. Rather than explaining model behavior post-hoc for research purposes, mechanistic interpretability for defense deploys the same techniques as *live detection infrastructure* during model inference.

> [!note] Distinct from "Operational XAI for Action Gating"
> "Explainable AI" has two senses in the wiki. **This page** covers the *researcher / defender* sense: probing model internals to detect malicious intent. The *operator* sense — agents emitting human-readable justifications that gate high-impact actions before they execute — lives at [[operational-xai-for-gating|Operational XAI for Action Gating]]. The two are complementary: mechanistic interpretability gives the defender a model-internal signal; operational XAI gives the operator a model-external artifact. Both are referenced under [[maais-multilayer-agentic-ai-security|MAAIS]] Layer 5 (Accountability and Trustworthiness) but solve different problems.

## Background: mechanistic interpretability as a research field

Mechanistic interpretability (MI) is a branch of AI interpretability research that attempts to reverse-engineer the *internal computations* of neural networks — identifying which neurons, layers, and circuits are responsible for which behaviors. The field's central hypothesis is the **linear representation hypothesis**: that neural networks represent concepts as linear directions in high-dimensional activation space, such that concepts can be located, measured, and manipulated via linear algebra.

Key techniques in the field (referenced in Hurd's talk and sourced from Anthropic and Google DeepMind blog posts):

| Technique | What it does |
|---|---|
| **Linear probes** | Train a simple linear classifier on activation vectors to detect whether a specific concept is encoded in a layer |
| **Sparse autoencoders (SAEs)** | Decompose polysemantic activation vectors into monosemantic features — each feature ideally corresponding to one interpretable concept |
| **Differential prompt analysis** | Compare activations across paired prompts to isolate the computational signature of a specific concept or capability |
| **Forward-pass hooks** | Attach inspection callbacks to specific layers in the model's computation graph, collecting activation tensors without modifying inference |

## From research to defense: the security translation

The research use of MI is to understand models. The defensive use, as proposed by [[carl-hurd|Carl Hurd]] via [[glass-box-security|Glass-Box Security]], is to detect malicious intent in real time during inference:

| Research use | Defensive use |
|---|---|
| "What does this neuron compute?" | "Is a dangerous concept activating strongly during this inference?" |
| Identify circuits responsible for capabilities | Identify layers that are maximally predictive of high-risk concept activations |
| Post-hoc explanation of model behavior | Real-time alerting and blocking before the model outputs a harmful action |
| Build understanding of model internals | Build detection rules (YARA modules, Cedar policies) that fire on activation patterns |

## Technical primitives

### Concept vectors

A concept vector (also called a *direction* or *feature vector*) is a unit vector in the model's activation space that represents a specific concept (e.g., "file deletion," "illegality," "exfiltration"). Derived empirically: run hundreds to thousands of labeled prompts through a hooked model, collect residual-stream activations at a target layer, compute the mean activation for positive examples minus mean for negative examples, normalize to unit length.

**Model-specificity:** Concept vectors are not transferable across model architectures. A "file deletion" concept vector derived from Llama 3.1 is not the same direction in activation space as the analogous concept in GPT-4o or Claude 3.7 Sonnet.

### Cosine similarity for intent detection

The dot product of the current activation vector with the concept vector, divided by the product of their magnitudes, gives a scalar in [-1, 1] indicating how closely aligned the current activation is with the concept direction. A value near +1 indicates high intent alignment; near 0 indicates orthogonality; near -1 indicates the concept is suppressed or reversed.

### Scalar projection for strength measurement

A high cosine similarity says the activation *points in the same direction* as the concept. It does not say whether that direction is *dominant* in the current activation. Scalar projection (dot product without normalization by the current activation magnitude) gives the *component* of the current activation along the concept direction. Dividing by total tensor magnitude yields a normalized strength measure.

**Neither measure suffices alone.** A prompt that briefly touches on financial crime while primarily discussing tax filing might have moderate cosine similarity with "illegality" but a very small scalar projection (the concept is a tiny component of a large, diverse activation). Only strong cosine similarity *and* significant scalar projection together justify a high-confidence detection.

### Superposition and the MI challenge

Neural networks exhibit **superposition**: because there are far more concepts than neurons (the embedding dimension), multiple concepts share the same neurons, represented as different directions in the same high-dimensional space. This means:

- A single neuron is not monosemantic — it responds to many concepts.
- The relevant signal for a specific concept is spread across many neurons.
- Sparse autoencoders are one approach to decomposing superposed representations, but are computationally expensive at inference time.

Hurd's approach sidesteps full SAE decomposition by working at the residual-stream level (the sum of all layer contributions, rather than individual attention heads) and measuring the projection of specific concept directions — a coarser but computationally tractable approximation.

### Layer selection

Not all layers are equally predictive for all concepts. Early layers in a transformer encode general syntactic and lexical features; later layers encode higher-level semantic and task-relevant features. The defensive practitioner's method is empirical: run labeled prompts through all layers, identify which layers show the clearest separation between positive and negative concept examples, and hook only those layers. This reduces computational overhead and focuses on the signal-rich layers.

## Engineering constraints and solutions

| Constraint | Implication | Solution |
|---|---|---|
| Frontier models (Claude, GPT-4o) don't expose activations | Can't hook a model you don't control | **Canary model** — smaller open-weight model instrumented in parallel; activation geometry assumed predictive of frontier model intent |
| Raw activation data is enormous | GPT-OPT-20B ≈ 4 MB / first token; full context ≈ 10 TB | **Hook only residual stream at target layers** — avoids quadratic self-attention cost, reduces data by 10–100× |
| No universal thresholds | Every model has its own concept geometry | **Empirical per-model calibration** — labeled prompt corpus + calibration run per deployment |

## Connection to the broader observability stack

Mechanistic interpretability for defense sits at the innermost layer of the agent observability stack:

```
[host / network plaintext layer] ← eBPF, ETW, prompt firewalls
       ↓
[tool call / action layer]       ← Cedar policies, ToolAnnotations, capability warrants
       ↓
[model output layer]             ← output hardening, link sanitization, LLM-as-judge
       ↓
[model forward-pass layer]       ← residual-stream hooks, cosine similarity, scalar projection
                                    ← THIS IS WHAT MI-FOR-DEFENSE ADDS
```

It is not a replacement for the other layers but the deepest available signal: the only layer where the model's actual intent (rather than its output) is directly observable. It targets exactly the lack of direct visibility into model properties that [[nist-ai-800-4|NIST AI 800-4]] documents as a cross-cutting barrier to monitoring deployed AI.

## Open research questions

> [!gap] Cross-model activation transfer
> The canary model pattern assumes intent directions in Llama 3.1 are predictive of behavior in Claude or GPT-4o. As of early 2026, no published research validates cross-architecture activation transfer for security-relevant concept directions. This is one of two load-bearing assumptions in any Glass-Box Security deployment that relies on a canary model rather than direct frontier-model instrumentation; the callout below names the second.

> [!gap] Adversarial robustness of concept vectors
> A detector an attacker knows about can be attacked. The general form of this question is settled in published taxonomy: the [[owasp-ai-exchange|OWASP AI Exchange]] names detector networks run as preprocessing or in parallel to the main model as an implementation of `EVASION INPUT HANDLING`, and states that adversarial attacks could be crafted to circumvent the detector network and fool the main model, with samples craftable to evade both ([`/go/evasioninputhandling/`](https://owaspai.org/go/evasioninputhandling/)). A canary model hooked in parallel is a detector of that shape. Two questions stay open for the activation-space case specifically: whether a residual-stream projection is attackable on the same terms as a separately trained detector network, and how much perturbation budget suppressing a concept direction costs an attacker who must still reach the malicious goal. The open-weight canary the engineering constraints table selects puts the detector in the Exchange's perfect-knowledge tier for any attacker who learns which canary is deployed, since the weights and gradients are public ([`/go/perfectknowledgeevasion/`](https://owaspai.org/go/perfectknowledgeevasion/)). The canary-model pattern therefore carries two load-bearing assumptions rather than one: that activations transfer across architectures, and that the detector's public weights do not hand an attacker the gradient.

> [!gap] Real-time performance at production scale
> Hurd's architectural choices (residual-stream only, target layers only) are motivated by reducing computation, but no performance numbers are provided in the talk. Production-scale latency benchmarks for activation-based detection do not appear in the public literature as of the talk date.

## See also

- [[glass-box-security|Glass-Box Security]] — the paradigm built on these techniques (Hurd / Starseer)
- [[agent-observability|Agent Observability]] — the practice context
- [[glass-box-security-talk|Hurd — Glass-Box Security]] — primary source
- [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]] — the architectural fork; MI-for-defense is the extreme-instrumentation pole

- [[measuring-agent-effectiveness-talk|Measuring Agent Effectiveness]] — frames this as one of only two available paths to verifying an agent will act correctly, the other being behavioral evaluation. The talk's argument is that the verification problem is what trusting an agent with consequential actions actually creates.
