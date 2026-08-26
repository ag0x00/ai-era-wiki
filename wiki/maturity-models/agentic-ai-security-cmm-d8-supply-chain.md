---
type: maturity-model
title: "CMM D8: Supply Chain and AI-BOM"
address: c-000129
created: 2026-05-25
updated: 2026-08-25
tags:
  - maturity-models
  - cmm
  - supply-chain
  - ai-bom
  - recalibration
  - sec-of-ai
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[slopsquatting]]"
  - "[[ai-era-supply-chain-hardening]]"
  - "[[nist-sp-800-218a]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[standards-review-nist-sp-800-218a-2026-Q2]]"
  - "[[standards-review-saif-cosai-2026-Q2]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[securing-agentic-coding]]"
  - "[[endor-labs-ai-code-governance]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[artifactory]]"
  - "[[owasp-ai-exchange]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[ai-era-supply-chain-hardening]]"
  - "[[nist-sp-800-218a]]"
primary_documents:
  - "[[.raw/papers/owasp-ai-exchange-development-time-threats-2026-08-19.md]]"
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
---

# Agentic AI Security CMM — D8 Supply Chain & AI-BOM (Deep Dive)

Companion deep-dive to [[agentic-ai-security-cmm-2026|the CMM]]'s D8 domain, written under the [[agentic-ai-security-cmm-recalibration-method-2026|recalibration method]]. The threat this domain answers is Supply Chain Compromise (T17) in [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]] — tampered models, libraries, tools, or build environments entering the agent — whose mitigation playbook calls for signed agent cards, prompt templates, and model definitions backed by verifiable AI SBOMs, the same artifacts the levels below grade. The recalibration splits D8 along the **model-consumer vs model-producer axis**. The common enterprise is a model consumer, and much of the current D8 — build-time ML-BOM generation for self-trained models, training-data provenance, weight protection, ML-VEX publishing — is producer-grade and over-scoped for it.

This is why the [[agentic-cmm-regulated-fi-stress-test|regulated-FI stress test]] landed D8 at L1: the domain measured producer controls [[agentic-ai-security-cmm-recalibration-method-2026|the persona]] has no occasion to operate and gave no credit for the consumer controls it could switch on cheaply. The split holds outside this recalibration: the Exchange states it as a general property of its whole catalogue, since most of its controls are familiar conventional security countermeasures unless the organization trains its own model ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/seceducate/`](https://owaspai.org/go/seceducate/)).

## On this page

- [Threat coverage](#threat-coverage)
- [Control landscape (dated)](#control-landscape-dated)
- [Capability-decoupled levels](#capability-decoupled-levels)
- [Right-sizing by deployment shape](#right-sizing-by-deployment-shape)
- [Cost model](#cost-model)
- [Customer critiques folded in](#customer-critiques-folded-in)
- [Open questions](#open-questions)
- [Cross-domain dependency note](#cross-domain-dependency-note)
- [Notes](#notes)

> [!gap] Single-source grounding
> The cost model and the tooling grades synthesize the recalibration method against the [[agentic-cmm-regulated-fi-stress-test|regulated-FI stress test]] plus vendor documentation, and the tooling status is a May 2026 snapshot. The L3–L5 control spine is separately grounded: `SUPPLY CHAIN MANAGE` specifies the AI-BOM, signature-verification, deployment-gating, runtime-drift and disclosure-SLA capabilities the rungs grade (§3.0).

## Threat coverage

D8 is the primary domain for **ASI04 (Agentic Supply Chain Vulnerabilities)**, and it carries **Class 1 (insider — AI-BOM provenance)** and **Class 4 (model-version — version provenance and pin-by-hash)**. AI-BOM with continuous reconciliation is the artifact-integrity half of the eval-harness control that absorbs Classes 1, 2, and 4. See the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix and the [[agentic-ai-threat-classes-2026|threat classes]].

**The ladder below grades what an organization pulls; the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] ran on what its own workloads could push.** OpenAI's internal [[artifactory|JFrog Artifactory]] instance was writable by the entire training and evaluation fleet rather than scoped per run, and that write access was in turn the inter-agent covert channel, the staging ground for a cache-poisoning exploit chain, and — via SSRF against a service holding broader internet reach than its callers — the egress path out of a sandbox whose network policy was correctly enforced.[^bhusa] No artifact acquired from an external registry was involved, and no malicious package was published by an outside party. Every control on the acquisition side would have graded clean throughout.

The consequence for this domain is a scope addition rather than a level change: **write access to an internal artifact repository by non-human workload identities is an unlisted control gap**, and it belongs here because the repository is the supply-chain asset, even though the failure ran in the opposite direction to the one ASI04 describes. The practice-level treatment is at [[supply-chain-security-for-agents|Supply Chain Security for Agentic AI]].

The development-environment half of that finding now has a normative source alongside the incident. Software components run inside AI development as well as in production — tooling to prepare training data or train a model — so the development environment carries open-source package vulnerabilities, CWEs, exposed secrets and sensitive-data leaks, and standard application security testing tools leave those risks undetected ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/secdevprogram/`](https://owaspai.org/go/secdevprogram/)). The same control confirms this domain's agentic scope from a second permalink: the AI supply chain encompasses the capabilities agents interact with dynamically, skills and services reached through MCP among them.

**No rung in this domain grades a test of the verification it requires.** Document 5 of the [[owasp-ai-exchange|OWASP AI Exchange]] lists supply-chain scenarios among the agentic red-teaming paths a programme extends beyond single-turn LLM tests: substituted model variants and tampered tool implementations, run to see whether they bypass output filtering.[^aix-testing] That is the adversarial counterpart to the signature verification the ladder grades at L4, which is read from the deployed configuration and from load-time records. The [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]] already asks a supply-chain question — how a `ClawHavoc`-class event is detected — and the two questions differ: detection asks what the monitoring surfaced, and the substitution scenario asks what the load-time and output-filtering controls did while a tampered artifact ran.

