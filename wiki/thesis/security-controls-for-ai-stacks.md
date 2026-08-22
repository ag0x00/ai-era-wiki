---
type: thesis
title: "Security Controls for AI Stacks"
created: 2026-04-30
updated: 2026-08-21
tags:
  - thesis
  - controls
  - ai-stack
  - layered-defense
status: developing
origin: produced
scope_axis:
  - sec-of-ai
question: "What security controls exist for agentic AI stacks, where do they live in the stack, and what gaps remain?"
current_position: "Working controls converge into six layers — identity, observability, containment, network/protocol, model, and data — with mature options at the identity and observability layers and emerging options at the network layer. The model, data, and containment layers now split the same way: published, normative control guidance against thin shipping tooling, with the model layer's evasion entries a third case in which the published guidance cites the papers refuting it. Identity and observability carry guidance and shipping tooling together; the network layer is early on both. The open-source ecosystem is delivering working reference implementations faster than published frameworks."
last_revised: 2026-08-20
related:
  - "[[agent-identity-architecture]]"
  - "[[agent-observability]]"
  - "[[agent-sandboxing]]"
  - "[[mcp-security]]"
  - "[[ai-security-standards-in-q1-2026]]"
  - "[[securing-the-autonomous-future]]"
  - "[[credential-proxy-pattern]]"
  - "[[supply-chain-security-for-agents]]"
  - "[[ai-bom]]"
  - "[[prompt-injection-containment]]"
  - "[[owasp-ai-exchange]]"
  - "[[least-agency-principle]]"
  - "[[mythos-ready-security-program]]"
  - "[[agent-escape]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[jasons-mental-model]]"
  - "[[securing-ai-talking-points]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - "[[.raw/papers/securing-the-autonomous-future.md]]"
  - "[[.raw/papers/emerging-cybersecurity-practices-for-agentic-ai-applications.md]]"
---

# Security Controls for AI Stacks

Six layers organize the controls available to an agentic AI deployment, and each layer is graded here on published guidance against shipping tooling. Each layer names the systems whose identifiers it uses before using them — the control names of the [[owasp-ai-exchange|OWASP AI Exchange]] catalogue, the span conventions of [[opentelemetry-gen-ai|OpenTelemetry `gen_ai.*`]], and the policy vocabulary of [[cedar|Cedar]] among them.

## On this page

- [Question](#question)
- [Current position](#current-position)
- [The six layers](#the-six-layers)
- [Framework coverage and gaps](#framework-coverage-and-gaps)
- [Position history](#position-history)
- [Open sub-questions](#open-sub-questions)

## Question

What security controls exist for agentic AI stacks, where do they live in the stack, and what are the gaps?

## Current position

Working controls converge into six layers. Mature options exist at the identity and observability layers, and emerging options at the network layer. The model, data, and containment layers split the same way: published, normative control guidance against thin shipping tooling. The model layer carries a third case inside that split, where the published guidance cites the papers refuting it. The three remaining layers sit differently against that split. Identity and observability carry guidance and shipping tooling together, which is why they grade mature. The network layer's guidance and its tooling are both early, which is why it grades emerging.

**Platform-layer over prompt-layer.** The strongest practitioner consensus across the ingested sources ([[ai-security-standards-in-q1-2026|AI Security Standards in Q1 2026: Agentic Threats Outpace Frameworks]], [[securing-the-autonomous-future|Securing the Autonomous Future: Trust, Safety, and Reliability of Agentic AI]], [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]]) is that **security controls must now be enforced at the platform layer**. Prompt-level guardrails are bypassable; platform-level enforcement (input filtering at the broker, egress control, [[capability-based-authorization|capability-based authorization]], sandboxing) holds under the same pressure.

Evidence from [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]]: the APort Agent Guardrail case study states it directly: "pre-action authorization must run in the runtime/platform, so the platform invokes the guardrail for every tool call regardless of what the model outputs." That requirement restates a standing security principle: **controls must be enforced at a layer below the layer they protect**. Network firewalls enforce below the application; tool call interception must enforce below the LLM.

## The six layers

### 1. Identity layer (mature)

This layer assigns verifiable identities to AI agents and traces actions back to invoking humans.

| Control | Page | Status |
|---|---|---|
| Workload identity (SPIFFE/SPIRE) | [[spiffe\|SPIFFE / SPIRE]] | Stub — adoption growing |
| Non-Human Identity (NHI) governance | [[non-human-identity\|Non-Human Identity]], [[nhi-governance-for-agents\|NHI Governance for Agents]] | Developing |
| Reference architecture | [[agent-identity-architecture\|AI Agent Identity Architecture]] | Developing |
| Credential lifecycle (Credential Zero) | [[nhi-governance-for-agents\|NHI Governance for Agents]] | Developing |
| Capability-based authorization (Warrants) | Task-scoped, signed, ephemeral capability authorizations | Developing |
| Credential proxy (proxy token, vault injection) | [[credential-proxy-pattern\|Credential Proxy Pattern]] | Developing — multi-tool convergence confirms this is load-bearing |

Verdict: mature options exist; integration discipline (action-to-identity tracing) distinguishes well-postured organizations from exposed ones. The [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]] has independently converged across five or more tools in the OpenClaw ecosystem, a signal that credential exposure is a distinct control gap alongside credential rotation. See [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]].

### 2. Observability layer (mature)

This layer makes an agent's reasoning, tool calls, and identities inspectable at runtime. Four named systems supply the identifiers the table below uses: [[opentelemetry-gen-ai|OpenTelemetry's `gen_ai.*` semantic conventions]] for span naming, [[cedar|Cedar]] for policy on mediated actions, [[ai-bom|AI-BOM]] for runtime inventory, and [[miggo-security|Miggo]]'s DeepTracing for behavioral discovery. Section numbers point into [[agent-observability|Agent Observability]], which carries the implementation detail.

