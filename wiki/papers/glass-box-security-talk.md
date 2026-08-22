---
type: talk
title: "Glass-Box Security: Interpretability for AI Defense"
created: 2026-05-03
updated: 2026-05-03
tags:
  - papers
  - talks
  - mechanistic-interpretability
  - glass-box-security
  - behavior-based-detection
  - latent-space
  - agent-observability
  - detection-engineering
  - starseer
  - unprompted-2026
status: summarized
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
year: 2026
authors: ["Carl Hurd"]
venue: "Unprompted Conference, San Francisco — Stage 2 Lecture 10, Tuesday March 3, 2026, 14:55"
source_url: "https://drive.google.com/file/d/1-0hIPu-U3rnMo6EPT-TtIcum7NMJVH9a/view"
archived_copy: ".raw/talks/2026-03-03_Carl-Hurd_Glass-Box-Security_transcript.md"
no_public_url: ""
key_claim: "Current AI security (eBPF/ETW + regex/LLM-as-judge) is the model-layer equivalent of signature-based AV: it operates only on plaintext surfaces and ignores everything that happens inside a model's forward pass. Glass-Box Security adds a second layer — mechanistic interpretability hooks that capture intent (cosine similarity of activation vectors against known-concept directions) and measure its strength (scalar projection / dot product against the total tensor magnitude) — enabling behavior-based detection rules at the neuron / residual-stream level, expressed as YARA-style semantic tripwires."
methodology: "Practitioner talk by Carl Hurd, co-founder and CTO of Starseer. No formal research paper; the talk is an engineering synthesis grounded in Hurd's background in zero-day research (Cisco Talos — ICS/embedded focus), reverse engineering (VPNFilter, Badgerboard PLC IDS), and applied ML. Includes a worked Q&A on: confidence-score limitations (addressed via model-specific empirical calibration), activation-data volume (addressed via residual-stream-only hooks), indirect prompt injection detection (activation-delta before/after RAG pipeline), detection-manifold drift under fine-tuning (layer-freeze scope determines whether re-calibration is needed)."
supports:
  - "[[agent-observability]]"
  - "[[glass-box-security]]"
  - "[[mechanistic-interpretability-for-defense]]"
  - "[[agentic-ai-security-reference-architecture]]"
related:
  - "[[carl-hurd]]"
  - "[[starseer]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[inline-gateway-vs-runtime-instrumentation]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[capability-based-authorization-talk]]"
  - "[[securing-workspace-genai-at-google-talk]]"
sources:
  - .raw/talks/2026-03-03_Carl-Hurd_Glass-Box-Security_transcript.md
  - .raw/talks/2026-03-03_Carl-Hurd_Glass-Box-Security_slides.pdf
aliases:
  - papers/glass-box-security-hurd-talk
  - glass-box-security-hurd-talk

---

# Glass-Box Security: Operationalizing Mechanistic Interpretability for Defending AI Agents