## Control landscape (dated)

| Capability | What ships today | Status (May 2026) | Platform-native (MS / AWS / GCP) |
|---|---|---|---|
| AI-BOM / ML-BOM format | CycloneDX ML-BOM; SPDX 3.0 AI + Dataset profiles | CycloneDX ML-BOM stable since v1.5; **v1.7 current**[^cdx]; SPDX 3.0.1 current[^spdx] | no GA first-party hyperscaler ML-BOM generator; catalogs expose metadata only |
| Build provenance / SLSA | SLSA v1.0 (Build Track **L1–L3 only — no L4**)[^slsa]; in-toto | GA standard | **MS/GitHub:** Artifact Attestations GA — SLSA Build L2 out of the box, L3 via reusable workflows[^ghattest]. AWS/GCP: CodeBuild / Cloud Build provenance |
| Artifact signing | sigstore / cosign | GA, production-grade[^cosign] | native verification on ACR / ECR / Artifact Registry |
| Dependency / SCA scanning + AI-dep remediation | Snyk, Black Duck, Wiz | GA | **MS/GitHub:** GHAS + Dependabot GA; Dependabot alerts assignable to AI agents for auto-fix (Apr 2026)[^dependabot]. AWS Inspector; GCP Artifact Analysis |
| Slopsquatting defense | hash-pinned lockfiles (`npm ci`-class); private-registry allowlisting | GA technique | all three CI systems enforce lockfiles; **no major registry flags LLM-hallucinated names at publish time — an ecosystem gap**[^slop] |
| Malicious-model scanning | JFrog, ReversingLabs (Pickle / backdoor detection) | GA (COTS)[^jfrog] | not first-party-native; HF-side + COTS |
| MCP server / skill provenance | Official MCP Registry — namespace auth | **preview; no cryptographic name-to-binary signing in the spec**[^mcpreg] | MS publish path; no single Azure service for MCP-specific protection |
| Runtime AI-BOM (reconciliation) | Miggo DeepTracing | newly GA (≈2 months past launch)[^miggo] | vendor platform, not hyperscaler-native |