| Control | Page | Status |
|---|---|---|
| Lifecycle hooks + reference monitors | [[agent-observability\|Agent Observability]] §1 | Developing |
| OpenTelemetry `gen_ai.*` semantic conventions | [[agent-observability\|Agent Observability]] §2 | Developing |
| Identity multiplexing in logs | [[agent-observability\|Agent Observability]] §3 | Developing |
| Cedar policy for action mediation | [[agent-observability\|Agent Observability]] §4A | Developing |
| Agent Cards (System of Record) | [[agent-observability\|Agent Observability]] §4C | Developing |
| Context-aware trimming with pinned tags | [[agent-observability\|Agent Observability]] §5 | Developing |
| Agent behavioral monitoring (anomaly detection) | [[agent-observability\|Agent Observability]] §7 | Developing |
| AI-BOM runtime discovery (behavioral baseline) | [[ai-bom\|AI-BOM]] (Miggo DeepTracing) | Developing |
| Behavioral drift detection | [[agent-observability\|Agent Observability]] §7, [[ai-bom\|AI-BOM]] §Runtime | Developing |
| Cognitive file integrity monitoring | [[supply-chain-security-for-agents\|Supply Chain Security]] | Developing — extends FIM to identity files |

Verdict: mature catalogue, with strong practitioner consensus and real tooling (the OTel `gen_ai.*` conventions). From [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]]: Miggo's AI-BOM runtime discovery with DeepTracing adds a continuous behavioral BOM approach, and cognitive file integrity (monitoring SHA-256 drift on identity files) extends traditional FIM to agentic-specific artifacts.

### 3. Containment layer (guidance published, tooling emerging)

This layer prevents an agent from doing damage when other controls fail.

| Control | Page | Status |
|---|---|---|
| Agent sandboxing (OS-level isolation) | [[agent-sandboxing\|Agent Sandboxing]] | Developing — last-line-of-defense |
| Lethal Trifecta containment (egress control, human confirmation, tool annotation) | [[stripe\|Stripe]] (stub — needs full architecture page) | Stub |
| Human-in-the-Loop primitive | Confirmation gate before high-impact tool calls | Developing |
| Reversible-actions-only constraint | Constrain consequential actions to reversible ones, with circuit breakers | Developing |
| Least agency tiers (auto/notify/confirm/block) | [[least-agency-principle\|Least Agency Principle]] | Developing — [[emerging-cybersecurity-practices-for-agentic-ai-applications\|Emerging Practices]] §3.2 |
| Tool call interception at platform layer | [[prompt-injection-containment\|Prompt Injection Containment]] | Developing |
| AlignmentCheck (chain-of-thought auditing) | [[prompt-injection-containment\|Prompt Injection Containment]], [[llamafirewall\|LlamaFirewall]] | Developing |
| Kill switches / instant revocation | [[credential-proxy-pattern\|Credential Proxy Pattern]] | Developing |
| State rollback (Brain Git) | [[supply-chain-security-for-agents\|Supply Chain Security]] | Developing — agentic-specific |

