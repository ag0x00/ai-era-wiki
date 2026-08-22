---
type: concept
title: "Differential Privacy"
address: c-000009
created: 2026-05-07
updated: 2026-08-20
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
---

# Differential Privacy

Differential privacy (DP) is a mathematical framework for guaranteeing that the output of a computation reveals approximately the same information whether or not any individual record was included in the input. Coined by Cynthia Dwork and colleagues (2006), DP gives quantifiable privacy guarantees rather than ad-hoc anonymization, and is the canonical defensive primitive against [[model-layer-attacks|model inversion, membership inference, and certain extraction attacks]] on machine-learning systems. The [[owasp-ai-exchange|OWASP AI Exchange]] carries several DP-adjacent techniques inside its sensitive-data-limitation control group rather than as a standalone entry, a routing property this page records below.

## Definition

A randomized algorithm `M` satisfies **ε-differential privacy** if, for any two datasets `D` and `D'` that differ in a single record, and for any output `S`:

```
Pr[M(D) ∈ S] ≤ e^ε · Pr[M(D') ∈ S]
```

The parameter `ε` (epsilon) is the **privacy budget**: smaller ε means stronger privacy. **(ε, δ)-DP** is a relaxation that admits a small probability `δ` of the guarantee failing — common in practice because pure ε-DP is hard to achieve at useful utility.

## Core mechanisms