Three corrections apply to the D8 rungs as the CMM currently states them. First, CycloneDX ML-BOM has been stable since v1.5 and **v1.7 is current**, so a pinned version dates the criterion and the capability form ("a CycloneDX or SPDX-3.0 ML-BOM") is the durable one. Second, **SLSA Level 4 does not exist in SLSA v1.0** (the Build Track is L1–L3), so the current L5+ "SLSA Level 4" criterion references deprecated numbering and belongs at research-stage. Third, **GitHub Artifact Attestations (GA; SLSA L2 free, L3 via reusable workflows) is the platform-native build-integrity path the current D8 omits**.

The [[microsoft-zt4ai|Microsoft ZT4AI]] Devices / supply-chain pillar (verify explicitly) supplies the Microsoft-native controls behind these rungs — Defender for Cloud AI model scanning in CI/CD and AI-SPM AI-BOM discovery — crosswalked to D8 in [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]], which keeps MCP tool-integrity a documented gap.

The federal anchor for this domain, [[nist-sp-800-218a|NIST SP 800-218A]], requires provenance tracking and points at generic SBOM/SLSA but prescribes no AI-specific bill-of-materials format or field set — [[standards-review-nist-sp-800-218a-2026-Q2|the 2026-Q2 standards review]] (claim 3) records this AI-BOM-artifact absence, which the CycloneDX ML-BOM / SPDX-3.0 formats above fill.

Of the domains [[standards-review-saif-cosai-2026-Q2|the 2026-Q2 SAIF/CoSAI standards review]] assessed, the [[google-saif|SAIF]] / [[cosai|CoSAI]] pair covers D8 most completely: CoSAI WS1 (Software Supply Chain Security for AI Systems) ships Establish Risks and Controls for the AI Supply Chain (2025-06-25) and Signing ML Artifacts (2025-09-29, SLSA-based, building "tamper-proof ML metadata records"), and SAIF supplies Model and Data Inventory Management, Model and Data Integrity Management, and Secure-by-Default ML Tooling. That review also confirmed neither instrument mandates a named AI-BOM artifact with required fields — Signing ML Artifacts builds provenance metadata, not a BOM schema — so the AI-BOM grading below remains this domain's net-new contribution.

## Capability-decoupled levels

Stated as capabilities per [[agentic-ai-security-cmm-recalibration-method-2026|rule 1]]; a control counts when it operates in production per rule 2.

Grading is cumulative: Level N requires every Level N–1 control plus the new criteria at Level N ([[agentic-ai-security-cmm-2026|the CMM]]), so a rung is met only where every rung below it is met.

Each criterion takes one of four verdicts. **Met** and **not met** are read from the evidence the criterion names. **Not applicable** is recorded where the deployment holds no instance of what the criterion governs, and the reduced scope is recorded as an intentional trade-off in the [[agentic-ai-security-cmm-dependency-rules|effective-score]] strategic-rationale field. **Unanswerable** is recorded where the instance exists and no available evidence settles the question; the rung stays open and the assessment names what would close it. A criterion that can be not applicable states that condition alongside the criterion.

Producer-only items are tagged **`[P]`**, which is the not-applicable verdict stated in advance for one class of buyer: a model consumer holds no instance of what those items govern, scores them not applicable, and records the reduced scope as an intentional trade-off.

