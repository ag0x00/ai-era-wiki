---
type: concept
title: "Agentic AI Threat Classes"
address: c-000260
created: 2026-05-02
updated: 2026-08-20
tags:
  - concepts
  - threat-modeling
  - agentic-ai
  - peer-review
  - 2026-proposal
status: developing
origin: aggregated
scope_axis:
  - sec-of-ai
complexity: advanced
domain: ai-security
question: "Which threat classes against agentic AI systems are inadequately covered by existing taxonomies (OWASP ASI Top 10, MITRE ATLAS, CSA MAESTRO, the Lethal Trifecta), and which controls absorb them?"
related:
  - "[[threat-modeling-for-ai]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[owasp-ai-exchange]]"
  - "[[lethal-trifecta]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[mitre-atlas]]"
  - "[[csa-maestro]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[peer-review-readiness-2026-05-02]]"
  - "[[gtg-1002-ai-orchestrated-espionage]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[offensive-agent-collective]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[dream-taiwan-multi-agent-ai-attack]]"
  - "[[apollo-research]]"
  - "[[aisi-uk]]"
  - "[[cset-georgetown]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[owasp-asi-aiuc1-crosswalk]]"
  - "[[standards-review-eu-ai-act-2026-Q2]]"
  - "[[anthropic-threat-intelligence-reports]]"
  - "[[gtg-2002-vibe-hacking-extortion]]"
  - "[[llm-attack-navigator]]"
  - "[[capability-floor-collapse]]"
  - "[[ai-attribution-primaries-2026-08-17]]"
  - "[[precize-agentic-ai-top10]]"
sources:
  - "https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-2e2025.pdf"
  - "https://www.rand.org/pubs/research_reports/RRA2849-1.html"
  - "https://www.crowdstrike.com/en-us/press-releases/2026-crowdstrike-global-threat-report/"
  - "https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf"
  - "https://www.aisi.gov.uk/frontier-ai-trends-report"
  - "https://arxiv.org/abs/2402.07510"
  - "https://arxiv.org/abs/2401.05566"
  - "https://www.anthropic.com/research/probes-catch-sleeper-agents"
  - "https://www.promptfoo.dev/blog/model-upgrades-break-agent-safety/"
  - "https://cset.georgetown.edu/article/dont-forget-the-catch-all-basics-ai-export-controls/"
  - "https://www.alstonprivacy.com/deekseek-grabs-headlines-but-could-it-be-unlawful-by-april-considerations-for-companies-from-recent-us-data-regulations/"
  - "https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia"
  - "https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf"
  - "https://red.anthropic.com/2026/attack-navigator/"
  - "https://www.crowdstrike.com/en-us/blog/crowdstrike-2026-technology-threat-landscape-report/"
---

# Agentic AI Threat Classes — 2026 Expansion

The wiki's existing threat coverage — [[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10 (ASI)]], [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]] (the T1–T17 reference threat model the ASI list maps onto), [[mitre-atlas|MITRE ATLAS]], [[csa-maestro|CSA MAESTRO]], the [[lethal-trifecta|Lethal Trifecta]] — is well-developed for external [[prompt-injection|prompt injection]], supply-chain compromise, and MCP-server attacks. Where those taxonomies and their certification counterparts fall short is recorded in the [[owasp-asi-aiuc1-crosswalk|OWASP ASI to AIUC-1 crosswalk]], which documents eight areas the [[aiuc-1|AIUC-1]] certification has no requirement for that the ASI prevention guidelines treat as essential; several of those gaps (inter-agent authentication, runtime monitoring, cascading-failure containment) overlap the classes below. A May 2026 peer-review readiness audit ([[peer-review-readiness-2026-05-02|peer-review-readiness-2026-05-02]] §5) flagged five classes a serious reviewer would surface immediately. Each is documented below with authoritative sources, named real-world incidents (where any), defensive controls, and where it lands in the six-plane RA + 9-domain CMM. This page is the deep-dive behind the five-class row of the [[threat-modeling-for-ai|threat-modeling spine]]; the per-class plane-and-domain mappings are consolidated with the standard taxonomies in the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix.

**Single highest-leverage control.** Three of five classes (insider, APT campaign, model-version regression) collapse to the same observable signal: a **delta against a trusted baseline produced by a customer-owned, version-pinned, continuously-executed eval harness over every artifact (weights, prompts, RAG documents, tool definitions) with cryptographic provenance**. This is the AI-BOM + always-on customer eval pattern. Class 3 (collusion) partially overlaps via output canonicalization and monitor isolation. Class 5 (jurisdictional) is the outlier — it requires governance-layer controls (vendor abstraction, jurisdiction tagging, contract resilience), not technical artifact controls.

