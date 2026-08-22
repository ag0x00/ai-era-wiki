---
type: concept
title: "Glass-Box Security"
created: 2026-05-03
updated: 2026-06-22
tags:
  - concepts
  - mechanistic-interpretability
  - detection-engineering
  - agent-observability
  - behavior-based-detection
  - latent-space
status: seed
no_public_url: "Coined by Carl Hurd (Starseer) in a private conference talk (Unprompted 2026); no public canonical document"
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
related:
  - "[[mechanistic-interpretability-for-defense]]"
  - "[[agent-observability]]"
  - "[[inline-gateway-vs-runtime-instrumentation]]"
  - "[[glass-box-security-talk]]"
  - "[[starseer]]"
  - "[[carl-hurd]]"
  - "[[nist-ai-800-4|NIST AI 800-4]]"
sources:
  - .raw/talks/2026-03-03_Carl-Hurd_Glass-Box-Security_transcript.md
---

# Glass-Box Security

A paradigm for AI agent defense introduced by [[carl-hurd|Carl Hurd]] (Starseer) at Unprompted March 2026. Glass-Box Security is the practice of instrumenting a neural network's *forward pass* — rather than its input/output plaintext surfaces — to capture and measure the semantic concepts a model is actively processing, and using that information as the basis for detection and response rules.

## The naming distinction

"Glass box" is used in two distinct senses in this wiki; they are complementary but not the same:

| Usage | Source | Meaning |
|---|---|---|
| **Lidzborski / Google Workspace** | [[securing-workspace-genai-at-google-talk\|Lidzborski talk]] | "From black box to glass box" — comprehensive **audit logging** of all GenAI interactions for enterprise security teams; observability of what the agent *did* after the fact |
| **Hurd / Starseer** | [[glass-box-security-talk\|Hurd talk]] | **Mechanistic interpretability hooks** into the model's forward pass — observability of what the agent is *thinking* during inference, before it acts |

Hurd's usage is the more technically specific one. The term "Glass-Box Security" with a capital G is Hurd's coined paradigm name for the mechanistic interpretability approach.

## Core argument

Current AI security controls (eBPF/ETW host-based monitoring, prompt firewalls, regex, LLM-as-judge) operate only on the *plaintext* lifecycle stages of a model inference request. A forward pass through a model with 24+ layers involves far more computation than the plaintext stages capture. Every transformer layer is a block of processing that today's defenders are flying blind through.

This is the lack of direct visibility into model properties that [[nist-ai-800-4|NIST AI 800-4]] names as a cross-cutting barrier to monitoring deployed AI. Glass-Box Security is one attempt to close that specific gap by reading the forward pass rather than only the input/output surfaces.

Hurd's argument: this is exactly the situation that existed in endpoint security before behavioral EDR — defenders were using signature-based AV (pattern matching on plaintext patterns) while attackers operated in memory, in JIT-compiled code, in process injection. The industry solved it by moving to behavioral detection (process lineage, memory access patterns, API call sequences). AI security needs the same transition.

**Glass-Box Security is behavioral EDR for neural networks.** The two pillars — intent (cosine similarity of activations against concept reference directions) and strength (scalar projection of the concept against total tensor magnitude) — are the neural-network equivalents of "what is this process doing" (intent) and "how dominant is this behavior in the current execution" (strength) from EDR behavioral analytics.

## Two pillars

**Pillar 1 — Intent capture:** Forward-pass hooks collect activation vectors from the model's residual stream at selected layers. These vectors are compared against stored reference vectors for known concepts (e.g., "file deletion," "illegality," "exfiltration") using **cosine similarity**. A high cosine similarity score indicates the model is processing a semantic concept in a direction close to the reference.

**Pillar 2 — Strength measurement:** Cosine similarity alone yields high false-positive rates — many prompts will have minor alignment with dangerous concepts without those concepts being dominant. **Scalar projection** (dot product of the activation against the reference direction, normalized by reference magnitude) measures how large a "shadow" the concept casts against the total activation magnitude. Only when *both* cosine alignment is high *and* scalar projection is significant does the concept constitute a dominant part of what the model is processing.

Together, a detection rule can be expressed as: "block any request where the *file deletion* concept has cosine alignment > 0.85 AND scalar projection > 0.60 of total tensor magnitude at layers 12 and 18."

## Relationship to mechanistic interpretability research

Glass-Box Security applies existing mechanistic interpretability techniques — forward-pass activation collection, linear probes, cosine similarity, sparse autoencoders — to a *defensive security* use case rather than to AI alignment or capabilities research. The research foundations are established; the application to runtime threat detection is Starseer's contribution.

See [[mechanistic-interpretability-for-defense|Mechanistic Interpretability for Defense]] for the underlying technique.

## Current limitations

- **Sovereign infrastructure or canary model required.** API-only access to frontier models (Claude, GPT-4o, Gemini) does not expose activation vectors. Glass-Box Security at its most direct requires self-hosted inference. Starseer's workaround is a *canary model* — a smaller open-weight model (e.g., Llama 3.1) instrumented in parallel — but this depends on cross-model activation transfer, which has not been empirically validated.
- **Thresholds are empirically calibrated per model.** There is no universal cosine-similarity threshold for "dangerous." Every deployment requires a labeled prompt corpus and an empirical calibration run against the specific model being instrumented.
- **No public tooling as of March 2026.** The YARA AI module and the public Starseer SDK were not released at talk time.

## See also

- [[mechanistic-interpretability-for-defense|Mechanistic Interpretability for Defense]] — technique basis
- [[agent-observability|Agent Observability]] — practice context (§6 "Internal EDR / Glass-Box pillars")
- [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]] — Glass-Box Security is the clearest theoretical grounding for the runtime instrumentation camp
- [[glass-box-security-talk|Hurd — Glass-Box Security]] — source talk
