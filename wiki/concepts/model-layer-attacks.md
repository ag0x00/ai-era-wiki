---
type: concept
title: "Model-Layer Attacks"
address: c-000010
created: 2026-05-07
updated: 2026-08-20
tags:
  - concepts
  - threat-modeling
  - machine-learning
  - adversarial-ml
  - mitre-atlas
status: developing
origin: aggregated
scope_axis:
  - sec-of-ai
aliases:
  - "Model Extraction"
  - "Model Inversion"
  - "Membership Inference"
related:
  - "[[differential-privacy|Differential Privacy]]"
  - "[[mitre-atlas|MITRE ATLAS]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]"
  - "[[memory-poisoning|Memory Poisoning]]"
  - "[[maais-multilayer-agentic-ai-security|MAAIS Framework]]"
  - "[[nist-ai-600-1|NIST AI 600-1]]"
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
  - "[[precize-agentic-ai-top10|Precize Top 10 for Agentic AI Vulnerability]]"
  - "[[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain|CMM D8 Supply Chain]]"
sources:
  - "[[.raw/papers/maais-arora-hastings-2025-12-19.md]]"
---

# Model-Layer Attacks

A family of three named attack classes that target the deployed model rather than the agent's surrounding orchestration. Each recovers something the model owner did not intend to expose: weights and architecture (extraction); training-data records (inversion); or training-set membership (membership inference). Listed under [[maais-multilayer-agentic-ai-security|MAAIS]] Layer 3 (Model Security) and [[mitre-atlas|MITRE ATLAS]] adversarial techniques. Defensive primitives overlap heavily across the three, so the wiki treats them on one page.