This layer's guidance is documented across four wiki pages. [[least-agency-principle|The Least Agency Principle]] carries the four action-risk tiers (auto / notify / confirm / block) that [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]] §3.2 supplies, alongside the [[owasp-ai-exchange|OWASP AI Exchange]] oversight requirements that compose with them; [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]] covers tool-call interception, AlignmentCheck, and platform-level versus prompt-level enforcement; [[supply-chain-security-for-agents|Supply Chain Security for Agentic AI]] covers kill switches, instant revocation, and Brain Git rollback; and that same paper supplies much of the source material behind the rest of the layer. The lethal-trifecta architecture [[stripe|Stripe]] publishes is the layer's most-cited worked deployment and has no page of its own, so it is reachable here only through a company stub.

### 4. Network and protocol layer (emerging)

This layer secures inter-agent and agent-to-tool communication.

| Control | Page | Status |
|---|---|---|
| MCP Security taxonomy (CoSAI WS4) | [[mcp-security\|MCP Security]], [[cosai\|CoSAI]] | Developing |
| AgentGateway (open-source MCP gateway) | [[agentgateway\|AgentGateway]] | Stub |
| A2A Protocol with Agent Cards / opacity principle | [[a2a-protocol\|A2A Protocol]] | Stub |
| Egress control patterns | (no dedicated page yet) | **Gap — see update note** |
| Agent-to-agent cryptographic identity (Ed25519) | [[agent-identity-architecture\|Agent Identity Architecture]], [[emerging-cybersecurity-practices-for-agentic-ai-applications\|Emerging Practices]] §2.4 | Developing — Oktsec implementation |
| Multi-rule content scanning on inter-agent messages (Oktsec, 268 rules at v0.15.2) | [[a2a-protocol\|A2A Protocol]], [[emerging-cybersecurity-practices-for-agentic-ai-applications\|Emerging Practices]] §2.4 | Developing — Oktsec |

> [!gap] Egress control patterns: partially addressed
> Multiple sources reference controlled egress as a Lethal Trifecta containment primitive; a dedicated egress-control-patterns page is still warranted. [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]] documents the credential proxy pattern as a practical egress control: the proxy validates outbound requests against allowed targets before injecting credentials, providing destination-based filtering. Docker DOCKER-USER chain rules for container networking and network segmentation for agent runtimes appear in its Section 2.7.

### 5. Model layer (guidance published, tooling thin)

This layer detects malicious intent or deception inside the model itself, and it also holds the training-side controls that make a model resilient before deployment.

| Control | Page | Status |
|---|---|---|
| LlamaFirewall (open-source guardrail) | [[llamafirewall\|LlamaFirewall]] | Developing |
| Mechanistic interpretability for "internal EDR" | [[agent-observability\|Agent Observability]] §6 | Developing — research-stage |
| Prompt-level guardrails | (no dedicated page; widely seen as bypassable) | Acknowledged-weak |
| Prompt injection detection / input filtering | [[prompt-injection-containment\|Prompt Injection Containment]] §Layer 1 | Developing |
| Proof-of-Guardrail (TEE attestation) | [[emerging-cybersecurity-practices-for-agentic-ai-applications\|Emerging Practices]] §2.5 | Research-stage — novel primitive |