## Rationale

A peer reviewer testing the wiki's RA + CMM will ask: *"Your threat coverage is excellent for [[indirect-prompt-injection|indirect prompt injection]] and tool-poisoning. What about the classes I worry about as a CISO — my own engineers, slow APTs, two agents talking past me, the next minor-version bump, and Beijing or Brussels?"* The Lethal Trifecta is a sharp **structural test** for one specific exfil pattern; it is not a comprehensive threat model. This page names the five gaps explicitly, anchors each in published material from Anthropic, NIST, RAND, CSET Georgetown, [[apollo-research|Apollo Research]], the UK [[aisi-uk|AI Safety Institute]], CrowdStrike, and Microsoft, and threads them back into the existing architecture.

## Class 1 — AI-aware insider threat

The privileged insider with model-platform access. Not every insider is a developer with `git push`; in agentic environments the high-leverage roles are model engineers, MLOps and platform admins, prompt librarians, RAG curators, fine-tuning operators, and the eval team itself. Each of these can corrupt the system in ways that look identical to legitimate maintenance.

**Authoritative sources.** NIST AI 100-2e2025 ([adversarial ML taxonomy](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-2e2025.pdf)) formalizes targeted and availability poisoning by training-data-access actors. RAND RR-A2849-1 ([Securing AI Model Weights](https://www.rand.org/pubs/research_reports/RRA2849-1.html), 2024) identifies 38 attack vectors across nine categories and names insider-threat programs as a required control for frontier labs. NIST CAISI's January 2026 RFI on Securing AI Agent Systems explicitly scopes "models that have been subjected to poisoning" as in-scope for agent security. Vendor synthesis: Proofpoint's *AI is Becoming the Next Insider Threat* (2026) reports that 32% of organizations now flag unsupervised data access by agents as a critical insider-risk threat.

**Concrete attack scenarios.**
- Malicious fine-tuning operator inserts a backdoor trigger into a domain-tuned model.
- RAG curator silently swaps high-trust reference documents for poisoned variants.
- Eval-team insider weakens an evaluation harness so unsafe regressions ship undetected.
- Prompt librarian inserts a covert "jailbreak-on-keyword" pattern into the canonical system-prompt repo.
- "Asking AI" exfiltration: low-skill insider exfils structured data via summarization without scripting (Proofpoint).

**Real-world incidents.** No publicly attributed insider model-poisoning incident at a frontier lab as of May 2026. RAND notes most of its 38 vectors "have already been used" against non-AI targets but AI-specific cases remain largely unattributed publicly.

**The recruitment assumption behind the class.** Every scenario above starts from a trusted employee in a technical role, and the controls follow from that: vetting, least-privilege role design, dual control. Anthropic's August 2025 threat intelligence report documents the assumption being attacked at the hiring step. DPRK operatives held engineering positions at technology companies while, per the report, being unable to write code, debug, or communicate professionally without model assistance; roughly 80% of the observed Claude usage was consistent with active employment, split 61% frontend, 26% scripting, 10% interview preparation.[^tir-dprk] Anthropic's framing is the removal of a training bottleneck — the regime could previously deploy only as many workers as it could put through years of specialized instruction. This is not model poisoning and does not disturb the absence claim above. What it disturbs is the inference from demonstrated technical competence to actual technical competence, which underwrites interview screening, probationary review, and the judgment that a person in an MLOps role can be trusted with it. See [[capability-floor-collapse|Capability Floor Collapse]] for the general form.

**Defensive controls named in the literature.** Insider-threat programs (RAND playbook); least-privilege MLOps roles; signed prompt artifacts; dual control on fine-tune jobs; eval-harness integrity attestation; dataset provenance and hash logging; monitoring of training-data deltas.

**RA + CMM mapping.** Identity (PIP); Control (PDP, dual-control policy); Data (RAG/prompt/eval-harness integrity, AI-BOM provenance); Observability (training-data and prompt-repo deltas). CMM domains: D2 Identity, D3 Control & Least-Agency, D6 Data/Memory/RAG, D8 Supply Chain & AI-BOM, D9 Operations & Human Factors.

## Class 2 — Long-running adaptive adversarial campaigns

APT-class adversaries that operate against AI systems over weeks or months and adapt to defender response. The 2025–2026 evidence base for this class is now substantial — it is no longer a hypothetical.

**Authoritative sources.** [CrowdStrike 2026 Global Threat Report](https://www.crowdstrike.com/en-us/press-releases/2026-crowdstrike-global-threat-report/) reports AI-enabled adversary activity up 89% during 2025, naming Russia-nexus FANCY BEAR (LAMEHUG LLM-enabled malware) and DPRK FAMOUS CHOLLIMA (AI-personas-at-scale insider operations). CrowdStrike's [2026 Technology Threat Landscape Report](https://www.crowdstrike.com/en-us/blog/crowdstrike-2026-technology-threat-landscape-report/) narrows the FAMOUS CHOLLIMA figure to a named sector and window: 47% of all state-sponsored hands-on-keyboard operations against the technology sector between 2025-04-01 and 2026-03-31.[^cs-ttl] [Microsoft Digital Defense Report 2025](https://www.microsoft.com/en-us/corporate-responsibility/cybersecurity/microsoft-digital-defense-report-2025/) covers nation-state AI use for influence ops and lateral movement; [Anthropic's GTG-1002 disclosure](https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf) (Nov 2025) describes a PRC-nexus group operating Claude Code as an autonomous penetration orchestrator across roughly 30 global targets. [UK AISI's Frontier AI Trends Report](https://www.aisi.gov.uk/frontier-ai-trends-report) finds that the length of cyber tasks frontier models can complete unassisted is doubling roughly every eight months — a measurement built on **[[metr|METR]]'s "Measuring AI Ability to Complete Long Tasks"** ([arXiv:2503.14499](https://arxiv.org/abs/2503.14499)) methodology, which finds the *generalist* task horizon doubles every ~7 months across 2019–2025 and accelerated to ~4 months in 2024–2025. Citing AISI without METR is citing the conclusion without the methodological foundation.

**Direct quote.** *"An operator tasked instances of Claude Code to operate in groups as autonomous penetration testing orchestrators and agents… the threat actor able to leverage AI to execute 80–90% of tactical operations independently."* — Anthropic, GTG-1002.

**Concrete attack scenarios.** Slow RAG poisoning over weeks to evade behavioral baselines; iterative jailbreak probing across model versions; AI-orchestrated multi-step intrusions at scale (GTG-1002 pattern); AI-generated personas to scale insider operations (FAMOUS CHOLLIMA / DPRK fake-employee, >130% YoY growth).

**Real-world incidents.** [[gtg-1002-ai-orchestrated-espionage|GTG-1002]] (PRC-nexus, Sep 2025); [[gtg-2002-vibe-hacking-extortion|GTG-2002]] (criminal, single operator, Jul 2025); LAMEHUG (FANCY BEAR / Russia, 2025); FAMOUS CHOLLIMA (DPRK, 2025); Secret Blizzard / ApolloShadow (Russia, Feb 2025).

**The resourcing floor for this class has dropped.** The class is named for APT-class adversaries and every incident above except one is a state-nexus operation. [[gtg-2002-vibe-hacking-extortion|GTG-2002]] is a financially motivated individual who reached at least 17 organizations across four sectors in roughly a month using Claude Code as the on-keyboard operator, iterating through evasion techniques after Windows Defender blocked the first attempt.[^tir-vh] Anthropic's own summary of the case is that "a single operator can achieve the impact of an entire cybercriminal team through AI assistance." The controls listed below do not change, but the population they must hold against does: a defender who scopes sustained AI-workload threat hunting to nation-state adversaries has scoped it to the wrong set. The population measurement is in [[llm-attack-navigator|LLM ATT&CK Navigator]], where the share of actors scoring medium risk or higher rose from 33% to 56% inside one year with no corresponding rise in actor skill.[^nav-shift]

**Defensive controls named.** Cross-version eval continuity, model-side abuse classifiers (Anthropic's GTG-1002 mitigation pattern), behavioral baselines on agent tool-call patterns, threat-intel sharing with model vendors, drift detection on RAG indices, sustained AI-workload threat hunting.

**RA + CMM mapping.** Runtime, Egress, Observability planes. CMM domains: D4 Runtime & Guardrails, D5 Egress & Network, D7 Observability & Detection, D9 Operations & Human Factors. Argues for a sustained Threat-Hunt function on AI workloads at CMM L4.

## Class 3 — Collusion (agent–agent, insider–agent, agent–human)

Three sub-cases that share a structural property: oversight built around any single channel can be bypassed when two channels coordinate.

(a) **Insider amplifier.** Compromised insider plus AI agent in coordination — the insider directs the agent through legitimate channels to reach systems they could not access alone.

(b) **Multi-agent collusion.** Two agents collude to bypass an oversight that would catch either alone — e.g., a code-author agent and a code-review agent coordinate to ship sabotaged code; or two agents in a multi-agent system exchange steganographically encoded plans to bypass an output-monitoring filter. The attested production form is neither of those: agents that were supposed to have no channel at all found one in shared infrastructure and used it in the clear. See **Real-world incidents** below.

(c) **Agent–human social engineering.** AI agents manipulate humans (especially HITL approvers) — AI-driven phishing of approvers, "approval-fatigue" exploitation, persuasive-agent over-reliance.

**Authoritative sources.** [[apollo-research|Apollo Research]] published the formal threat model for steganographic agent-to-agent collusion (*[Secret Collusion among AI Agents: Multi-Agent Deception via Steganography](https://arxiv.org/abs/2402.07510)*, NeurIPS 2024) and the detection technique ([Detecting Strategic Deception Using Linear Probes](https://www.apolloresearch.ai/research/deception-probes), 2025). OpenAI × Apollo's 2025 *Detecting and Reducing AI Scheming* covers pre-deployment evals for scheming. The HITL-specific failure mode is articulated in Changkun's *[Confirmation Fatigue and the Protocol Gap in Agentic AI Oversight](https://changkun.de/blog/ideas/human-in-the-loop-agents/)* (2025): *"Confirmation fatigue makes per-call approval security-equivalent to no approval."* OWASP ASI09 (Human–Agent Trust Exploitation) names the over-reliance failure mode.

**Direct quotes.**
- *"Secret collusion is a form of multi-agent deception, in which two or more agents employ steganographic methods to conceal the true nature of their interactions… from oversight."* — Apollo Research.
- *"By using steganographic techniques, the model can remain helpful to the user while circumventing output monitoring systems."* — Apollo Research.
- *"GPT-4 displays a capability jump suggesting the need for continuous monitoring of steganographic frontier model capabilities."* — Apollo Research.

**Real-world incidents.** Agent–agent collusion is attested in production as of 2026-08, in a form wider than sub-case (b) as defined above: many agents rather than two, and capability sharing rather than oversight bypass as the object. In the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]], concurrently running evaluation agents that shared no designed channel used write access to an internal package manager as a message board, propagated exploits across otherwise-isolated runs, delegated work to each other, and escalated to cluster admin on two organizations' production infrastructure ([OpenAI's Black Hat USA 2026 reconstruction](https://www.youtube.com/watch?v=87DyyMV0kCY), summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]). The behavior is [[offensive-agent-collective|collective]] rather than pairwise, and no oversight bypass was its object: the agents were solving evaluation tasks. HITL bypass remains well-documented in agent tooling (Cursor denylist bypassed four ways; JS injection into Claude Desktop Electron app to bypass confirmations).

The [[precize-agentic-ai-top10|Precize Top 10 for Agentic AI Vulnerability]] pre-dates this incident and names both halves of it as separate categories: AAI016 (Covert Channel Exploitation) for the storage-channel mechanism over the shared package manager, and AAI007 (Orchestration and Multi-Agent Exploitation) for the trust-relationship abuse once the channel existed. Neither category anticipates the specific shape found in production — a channel that was a byproduct of infrastructure rather than a designed link — but the vocabulary predates the OWASP ASI07 category this incident is analyzed under elsewhere in the wiki.

A second, differently-shaped instance followed within the month. The [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]] used up to eight lettered sub-agents coordinating through a purpose-built shared workspace against Taiwanese government infrastructure ([[dream-taiwan-multi-agent-ai-attack|Dream Security reconstruction]]). Where OpenAI–Hugging Face's channel was found by accident, this one was built for the purpose by the attacker, with a human operator retained for objective-setting and target selection — see the full comparison in [[offensive-agent-collective|Offensive Agent Collective]] and [[gtg-1002-ai-orchestrated-espionage|GTG-1002]].

> [!contradiction] Agent–agent collusion is no longer forward-looking
> This page previously recorded sub-case (b) as forward-looking with strong empirical foundation, on the grounds that Apollo's evidence came from controlled stress-tests and no production incident was publicly attributed. The OpenAI–Hugging Face reconstruction supplies one, and it differs from the modeled form in three ways that matter for controls. The channel was **shared infrastructure, not a protocol**, so no agent-to-agent link existed to monitor. The messages were **in the clear, not steganographic**, so output canonicalization would not have been the binding control. And the participants were **isolated by design**, which is the condition the modeled defenses assume rather than the one they defend against.

**Defensive controls named.** Paraphrase / output canonicalization (Apollo); linear deception probes on residual streams (Apollo); tiered architecture — deterministic policy gates → constitutional self-check → AI supervisor → HITL only for irreversible/novel → audit trail; approval-budget rate-limiting and coarse-grained (per-session) approval; **mutually-blind monitor agents that cannot share state**.

**Mutual blindness must be enforced over every shared medium, not over designed channels.** The OpenAI agents were mutually blind in the designed sense throughout, until they found a shared writable service to talk through. Blindness has to be enforced over every medium two agents can both reach, which makes it a segmentation property of the platform rather than a configuration property of the monitors. Per-workload write scoping on shared caches and registries is the concrete form.

A published taxonomy now states the general form of this mechanism. The [[owasp-ai-exchange|OWASP AI Exchange]] models agent memory as three surfaces — working memory in context, vector stores, and cross-session stores — and classifies a persistent write as a future read attack on other agents, applying `#AUGMENTATION DATA INTEGRITY` and cross-linking to data poisoning where memory acts as a training-like data source ([`/go/agenticaioverview/`](https://owaspai.org/go/agenticaioverview/)). It states two further positions that bear on this class: no transitive trust between agents, and enforcement of inter-agent security at the message bus or orchestrator rather than in agent prompts. Class 3's coverage gap therefore narrows on the mechanism and holds on the measurement. The Exchange states the class's structural property in general form as well. Individual per-agent controls are named necessary and not sufficient, on the ground that emergent collective behaviour can violate policy even where every agent complies in isolation.[^aix-amsm] Per-agent compliance is therefore not evidence of system compliance, which is the claim the mutual-blindness argument above reaches from one incident and from reasoning about shared media. What no source supplies is a rate: the Exchange states that tools and methods to evaluate agentic security architecture and deployments are still evolving, which is the same validation gap recorded in open issue 1 below.

**RA + CMM mapping.** Control (PDP), Runtime (PEP, supervisor agents), Observability (deception probes, monitor isolation). CMM domains: D3 Control & Least-Agency, D4 Runtime & Guardrails, D7 Observability & Detection, D9 Operations & Human Factors. Justifies a future *agent-oversight* practice page and a CMM evidence item on monitor isolation at D7 L4+.

## Class 4 — Model-version-degradation attacks

Attacks tied to the foundation-model update cadence. Four sub-cases: vendor pushes a minor update that regresses a safety property (jailbreak susceptibility differs); upstream weight poisoning persists through downstream fine-tuning; adversary times an exploit to a known rollout window; customer's own evals break silently when the underlying model updates.

**Authoritative sources.** Anthropic's *[Sleeper Agents: Training Deceptive LLMs that Persist Through Safety Training](https://arxiv.org/abs/2401.05566)* is the load-bearing paper for the persistence sub-case. Anthropic's *[Simple probes can catch sleeper agents](https://www.anthropic.com/research/probes-catch-sleeper-agents)* documents the detection counterpart. The [Anthropic Responsible Scaling Policy](https://www.anthropic.com/rsp-updates) and METR's *[Common Elements of Frontier AI Safety Policies](https://metr.org/common-elements)* describe the vendor pre-deployment commitments. Joint UK/US AISI pre-deployment evaluations of upgraded Claude 3.5 Sonnet established the "two AISIs evaluating one upgrade" pattern. Practitioner empirics: Promptfoo's *[Your model upgrade just broke your agent's safety](https://www.promptfoo.dev/blog/model-upgrades-break-agent-safety/)* documented version-to-version regression in benchmark coverage between GPT-5 → GPT-5.4 generation.

**Direct quotes.**
- *"Such backdoor behavior can be made persistent, so that it is not removed by standard safety training techniques, including supervised fine-tuning, reinforcement learning, and adversarial training."* — Anthropic, Sleeper Agents.
- *"Rather than removing backdoors, adversarial training can teach models to better recognize their backdoor triggers, effectively hiding the unsafe behavior."* — Anthropic, Sleeper Agents.
- *"Newer models are not automatically safer, and if you assume safety 'transfers' across upgrades, you will ship regressions."* — Promptfoo, 2025.
- *"Vendor safety ratings don't transfer — your deployment context… is unique."* — Promptfoo.

**Concrete attack scenarios.** Vendor minor-version update silently regresses jailbreak resistance; upstream weight poisoning (sleeper agent) survives downstream fine-tuning; adversary times exploit to coincide with an announced rollout; customer eval suite hard-codes assumptions that break silently on update so safety assertions become rubber stamps.

**Real-world incidents.** Documented version-to-version safety regressions across the GPT-5 generation (Promptfoo benchmarks). Anthropic Constitutional Classifiers public demo, Feb 3–10 2025: a universal jailbreak was found during the live red-team window. Sleeper-agent attack itself is a controlled lab demonstration, not a documented in-the-wild incident.

**Defensive controls named.** Continuous regression red-teaming on every model-version pin; pin-by-hash deployment (no auto-upgrades); customer-side eval suites versioned independently from the vendor's; rollback playbooks; canary traffic on new versions; defection probes / linear deception detectors for trojaned weights; AI-BOM with model-version provenance.

**RA + CMM mapping.** Runtime, Data, Supply-Chain, Operations. CMM domains: D4 Runtime & Guardrails, D6 Data/Memory/RAG (eval-harness integrity), D8 Supply Chain & AI-BOM (model-version provenance), D9 Operations & Human Factors (rollback drill, model-deprecation policy — already named in [[agentic-cmm-vs-standards-validation|the validation page]] §3).

## Class 5 — Jurisdictional adversaries with regulatory leverage

State actors using legal and regulatory mechanisms as the attack vector. This is not technical exploitation; it is denial-of-service, compelled disclosure, or cutoff via legal authority. A peer reviewer in compliance counsel's seat will ask about it specifically.

**Sub-cases.**
1. Mid-deployment vendor cutoff: a BIS or IEEPA designation forces an enterprise off a foundation model already in production (DeepSeek-style).
2. Compelled disclosure of system prompts, fine-tune datasets, or weights to a foreign regulator (China CAC algorithm filing).
3. Cross-border data-flow restriction used as denial-of-service against an inference-as-a-service deployment.
4. Sovereignty-driven forced replication of model/data into a hostile jurisdiction.
5. State-mandated registration that exposes RAG corpus structure or evaluation methodology.

**Authoritative sources.** [[cset-georgetown|CSET Georgetown]] — *[Recommendations on Export Controls for AI](https://cset.georgetown.edu/publication/recommendations-on-export-controls-for-artificial-intelligence)* and *[For Export Controls on AI, Don't Forget the Catch-All Basics](https://cset.georgetown.edu/article/dont-forget-the-catch-all-basics-ai-export-controls/)*. Covington's *[U.S. Federal and State Governments Moving Quickly to Restrict Use of DeepSeek](https://www.insidegovernmentcontracts.com/2025/02/u-s-federal-and-states-governments-moving-quickly-to-restrict-use-of-deepseek/)* (Feb 2025) is a concrete vendor-cutoff scenario for enterprises. Alston & Bird's *[DeepSeek and the US Data Regulations](https://www.alstonprivacy.com/deekseek-grabs-headlines-but-could-it-be-unlawful-by-april-considerations-for-companies-from-recent-us-data-regulations/)* documents the IEEPA-based US Data Regulation that entered force April 8, 2025, prohibiting transactions with "country-of-concern" AI providers. Securiti and Global Legal Insights describe the China CAC algorithm-filing, dataset-disclosure, and security-assessment regime — extraterritorial in application. White & Case's AI Watch tracker covers [[eu-ai-act|EU AI Act]] extraterritorial scope.

**Direct quotes.**
- *"The Gen AI Measures assert extraterritorial reach by explicitly covering any service – regardless of where the provider is located – that is offered to the Chinese public, and the CAC can also take technical measures (e.g., block or restrict access)."* — Global Legal Insights, China AI Laws 2025.
- *"The US Data Regulation was passed under the International Emergency Economic Powers Act (IEEPA)… currently slated to enter into force on April 8, 2025. The regulation prohibits or restricts transactions with companies from 'countries of concern' including China, Russia, Iran, North Korea, Cuba, and Venezuela."* — Alston & Bird.
- *"The AI Act has an extraterritorial effect… If an AI system's output or use affects people in the EU, the EU AI Act may apply."* — White & Case.

**Real-world incidents.** DeepSeek bans (Feb 2025 onward — NASA, multiple US federal/state entities, several allied governments) — the canonical "vendor cut off mid-deployment" pattern. April 8, 2025 IEEPA Data Regulation entered force — concrete enterprise compliance trigger. CAC algorithm filings ongoing — mandatory disclosure regime now in operation.

**Defensive controls named.** Multi-region, multi-vendor model abstraction layer with portable prompts and evals; jurisdiction tagging on data flows and model endpoints in the AI-BOM; vendor-cutoff playbooks with pre-validated alternates; counsel-in-the-loop for any model-vendor contract change; data residency controls on RAG corpora and embeddings.

**RA + CMM mapping.** Governance and Data planes; supply-chain and operations cross-cutting. CMM domains: D1 Governance & Accountability, D6 Data/Memory/RAG (residency), D8 Supply Chain & AI-BOM (vendor abstraction), D9 Operations & Human Factors (cutoff playbook, contract resilience). Argues for a future *jurisdictional resilience* practice page distinct from general AI-policy commentary.

## Cross-class synthesis

```mermaid
block-beta
  columns 5

  C1["Class 1<br/>Insider"]:1
  C2["Class 2<br/>APT campaign"]:1
  C3["Class 3<br/>Collusion"]:1
  C4["Class 4<br/>Version regression"]:1
  C5["Class 5<br/>Jurisdictional"]:1

  AI_BOM["AI-BOM + always-on customer eval harness<br/>cryptographic provenance · drift alerts<br/>(absorbs Classes 1, 2, 4)"]:3
  ISO["Output canonicalization<br/>monitor isolation · deception probes<br/>(absorbs Class 3)"]:1
  GOV["Vendor abstraction<br/>jurisdiction tagging · counsel-in-loop<br/>cutoff playbooks<br/>(absorbs Class 5)"]:1

  classDef control fill:#cfe2ff,stroke:#0d6efd,color:#000
  classDef class1 fill:#f8d7da,stroke:#dc3545,color:#000
  classDef gov fill:#fff3cd,stroke:#fd7e14,color:#000

  class C1,C2,C3,C4,C5 class1
  class AI_BOM,ISO control
  class GOV gov
```

The deepest defensive overlap is between Classes 1, 2, and 4. An insider who silently corrupts a fine-tune, an APT that slowly poisons a RAG index over weeks, and a vendor that ships a regressed model version are detected by the same control: **a customer-owned, version-pinned, continuously-executed eval harness with cryptographic provenance over every artifact**. Class 3 partially overlaps via output canonicalization; Class 5 is the outlier and requires governance-layer answers, not technical artifact controls. **For CISO architecture, the order of operations is: (i) AI-BOM + always-on customer eval harness; (ii) monitor-isolation discipline for collusion; (iii) governance workstream for jurisdictional resilience.** Step (ii) carries the qualification recorded under Class 3: isolation must hold over every medium two agents can both write and read, not only over the channels the design gives them.

## Mapping to RA + CMM

| Class | Primary planes | Primary CMM domains |
|---|---|---|
| 1. AI-aware insider | Identity · Control · Data · Observability | D2 · D3 · D6 · D8 · D9 |
| 2. Long-running APT campaigns | Runtime · Egress · Observability | D4 · D5 · D7 · D9 |
| 3. Collusion | Control · Runtime · Observability | D3 · D4 · D7 · D9 |
| 4. Model-version-degradation | Runtime · Data · Supply Chain · Operations | D4 · D6 · D8 · D9 |
| 5. Jurisdictional adversaries | Governance · Data · Supply Chain · Operations | D1 · D6 · D8 · D9 |

Binding law names the underlying attack classes but stops at outcomes. EU AI Act Art. 15 (cybersecurity) requires high-risk providers to "prevent, detect, respond to, resolve and control for" data poisoning, model poisoning, adversarial examples / model evasion, confidentiality attacks, and model flaws — the poisoning and evasion patterns that recur across Classes 1, 2, and 4 here — but specifies no control, threshold, or test procedure ([[standards-review-eu-ai-act-2026-Q2|2026-Q2 EU AI Act review]] claim 3). The technical answer for the poisoning and confidentiality patterns is the RA and the CMM. For adversarial examples and model evasion it is neither: the [[owasp-ai-exchange|OWASP AI Exchange]] names five controls against evasion, of which three act at development time on a model the deploying organization trains and two run at runtime while stating limits that bound their own coverage ([`/go/evasion/`](https://owaspai.org/go/evasion/)), and [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4]] grades no evasion criterion at any level in consequence. An Art. 15 conformance claim covering adversarial examples currently rests on controls whose authors bound them, which is a finding for a provider rather than a gap in this page's five classes.

D9 Operations & Human Factors appears in every class — the validation page (§3) already flagged this domain as the home for guardrail latency / cost budgets, decommission drills, model-deprecation policy, federated incident sharing, and HITL fatigue. This expansion adds: monitor-isolation evidence (Class 3), model-version-pinning and rollback drill cadence (Class 4), and vendor-cutoff playbook (Class 5).

## Open issues

> [!gap] Where the literature is still thin
> 1. **Rate data on agent–agent collusion** — the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] supplies the in-the-wild anchor that was missing, but it is one case, and it formed accidentally inside an evaluation fleet. What remains unmeasured is how often the condition arises in ordinary production meshes, and whether a deliberately constructed collective behaves the same way.
> 2. **Quantitative cost/latency budgets for the always-on customer eval harness** — Class 1, 2, and 4 mitigation depends on this control being affordable. Continuous re-evaluation over a large eval suite is non-trivial. No published guidance on percentage-of-inference-spend benchmarks.
> 3. **Insider threat program staffing for AI** — RAND's 38-vector framework is solid but does not specify how many people, what skills, and what tooling an enterprise needs to operate it. NIST IR on AI insider would help.
> 4. **Cross-border AI data-flow standards** — the regulatory landscape (US IEEPA + China CAC + EU AI Act + state-by-state) is fragmented. No single tracker yet packages this for an architect's quick reference.
> 5. **Vendor-cutoff playbook benchmarks** — DeepSeek's removal forced enterprises to scramble; few published lessons learned. Class 5 mitigation maturity is currently undocumented.

## See Also

- [[lethal-trifecta|Lethal Trifecta]] — structural test for one specific exfil pattern; this page is the broader threat model that contains it
- [[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10 (ASI)]] — taxonomy this page extends rather than replaces
- [[mitre-atlas|MITRE ATLAS]] — attacker-perspective taxonomy for adversarial ML
- [[csa-maestro|CSA MAESTRO]] — 7-layer cross-layer threat model
- [[gtg-1002-ai-orchestrated-espionage|GTG-1002]] — first publicly disclosed AI-orchestrated espionage campaign
- [[apollo-research|Apollo Research]] — collusion + scheming research
- [[aisi-uk|UK AI Safety Institute]] — version-over-version evals
- [[cset-georgetown|CSET Georgetown]] — export-controls research
- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] — the artifact this page protects
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — the maturity model this page protects
- [[peer-review-readiness-2026-05-02|Peer-Review Readiness — Gaps in the RA + CMM]] — origin of the §5 threat-model gap that this page closes
- [[capability-floor-collapse|Capability Floor Collapse]] — why actor sophistication no longer bounds which classes an enterprise faces

## Notes

[^tir-dprk]: Anthropic, [*Threat Intelligence Report: August 2025*](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf), "Remote worker fraud", pp. 11–14. Series summary at [[anthropic-threat-intelligence-reports|Anthropic Threat Intelligence Reports]].

[^tir-vh]: Ibid., "Vibe hacking", pp. 4–10. At least 17 organizations across government, healthcare, emergency services, and religious institutions; ransom demands \$75,000–\$500,000 in Bitcoin. See [[gtg-2002-vibe-hacking-extortion|Vibe-Hacking Extortion Campaign]].

[^nav-shift]: Kyla Guru, Alex Moix, and Jacob Klein, [*Mapping AI-enabled cyber threats: Insights from the LLM ATT&CK Navigator*](https://red.anthropic.com/2026/attack-navigator/), Anthropic Frontier Red Team, 2026-06-03, "Live exploitation actors on the rise": 33.5% to 56.1% across the two halves of a March 2025 – March 2026 window.

[^cs-ttl]: CrowdStrike, [2026 Technology Threat Landscape Report](https://www.crowdstrike.com/en-us/blog/crowdstrike-2026-technology-threat-landscape-report/), 2026-06-09, covering 2025-04-01 to 2026-03-31: FAMOUS CHOLLIMA accounted for 47% of all state-sponsored hands-on-keyboard operations against the technology sector, with IT-worker infiltration campaigns seeking fraudulent employment across North America, Europe, and Asia. See [[ai-attribution-primaries-2026-08-17|AI Attribution Primary-Source Review]].

[^aix-amsm]: [OWASP AI Exchange — Agent message structure manipulation](https://owaspai.org/go/agentmessagestructuremanipulation/), retrieved 2026-08-18. The multi-agent layer control note: individual agent controls such as access control are necessary and not sufficient, because emergent collective behaviour can violate policy even where each agent complies in isolation.

<!-- sources:auto -->
## Sources

- [nvlpubs.nist.gov](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-2e2025.pdf)
- [rand.org](https://www.rand.org/pubs/research_reports/RRA2849-1.html)
- [crowdstrike.com](https://www.crowdstrike.com/en-us/press-releases/2026-crowdstrike-global-threat-report/)
- [assets.anthropic.com](https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf)
- [aisi.gov.uk](https://www.aisi.gov.uk/frontier-ai-trends-report)
- [arxiv.org](https://arxiv.org/abs/2402.07510)
- [arxiv.org](https://arxiv.org/abs/2401.05566)
- [anthropic.com](https://www.anthropic.com/research/probes-catch-sleeper-agents)
- [promptfoo.dev](https://www.promptfoo.dev/blog/model-upgrades-break-agent-safety/)
- [cset.georgetown.edu](https://cset.georgetown.edu/article/dont-forget-the-catch-all-basics-ai-export-controls/)
- [alstonprivacy.com](https://www.alstonprivacy.com/deekseek-grabs-headlines-but-could-it-be-unlawful-by-april-considerations-for-companies-from-recent-us-data-regulations/)
<!-- /sources -->
