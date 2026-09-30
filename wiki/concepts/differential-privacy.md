---
type: concept
title: "Differential Privacy"
address: c-000009
created: 2026-05-07
updated: 2026-09-29
tags:
  - concepts
  - data-privacy
  - cryptography
  - machine-learning
  - agentic-ai
status: developing
origin: aggregated
scope_axis:
  - sec-of-ai
aliases:
  - "DP"
  - "ε-Differential Privacy"
related:
  - "[[model-layer-attacks|Model-Layer Attacks]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]"
  - "[[maais-multilayer-agentic-ai-security|MAAIS Framework]]"
  - "[[non-human-identity|Non-Human Identity]]"
  - "[[owasp-ai-exchange]]"
  - "[[inference-exposure]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
sources:
  - "[[.raw/papers/maais-arora-hastings-2025-12-19.md]]"
  - "https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-226.pdf"
  - "https://arxiv.org/abs/1607.00133"
  - "https://arxiv.org/abs/1802.08908"
verified: 2026-09-29
verified_against:
  - ".raw/papers/maais-arora-hastings-2025-12-19.md"
verified_findings: 0
verified_note: "MAAIS archive read in full; NIST SP 800-226 and cited primary papers checked 2026-09-29."
---

# Differential Privacy

Differential privacy (DP) limits what a released result reveals about the contribution of a defined privacy unit. For an agent system, the first question is what data the guarantee covers: a training record, a person's complete data, a document, or a telemetry event. A claim of “DP” without that unit, a mechanism, and a cumulative privacy loss is incomplete. [NIST's evaluation guidance](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-226.pdf) makes the unit of privacy an explicit part of evaluating a guarantee.

## Guarantee and boundary

A randomized mechanism M is (ε, δ)-DP when, for every pair of neighboring datasets D and D′ and every set of possible outputs S, Pr[M(D) ∈ S] ≤ exp(ε) Pr[M(D′) ∈ S] + δ. The neighbor rule defines whose contribution changes between D and D′. Smaller ε usually means a tighter bound at the same δ. The δ parameter is additive slack in the bound, not a general probability that privacy “fails.” Repeated releases consume a combined budget. The guarantee applies to the mechanism's stated inputs and outputs under its assumptions, not to every datum an agent can access. [Dwork et al.](https://people.csail.mit.edu/asmith/PS/sensitivity-tcc-final.pdf) established noise calibrated to query sensitivity; [NIST](https://www.nist.gov/blogs/cybersecurity-insights/how-deploy-machine-learning-differential-privacy) explains the privacy and utility trade-off in deployed ML.

## Established mechanisms

- **DP-SGD:** Clip per-example gradients and add calibrated noise during training, with an accountant for the full training run. The resulting guarantee bounds the effect of the chosen training-data unit on the released model. It does not assert that the model memorizes nothing or that every sensitive string is impossible to extract. [Abadi et al.](https://arxiv.org/abs/1607.00133) introduced the method.
- **Private Aggregation of Teacher Ensembles (PATE):** Train teachers on disjoint sensitive-data partitions, aggregate their answers with a DP noise mechanism, and use those answers to train a student. The privacy guarantee comes from the aggregation and its accounting. PATE is a DP method, even if a control catalogue calls it only “privacy-preserving.” [Papernot et al.](https://arxiv.org/abs/1802.08908) prove and evaluate its guarantees.
- **DP output mechanisms:** Release a statistic or selection using noise calibrated to sensitivity, such as Laplace or Gaussian noise for bounded numerical queries. A claim needs the actual query, sensitivity bound, noise distribution, neighboring relation, and accounting across releases. Adding arbitrary noise to model logits or prose does not establish DP. [Dwork et al.](https://people.csail.mit.edu/asmith/PS/sensitivity-tcc-final.pdf) give the underlying sensitivity principle.

## Agent-system claims

DP-SGD and PATE address **training-data participation** in a model released from those procedures. A fine-tuned agent model can use them if its training process and privacy accounting are in scope. Neither mechanism authorizes retrieval, prevents a poisoned memory write, or protects a private document placed verbatim in a model's context.

A DP telemetry aggregate can be useful when the unit and release schedule are defined. Applying local DP to raw agent events, or DP to a retrieved-document response, requires a separately specified mechanism and utility test. For example, [Koga et al.](https://arxiv.org/abs/2412.04697) study a DP RAG algorithm and its privacy–accuracy cost; that research does not make ordinary RAG output private. Per-query noise without composition accounting is insufficient for an agent that can ask repeatedly.

## Assessment use

A reviewer of a DP claim should obtain:

- The protected data and privacy unit, including how one person's or document's multiple records are grouped.
- The neighbor definition, mechanism, sensitivity or clipping rule, and released outputs.
- The ε and δ values, accountant, number of training steps or releases, and budget exhaustion rule.
- A utility result on the deployment's task and an attack test relevant to the claimed exposure.

[[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]] assesses retrieval entitlements, corpus integrity, memory boundaries, and relevant inference risks. DP may treat a separate privacy risk, but it does not satisfy those outcomes by itself. See [[model-layer-attacks|Model-Layer Attacks]] for training-data extraction and membership inference; model-function replication is a different objective.

<!-- sources:auto -->
## Sources

- [Securing Agentic AI Systems -- A Multilayer Security Framework](https://arxiv.org/abs/2512.18043)
- [NIST SP 800-226, final](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-226.pdf)
- [Deep Learning with Differential Privacy](https://arxiv.org/abs/1607.00133)
- [Scalable Private Learning with PATE](https://arxiv.org/abs/1802.08908)
<!-- /sources -->