- **Laplace mechanism** — add Laplace-distributed noise to numerical outputs. Provides ε-DP for queries with bounded sensitivity.
- **Gaussian mechanism** — add Gaussian noise. Provides (ε, δ)-DP; tighter composition properties than Laplace for repeated queries.
- **Exponential mechanism** — for discrete outputs (e.g., model selection), sample from a distribution weighted by a utility function.
- **DP-SGD** (Differentially Private Stochastic Gradient Descent) — clip per-example gradients then add Gaussian noise during training. The standard DP-training algorithm; implemented in TensorFlow Privacy, Opacus (PyTorch), and JAX-based libraries. Privacy budget accumulates across training steps via composition theorems.
- **Local differential privacy (LDP)** — noise added at the data source before centralization. Used by Apple (telemetry) and Google (Chrome's RAPPOR, federated analytics). Trades off privacy budget against statistical utility more aggressively than central DP.
- **Objective function perturbation** — add noise to a model's training objective, calibrated to the objective function's sensitivity to individual data points and to a desired privacy level (epsilon), so the trained model does not exactly fit the original data. The [[owasp-ai-exchange|OWASP AI Exchange]] names this a differential privacy technique under `OBFUSCATE TRAINING DATA`.[^aix-obfuscate]

Private Aggregation of Teacher Ensembles (PATE) sits adjacent to this list rather than inside it. The Exchange calls it "a privacy-preserving machine learning technique," not a differential privacy technique: disjoint teacher models each train on a separate data subset, their predictions are aggregated with added noise, and a student model trains on that noised aggregate rather than on the sensitive data directly.[^aix-obfuscate] Tokenization is stated to *align with the principles of* differential privacy rather than to implement it, which keeps it out of the mechanism list above on the same basis.[^aix-obfuscate]

## Application to agentic AI

| Surface | DP application | Why it matters |
|---|---|---|
| **Training / fine-tuning** | DP-SGD on model training; output a model that does not memorize individual training records | Defends against [[model-layer-attacks\|model inversion]] and membership inference attacks recovering proprietary or sensitive training data |
| **RAG / retrieved-context responses** | Noise the answers an agent returns about sensitive documents; or apply DP at retrieval-aggregation time | Limits how much an attacker can extract about a single document by querying repeatedly |
| **Federated learning across agents** | Local DP on each agent's gradient updates before aggregation | Prevents the central aggregator (or any single peer) from recovering an individual agent's training data |
| **Agent-to-agent telemetry** | Local DP on behavior signals shared between agents (e.g., trust scores) | Prevents downstream agents from inferring individual upstream-agent activity patterns |
| **Inference-time output randomization** | Small Gaussian noise on logits before sampling | Slows model-extraction attacks (which rely on stable query-response pairs) |

## Standards and tooling

- **NIST SP 800-188** — Trustworthy and Responsible AI: data sanitization and de-identification practitioner guidance.
- **NIST IR 8053** — De-Identification of Personal Information.
- **OpenDP** — open-source DP library (Harvard / Microsoft / OpenDP Initiative). Production-grade implementation of standard mechanisms.
- **TensorFlow Privacy** — Google's reference DP-SGD implementation.
- **Opacus** — Meta's PyTorch DP library; widely used for DP-SGD training.
- **Google's Differential Privacy library** — Java / Go / C++ / Python; production-tested at Google.

## Limitations

- **Privacy-utility trade-off.** Small ε buys strong privacy but degrades model accuracy. Real deployments routinely use ε ∈ [1, 10]; ε = 1 is "strong" by academic convention; ε = 10 is closer to "directional." Apple's iOS-telemetry deployments use ε per-event in single digits but accumulate across days.
- **Privacy-budget management.** Composition theorems govern how much privacy is spent across queries. Long-running agents that re-query the same DP-protected interface accumulate privacy cost; eventually the budget exhausts and the interface must refuse further queries or rotate. Naïve deployments forget composition and effectively provide much weaker DP than advertised.
- **Doesn't defend against all model attacks.** DP at training defends against inversion and membership inference but **does not** defend against [[model-layer-attacks|model extraction]] (which can occur even with DP-trained models, since the attacker is recovering the *function*, not training records). Inference-time output randomization is needed to slow extraction.
- **Hard to compose with RAG.** Differential privacy on retrieved-context responses is an active research area; production-grade DP for RAG isn't yet standard.
- **Not a substitute for access control.** DP protects against information leakage *given* a query; it does not control who can query in the first place. Pair with [[non-human-identity|NHI]] / authorization controls.
- **Residual re-identification risk.** The [[owasp-ai-exchange|OWASP AI Exchange]] states two residuals for obfuscation controls more broadly: removing or obfuscating personal data is often insufficient, because identity can be induced from other retained data such as locations, times, visited sites, and timestamped activity; and token-based approaches add risk if their mapping tables are compromised.[^aix-obfuscate]

K-anonymity, L-diversity, and T-closeness are not defenses. The Exchange states they are statistical properties experts use to assess a dataset's re-identification risk, not mechanisms that reduce it.[^aix-obfuscate] Anonymity is a statistical property rather than an absolute one, and the Exchange names differential privacy as the framework for analyzing the level of anonymity such properties describe.[^aix-obfuscate]

## Capacity reduction as the alternative mitigation

Differential privacy attacks memorization by adding calibrated noise and accounting for the privacy spent. The [[owasp-ai-exchange|OWASP AI Exchange]] reaches the same threats by constraining what a model can memorize. Its `SMALL MODEL` control keeps a model small enough that it lacks the capacity to store detail at the level of individual training samples, and its threat entry for model inversion and membership inference states that models with excessive capacity or parameter counts are more capable of memorizing fine-grained detail, recommending linear models or Naive Bayes classifiers over neural networks and decision trees where the risk applies.[^aix-inversion][^aix-smallmodel]

The two mitigations trade utility for privacy on different axes. DP-SGD holds the architecture fixed and spends a budget; capacity reduction changes the architecture and spends representational power, with no ε, no composition accounting, and no guarantee that a given model is small enough for a given dataset. Capacity reduction also constrains the task: a model family chosen to limit memorization is chosen before its accuracy on the task is known, which is why the Exchange states the preference as conditional on the risk applying.[^aix-inversion]

`SMALL MODEL` remains the only control the Exchange lists as specific to model inversion and membership inference in §2.3. The entry's other routes are four general input controls — `MONITOR USE`, `RATE LIMIT`, `MODEL ACCESS CONTROL`, and `OBSCURE CONFIDENCE` — which raise the attacker's cost without bounding what the model memorized, and, separately, sensitive data limitation ([`/go/datalimit/`](https://owaspai.org/go/datalimit/)) — a group of five data controls rather than an input control.[^aix-inversion][^aix-datalimit] The `SMALL MODEL` entry is a single sentence with no Objective, Applicability, Implementation, or Limitations subsection, and states that ISO/IEC standards do not yet cover it.[^aix-smallmodel] The threat entry itself names no differential privacy, but what it routes to does: sensitive data limitation carries an obfuscation control naming objective function perturbation as a differential privacy technique, a minimization control routing through OpenCRE to ENISA and NIST AI 100-2 differential-privacy entries, and a definition of the framework and its noise calibration ([`/go/obfuscatetrainingdata/`](https://owaspai.org/go/obfuscatetrainingdata/), [`/go/dataminimize/`](https://owaspai.org/go/dataminimize/)).[^aix-obfuscate][^aix-dataminimize] The framework's memorization-side coverage for these two threats is at least as deep as this page's — it sits one hop away, in the group the threat entry routes to, and a crosswalk that reads only the threat entry will under-count it.

## Relation to wiki

- **CMM D6 (Data, Memory & RAG)** — DP-SGD belongs as an L4/L5 control for training-data privacy. Inference-time DP on RAG responses is an L5+ research area.
- **CMM D4 (Runtime & Guardrails)** — output randomization for extraction-attack mitigation belongs as an L4 control.
- **CMM D9 (Operations & Human Factors)** — privacy-budget tracking belongs as an operational discipline; agents that exhaust budgets must be detected and quotas refreshed/rotated.
- **MAAIS Layer 2 (Data Security)** — explicitly names differential privacy as a control. The wiki adopts the same positioning.
- **[[model-layer-attacks|Model-Layer Attacks]]** — DP is the primary defense against inversion and membership inference; partial defense against extraction (when paired with rate limits and output noise).

## Provenance

The framework was introduced by Dwork, McSherry, Nissim, and Smith (2006), *Calibrating Noise to Sensitivity in Private Data Analysis*. DP-SGD is from Abadi et al. (2016), *Deep Learning with Differential Privacy*. The wiki references DP via [[maais-multilayer-agentic-ai-security|MAAIS Layer 2]] which names it as a Data Security control for agentic AI.

## Notes

[^aix-inversion]: [OWASP AI Exchange — Model inversion and membership inference](https://owaspai.org/go/modelinversionandmembership/), retrieved 2026-08-19. The two attack definitions, the overfitting relationship, and the linear-model / Naive Bayes preference stated as conditional on the risk applying.
[^aix-smallmodel]: [OWASP AI Exchange — SMALL MODEL](https://owaspai.org/go/smallmodel/), retrieved 2026-08-19. The capacity argument, the single-sentence entry structure, and the ISO/IEC coverage gap.
[^aix-datalimit]: [OWASP AI Exchange — General controls for sensitive data limitation](https://owaspai.org/go/datalimit/), retrieved 2026-08-20. The group thesis and the five controls it holds.
[^aix-obfuscate]: [OWASP AI Exchange — OBFUSCATE TRAINING DATA](https://owaspai.org/go/obfuscatetrainingdata/), retrieved 2026-08-20. Objective function perturbation named as a differential privacy technique; PATE named a privacy-preserving technique rather than a differential privacy technique; tokenization stated to align with the principles of differential privacy; the two stated limitations on obfuscation (identity inducible from retained non-PII, token mapping-table compromise); and the statement that K-anonymity, L-diversity, and T-closeness are statistical properties experts use to assess re-identification risk, with differential privacy named as the framework for analysing the level of anonymity.
[^aix-dataminimize]: [OWASP AI Exchange — DATA MINIMIZE](https://owaspai.org/go/dataminimize/), retrieved 2026-08-20. The OpenCRE routing to ENISA and NIST AI 100-2 differential-privacy entries.

<!-- sources:auto -->
## Sources

- [Securing Agentic AI Systems -- A Multilayer Security Framework](https://arxiv.org/abs/2512.18043)
<!-- /sources -->