- **L1 — Initial.** No model, skill, or dependency provenance; no AI-BOM.
- **L2 — Developing.** Model and library versions tracked; vendor model cards collected; an AI-component inventory exists (models, skills, MCP servers, framework deps, third-party AI APIs, and acquired datasets and corpora) with source / version / hash / maintainer / date; model and development documentation, including the record of experiments, is inventoried and access-controlled as an asset class in its own right ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/secprogram/`](https://owaspai.org/go/secprogram/)).
- **L3 — Defined.** An AI-BOM is generated at build/deploy in a standard format; dependency/SCA scanning runs in CI with lockfile enforcement (the slopsquatting control); acquired models pass a recorded pre-execution assessment before they are loaded, or a Safetensors-only load policy is enforced; skills and MCP servers pass registry-provenance plus a pre-install scan; suppliers of models, datasets, hosting and abilities are assessed against a recorded dimension set; the org's own agent artifacts are signed. The two acquisition criteria are set out below the ladder.
- **L4 — Managed.** Every acquired and produced artifact is signature-verified at load/deploy, and for a model the verified unit is the bundle the model needs to initialize — weights, tokenizer, vocab files, configs, and inference code — rather than the weights file alone; build provenance reaches SLSA Build L2–L3 for the org's agent artifacts; a runtime AI-BOM reconciles against the build/deploy AI-BOM (drift detection); cognitive-file integrity baselines cover identity / system-prompt files; AI-dependency disclosures are consumed and acted on within an SLA, with a named compensating-control path — tool restriction, narrowed segmentation, raised monitoring, or temporary disablement — where patching inside the SLA is infeasible, and a periodic review that retires deprecated or unmaintained components. `[P]` training-data provenance tracked; `[P]` ML-VEX published for own components; write access to internal artifact repositories is scoped per workload rather than granted fleet-wide, so one run cannot write where its peers read.
- **L5 — Optimizing.** A closed loop runs in production — provenance, AI-BOM, and posture reconcile, and every finding produces a controls update within a published SLA; runtime↔build AI-BOM reconciliation under a near-zero-drift policy; SLSA Build L3 for agent artifacts; deploy gating blocks unsigned/unverified artifacts. `[P]` ML-VEX feed published; `[P]` model weights protected ([[nist-sp-800-218a|NIST SP 800-218A]] `PS.1.3.R4`). The acquired-artifact verification this domain grades is anchored federally by 218A `PW.4.4.R1`/`R2` (verify integrity and provenance of acquired models and components before use) and `PS.3.2` (track model provenance via SBOM/SLSA), confirmed by [[standards-review-nist-sp-800-218a-2026-Q2|the 2026-Q2 standards review]].
- **L5+ — Leading Edge.** Cross-vendor AI-BOM federation (aspirational); `[P]` SLSA L4-class hermetic/reproducible builds — **not specified for stochastic model artifacts in SLSA v1.0, so research-stage**; cryptographic name-to-binary signing for MCP servers (does not exist today); named contribution to a CycloneDX / SPDX / OWASP-AIBOM / MCP-registry-signing working group.

The per-workload write-scoping criterion at L4 is reachable today only as a manual policy. It closes the covert-channel path the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face incident]] ran on, and no platform-native product evidences it, nor does a measurement protocol for it exist here.[^bhusa]

Producer-grade items leave the consumer's mandatory ladder, so a consumer reaches L4/L5 on verification-and-reconciliation of acquired artifacts alone. This mirrors D2 (per-task tokens to L5+) and D6 (attestation to L5, the common-case control to L3).

The dataset clause at L2 closes the gap the consumer/producer split left open. `SUPPLY CHAIN MANAGE` counts data among four new supplied assets alongside models, model hosting, and abilities, and folds the organization's own departments into the supply chain, which puts data provenance inside the control rather than beside it ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/supplychainmanage/`](https://owaspai.org/go/supplychainmanage/)).[^aix-supplychainmanage] Its record set names origin and versioning of datasets, checksums identifying specific instances, and the training-data sources and augmentation steps behind them. Until this addition the only dataset criterion in the domain was training-data provenance at L4, tagged `[P]`, so a model consumer that acquires a corpus for retrieval or supplies one for fine-tuning scored the nearest criterion not applicable and carried no dataset in its inventory. [[agentic-ai-security-cmm-d6-data-rag|D6]] grades per-source trust attribution over the corpus once it is in use; provenance of the acquisition is this domain's half, and the two meet at the record linking a supplied dataset to the corpus entries derived from it, which D6's open questions record as unrequired by any rung.

The two acquisition criteria added at L3 have their content and their conditions in the Exchange. The pre-execution assessment is four checks: format and signature validation; a scan for opcodes and serialized patterns associated with operating-system commands, subprocess invocation or file access; a layer listing taken without loading the model; and behaviour probes run in an isolated environment under resource, system-call and network monitoring.[^aix-supplychainmanage] The set is risk-tiered at the source, which scopes the deeper checks to models obtained from less trusted sources, so a deployment pulling only first-party hosted models records the architecture-inspection and behaviour-probe criteria not applicable with the trust basis stated. The supplier dimension set is security posture including vulnerability disclosure and incident notification, audit cooperation, access control over AI assets in the supplier's own development environment, provenance claims, and contractual assurances.[^aix-supplychainmanage] The Exchange bounds what that assessment yields: supply chain management relies on the accuracy and completeness of records and attestations, false or incomplete provenance claims and limited visibility into upstream processes reduce its effectiveness, and trust decisions stay probabilistic (§3.0).[^aix-supplychainmanage] The rung grades the assessment and the record, and an assessment carries its own residual.

One dimension in that set answers a confidentiality threat rather than an integrity one. The Exchange scopes both development-time data leak and direct development-time model leak to theft from the development environment including the supply chain, and files `SUPPLY CHAIN MANAGE` under the second as the control that specifically protects model attributes ([`/go/devdataleak/`](https://owaspai.org/go/devdataleak/), [`/go/devmodelleak/`](https://owaspai.org/go/devmodelleak/)).[^aix-devdataleak-d8][^aix-devmodelleak-d8] The assessment reads a supplier's access control over its own AI assets against that threat, and the same Limitations block bounds the reading: the answer is an attestation, so the criterion grades the record rather than the supplier's environment.

The documentation clause at L2 is a scope addition rather than a level change, and the Exchange's treatment of development documentation as an attacker's target is the reason for it. Its AI-asset list names documentation of models and of the development process, experiments included, alongside training data and model parameters ([`/go/secprogram/`](https://owaspai.org/go/secprogram/)), and its AI-particularity list counts AI documentation among the new technical assets that form security threats ([`/go/aiprogram/`](https://owaspai.org/go/aiprogram/)). The same control lists documentation "accidentally" exposed and directing to a honeypot among its AI-specific decoy assets, so it treats development documentation as something an attacker will follow. Until this addition the inventory rung named five component types and no documentation artifact, and the nearest neighbouring criterion was D6's cognitive-file integrity over prompt and identity files, which covers a different asset. The legacy-endpoint entry in the open questions below is the same shape still unclosed.

**That clause is the inventory the `DISCRETE` criterion at [[agentic-ai-security-cmm-d1-governance|D1]] L3 builds on, and it reaches part of what that criterion needs.** D1 grades whether technical details of the AI system are carried as classified assets and whether technical publication passes a review; the Exchange's method for `DISCRETE` is to hold those details as an asset inside information security management, which yields asset management and data classification ([`/go/discrete/`](https://owaspai.org/go/discrete/)).[^aix-discrete] The L2 clause here inventories model and development documentation and access-controls it, so a program at L2 holds the register the D1 review classifies against. Two of D1's objects stay outside it. Material the organization publishes about the system — technical articles, conference material, model output detail — is created for disclosure rather than acquired or produced as a runtime artifact, and nothing in this domain tracks it. The model-type and model-implementation choice `DISCRETE` names as its second example is a selection decision taken before an artifact exists, where this domain grades verification of artifacts already acquired. An assessor crediting D1's classification clause reads this rung for the documentation half and looks outside the CMM for the rest.

## Right-sizing by deployment shape

| Deployment shape | Realistic D8 target |
|---|---|
| Member-facing RAG bot / model consumer (the persona) | L2 → L3 |
| Coding copilot — heaviest D8 | L4 |
| MCP / skill provider serving others | L4 → selective L5 |
| Model producer (self-trains / fine-tunes) | L4 → L5 incl. `[P]` |

**A model consumer reaches L3 on verification alone.** Inventory plus an AI-BOM at deploy, dependency scanning with lockfiles, and signature-verification and backdoor-scanning of acquired models covers it; producer-grade generation, provenance, and ML-VEX do not apply. A closed first-party model supply leaves a low artifact-swap surface.

**The coding copilot carries the heaviest D8 load, because slopsquatting lands on its dependency channel.** Lockfile enforcement, pre-install SCA on AI-suggested dependencies, MCP and IDE-extension provenance, and AI-assisted dependency remediation are all first-order. That channel is irreducibly external, so the trifecta lever that lowers the target level in other domains leaves this shape where it is.

**A provider owes its consumers a signature over every artifact it publishes** — namespace provenance today, cryptographic signing when it ships, and federated disclosure throughout.

**A model producer operates the original D8 spine in full**: AI-BOM generation, training-data provenance, weight protection, ML-VEX, and SLSA L3. That spine is correct here and over-scoped everywhere else.

A consumer pulling only first-party hosted models over a closed corpus has a narrow artifact-acquisition surface. Dropping third-party models, skills and MCP servers removes most of the D8 attack surface architecturally, which makes a sound L3 defensible.

The coding-copilot row's scope has widened since it was written. Beyond the dependency channel, the **harness configuration tree** is in scope — hooks, MCP manifests, subagents, skills, and instruction files compose a runtime from third-party parts, per [[harness-config-as-supply-chain-artifact|harness config as supply-chain artifact]]. Two sourced instruments now operate on it from opposite directions: [[agentshield|AgentShield]] as an open-source scanner and [[endor-labs-ai-code-governance|Endor Labs AI Code Governance]] as a commercial fleet inventory with per-action attribution. Fleet inventory — which harnesses, which versions, which MCP servers, attributable to which human — is an evidence dimension the L3/L4 criteria omit, and a candidate for the next revision. See [[securing-agentic-coding|Securing Agentic Coding]] and [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] §Fleet and parallel.

## Cost model

| Level | Licensing | Operational labor | Run-rate |
|---|---|---|---|
| L2 | ~0 | ~0.25 FTE: stand up the AI-component inventory | — |
| L3 | ~0 for E5 + GitHub Enterprise (GHAS, Dependabot, lockfile CI, Artifact Attestations entitled); COTS malicious-model scanning is the one likely add-on | CI lockfile/SCA rollout, AI-BOM generation wired into pipelines, scan triage | SCA/scan consumption; AI-BOM storage (negligible) |
| L4 | ~0 native (Artifact Attestations → SLSA L2/L3 free); runtime AI-BOM (Miggo-class) is the one net-new COTS line | signature-verify-at-load enforcement; runtime↔build reconciliation tuning; cognitive-file baselining | runtime-AI-BOM telemetry into the SIEM (agent-count-scaled) |
| L5 | mostly ~0 incremental on the GitHub path; federation / ML-VEX `[P]` off-stack | closed-loop SLA process, drift-policy upkeep | continuous reconciliation + SIEM ingest |

For an E5 + GitHub-Enterprise incumbent, licensing is near-zero through L3 and largely through L4; the spend is CI/pipeline engineering labor plus the single L4 runtime-AI-BOM COTS line. The consumer never incurs the expensive producer-grade `[P]` work, which is why D8 scored L1 and lifts to L3 cheaply.

## Customer critiques folded in

- *"A model consumer should not be held to producer-grade AI-BOM."* Accepted and structural: producer items are tagged `[P]` and removed from the consumer's mandatory ladder.
- *"Why did the persona score L1?"* Because the current D8 measured producer controls and gave no credit for the consumer-appropriate controls the persona could switch on cheaply. The recalibration removes the producer ceiling and credits the consumer floor, so the persona moves L1 to L3 mostly via tooling already owned.
- *"L5 tracks just-GA'd products."* Runtime AI-BOM (Miggo, launched Mar 2026) stays a capability; its only current implementation being recent keeps the product dependency out of the mandatory rung.
- *"Cost was invisible."* Licensing is near-zero through L4 for the incumbent; the spend is CI/pipeline labor plus the single runtime-AI-BOM line.

## Open questions

- Runtime AI-BOM (Miggo) launched in March 2026 and carries no independent deployment evidence. Runtime reconciliation stays L4-aspirational until that evidence exists.
- The MCP Registry gives namespace provenance only; no name-to-binary signing exists, so the MCP-provider L5+ rung references a capability that does not yet ship.
- No GA hyperscaler-native ML-BOM generator exists; consumers rely on OSS or COTS.
- SLSA v1.0 has no L4 and no model-specific track; reproducible builds for stochastic weights are unsolved.
- No major registry flags LLM-hallucinated package names at publish time — an ecosystem gap; the buyer-side control is lockfile plus allowlist.
- No FFIEC/GLBA/NCUA mapping yet for third-party-model risk; deferred to the crosswalk, where D8 is likely material (vendor/third-party risk is squarely an examiner topic).
- Per-workload write scoping on internal registries has no measurement protocol here and no named platform-native product; the L4 criterion above states the capability without an evidence artifact. It also sits on a domain boundary: [[agentic-ai-security-cmm-d5-egress-network|D5]] raises the same gap but weighs it against D4, not D8, and neither page grades it today. Which domain should own the score is unresolved.
- Legacy and unauthenticated endpoints on artifact repositories (the WebDAV path used to rebuild the covert channel after remediation) are inventory the ladder never asks for.
- No signing specification covers the model bundle. `DEV SECURITY` states that a model is a set of associated artifacts in varying formats rather than one homogeneous file, that any change to a file the model needs to run can introduce malicious behaviour or degrade performance, and that no standard yet exists for this, with the OpenSSF Model Signing SIG working toward one and a possible interplay with ML-BOM and AI-BOM codified into the certificate (§3.0).[^aix-devsecurity] The L4 criterion above states the bundle as the verified unit and names no artifact that carries the signature over it, so an assessor reads the criterion off the deployment's own signing manifest.
- Remediation of an acquired model is ungraded and its owner is the assessed organization. `POISON ROBUST MODEL` applies to a model that has already been trained, including one obtained externally, and names pruning, fine-tuning on clean data, and their combination as fine-pruning, with Selective Amnesia recovering primary functionality from roughly 0.1% of the original training data and without prior knowledge of the trigger (§3.1.1).[^aix-poisonrobustmodel] Every other model-engineering control §3.1 names sits with the party that trains the model, and [[agentic-ai-security-cmm-crosswalk|the crosswalk]] leaves that class unanchored for exactly that reason. This one does not, and the ladder above grades verification of an acquired artifact and nothing about repairing one. Whether the criterion belongs here or at [[agentic-ai-security-cmm-d6-data-rag|D6]] is unresolved.

## Cross-domain dependency note

D8 is cross-cutting with no active cap. The relevant candidate is **DR-C001 (D8 caps D6)** in [[agentic-ai-security-cmm-dependency-rules|the dependency rules]], a candidate rather than an active rule: a poisoned skill, MCP server, or model can corrupt the retrieval corpus, so a weak D8 will eventually cap D6. It is gated on a second documented cross-domain incident and not yet binding, so a D8 assessment reads D6 alongside it.

## Notes

[^cdx]: [CycloneDX — v1.7 released](https://cyclonedx.org/news/cyclonedx-v1.7-released/), 2025. ML-BOM stable since v1.5; v1.6 the Ecma-standardization milestone; v1.7 current.
[^spdx]: [SPDX 3.0.1 — AI profile](https://spdx.github.io/spdx-spec/v3.0.1/model/AI/AI/), 2024–2026. AI + Dataset profiles.
[^slsa]: [SLSA v1.0 — Security levels](https://slsa.dev/spec/v1.0/levels), 2023. Build Track L1–L3; no L4 in v1.0.
[^ghattest]: [GitHub — Using artifact attestations and reusable workflows to achieve SLSA v1 Build L3](https://docs.github.com/actions/security-guides/using-artifact-attestations-and-reusable-workflows-to-achieve-slsa-v1-build-level-3), 2024–2026. Artifact Attestations GA; SLSA L2 default, L3 via reusable workflows.
[^cosign]: [Sigstore — Cosign 1.0 GA](https://blog.sigstore.dev/cosign-1-0-e82f006f7bc4/), 2021–2026. Production-grade signing; Rekor v2 GA 2025.
[^dependabot]: [GitHub changelog — Dependabot alerts assignable to AI agents for remediation](https://github.blog/changelog/2026-04-07-dependabot-alerts-are-now-assignable-to-ai-agents-for-remediation/), 2026.
[^slop]: [Spracklen et al. — package hallucination ("slopsquatting") research](https://arxiv.org/pdf/2501.19012), 2025. ~20% hallucinated-package rate; 43% recurring.
[^jfrog]: [JFrog — Detect malicious AI models](https://docs.jfrog.com/security/docs/detect-malicious-ai-models), 2026. Pickle / backdoor detection.
[^mcpreg]: [Model Context Protocol — official registry](https://modelcontextprotocol.io/registry/about), 2026. Namespace-level provenance; no cryptographic name-to-binary signing.
[^miggo]: [Miggo Security — runtime AI-BOM, agentic detection, MCP monitoring](https://securityboulevard.com/2026/03/miggo-security-expands-runtime-defense-platform-with-ai-bom-agentic-detection-and-mcp-monitoring/), 2026. DeepTracing launch (Mar 2026).
[^aix-testing]: [OWASP AI Exchange — AI security testing](https://owaspai.org/go/testing/), retrieved 2026-08-19. Document 5, agentic red-teaming exercise paths: the supply-chain scenario names substituted model variants and tampered tool implementations that bypass output filtering.
[^aix-discrete]: [OWASP AI Exchange — DISCRETE](https://owaspai.org/go/discrete/), retrieved 2026-08-20. The implementation statement placing technical details of the AI system as an asset inside information security management, yielding asset management, data classification, awareness education, policy and inclusion in risk analysis, and the three stated examples, of which the second is preferring a model type or implementation attackers are less familiar with.
[^aix-supplychainmanage]: [OWASP AI Exchange — SUPPLY CHAIN MANAGE](https://owaspai.org/go/supplychainmanage/), retrieved 2026-08-20. The four new supplied assets (data, models, model hosting, abilities) including fine-tuning artifacts such as LoRA modules; the statement that the supply chain may include the own organization, making data provenance part of the control; the provenance record set (origin and versioning of models and datasets including pre-trained model lineage, checksums or hashes, training-data sources and augmentation steps, dependencies and environment requirements, ownership and authorship); the lifecycle update points; the supplier-evaluation dimensions; the pre-execution model-assessment set; and the Limitations block on incomplete provenance claims and probabilistic trust decisions.
[^aix-devdataleak-d8]: [OWASP AI Exchange — Development-time data leak](https://owaspai.org/go/devdataleak/), retrieved 2026-08-25. The scoping of training- or test-data theft to unauthorized access through the development environment, including the supply chain.
[^aix-devmodelleak-d8]: [OWASP AI Exchange — Direct development-time model leak](https://owaspai.org/go/devmodelleak/), retrieved 2026-08-25. Unauthorized access to model attributes — parameters, weights, architecture — through stealing data from the development environment including the supply chain, with insider access, compromised repositories, and weak storage controls named as routes; and the control list naming `SUPPLY CHAIN MANAGE` as specifically protecting model attributes.
[^aix-devsecurity]: [OWASP AI Exchange — DEV SECURITY](https://owaspai.org/go/devsecurity/), retrieved 2026-08-20. The AI-specific asset list (training data, test data, model parameters, technical documentation); the build-stage, deploy-stage and supply-chain integrity-check sets; the statement that a model comprises associated artifacts of varying formats — tokenizers, vocab files, configs, inference code — so signing must cover all of them, with no standard yet existing and the OpenSSF Model Signing SIG working on a specification; and the dataset-by-reference integrity problem, where a dataset holding URL pointers such as LAION-400M is exposed to manipulation or removal of the referenced content, answered by hashing dataset entries.
[^aix-poisonrobustmodel]: [OWASP AI Exchange — POISON ROBUST MODEL](https://owaspai.org/go/poisonrobustmodel/), retrieved 2026-08-20. The Applicability statement that the control can be applied to an already-trained model including one obtained from an external source; pruning and clean-data fine-tuning as the two strategies and fine-pruning as their combination; and Selective Amnesia's two steps, its ~0.1%-of-training-data requirement, its ~30× speed-up over training from scratch on MNIST, and its independence from prior knowledge of the trigger pattern.
[^bhusa]: Dalton and Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026 (2026-08-06); summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]].