**Source:** Unprompted Conference 2026, Stage 2 Lecture 10 (Carl Hurd, Starseer). Transcript via attendee [Google Drive share](https://drive.google.com/file/d/1-0hIPu-U3rnMo6EPT-TtIcum7NMJVH9a/view); slides PDF (conference-only share). Local copies: `.raw/talks/2026-03-03_Carl-Hurd_Glass-Box-Security_{transcript.md,slides.pdf}`.

A practitioner talk by [[carl-hurd|Carl Hurd]], co-founder and CTO of [[starseer|Starseer]], delivered on Day 1 of [[unprompted-conference-march-2026|Unprompted]] (March 3 2026). The talk argues that the current state of AI security — host-based (eBPF/ETW) and network-based (prompt firewalls, LLM-as-judge) controls — is the direct analogue of pre-behavioral-EDR signature detection. Glass-Box Security is Hurd's name for the next step: using mechanistic interpretability hooks into a model's forward pass to capture *intent* and measure its *strength* as the basis for behavior-based detection rules.

## The current state: two detection families, the same blind spots

Hurd's opening taxonomy is blunt:

> "Regardless of how marketing may dress this up, we have host-based solutions, and we have network-based solutions, much like we do in all of cybersecurity."

- **Host-based (eBPF / ETW):** Intercepts plaintext prompts at the inference host. Vendors that say "we can find all the AI you're using" are using ETW or eBPF.
- **Network-based (prompt firewalls / gateways):** Intercepts in-transit plaintext — the network call from host to model provider. "If they're saying you've got to do a code change, or take this trusted certificate, they're probably using a network-based solution or prompt firewall."

Both families operate on plaintext: text before it is tokenized and embedded. Neither has any view of what happens inside the model. Both rely on the same detection primitives: regex, input sanitization, and LLM-as-judge.

**Only 5 of ~10 lifecycle stages are plaintext-visible.** Hurd walks through the prompt lifecycle: plaintext from host → network → gateway → routing (model providers route cheap queries away from expensive models) → **embedding** → **embedded routing** → **layer N … layer N+k**. The circles on this diagram (the plaintext stages) are roughly half of the full lifecycle. GPT-OPT-20B already has 24 layers. Every layer is a block where detection could theoretically happen but currently doesn't. Host-based and network-based defenses are fishing in the first half of the pond.

## Glass-Box Security: two pillars

Hurd introduces two primitives that jointly enable what he calls **behavior-based detection for neural networks** — the LLM equivalent of what modern EDRs do for OS processes.

### Pillar 1: Intent capture via cosine similarity

**Mechanism:** Forward-pass hooks collect activation vectors at specific layers of the residual stream during inference. These activations are then compared — via cosine similarity — against reference vectors representing known *concepts* (e.g., "illegality," "file deletion," "exfiltration").

**Activation matching outperforms keyword blocking.** A prompt asking "how do I rob a bank?" has no single keyword that can be reliably blocked without collateral damage. A model's activation vector for that prompt, however, will have measurable cosine alignment with a reference vector for "illegal." The detection fires on the *semantic concept* rather than the surface form.

**Model-specificity:** Intent vectors must be derived empirically per model. Hurd's example uses Llama 3.1 — running thousands of labeled prompts through a hooked instance to extract the activation cluster that represents a given concept. Cosine similarity is then measured against that model-specific cluster. There is no universal cross-model embedding.

### Pillar 2: Strength measurement via latent-space geometry

**Problem with cosine similarity alone:** Cosine similarity tells you whether two vectors *point in the same direction* in high-dimensional space. It says nothing about whether that direction is *dominant* in the current activation. A prompt might have slight cosine alignment with "illegal" while being overwhelmingly about something benign. Cosine similarity alone gives a high false-positive rate.

**Mechanism:** Scalar projection (dot product of the activation against the reference direction, divided by the reference magnitude) measures the *shadow* of the concept against the full tensor magnitude. This is "how big is the illegal intent component compared to everything else the model is currently thinking about."

**Worked example:** "How do I steal from a bank?" vs. "How do I steal a pen from a bank?" — identical surface structure, but the scalar projection of the "theft" concept against total tensor magnitude is far larger in the first case. Both have some cosine alignment; the dot-product strength distinguishes them. Hurd notes that Gemini's existing behavior already implicitly does this (it responds differently to the two prompts, though its internal mechanism is a black box to external observers).

**Behavior-based detection requires both direction and magnitude.** Intent without strength = too many false positives. Strength without direction = measuring signal without knowing what signal. The combination — cosine similarity for direction, scalar projection for magnitude — mirrors the two-component structure of mature EDR behavioral rules: "this process is doing X AND X constitutes a high fraction of its current activity."

## The OODA loop framing for agents

Hurd makes an architectural claim worth noting:

> "Agents are just models in an OODA loop."

The detection problem compounds across the loop: an agent makes *decisions* based on earlier inferences, and those decisions invoke tools, which produce new inputs that re-enter the model. Writing syntactic rules for "everything an OODA-looping agent might do" is impossible — the agent knows all the same creative bypass paths a human red-teamer would explore.

The semantic traceability argument: what you want is not "did the agent call `rm -rf`?" but "when the agent decided to call `rm -rf`, was the file-deletion intent dominant and strong throughout the reasoning chain, or did something in the environment redirect the model mid-loop?" This is **semantic traceability vs. syntactic traceability** — the distinction Hurd argues is the trust foundation for production agent deployments.

> "You need to have some traceability, and this semantic traceability versus syntactic traceability is what is going to give you more trust in the models and the agents that they did the job the way they needed to, and they were not, you know, reward hacking, or finding some loophole, removing your tests."

## The detection rule format

Hurd's proposed detection rule format is a **YARA module extension** with a semantic AI module:

```yara
rule file_deletion_intent {
    condition:
        ai.intent("file_deletion", layer=[12,18]) > 0.85
        and ai.strength("file_deletion") > 0.60
}
```

The rule would:
1. Hook residual-stream activations at layers 12 and 18.
2. Compute cosine similarity against a stored "file deletion" concept vector.
3. Compute scalar projection of the concept against total tensor magnitude.
4. Block or alert if both thresholds are exceeded.

He also endorses **Cedar** (Matt Maisel's talk, also at Unprompted) as a complementary policy language for action-level mediation. YARA for semantic detection rules on activations; Cedar for deterministic action-gate policies on tool calls.

## Engineering solutions to the four barriers

Hurd identifies four barriers to adopting this approach in production and offers concrete engineering responses:

| Barrier | Problem | Engineering solution |
|---|---|---|
| Activations unavailable for frontier models | Claude, GPT, Gemini don't expose activations via API | **Canary model for interpretability** — instrument a smaller open-weight model (e.g., Llama 3.1), run it in-line or async alongside the frontier model, use its activations as the detection signal |
| Activation data volume is enormous | GPT-OPT-20B: ~4 MB per first token; full context window ≈ **10 TB** of activation data | **Hook only the residual stream** (avoids the quadratic cost of self-attention heads); hook only the **specific layers** most predictive for the target concept (determined empirically) |
| Detection content is hard to write | Security engineers ≠ ML researchers | **Progressive context enhancement** — pass host-based and network-based metadata to the instrumented model so engineers can write rules in terms of intent × strength without knowing high-dimensional math |
| Rules can't be universal | Different agents, different use cases, different baselines | **Open-source detection standards (YARA, Cedar) extended with AI modules** — same authorship + tooling ecosystem; context passed at ingest time to handle per-agent calibration |

> [!gap] Canary model accuracy gap
> The canary-model approach assumes that a smaller open-weight model (e.g., Llama 3.1 at the time of this talk) produces activation geometry that is predictive of what a larger frontier model (Claude, GPT-4o, o3) would do. Hurd does not provide empirical evidence for cross-model activation transfer. This is the most significant unresolved assumption in the architecture. Cross-model intent generalization is an open research question in mechanistic interpretability.

## Sovereign infrastructure as a prerequisite

Hurd's third conclusion:

> "Sovereign infrastructure is a requirement for anyone that wants to take secure model usage seriously."

The argument: you cannot hook a model's forward pass if you do not control the inference stack. API-only access gives you only the output token stream. This creates a two-tier market: organizations willing to run self-hosted or private-cloud inference can use Glass-Box Security; organizations that exclusively use SaaS model APIs are limited to plaintext-surface controls (the current state).

This is a structural tension against the direction the enterprise market is moving (toward managed API endpoints from frontier-model providers). Hurd's canary-model workaround is an attempt to bridge this gap without requiring sovereign infrastructure for the frontier model itself.

## Q&A highlights

**Q: LLMs are unreliable at confidence scores. How do you handle intent and strength being relative, not absolute?**

Hurd: the calibration is model-specific and empirical, not universal. The concept vectors (reference directions) are derived from that specific model's activations on labeled training prompts. Cosine similarity is then measured against those model-derived directions — "apples to apples." There is no claim that a "file deletion intent score of 0.87" means the same thing across Llama, Claude, and GPT-4o.

**Q: Could intent and strength be plotted as a 2D vector — XY axes — and stored as a simple number pair?**

Hurd: yes, that is a valid mental model. Storing intent vectors (the reference directions) as activation vectors in a vector database, and the strength as a scalar projection, maps naturally to a 2D detection space. The practical storage requirement is modest once you've selected the relevant layers.

**Q: Can this detect indirect prompt injection — specifically, intent *change* before and after a RAG pipeline?**

Hurd: yes, and you may not even need before/after for context-enhancement RAG (which runs in the pre-fill pass). For tool-based RAG, where new content arrives mid-inference, a before/after activation comparison can detect when the injected retrieval content shifted the model's dominant intent. Hurd notes Starseer has blog posts covering this in more detail.

**Q: What if the agent's baseline intent is itself the problem — e.g., the agent was *instructed* to do something illegitimate?**

Hurd: defense in depth. Glass-Box Security is not a silver bullet — it detects *concept activation strength* against reference points, but if the operator set a malicious reference or the agent's legitimate scope includes the dangerous action, the rule won't fire. Authorization around agents (identity, decision rights, least agency) is the complementary layer. This maps exactly to the Niyikiza/Tenuo warrant architecture: warrant attenuation constrains what the agent is *allowed* to do; activation monitoring detects whether the agent is *attempting* actions outside its expected behavioral envelope.

**Q: How do detection manifolds drift under fine-tuning?**

Hurd: it depends on which layers are frozen. Standard fine-tuning practice freezes 90% of layers (early layers encode general language comprehension) and tunes the last 10% (which encode high-level concept representations). If detections were anchored to the frozen layers, they survive fine-tuning unchanged. If anchored to the tuned layers, re-calibration is needed — but only for the manifold attached to those layers.

## Placement in this wiki

| Wiki artifact | Update from this talk |
|---|---|
| [[agent-observability\|Agent Observability]] | Existing practice page has a "Glass-Box pillars" section (§6) that exactly describes this talk's approach. The talk is now its named primary source. The "canary model" and "residual-stream-only hooks" engineering solutions are new additions. |
| [[glass-box-security\|Glass-Box Security]] | **NEW concept page** — Hurd's paradigm name; distinct from the Lidzborski "black box to glass box" metaphor (audit logging) — Hurd's usage means mechanistic interpretability hooks into the model forward pass. |
| [[mechanistic-interpretability-for-defense\|Mechanistic Interpretability for Defense]] | **NEW concept page** — the underlying technique; forward-pass hooks, linear probes, sparse autoencoders, cosine similarity, scalar projection applied to security use cases rather than model research. |
| [[starseer\|Starseer]] | **NEW organization entity** — Hurd's company; building detection engineering tooling grounded in mechanistic interpretability; co-founded by Hurd (CTO). |
| [[carl-hurd\|Carl Hurd]] | **NEW person entity** — co-founder and CTO of Starseer; Cisco Talos ICS/embedded zero-day researcher; Badgerboard PLC IDS; VPNFilter reverse engineer. |
| [[inline-gateway-vs-runtime-instrumentation\|Inline Gateway vs Runtime Instrumentation]] | This talk is the clearest theoretical argument for the *runtime instrumentation* camp — specifically, instrumentation inside the model forward pass (residual-stream hooks), not just outside-the-model gateway interception. |
| [[agentic-ai-security-reference-architecture\|Agentic AI Security Reference Architecture]] | Adds a concrete implementation pattern for the Observability plane: activation-level hooks into the model forward pass as the highest-fidelity observability signal; paired with the existing OpenTelemetry-based telemetry story. |

## Comparison with sibling talks at Unprompted March 2026

| Speaker | Surface of control | Mechanism | Plaintext or latent? |
|---|---|---|---|
| [[breaking-the-lethal-trifecta-talk\|Bullen (Stripe)]] | Egress + tool calls | Smokescreen network proxy + ToolAnnotations schema | Plaintext / structural |
| [[capability-based-authorization-talk\|Niyikiza (Tenuo)]] | Delegation chain | Cryptographic warrant attenuation | Structural (pre-execution) |
| [[securing-workspace-genai-at-google-talk\|Lidzborski (Google)]] | Input + orchestration + output | 4-layer fortress + Plan-Validate-Execute | Plaintext + structural |
| **Hurd (Starseer)** | **Model internals** | **Forward-pass activation hooks (residual stream)** | **Latent / semantic** |

Hurd's talk is the only one at Unprompted March 2026 that operates *inside* the model rather than at its boundaries. The four talks are complementary and non-redundant: Bullen defends the perimeter, Niyikiza constrains delegation, Lidzborski hardens the input/output surface, and Hurd instruments the reasoning process itself.

## Open questions

> [!gap] Cross-model activation transfer
> The canary-model design assumes that activation geometry in Llama 3.1 predicts behavior in Claude or GPT-4o. No empirical support is given. Mechanistic interpretability research as of early 2026 has not established reliable cross-architecture intent transfer. This is the key validity assumption that needs third-party validation before the canary-model architecture can be treated as production-grade.

> [!gap] Threshold calibration methodology
> Hurd's rule example uses "85% cosine alignment" and "large fraction of tensor magnitude" as thresholds but states explicitly that "there is not a specific number threshold — we need to do empirical data gathering based on every model." No public benchmark exists for evaluating these thresholds at scale. Until a shared evaluation harness exists, every deployment is effectively self-calibrated.

> [!gap] Integration with existing SIEM / YARA tooling
> The YARA-with-AI-module proposal is conceptually elegant but the module does not yet exist as a public artifact (as of the talk date). Starseer's blog is referenced but no public GitHub or specification was shared in the talk or transcript.

## See also

- [[glass-box-security|Glass-Box Security]] — the paradigm concept page
- [[mechanistic-interpretability-for-defense|Mechanistic Interpretability for Defense]] — the underlying technique concept page
- [[agent-observability|Agent Observability]] — the practice page this talk extends with a latent-space tier
- [[starseer|Starseer]] — Hurd's company
- [[carl-hurd|Carl Hurd]] — speaker profile
- [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]] — architectural fork concept; this talk is the clearest argument for the instrumentation camp
- [[unprompted-conference-march-2026|Unprompted Conference — March 3–4, 2026]] — conference catalog