The three classes share a direction, and the model layer carries an opposite one this page does not treat. Extraction, inversion and membership inference are confidentiality attacks that recover something; poisoning is an integrity attack that installs something, and the [[owasp-ai-exchange|OWASP AI Exchange]] divides it into three development-time subtypes — manipulation of the training or in-context-learning data, manipulation of model parameters and the engineering elements that produce them, and use of a supplied model already manipulated at its source ([`/go/modelpoison/`](https://owaspai.org/go/modelpoison/)).[^aix-modelpoison] The second of those is a model-layer attack in the sense this page uses: it reaches the model directly, through storage of parameters, injected custom or lambda layers, weight and architecture modification, or deserialization payloads executing during unpacking.[^aix-devmodelpoison] The Exchange's own detectability argument is why it stays outside the classes above rather than joining them — a backdoored model exposes nothing an attacker can query for, since it holds no reviewable code, its parameters are unreadable, and it behaves normally on every input but the trigger.[^aix-datapoison-mla] The wiki grades the poisoning family at [[agentic-ai-security-cmm-d6-data-rag|CMM D6]] and [[agentic-ai-security-cmm-d8-supply-chain|D8]] and maps it in [[threat-taxonomy-reconciliation|the threat taxonomy reconciliation]].

## The three attack classes

### Model Extraction

Recovering a black-box model's parameters, architecture, or function via repeated queries. Two flavors:

- **Functional extraction** — clone the input-output mapping; the attacker ends up with a model that behaves like the target without recovering exact weights. Tramèr et al. (2016) showed extraction of decision-tree, logistic-regression, and neural-network models from public ML-as-a-service APIs.
- **Architectural extraction** — recover hyperparameters and architecture details (layer count, activations, hidden sizes). Often a precursor to functional extraction.

Cost-of-attack ranges from thousands to millions of queries depending on model complexity and defenses.

[[mitre-atlas|MITRE ATLAS]] technique: `AML.T0044` (ML Model Inference API Access; Full ML Model Access).

### Model Inversion

Reconstructing training data records from model outputs. Fredrikson et al. (2014, 2015) demonstrated face-image recovery from a face-recognition model and recovery of private medical-record values from a regression model. Modern instances target large language models — extracting training-set verbatim text by carefully crafted prompts (Carlini et al. 2021, "Extracting Training Data from Large Language Models").

For agentic AI specifically, **inversion attacks against RAG-grounded agents** can recover the corpus contents: an attacker who can query the agent extensively can reconstruct individual retrieved documents, even if direct corpus access is blocked.

[[mitre-atlas|MITRE ATLAS]] techniques: `AML.T0048` (Erode Dataset); related to `AML.T0024` (Exfiltration via ML Inference API).

### Membership Inference

Determining whether a specific record was part of a model's training set. Shokri et al. (2017) showed this is achievable against ML-as-a-service models with high accuracy by training shadow models. Less data-revealing than inversion but more broadly applicable; even defended models often leak membership signal at moderate ε.

For agentic AI, membership-inference attacks against agent memory or session-derived fine-tuned models can confirm whether sensitive interactions occurred (e.g., "did this user ever discuss topic X with this agent?").

[[mitre-atlas|MITRE ATLAS]] technique: similar surface to extraction; `AML.T0024` (Exfiltration via ML Inference API).

## Attacker knowledge and the surrogate supply

The three classes above are graded by what an attacker recovers. A second axis grades what an attacker already knows, and the two meet where a successful extraction supplies the knowledge for a different attack. The [[owasp-ai-exchange|OWASP AI Exchange]] splits evasion — crafting input so a model performs its task incorrectly — into five subtypes on exactly this axis: zero-knowledge, perfect-knowledge, partial-knowledge, transferability-based, and evasion after data poisoning ([`/go/evasion/`](https://owaspai.org/go/evasion/)).

The transferability-based subtype connects directly to the three classes above. An attacker builds adversarial samples against a surrogate model and applies them to the target, exploiting the tendency of models trained for the same task to share decision boundaries. The Exchange enumerates six surrogate sources, three of which supply the target model itself: a copy stolen at development time or runtime, a purchased or freely downloaded copy, and a replica built through model exfiltration ([`/go/transferattack/`](https://owaspai.org/go/transferattack/)). Of those three, the replica route is the one an attack class on this page produces, model exfiltration being the query-based replication this page calls extraction. A purchased or publicly downloadable model supplies the same surrogate with no attack at all, which is why the Exchange scopes model-theft controls to models that are not already public ([`/go/runtimemodelleak/`](https://owaspai.org/go/runtimemodelleak/)). Transfer success rises the more closely the surrogate resembles the target, and the Exchange states that even simpler surrogates tend to transfer effectively ([`/go/transferattack/`](https://owaspai.org/go/transferattack/)).

`DISCRETE` bears on the purchased-or-downloadable route to a surrogate, a connection this page draws and the Exchange does not. The control minimizes access to technical details that could help an attacker select and tailor an attack, and two of its three stated examples reach this route: weigh the risk before publishing technical articles about the AI system, and, where a choice exists, prefer a model type or model implementation attackers are less familiar with ([`/go/discrete/`](https://owaspai.org/go/discrete/)).[^aix-discrete] Its references cite MITRE ATLAS `AML.M0000` (Limit Public Release of Information), `AML.M0001` (Limit Model Artifact Release), and `AML.T0002` (Acquire Public ML Artifacts) — the last of which names the technique this section's "purchased or freely downloadable copy" route describes.[^aix-discrete]

This changes what a successful extraction costs its victim. The consequence stated above is the loss of model IP and of the training-data privacy that the model's parameters encode. The consequence the Exchange adds is an integrity attack the extraction makes cheap: an attacker holding a functional replica runs an unlimited offline search for adversarial samples, then spends a single query against the production system. The Exchange records the same relationship for runtime model theft, stating that a stolen model lets an attacker rehearse input attacks against their own copy, free of the rate limiting, access control, and detection the production system applies ([`/go/runtimemodelleak/`](https://owaspai.org/go/runtimemodelleak/)).

The defense consequence is a scoping rule. Rate limiting, series-level input analysis, and confidence obscuring all defend against an attacker searching for samples against the target, and the Exchange excludes all three by name for transferability-based evasion because the search runs against the surrogate ([`/go/transferattack/`](https://owaspai.org/go/transferattack/)). The query-budget defenses in the Defenses table below therefore bound the extraction step and leave unbounded what the extraction enables. Model confidentiality is the control that reaches the second step.

## Relevance to agentic AI

The wiki's threat-modeling has prioritized **prompt-injection-class threats** (orchestration-layer compromise of agent behavior). Model-layer attacks are a separate threat surface that the wiki has under-treated:

- **Long-running agents accumulate query budget.** Scope-3 / Scope-4 agents per the [[aws-agentic-ai-security-scoping-matrix|AWS Scoping Matrix]] make many queries during normal operation. An attacker who controls a fraction of those queries (e.g., via [[indirect-prompt-injection|indirect prompt injection]]) can run extraction-style queries piggybacking on legitimate agent behavior.
- **RAG corpus is the new training data.** RAG-grounded agents can be inverted to recover the corpus contents — a class of leak the wiki has not enumerated.
- **Agent memory is fine-tunable.** Agents that fine-tune on session interactions inherit the membership-inference surface of the base model + the new attack surface of recovering session-derived records.
- **Multi-agent systems have privileged queriers.** A compromised peer agent in an [[a2a-protocol|A2A]] mesh can run extraction queries against other agents at high volume without external rate limits applying.

> [!gap] Agent-scaffold extraction is a distinct, uncatalogued surface
> The [[precize-agentic-ai-top10|Precize Top 10 for Agentic AI Vulnerability]] names AAI015, Agent Inversion and Extraction Vulnerability, as a **fourth** class alongside the three on this page — but it targets a different asset. Where extraction, inversion, and membership inference above recover something about the *model* (weights, training records, membership), AAI015 targets the *agent scaffold*: goal hierarchies, tool-orchestration logic, and workflow blueprints, reverse-engineered from observed agent behavior into a functional clone that need not touch the underlying model at all. The two surfaces share a query-based reconnaissance pattern and overlap in defense (rate limits, behavioral obfuscation), but a model-confidentiality control that stops weight extraction does nothing against an attacker cloning the agent's plan structure from its outputs. This wiki has not yet dedicated a page to agent-scaffold extraction as its own concept; AAI015 is the only cataloguing of it found so far.

## Defenses

| Defense | Extraction | Inversion | Membership inference |
|---|---|---|---|
| **Rate limiting / query budgets** | Strong | Moderate | Moderate |
| **[[differential-privacy\|DP-SGD at training]]** | Weak (function still extractable) | Strong | Strong |
| **Output randomization** (Gaussian noise on logits) | Strong | Weak | Moderate |
| **Output abstraction** (return label only, not confidence) | Moderate | Weak | Strong |
| **Watermarking** (attribution-only) | Weak for query-based extraction; entangled watermarks address the gap | n/a | n/a |
| **Per-query monitoring + anomaly detection** | Moderate (patterns overlap benign intensive use) | Moderate | Moderate |
| **Model isolation / minimal-permission API** | Strong | Strong | Strong |

The table above grades no obfuscation techniques of its own — masking, tokenization, and encryption, three of the five techniques the [[owasp-ai-exchange|OWASP AI Exchange]] names under `OBFUSCATE TRAINING DATA`, reduce the likelihood that training data can be reconstructed or linked back to individuals, but §1.2 supports no Strong / Moderate / Weak grading of them against the three attack columns.[^aix-obfuscate] Two residuals are stated: identity can still be induced from other retained data such as locations, times, visited sites, and timestamped activity, and a token-based approach adds risk if its mapping table is compromised.[^aix-obfuscate] Recording them as a graded row would invent a criterion the source does not supply.

Of these, **[[differential-privacy|differential privacy]] is the only *named* mechanism with a quantifiable privacy guarantee against inversion and membership inference**. The Exchange states that obfuscation effectiveness can be evaluated through attack testing or by relying on a formal privacy guarantee "such as differential privacy or an equivalent mathematical framework," and names no second member of that equivalent class.[^aix-obfuscate] K-anonymity, L-diversity, and T-closeness sit on the other side of that claim: the Exchange names them as statistical properties experts use to assess a dataset's re-identification risk, not as guarantee-bearing mechanisms, and states that anonymity is a statistical rather than an absolute concept, with differential privacy as the framework for analysing the level.[^aix-obfuscate] Everything else reduces attack success rate empirically without strong formal guarantees.

Differential privacy protects record-level detail; extraction recovers the model's function itself, which DP's guarantee does not bound. Defense relies on **rate limits + output randomization + monitoring for high-volume query patterns characteristic of extraction**.

Both extraction-column entries above carry a limit the [[owasp-ai-exchange|OWASP AI Exchange]] states plainly. Watermarking is effective evidence for direct model theft and limited for query-based extraction, because typical watermark markers sit in data that is not in-distribution for the queries an extraction attack sends; entangled watermarking is the named technique that closes the gap ([`/go/modelwatermarking/`](https://owaspai.org/go/modelwatermarking/)). The Exchange also files watermarking under post-theft ownership verification for legal claims, contractual enforcement, and regulatory investigation rather than under detection of an attack in progress, which puts its output in a dispute rather than in an alert ([`/go/modelwatermarking/`](https://owaspai.org/go/modelwatermarking/)). Query-pattern monitoring is bounded on the other side: the Exchange states that detection always requires further analysis because the same usage pattern may be benign, and that where an attacker can reach the model and the model allows intensive use, this threat is typically hard to protect against ([`/go/modelexfiltration/`](https://owaspai.org/go/modelexfiltration/)). A monitoring control credited against extraction therefore produces a candidate set for review rather than a determination.

## Relation to wiki

- **CMM D4 (Runtime & Guardrails)** — output randomization and per-query rate limits belong as L4 controls; long-window query-pattern monitoring (extraction detection) belongs at L5.
- **CMM D6 (Data, Memory & RAG)** — DP-SGD on training/fine-tuning belongs at L4/L5; RAG-corpus inversion defenses (per-document query budgets, response abstraction) belong at L5+.
- **CMM D7 (Observability and Detection)** — query-pattern anomaly detection for extraction attempts belongs at L4.
- **MAAIS Layer 3 (Model Security)** — explicitly names "model extraction, backdoor injections, and inversion attacks" as in-scope. This page treats the extraction and inversion half; backdoor injection is the poisoning family, scoped above and mapped in [[threat-taxonomy-reconciliation|the threat taxonomy reconciliation]].
- **[[differential-privacy|Differential Privacy]]** — primary defense against inversion and membership inference.
- **[[mitre-atlas|MITRE ATLAS]]** — `AML.T0024` (Exfiltration via ML Inference API), `AML.T0044` (ML Model Inference API Access), `AML.T0048` (Erode Dataset).
- **[[nist-ai-600-1|NIST AI 600-1]]** — the GenAI Profile names model extraction under its §2.10 intellectual-property category, whose Suggested Action `MS-2.10-001` covers detecting protected content in outputs; the inversion and membership-inference classes here are the data-privacy face of the same model-exposure surface the Profile flags.

## Provenance

- Tramèr et al. (2016), *Stealing Machine Learning Models via Prediction APIs* — foundational extraction work.
- Fredrikson et al. (2014, 2015), *Privacy in Pharmacogenetics* and *Model Inversion Attacks That Exploit Confidence Information* — foundational inversion work.
- Shokri et al. (2017), *Membership Inference Attacks Against Machine Learning Models* — foundational MIA work.
- Carlini et al. (2021), *Extracting Training Data from Large Language Models* — modern LLM-specific inversion.
- The wiki's enumeration here was prompted by [[maais-multilayer-agentic-ai-security|MAAIS]] Layer 3 naming the three attack classes alongside backdoor injection.

## Notes

[^aix-discrete]: [OWASP AI Exchange — DISCRETE](https://owaspai.org/go/discrete/), retrieved 2026-08-20. The description and objective, the three stated examples, and the MITRE ATLAS `AML.M0000`, `AML.M0001`, and `AML.T0002` references.
[^aix-obfuscate]: [OWASP AI Exchange — OBFUSCATE TRAINING DATA](https://owaspai.org/go/obfuscatetrainingdata/), retrieved 2026-08-20. The named obfuscation techniques (masking, tokenization, encryption), the risk-reduction and effectiveness-evaluation statement naming differential privacy or an equivalent mathematical framework, the two stated limitations, and the statement that K-anonymity, L-diversity, and T-closeness are statistical properties experts use to assess re-identification risk.
[^aix-modelpoison]: [OWASP AI Exchange — Broad model poisoning development-time](https://owaspai.org/go/modelpoison/), retrieved 2026-08-20. The definition of development-time model poisoning as manipulation of development elements to alter model behaviour, its three subtypes (data poisoning, development-environment model poisoning, supply-chain model poisoning), the stated impact on model-behaviour integrity, and the four-stage poisoning threat model covering supplied data, supplied models, the data-preparation and training environments, runtime-collected training data, and direct model alteration.
[^aix-devmodelpoison]: [OWASP AI Exchange — Direct development-time model poisoning](https://owaspai.org/go/devmodelpoison/), retrieved 2026-08-20. Manipulation of model parameters, storage of parameters, replacement of the model, command or code injection through custom or lambda layers, weight and architecture modification, and embedded deserialization attacks executing during model unpacking or execution; and `CONTINUOUS VALIDATION` as the named performance-deviation detection control.
[^aix-datapoison-mla]: [OWASP AI Exchange — Data poisoning](https://owaspai.org/go/datapoison/), retrieved 2026-08-20. The statement that backdoor poisoning is hard to detect because a model holds no reviewable code, its parameters make no sense to the human eye, and testing runs on normal cases with blind spots for triggers.

<!-- sources:auto -->
## Sources

- [Securing Agentic AI Systems -- A Multilayer Security Framework](https://arxiv.org/abs/2512.18043)
<!-- /sources -->