**The model layer is the thinnest of the six.** Prompt-level guardrails are advisory, and platform-layer enforcement is the practical answer. Mechanistic interpretability is promising but research-stage. The model layer offers detection rather than prevention, and that detection is itself unreliable. The training-side half of the layer is documented rather than tooled: the [[owasp-ai-exchange|OWASP AI Exchange]] names thirteen data- and model-engineering controls under its *Have resilient models* category, including `EVASION ROBUST MODEL`, `POISON ROBUST MODEL`, `TRAIN ADVERSARIAL`, `ADVERSARIAL ROBUST DISTILLATION`, `MODEL ENSEMBLE`, and `TRAIN DATA DISTORTION` ([`/go/controlsoverview/`](https://owaspai.org/go/controlsoverview/)). Eleven of the thirteen presume a team that trains or fine-tunes its own model. The Exchange states that dependency across its whole catalogue, this layer included: most of its controls are familiar conventional security countermeasures, unless the organization trains its own model ([`/go/seceducate/`](https://owaspai.org/go/seceducate/)). The two exceptions, `CONTINUOUS VALIDATION` and `UNWANTED BIAS TESTING`, are also listed under the Exchange's *Limit model behaviour* sub-category, where they apply to a bought-in model and belong to operations rather than to training.

Three of those thirteen carry the Exchange's evasion controls, and reading their entries adds a third state to the layer's guidance grade. `ADVERSARIAL ROBUST DISTILLATION` is the only control in its range with no Objective, Applicability, Implementation, Risk-Reduction or Limitations subsection, and its two primary references are the paper introducing defensive distillation and the paper refuting its robustness, placed one after the other ([`/go/adversarialrobustdistillation/`](https://owaspai.org/go/adversarialrobustdistillation/)). `EVASION ROBUST MODEL` recommends gradient masking and cites, in the same entry, the paper on obfuscated gradients giving a false sense of security ([`/go/evasionrobustmodel/`](https://owaspai.org/go/evasionrobustmodel/)). `TRAIN ADVERSARIAL` states significant training overhead, poor scaling with model complexity and input dimension, a risk of overfitting, weak generalization to new attack methods, and a cited robustness-versus-accuracy trade ([`/go/trainadversarial/`](https://owaspai.org/go/trainadversarial/)). A control entry that cites its own refutation supplies a name and a literature pointer, and a buyer who had the missing tooling would still lack a basis for choosing the control. All three leave the benefit contested or unstated, and two of them state what operating the control costs.

A fourth evasion control settles the layer's boundary. `INPUT DISTORTION` sits outside the thirteen, in the Exchange's *Watch* category alongside the runtime I/O handlers ([`/go/controlsoverview/`](https://owaspai.org/go/controlsoverview/)). It adds noise, smoothing, or JPEG compression to input at inference time and doubles as a detector by comparing the outputs of the original and distorted input, which reads as the one model-layer control operable without a training team. It is a separate control from `TRAIN DATA DISTORTION` named above, which distorts the training set against poisoning. The Exchange closes the runtime reading: distorted input reduces accuracy on regular data, so the model must be retrained with the random transformations in place, and zero-knowledge evasion attacks do not rely on gradients and are unaffected by the mechanism ([`/go/inputdistortion/`](https://owaspai.org/go/inputdistortion/)). The eleven-of-thirteen split above holds, and this control is the worked instance of why.

A third control distorts training data, and the Exchange states the three-way split itself. `OBFUSCATE TRAINING DATA` sits outside the thirteen, in the *Limit* category under sensitive data limitation, while `TRAIN DATA DISTORTION` and `EVASION ROBUST MODEL` are both named among the thirteen above. The Exchange writes that all three distort training data for different purposes — obfuscation against disclosure of the data, distortion against data poisoning, and robustness against evasion attacks ([`/go/obfuscatetrainingdata/`](https://owaspai.org/go/obfuscatetrainingdata/)). The distinction matters to this inventory because the three sit on two sides of the training-team boundary: two are model-engineering controls a deploying organization does not operate, and the third applies to any data the organization supplies for fine-tuning.

Per [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]], [[llamafirewall|LlamaFirewall]] carries three components: PromptGuard 2 (injection detection, reported substantial reduction in attack success rate), AlignmentCheck (chain-of-thought auditing for goal hijacking, a prospective control), and CodeShield (static analysis for generated code). Proof-of-Guardrail uses AWS Nitro Enclaves to cryptographically attest that guardrails executed, moving from trusting the vendor to verifying the guardrail ran. Both remain research-stage.

### 6. Data layer (guidance published, tooling thin)

This layer protects training data, RAG sources, and model artifacts, and provides supply-chain assurance.

| Control | Page | Status |
|---|---|---|
| AI-BOM / ML-BOM | [[ai-bom\|AI-BOM]] | Developing — gap closed |
| Supply-chain multi-layer defense | [[supply-chain-security-for-agents\|Supply Chain Security for Agents]] | Developing — gap closed |
| Supply-chain attack disclosure | [[clawhavoc\|ClawHavoc]], [[sandworm-mode-npm-worm\|SANDWORM_MODE]], [[litellm-supply-chain-compromise\|LiteLLM]] | Incident pages |
| Cognitive file integrity (SOUL.md / IDENTITY.md) | [[supply-chain-security-for-agents\|Supply Chain Security]] §Layer 4 | Developing — new category |
| Data poisoning defenses | (no dedicated page) | **Gap — still open** |
| RAG poisoning defenses | (no dedicated page) | **Gap — still open** |

> [!gap] Data layer: partially closed
> Three Q1 2026 incidents are pure supply-chain compromises: [[clawhavoc|ClawHavoc]], [[sandworm-mode-npm-worm|SANDWORM_MODE npm worm]], and [[litellm-supply-chain-compromise|LiteLLM Supply Chain Compromise]]. The [[ai-bom|AI-BOM]] page documents the AI Bill of Materials control (static and runtime, CycloneDX format, Miggo runtime-discovery pattern), and [[supply-chain-security-for-agents|Supply Chain Security for Agentic AI]] covers the multi-layer defense model: registry scanning, pre-install scanning, checksum verification, cognitive file integrity, and behavioral drift detection. Cognitive file integrity (SHA-256 monitoring of SOUL.md and IDENTITY.md, Brain Git rollback) is an agentic-specific category from that source.
>
> Still open: data poisoning defenses and RAG poisoning defenses have no dedicated pages; both need a separate source or working session.

The data layer's guidance side carries the deepest published control text in this inventory. The [[owasp-ai-exchange|OWASP AI Exchange]] groups five controls under sensitive data limitation — `DATA MINIMIZE`, `ALLOWED DATA`, `SHORT RETAIN`, `OBFUSCATE TRAINING DATA` and `DISCRETE` — whose stated purpose is to reduce the impact of confidentiality and integrity threats by cutting the amount and variety of data held and the duration it is kept ([`/go/datalimit/`](https://owaspai.org/go/datalimit/)). All five carry a Description and an Objective, and four add an Implementation section; `ALLOWED DATA` states a requirement and no method. `OBFUSCATE TRAINING DATA` is the deepest entry in the group, naming five techniques — PATE, objective function perturbation, masking, encryption, and tokenization — two models of encryption in machine learning, and two stated limitations on what obfuscation leaves behind ([`/go/obfuscatetrainingdata/`](https://owaspai.org/go/obfuscatetrainingdata/)). No entry in the group names a product that operates it. The layer's grade is unchanged and is now evidenced rather than asserted: normative control text at this depth against no shipping tooling.

## Framework coverage and gaps

| Framework | Layers covered well | Layers under-covered |
|---|---|---|
| [[nist-ai-rmf\|NIST AI RMF]] | Governance overlay; risk-management process | Technical controls per layer |
| [[mitre-atlas\|MITRE ATLAS]] | Threat taxonomy (attacker perspective) | Defender controls |
| [[owasp-llm-top-10\|OWASP LLM Top 10]] | Model-layer awareness | Identity, network, data |
| [[owasp-agentic-ai-top-10\|OWASP Agentic AI Top 10]] | Agent-orchestration risks | Implementation guidance |
| [[iso-iec-42001\|ISO/IEC 42001]] | Management system and governance | Technical security controls |
| [[google-saif\|Google SAIF]] | Lifecycle conceptual model | Concrete operational controls |
| [[cosai\|CoSAI]] (MCP white paper; secure-by-design) | Network/protocol layer (MCP); secure-by-design concepts | Reference implementations |
| [[microsoft-rai\|Microsoft RAI]] / [[microsoft-zt4ai\|ZT4AI]] | Broad control coverage across the Zero Trust pillars | Microsoft-stack-specific |
| [[csa-maestro\|CSA MAESTRO / ATF]] | Cross-layer threat model; autonomy promotion gates | Operational tooling |
| [[owasp-ai-exchange\|OWASP AI Exchange]] | Model layer (13 data/model-engineering controls); data layer; containment; governance (six named controls, below) and supply chain; 50+ controls | Reference implementations and tooling; no organizational maturity criteria, though it grades some named standards' coverage of a control; no AI-BOM artifact schema |

The Exchange changes what the model-layer gap is a gap in. Adversarial training, poison-robust and evasion-robust model construction, distillation, ensembles, and continuous validation are named controls with published guidance ([`/go/controlsoverview/`](https://owaspai.org/go/controlsoverview/)); what the six-layer inventory above cannot populate is a shipping product that operates them for a team that does not train its own models. The Exchange states the cost side of the same point: many of these controls are expensive and trade off against other AI properties affecting correctness and normal operation, and controls that alter the learning process or the data distribution can carry unintended downstream side effects. The model-layer entry is therefore a tooling gap over guidance of uneven standing: some entries are specified well enough to build against, and the evasion entries cite their own refutations. The data layer keeps its split between published guidance and missing tooling.

The governance cell above is specified rather than asserted. Its six controls are `AI PROGRAM`, `SEC PROGRAM`, `SEC DEV PROGRAM`, `DEV PROGRAM`, `CHECK COMPLIANCE` and `SEC EDUCATE`, and between them they carry a twelve-item AI asset list, a six-step AI Use Case Privacy and Security Analysis process (describe the ecosystem, assess the system of interest, identify the concerns, identify the risks, identify the controls, identify the assurance concerns), a nine-row AI regulatory map, and one control this inventory cannot place at all ([`/go/secprogram/`](https://owaspai.org/go/secprogram/), [`/go/aiprogram/`](https://owaspai.org/go/aiprogram/), [`/go/checkcompliance/`](https://owaspai.org/go/checkcompliance/)).

**The Exchange names a control the six-layer inventory has no layer for.** AI-specific honeypots are neither telemetry over the running system nor a boundary that limits damage after a control fails. They are planted assets an attacker is meant to reach first, and an inventory built around identity, observability, containment, network, model and data has nowhere to put one.

The containment layer takes the same correction. Document 4 of the Exchange specifies agent sandboxing as a control with named criteria: a dedicated container, microVM, or OS-enforced sandbox with separate namespaces; mandatory access control; a read-only root with ephemeral writable layers; default-deny egress through a monitored proxy; tool credentials held outside the sandbox; destruction of transient state on termination; and platform-enforced per-agent quotas ([`/go/agentsandboxing/`](https://owaspai.org/go/agentsandboxing/)). The same section states four things the control does not reach: container or hypervisor escape, shared inference, credential, and policy services acting as cross-agent channels, exfiltration through legitimately permitted APIs, and host-OS behaviour that differs across hosts. Document 4 also separates [[agent-escape|agent escape]] from jailbreak by enforcement layer and warns that conflating them yields controls only partially effective against each ([`/go/agentescape/`](https://owaspai.org/go/agentescape/)). The containment entry above therefore describes a tooling and integration gap over published guidance, and the shared-services channel among those four limits is the one this inventory cannot populate at all.

## Position history

- **2026-04-30** — Initial synthesis from three ingested sources identified the six layers and flagged the model and data layers as material gaps.
- **2026-04-30** — [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]] closed several gaps. The identity layer gained the [[credential-proxy-pattern|Credential Proxy Pattern]] with multi-tool convergence evidence. The observability layer gained AI-BOM runtime discovery (Miggo DeepTracing) and cognitive file integrity. The containment layer gained that paper's §3.2 action-risk tiers (see [[least-agency-principle|Least Agency Principle]]) and [[prompt-injection-containment|Prompt Injection Containment]] with AlignmentCheck. The data layer gained [[ai-bom|AI-BOM]] and [[supply-chain-security-for-agents|Supply Chain Security for Agentic AI]], though RAG and data poisoning defenses stayed open. The network layer gained Ed25519 agent-to-agent signing (Oktsec, vendor-side, not in the [[a2a-protocol|A2A v1.0]] spec) and Oktsec content scanning.
- **2026-08-17** — The [[owasp-ai-exchange|OWASP AI Exchange]] split the model- and data-layer gap in two. Published control guidance exists for both layers across a 50+ control catalogue, so the model-layer entry moves from a guidance gap to a tooling gap, and the data layer keeps a guidance-plus-tooling split. The four remaining layers are unchanged.
- **2026-08-18** — Document 4 of the [[owasp-ai-exchange|OWASP AI Exchange]] applied the model layer's guidance-versus-tooling split to the containment layer. Sandboxing and [[agent-escape|agent escape]] prevention now carry normative control text with stated limitations, so the layer's emerging grade describes its tooling. The shared-services cross-agent channel the Exchange names sits outside all six layers and is recorded as sub-question 7.
- **2026-08-19** — Section 2.1 of the [[owasp-ai-exchange|OWASP AI Exchange]] qualified the model layer's guidance grade rather than its tooling grade. Three of the layer's named controls carry self-undercutting citations or stated costs against unstated benefits, so published guidance splits into guidance a buyer can build against and guidance that cites its own refutation. The `INPUT DISTORTION` entry supplies the worked instance of the eleven-of-thirteen training-team split, since its runtime mechanism requires retraining the model.
- **2026-08-19** — Section 1.1 of the [[owasp-ai-exchange|OWASP AI Exchange]] added a control outside the six layers rather than changing any layer's grade. AI-specific honeypots are a named control with eight specified decoy assets, and no layer holds a deception primitive, so the inventory gained sub-question 8. The Exchange's governance cell moved from an asserted "management" label to six named controls, and its "no maturity criteria" entry narrowed: the catalogue supplies no organizational maturity criteria and does grade named standards' coverage of each control.
- **2026-08-20** — Section 1.2 of the [[owasp-ai-exchange|OWASP AI Exchange]] evidenced the data layer's guidance-versus-tooling split rather than changing it. Five named controls for sensitive data limitation carry normative text down to five obfuscation techniques and two encryption models, and none of them names a product, which makes the group the inventory's strongest instance of published guidance against absent tooling. The same section supplied the third member of the training-data-distortion set and the Exchange's own statement of the three-way purpose split.

## Open sub-questions

> [!gap] Open sub-questions
> 1. **Egress control patterns** — which egress mechanisms work: OPA/Cedar at the broker, a separate egress proxy, or network-segment isolation? Partially addressed: the credential proxy provides destination-based filtering and Docker DOCKER-USER chain rules are documented in [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]]; a dedicated page is still needed.
> 2. **AI-BOM operationalization** — beyond the CycloneDX ML extension, what is the production workflow? Partially addressed in [[ai-bom|AI-BOM]] (runtime AI-BOM via Miggo DeepTracing); full enterprise integration is still thin.
> 3. **Per-agent authorization at scale** — how does capability-based authorization (warrants) interact with traditional RBAC/ABAC at enterprise scale?
> 4. **Detection versus prevention for prompt injection** — [[prompt-injection-containment|Prompt Injection Containment]] documents the two-layer model: input detection plus execution containment. Detection reduces attack success but cannot eliminate it; containment limits blast radius when detection fails, and HITL confirmation closes the prevention gap for high-risk-tier actions.
> 5. **Data poisoning and RAG poisoning defenses** — no dedicated practice page exists. Three incidents and [[agentic-ai-security-cmm-2026|the CMM]] D6 (Data, Memory & RAG) reference these, with the CMM at L4 calling for RAGShield/TrustRAG-class document attestation, a memory-poisoning detector, and PoisonedRAG defense.
> 6. **Emergent multi-agent behaviors** — ASI07 (Insecure Inter-Agent Comms), ASI08 (Cascading Failures), and ASI10 (Rogue Agents) have no traditional equivalent. Partially addressed: see [[multi-agent-runtime-security|Multi-Agent Runtime Security]] for cascade detection, behavioral baselining, and inter-agent IR, and [[a2a-protocol|A2A Protocol]] for the v1.0.0 spec analysis. Cascade detection at scale is still at the academic-prototype stage.
> 7. **Shared services above the isolation boundary** — per-agent containment partitions execution and leaves the inference endpoint, the credential service, and the policy decision point shared. The Exchange names all three as implicit cross-agent channels ([`/go/agentsandboxing/`](https://owaspai.org/go/agentsandboxing/)). No layer in this inventory holds a control that closes the channel, because the containment layer's primitives enforce a boundary the channel sits above. What exists is a disclosure discipline: [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4]] grades at L5 whether a program has enumerated the three services and either partitioned each per agent or recorded it as an accepted channel with its residual risk stated. That criterion measures whether the operator knows about the channel, and the control that would partition inference, credential issuance, or policy evaluation per agent has no listed reference implementation.
> 8. **Deception has no layer** — the Exchange names AI-specific honeypots as a control and specifies eight decoy assets: a hardened data service left with an unpatched vulnerability, Elasticsearch being its named case; exposed data lakes revealing nothing about the real assets; data-access APIs vulnerable to brute force; "mirror" data servers that resemble development facilities but sit exposed in production with SSH access and a name like "lab"; documentation deliberately exposed and pointing at a honeypot; a data-science Python library exposed on the server; external access granted to a specific library; and models imported as-is from GitHub ([`/go/secprogram/`](https://owaspai.org/go/secprogram/)). No layer in this inventory holds it, for the reason stated above. Populating the layer needs an agent-estate decoy pattern with placement, alerting, and cleanup rules, and no source in the wiki carries one. The adjacent wiki instrument is [[mythos-ready-security-program|the Mythos-ready security program]], whose Priority Action 9 treats deception as a first-class action while this inventory has no layer to receive it.
