---
type: maturity-model
title: "CMM D4: Runtime and Guardrails"
address: c-000126
created: 2026-05-25
updated: 2026-09-18
tags:
  - maturity-models
  - cmm
  - guardrails
  - recalibration
  - sec-of-ai
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[prompt-injection]]"
  - "[[lethal-trifecta]]"
  - "[[gke-agent-sandbox]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[anthropic-sandbox-runtime]]"
  - "[[securing-agentic-coding]]"
  - "[[cmm-known-limitations]]"
  - "[[security-guidance-plugin]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[owasp-ai-exchange]]"
  - "[[agent-escape]]"
  - "[[agent-sandbox-isolation-landscape]]"
  - "[[inference-exposure]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[oversight-layer]]"
  - "[[least-agency-principle]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[cyera-agent-guardian-release]]"
  - "[[falcon-guardian]]"
  - "[[agent-runtime-protection-canvass-2026-09]]"
  - "[[chain-of-thought-monitorability]]"
  - "[[claude-cowork]]"
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[prompt-injection]]"
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
verified: 2026-09-18
verified_against: []
verified_findings: 0
verified_note: "Fresh-eyes and source read of the desktop-agent productivity-assistant row against Anthropic's live Cowork documentation: the Team/Enterprise, architecture, OTel and enterprise-administrator articles, the Cowork overview and monitoring reference, and the Compliance API announcement. Nothing archived to .raw/. Scoped to the desktop-agent content this pass added; the rest of the page was not re-read."
---

# Agentic AI Security CMM — D4 Runtime & Guardrails (Deep Dive)

Companion deep-dive to [[agentic-ai-security-cmm-2026|the CMM]]'s D4 domain, written under the [[agentic-ai-security-cmm-recalibration-method-2026|recalibration method]]. D4 is the [[oversight-layer|Policy Enforcement Point]] at runtime: it enforces what [[agentic-ai-security-cmm-d3-control-least-agency|D3]] decides. The runtime threats it answers map to [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]]: Tool Misuse (T2), Intent Breaking and Goal Manipulation (T6), Unexpected RCE and Code Attacks (T11), and Rogue Agents in Multi-Agent Systems (T13), whose playbooks call for execution sandboxing with per-call reset and in-path reasoning-manipulation controls. The recalibration regrades this domain against what ships. The L2/L3 input-and-output controls are GA and cheap, and the L4 spine the current CMM names as deployable (chain-of-thought auditing, groundedness checking) sits at preview or experimental status, short of GA.

**The levels and the cost model rest on one line of grounding.** They synthesize the recalibration method against the [[agentic-cmm-regulated-fi-stress-test|regulated-FI stress test]] plus vendor documentation, rather than on independent sources that agree. The tooling status below is a May 2026 snapshot.

## Threat coverage

D4 is the primary domain for **ASI01 (Agent Goal Hijack)** and **ASI05 (Unexpected Code Execution)**, and the runtime home of **Class 2 (APT — cross-version eval continuity)**, **Class 3 (collusion — supervisor agents)**, and **Class 4 (model-version regression — continuous red-teaming)**. Its effective score is capped by D3, since a guardrail cannot enforce a decision the policy decision point never makes. See the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix and the [[agentic-ai-threat-classes-2026|threat classes]].

The [[agent-escape|agent escape]] threat entry is graded primarily in [[agentic-ai-security-cmm-d3-control-least-agency|D3]], which holds the capability-based access control that decides a call. D4 holds the boundary that bounds the reach of a decision that was wrong, and the [[owasp-ai-exchange|OWASP AI Exchange]] keeps the two layers distinct: infrastructure-layer enforcement blocks escape even where jailbreak has already succeeded at the reasoning layer.[^aix-escape]

The Exchange cuts the same pair on a different axis. `LEAST MODEL PRIVILEGE` is preventative and `OVERSIGHT` is detective — reactive or gate-based — and the Exchange states that both may apply to the same action tier.[^aix-oversight] A domain boundary drawn on decide-versus-enforce and a control boundary drawn on prevent-versus-detect cross rather than coincide, so a control anchored at D3 can be the detective one and a control anchored here can be the preventative one. `OVERSIGHT` itself is categorised as a runtime control. The Exchange divides it into automated and human oversight, and the automated side carries two mechanisms that sit on different planes. The first recognizes unwanted output and suspicious action sequences in model output, before or after execution, and this domain grades it. The second withholds an action until a policy gate returns, and [[agentic-ai-security-cmm-d3-control-least-agency|D3]] grades that. The three-domain split is set out in [[agentic-ai-security-cmm-crosswalk|the CMM crosswalk]], and [[oversight-layer|the oversight layer]] states the same automated side as detection at the Policy Information Point (PIP) and enforcement at the PEP.[^aix-oversight]

## Control landscape (dated)

| Capability | What ships today | Status | Microsoft | AWS | GCP |
|---|---|---|---|---|---|
| Input prompt-injection / jailbreak classifier | Meta PromptGuard 2 (OSS); NVIDIA NemoGuard NIM; Lakera, HiddenLayer | GA | Content Safety Prompt Shields, direct + indirect, GA[^ps] | Bedrock Guardrails prompt-attack filter, GA[^bedrock] | Model Armor, launch stage not stated[^ma] |
| Chain-of-thought / alignment auditing (goal-hijack) | Meta AlignmentCheck (OSS) | **Experimental**[^pg2] | Content Safety Task Adherence — **public preview**[^ta] | none native | none native |
| Code-gen static safety | Meta CodeShield (OSS) | GA-equivalent (OSS)[^pg2] | none native | none native | none native |
| Output content safety / filtering | NeMo Guardrails; Guardrails AI | GA | Content Safety, GA | Bedrock filters, GA | Model Armor, launch stage not stated[^ma] |
| Groundedness / hallucination check | — | **preview / partial** | Groundedness Detection — **public preview, English-only**[^ground] | Bedrock Automated Reasoning checks, GA (US-East)[^ar] | check grounding API, launch stage not stated[^grounding] |
| Tool-call interception / gating | Microsoft Agent Governance Toolkit (OSS); AgentShield (OSS) | OSS GA-equivalent | Defender real-time protection for Agent 365 tooling servers — **GA 2026-07-27**; agent threat detection — **public preview**[^def] | — | — |
| Sandboxing of high-risk tasks | MiniClaw (OSS reference); [[gke-agent-sandbox\|Agent Sandbox]] (gVisor) | OSS primitive + managed GKE | Foundry hosted-agent microVM sandbox — preview[^foundry] | AgentCore code-interpreter sandbox | [[gke-agent-sandbox\|GKE Agent Sandbox]][^agentsandbox] + Vertex sandboxed execution |

The input/output safety layer (L2/L3) is GA on the Microsoft and AWS stacks and sits largely inside Azure entitlements. Google announces general availability for named Model Armor features and integrations and states no launch stage for the core prompt-and-response screening service,[^ma] and the filter set a template runs is region-gated, so a template in a limited-support region with data-residency compliance enabled loses malicious URL detection, multi-language detection, CSAM support, image support and antivirus scanning.[^maregion] The agentic-reasoning layer (L4) has not reached that status either: chain-of-thought auditing ships as preview or experimental, Microsoft's groundedness detection is preview, and Google states no launch stage for the check grounding API,[^grounding] so an L4 program assembles the layer rather than buying it.

The [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]] is an operational example of the failure class the jailbreak-classifier row grades against. A framing-based jailbreak ("authorized penetration testing") defeated the underlying models' refusals across a four-day, 12-wave campaign.[^taiwan] The graded capability is holding a refusal against a social-engineering frame that persists for days. A GA classifier in the path establishes the precondition for that grade and supplies no evidence of it.

Cyera's Protect phase is a further vendor example of the tool-call interception row above: Cyera states it blocks a risky tool call during execution ([[cyera-agent-guardian-release|Cyera Agent Guardian Release]]). Cyera names the block and gives no efficacy figure, no bypass rate, and no decision mechanism behind that block, so that row's graded status is unchanged.

[[falcon-guardian|CrowdStrike Falcon Guardian]] is a second vendor example on that row and the first placed at the endpoint. CrowdStrike states that Falcon Guardian defines which AI agent types may run on a managed endpoint and blocks the rest, and that it correlates a user prompt to the agent's skill use, tool calls, MCP server invocations and downstream system actions. Analyst coverage of the announcement reports CrowdStrike as claiming 99% detection efficacy against prompt attacks at under 100 ms response latency,[^nand-fg] which is the first efficacy figure attached to this row from any source. The figure is a single aggregate with no published methodology, no bypass library, and no per-language breakdown, and the L5 criterion below requires a per-language miss rate measured against a current bypass library with classifier-refresh receipts, so the row's graded status is unchanged. CrowdStrike gives no general-availability date, so rule 2 is unmet regardless.

The independent runtime-protection category was canvassed as a whole in September 2026, and it supplies no row of this table at the L4 tier. Twenty-one vendors were graded against the five L4 capabilities: chain-of-thought auditing drew no documented implementation, groundedness drew one product naming the concept with no method behind it, generated-code safety drew one (Operant AI CodeInjectionGuard, launched 2026-04-21), trusted/untrusted context boundaries drew one documented mechanism whose product is in early access, and semantic tool-call validation drew implementations that match schemas and patterns rather than comparing intent against a proposed action. Seven vendors market agentic runtime security over documentation that describes input and output filtering. No capability the canvass found clears rule 2's two-quarter window: the generally available implementations reached GA inside it, and the rest stands at preview, early access, or undocumented status. The assembled route this domain already describes therefore remains the route, and no row's graded status changes. [[agent-runtime-protection-canvass-2026-09|The canvass]] carries the per-vendor grades.

The [[microsoft-zt4ai|Microsoft ZT4AI]] Apps & Workloads pillar (assume breach) supplies the Microsoft-native runtime controls behind these rungs — Prompt Shields, Groundedness Detection, and Task Adherence — crosswalked to D4 in [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]], which records the same GA-versus-preview split.

## Capability-decoupled levels

Stated as capabilities per [[agentic-ai-security-cmm-recalibration-method-2026|rule 1]]; a control counts when it operates in production per rule 2. A control implemented through a product in the organization's approved-vendor pipeline with a documented production date satisfies its criterion on that basis alone; a product the organization does not yet run in production satisfies none, whatever the vendor has announced.

- **L1 — Initial.** No runtime guardrails, or only system-prompt instructions. No enforcement boundary.
- **L2 — Developing.** A default safety filter runs on input and a content-safety classifier on output. Single-layer, no agentic-reasoning coverage. Universally reachable GA.
- **L3 — Defined.** Input arrives canonicalized and screened in path for direct *and* indirect [[prompt-injection|prompt injection]], output leaves through a content-safety classifier whose data-class scope is recorded, platform lifecycle hooks intercept the agent loop, and high-risk-tier actions run in a per-task sandbox built to the [[owasp-ai-exchange|OWASP AI Exchange]] specification. The classifiers and hooks ship on the major stacks at differing launch stages and the rest is configuration, so an assessor grades this rung off the deployment rather than off a product's status page.
- **L4 — Managed.** Runtime control reaches the agent's reasoning and the semantics of its tool calls — chain-of-thought auditing, code-safety analysis, groundedness checking, framework-enforced trusted/untrusted context boundaries, and semantic validation of high-impact calls — and human approval stays mandatory for configured high-blast-radius operations however clean those checks come back. The load-bearing controls are preview, experimental, or specification-only, so an L4 program assembles this rung and evidences part of it from its own pipeline.
- **L5 — Optimizing.** Every L4 control runs platform-level on every agent surface with no opt-out, and every guardrail carries a measured efficacy figure and an enforced budget — per-language bypass miss rates, encoding-aware response-leak scanning at egress, and latency and cost limits that fail closed on critical paths. The services every sandbox shares are enumerated, and each is either partitioned per agent or accepted in writing with its residual risk stated.
- **L5+ — Leading Edge.** Cryptographic TEE attestation that guardrails executed in an enclave (reference pilots only); a CaMeL-style [[camel-pattern|privileged/quarantined LLM split]] in production (research); measurable bypass-class evidence with vendor-acknowledged remediation cycles.

The level structure survives the recalibration largely intact. Product names move to the tooling map, and L4 gains an explicit maturity grade. The current text presents Azure Groundedness and AlignmentCheck as deployable. Both are preview or experimental, and a regulated buyer cannot deploy a preview control to claim a GA-grade L4.

Sandbox strength is graded from L3 rather than assumed, and the control's own limits bound what the grade asserts. The [[owasp-ai-exchange|OWASP AI Exchange]] states that container or hypervisor escape undermines containment and that OS-specific behaviour can weaken the same configuration on a different host.[^aix-sandbox] An L3 sandbox therefore asserts containment on the integrity of a boundary the buyer does not test, which is the same evidence problem the coverage callout below raises for scope and start order. Shared inference, credential, and policy services sit above every boundary in the deployment and reach across all of them, which the L5 criterion grades.

Two adjacent controls fall outside this domain. Exfiltration through legitimately permitted APIs is out of segmentation's reach and is graded in [[agentic-ai-security-cmm-d5-egress-network|D5]], which also takes the call-volume and tool-invocation halves of the Exchange's per-agent quota control, leaving D4 the compute and wall-clock ceilings the sandbox platform imposes. [[agent-sandbox-isolation-landscape|The sandbox and isolation landscape]] compares what each shipping sandbox partitions.

Tool-output sanitization before content enters the agent's context — stripping role-change patterns, validating against schema, labelling trust by source, and scanning for exfiltration-oriented encoding — is specified by the Exchange alongside these controls and is graded nowhere.[^aix-piioh] That control sits on the tool-call path, which [[agentic-ai-security-cmm-d5-egress-network|D5]] owns, and D5's nearest criterion is A2A content scanning at L4, scoped to inter-agent traffic and stopping short of a tool response returning to a single agent. This page records it as an ungraded control with an open domain assignment rather than as a D4 rung.

Recitation detection is a further output-side mechanism the Exchange specifies and this ladder does not grade. It matches output against an indexed set of the training data to catch the model reproducing training content verbatim, and the Exchange states its reach as limited to what is indexed, with shorter and paraphrased disclosures escaping it.[^aix-soh] The index is a development-time artifact of the party that trained or fine-tuned the model, so a deployment consuming a third-party model cannot construct it, and a rung graded against it would grade the model provider rather than the buyer. This page records it as an ungraded control whose owner sits upstream of the assessed organization, alongside the tool-output sanitization case above.

Ensemble deviation detection is a third such mechanism, and the Exchange files it as a runtime implementation of a development-time decision. `MODEL ENSEMBLE` deploys a model as several models trained on a randomly split training set, so that an output deviating from its peers can be ignored as possible evidence of a poisoned training set. The Exchange states that its effectiveness falls as the poisoned share of the dataset rises (§3.1).[^aix-modelensemble] The detection runs at inference and the decision that makes it possible — how the training set is split and how many models are trained — is taken by whoever trains the model, so a deployment consuming a third-party model has no instance of it. This page records it with recitation detection, as an ungraded control owned upstream. The L4 semantic-validation criterion below draws on the same diversity argument for a different purpose: a judge from a different model family is independent of the model it checks, and an ensemble's members are independent of one another; both hold only as far as the shared training corpus lets them.

## Assessor detail per level

L1, L2, and L5+ are graded from their statements above. The three rungs below carry criteria an assessor checks item by item, each list stating what its own rung adds.

Grading is cumulative: Level N requires every Level N–1 control plus the new criteria at Level N ([[agentic-ai-security-cmm-2026|the CMM]]), so a rung is met only where every rung below it is met.

Each criterion takes one of four verdicts. **Met** and **not met** are read from the evidence the criterion names. **Not applicable** is recorded where the deployment holds no instance of what the criterion governs, and the reduced scope is recorded as an intentional trade-off in the [[agentic-ai-security-cmm-dependency-rules|effective-score]] strategic-rationale field. **Unanswerable** is recorded where the instance exists and no available evidence settles the question; the rung stays open and the assessment names what would close it. A criterion that can be not applicable states that condition alongside the criterion. The lists below hold criteria only; a paragraph after a list carries maturity or market commentary and states no criterion.

### L3 detail

- **Input canonicalization ahead of the classifier.** Unicode NFKC normalization with zero-width and invisible characters removed, locale-independent case-folding rather than locale-sensitive lowercasing, and confusable-character collapsing under Unicode UTS #39 with mixed-script runs flagged.[^aix-piioh] The criterion is read off configuration rather than off a product's GA status, because the Exchange demonstrates that normalization and case-folding are separate and both required — `NFKC("İGNORE")` is unchanged and Cyrillic `а` (U+0430) survives NFKC — and that a locale-sensitive lowercase on a Turkish-locale host maps ASCII `I` to dotless `ı` and breaks a blocklist match.[^aix-piioh] A classifier graded GA does not answer whether the text reaching it was canonicalized first.
- **In-path injection detection and agent-loop interception.** An input classifier detects direct *and* indirect/document [[prompt-injection|prompt injection]], and platform lifecycle hooks intercept the agent loop. The Microsoft and AWS classifiers carry a GA grade (Prompt Shields indirect mode, Bedrock prompt-attack), and Google states no launch stage for Model Armor's core screening service.[^ma] The indirect half is evidenced by presenting attack inputs through the same insertion path untrusted data takes — a retrieved document, a tool output — rather than through the user channel, so the filtering, detection, and insertion mechanisms in that path are all in scope.[^aix-testing] A vendor's indirect-mode flag states that the capability exists and not that the deployment's augmentation route reaches it.
- **Output content-safety classifier with its data-class scope recorded.** Content safety and exposure-restricted data are separate detections, and the [[owasp-ai-exchange|OWASP AI Exchange]] specifies the second as a final safeguard on personal data, confidential identifiers, passwords, and tokens before output reaches a user or a downstream system, applying wherever the model has reached such data and the output can reach an unauthorized actor.[^aix-soh]
- **Per-task sandbox on high-risk-tier actions, meeting the Exchange specification.** Process spawning, out-of-workspace filesystem access, agent self-configuration, and host interaction confined; a mandatory access control profile with unneeded kernel capabilities disabled; transient state, cached data, and in-sandbox credential material destroyed at task completion or forced stop; per-agent compute and wall-clock ceilings set by the platform rather than by agent self-management.[^aix-sandbox] These criteria are configuration of the platform sandboxes in the tooling map, so an assessor reads them off that configuration rather than off a product's GA status.

### L4 detail

Per rule 2, a buyer satisfies L4 with a preview control documented in the approved pipeline with a production date.

- **Chain-of-thought / alignment auditing** on agentic workloads (goal-hijack detection).
- **Code-safety static analysis** on code-gen agents.
- **Output groundedness / hallucination checking.**
- **Injection-resistant context boundaries.** Trusted/untrusted segmentation enforced at the framework layer rather than in prompt text.
- **Semantic tool validation on high-impact calls**, checking what a call will do as well as who may make it: a proposed state change computed by dry-run where the tool supports one, an independent adversarial judge drawn from a different model family comparing the user's stated intent against the proposed parameters, and deterministic guardrails evaluating the parsed impact against cumulative session limits as well as per-call limits.[^aix-oversight]
- **Human approval on configured high-blast-radius operations, mandatory even where every automated check passes.** The judge informs that decision and does not make it.[^aix-oversight] The approval mechanism is graded outside this domain — the tier assignment at [[agentic-ai-security-cmm-d3-control-least-agency|D3]] L3 and the approval record at [[agentic-ai-security-cmm-d9-operations|D9]] L3 — so what this rung adds is that a clean semantic-validation result does not discharge the requirement, and the evidence is an approval on a call every automated check passed.
- **Optional post-hoc narrowing, named by the Exchange alongside the validation controls.** Read-only tool restriction after risky web access or after a judge failure, held until task end or human clearance, is the runtime counterpart to the dynamic narrowing on risk elevation that [[least-agency-principle|least agency]] carries as an implementation pattern and no [[agentic-ai-security-cmm-d3-control-least-agency|D3]] rung yet grades.[^aix-oversight]

The CoT-audit and groundedness controls are preview (Task Adherence, Groundedness Detection) or experimental (AlignmentCheck), so a defensible L4 today is assembled from preview and OSS components.

The semantic-validation criterion sits a step behind those preview controls, and its evidence comes from the deployment rather than from a vendor. Proposed state change and the cross-family adversarial judge are specifications with no named implementation, preview or otherwise, so a deployment assembles them itself and evidences them from its own pipeline — dry-run records, judge findings carrying the judge's model family, and the parsed-impact guardrail configuration showing session-cumulative thresholds — where the other L4 controls are evidenced from a vendor's status page.

### L5 detail

- **Platform-level enforcement of every L4 control across every agent surface**, with no opt-out for "internal-only" agents.
- **Multi-language and bypass-class injection coverage**, measured against a current bypass library with classifier-refresh receipts and reported as a per-language miss rate rather than an aggregate, since the Exchange states detection accuracy differs across languages, modalities, and attacker sophistication and directs that the per-language rate be measured rather than assumed.[^aix-piioh]
- **Response-leak scanning at egress**, catching credentials echoed in responses and measured against encoded forms as well as literal ones, since the Exchange states that an attacker can obfuscate output to circumvent detection and gives base64-encoding a token as the case.[^aix-soh]
- **Per-guardrail latency and cost budgets**, enforced with fail-closed on critical paths.
- **Enumeration of the services every sandbox shares** — the inference endpoint, the credential store, and the policy decision point — each either partitioned per agent or recorded as an accepted cross-agent channel with its residual risk stated ([[owasp-ai-exchange|OWASP AI Exchange]]).[^aix-sandbox]

## Right-sizing by deployment shape

| Deployment shape | Realistic D4 target | Why |
|---|---|---|
| Web/desktop chatbot (no tools) | L3 (L4 only for high-stakes content) | No tool-call surface means no CoT-audit or code-safety need; input PI filter + output content safety suffice. [[agentic-ai-security-cmm-recalibration-method-2026\|The persona]]'s bot sits here |
| In-suite productivity assistant (Gemini for Workspace, Microsoft 365 Copilot) | L3 | The injection path is the product's own retrieval surface and the screening against it runs inside the vendor, so the L3 criteria are recorded unanswerable, each naming the vendor evidence that would close it; what the customer holds is the enablement and data-loss-prevention configuration that bounds reach |
| Desktop-agent productivity assistant ([[claude-cowork\|Claude Cowork]] class) | L3 | The sandbox criterion is met from a documented boundary, stated per placement: a local session runs in a platform-hypervisor VM with syscall restriction and per-session user isolation, and the cloud sandbox reaches no private, link-local or metadata address; the injection screening runs inside the vendor and stays unanswerable |
| Copilot / assistant (RAG + light tools) | L3 → L4 | Add groundedness (RAG) and tool-call gating; CoT auditing earns its cost once tools can write |
| Generative coding harness (writes files, runs shell) | L3 → L4 | Code-safety analysis and sandbox scope carry the rung; chain-of-thought auditing and groundedness ship as preview or OSS, so the band tracks what the program assembles. Coverage note below |
| MCP / skill provider (real tool reach) | L4 | CoT/alignment auditing, tool-call interception, and sandboxing become first-order |
| Multi-agent mesh | L4 → L5 | Platform-level no-opt-out enforcement and response-leak scanning across every surface |

The [[lethal-trifecta|lethal-trifecta]] test lowers the required level. An agent that reaches no private data, or that holds no exfiltration path, makes a poor high-impact injection target, so the CoT-audit and response-leak stack covers only risk the deployment shape has already removed. A lower D4 score is then recorded as an intentional trade-off.

The core page assigns the generative coding shape the same L3-to-L4 band for this domain, and raises only [[agentic-ai-security-cmm-d8-supply-chain|D8]] to a flat L4, where the dependency channel stays external ([[cmm-known-limitations|CMM Known Limitations]] item 20).

**An assessment of the coding shape records what "sandboxed" covers rather than assuming it.** The load-bearing D4 control for an agentic coding harness is an OS boundary. A harness sandbox that covers shell subprocesses leaves in-process file tools, MCP servers and hooks on the host, the asymmetry through which the [[claude-code-github-action-credential-exposure|June 2026 CI credential exfiltration]] ran while the shell boundary held. Whole-process wrappers such as [[anthropic-sandbox-runtime|`@anthropic-ai/sandbox-runtime`]] close it without requiring containers, at beta-research-preview grade. Authoring-time instruments such as the [[security-guidance-plugin|Security Guidance plugin]] warn without blocking and therefore carry no D4 level on their own. [[securing-agentic-coding|Securing Agentic Coding]] holds the full catalog with availability grades.

Scope is one of two coverage questions, and the second is when the boundary begins. A sandbox established after the harness has read workspace-supplied configuration is absent for that window, whatever it contains afterwards. [[gemini-cli-workspace-trust-rce|GHSA-wpqr-6v78-jr5g]] is the case on the record, where headless Gemini CLI executed from an attacker-supplied `.gemini/` tree before its sandbox initialized, at CVSS 10.0, with the isolation control correctly implemented and never in the path. No vendor documentation this wiki holds states the ordering for any harness it tracks, so an assessor cannot verify it from published material and records it as **unanswerable**, an unanswered vendor question rather than a met or unmet criterion. A documented sandbox scope does not answer the ordering question.

## Cost model

An E5 or Azure incumbent already holds Prompt Shields, Content Safety and Foundry sandboxing, and the licensing column below prices that buyer. A Google Cloud buyer meets the input and output classification rows on Model Armor and the groundedness row on the check grounding API, and Google states no launch stage for either, while GKE Agent Sandbox leads the sandbox row. Google publishes a rate for Model Armor alone: 3 billion tokens a month at no cost on a Security Command Center Premium or Enterprise subscription, whose minimum annual fee is \$15,000, then 2 million tokens a month standalone and \$0.10 per additional million,[^maprice] so the Google buyer reads this licensing column against a subscription or a standalone purchase where the E5 buyer reads it against an entitlement.

| Level | Licensing | Operational labor | Run-rate |
|---|---|---|---|
| L2 | ~0 for an E5/Azure incumbent (Prompt Shields, Content Safety are Azure entitlements) | enable defaults | per-call content-safety transaction cost (low) |
| L3 | ~0 incremental (indirect-PI mode, hooks, sandbox are platform features) | hook wiring; sandbox config; tier-to-risk mapping | classifier calls scale with traffic; sandbox compute scales with concurrently sandboxed work |
| L4 | mostly ~0 on entitlements, but the load-bearing controls are preview/OSS | the real spend: integrating and tuning CoT-audit and groundedness, false-positive triage, context-boundary engineering | per-call cost roughly doubles (input + output + grounding + CoT passes); latency-budget engineering |
| L5 | some off-stack (bypass-class eval tooling; egress response-leak scanning overlaps D5) | continuous multi-language eval; classifier-refresh ops; fail-closed runbook | guardrail inference at every surface × every agent; agent log volume hits the SIEM bill here too |

Licensing is near-zero through L3 and largely zero through L4 for an E5 incumbent: Prompt Shields, Content Safety, Defender runtime protection, and Foundry sandboxing are inside existing entitlements. The spend is guardrail-tuning labor and per-call inference run-rate (each added guardrail pass is another model call), which is why D4 scores well at low cost for this buyer.

Sandbox overhead scales with the number of sandboxes held open concurrently.[^aix-sandbox] The L3 criterion sandboxes high-risk-tier actions, so for a deployment that confines only those actions the run-rate tracks the high-risk task rate. A mesh deployment that runs every agent confined for the length of a session pays the sandbox floor for the whole fleet, and reaching L5 across every agent surface converges on that shape. Cost the L3 line against the deployment's own confinement scope.

## Customer critiques folded in

The stress test rated D4 the persona's strongest domain (raw ~L2–L3, lifting to L3), because the defaults are on and entitled: Prompt Shields (direct + indirect) is GA, and groundedness lifts to L3. One correction applies: **Groundedness Detection is public preview and English-only**[^ground]. That is moot for a US English member bot, but where a regulated buyer cannot deploy preview features, the L3 lift rests on a preview control. Two further critiques addressed:

- *"L4 looks GA but isn't."* The recalibration grades L4 at its shipping status, so nothing in the model directs a regulated buyer to deploy a preview control to hit L4.
- *"Cost was invisible."* Licensing is near-zero for E5; the real cost is per-call inference run-rate and tuning labor.

## Open questions

- CoT / alignment auditing has not reached GA, and as of September 2026 the reliability of what it reads is reported as declining. OpenAI's GPT-6 Astra system card (2026-09-03) states that the model *"shows a substantial decrease in chain-of-thought monitorability compared to previous models"* ([OpenAI Deployment Safety Hub](https://deploymentsafety.openai.com/gpt-6-astra)), and a contemporaneous evasion paper reports 25-33% monitor evasion rates across monitorability benchmarks under deliberate plan injection ([arXiv:2609.15989](https://arxiv.org/abs/2609.15989), 2026-09-14). AlignmentCheck stays experimental and Task Adherence stays preview, so L4 continues to grade the capability rather than a GA product. See [[chain-of-thought-monitorability|Chain-of-Thought Monitorability]].
- Groundedness is English-only — a hard limit for non-English member bases, with no verified multilingual GA date.
- The check grounding API answers the Google groundedness row: it returns an overall support score of 0 to 1 with per-claim scores and citations, and Google documents it for filtering responses at inference time and states no launch stage for it.[^grounding] AWS Automated Reasoning is GA but US-East-only. A buyer whose procurement requires a supported-product commitment cannot read the Google row as entitled, and whether this ladder treats an unstated launch stage the way it treats preview is an open calibration question.
- Defender real-time protection for Agent 365 tooling servers reached GA on 2026-07-27, which is inside rule 2's two-quarter window, and agent threat detection stays in public preview. Under the cadence qualifier, L5 does not depend on either until it is production-hardened.
- Response-leak scanning at egress (L5) overlaps [[agentic-ai-security-cmm-d5-egress-network|D5]]; score it in one domain to avoid double-counting.
- **One output-side minimization target falls outside every data class the rungs name.** `DISCRETE` is anchored at [[agentic-ai-security-cmm-d1-governance|D1]] as a classification and publication control, and the third of the three examples it gives is minimizing technical details in model output ([`/go/discrete/`](https://owaspai.org/go/discrete/)).[^aix-discrete] The L3 criterion above grades an output classifier and requires its data-class scope to be recorded, and the two classes it names are content safety and the exposure-restricted data `SENSITIVE OUTPUT HANDLING` covers — personal data, confidential identifiers, passwords, and tokens.[^aix-soh] Detail about the system itself is in neither. The enforcement point for it is this domain's output path, and no rung here claims the class, because the Exchange gives the example with no mechanism, artifact, or threshold and a criterion would grade an assertion. An assessor recording the L3 scope states whether the class is inside it; the control's anchor stays at D1, where its method sits.

## D3→D4 dependency cap

D4's effective score is capped at D3's raw score (`effective(D4) ≤ raw(D3)`), because runtime enforcement acts only on decisions the control layer makes, so strong guardrails over a weak PDP add little. For the persona, D4 raw scores higher than any other domain while D3 sits at L1–L2, so effective D4 is pulled to roughly L1–L2. The headline "strongest domain" reports the raw score; the dependency-resolved score is lower. The cheapest move is therefore to firm up [[agentic-ai-security-cmm-d3-control-least-agency|D3]] before buying more guardrails. Report D4 as raw plus effective. See [[agentic-ai-security-cmm-dependency-rules|the dependency rules]].

## Notes

[^ps]: [Microsoft Learn — Content Safety what's new](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/whats-new), 2024–2026. Prompt Shields GA (Aug 2024); Groundedness detection listed under public preview.
[^ground]: [Microsoft Learn — Groundedness detection](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/concepts/groundedness), 2026. Preview status; correction mode; language coverage (English).
[^ta]: [Microsoft Learn — Task Adherence](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/concepts/task-adherence), 2025. Public preview; detects misaligned tool invocations / off-task behavior.
[^def]: [Microsoft TechCommunity — Securing AI Agents at Runtime: Real-Time Protection and Threat Detection for Microsoft Agent 365](https://techcommunity.microsoft.com/blog/microsoft-security-blog/securing-ai-agents-at-runtime-real-time-protection-and-threat-detection-for-micr/4541255), 2026-07-27. "Real-time protection for Microsoft Agent 365 tooling servers—now generally available"; "Threat detection for Microsoft Agent 365 agents—now in public preview."
[^foundry]: [Microsoft Learn — Foundry hosted agents](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/hosted-agents), 2026. Per-session microVM sandbox for untrusted code / computer use (preview).
[^agentsandbox]: [Google Cloud blog — Bringing you Agent Sandbox on GKE](https://cloud.google.com/blog/products/containers-kubernetes/bringing-you-agent-sandbox-on-gke-and-agent-substrate) and [kubernetes-sigs/agent-sandbox](https://github.com/kubernetes-sigs/agent-sandbox), May 2026. gVisor-default sandbox as Kubernetes SIG Apps CRDs (Apache 2.0); runs on any cluster; managed GKE adds warm pools (300 sandboxes/sec). The AWS and Azure managed sandboxes are platform-bound, while this one is an open primitive — see [[gke-agent-sandbox|GKE Agent Sandbox]]. GCP leads this row and no other row in the table.
[^pg2]: [Meta — LlamaFirewall architecture](https://meta-llama.github.io/PurpleLlama/LlamaFirewall/docs/documentation/llamafirewall-architecture/workflow-and-detection-components), 2025. PromptGuard 2; AlignmentCheck (experimental); CodeShield.
[^bedrock]: [AWS — Bedrock Guardrails prompt-attack filter](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-prompt-attack.html), 2026. Jailbreak / injection / leakage filter (Standard tier).
[^ar]: [AWS — Automated Reasoning checks](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-automated-reasoning-checks.html), 2026. Formal-logic factuality checks; GA, US East.
[^ma]: [Google Cloud — Model Armor overview](https://docs.cloud.google.com/model-armor/overview) and [Model Armor release notes](https://docs.cloud.google.com/model-armor/release-notes), both fetched 2026-09-16. Responsible-AI filters, prompt-injection and jailbreak detection, Sensitive Data Protection, malicious URL detection and document screening. The overview states a launch stage for image screening alone (Preview) and none for the core prompt-and-response screening service; the release notes announce general availability for named features and integrations — streaming sanitization, Agent Gateway, Gemini Enterprise, the Agent Platform, GKE, the MCP servers and the monitoring dashboard — and for no version of the service itself. The one page pairing `GA` with the product scopes it to the integration: "Model Armor Status GA. This product integration is fully supported by VPC Service Controls" ([VPC Service Controls supported products](https://docs.cloud.google.com/vpc-service-controls/docs/supported-products), fetched the same day).
[^grounding]: [Google Cloud — Check grounding with RAG](https://docs.cloud.google.com/generative-ai-app-builder/docs/check-grounding), fetched 2026-09-16. Returns an overall support score of 0 to 1 for an answer candidate, a claim-level support score, a citation threshold and citations to the facts supporting each claim, and is documented to "filter out responses at inference time" without "incurring a significant slowdown". The page states no launch stage.
[^maregion]: [Google Cloud — Model Armor: feature availability for templates by region](https://docs.cloud.google.com/model-armor/feature-availability-by-region), fetched 2026-09-16. Google splits regions into full support and limited support and states that where a template sits in a limited-support region with data-residency compliance enabled, "some Model Armor features are unavailable because they rely on services that might process data outside the jurisdiction of your chosen Model Armor region". The Toronto row (`northamerica-northeast2`, the only Canadian entry) keeps responsible-AI filters, Sensitive Data Protection, and prompt-injection and jailbreak detection, and carries `No` for multi-language detection, CSAM support, image support and antivirus scanning. Configurations that use floor settings keep every feature.
[^maprice]: [Google Cloud — Security Command Center pricing](https://docs.cloud.google.com/security-command-center/pricing), fetched 2026-09-16. Model Armor meters on the total tokens in prompts and responses, four characters per token excluding white space. A Premium or Enterprise subscription carries 3 billion tokens a month at no cost; a project-level or organization-level Premium activation and the standalone purchase each carry 2 million a month; every tier bills \$0.10 per additional million. The same page states that "the minimum annual cost of a Security Command Center Enterprise subscription is \$15,000" and repeats the figure for Premium, and that Sensitive Data Protection inside Model Armor carries no additional charge. The pricing page carries no shutdown date; Google states the Enterprise tier's shutdown on 2027-05-21 on its [service tiers page](https://docs.cloud.google.com/security-command-center/docs/service-tiers), fetched the same day.
[^taiwan]: Dream Security, "[Inside a Multi-Agent AI Framework Used to Compromise Government Entities in Asia](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia)," 2026-08-12. See [[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]].
[^aix-sandbox]: [OWASP AI Exchange — Agent sandboxing and isolation](https://owaspai.org/go/agentsandboxing/), retrieved 2026-08-18.
[^aix-testing]: [OWASP AI Exchange — Testing against prompt injection](https://owaspai.org/go/testingpromptinjection/), retrieved 2026-08-19. Step (3)'s direction to present attack inputs to the system API rather than to the model so the production filtering and detection mechanisms are in the path, and step (4)'s direction to present the inputs to the insertion mechanisms untrusted data uses — tool outputs in an agentic system — which may require a dedicated testing API that routes the input through every filtering, detection, and insertion step.
[^aix-escape]: [OWASP AI Exchange — Agent escape](https://owaspai.org/go/agentescape/), retrieved 2026-08-18.
[^aix-oversight]: [OWASP AI Exchange — OVERSIGHT](https://owaspai.org/go/oversight/), retrieved 2026-08-19. Category statement (runtime control), the automated-oversight detection strategies including simulate-before-execute, the preventative/detective distinction against `LEAST MODEL PRIVILEGE`, and the optional read-only restriction after risky web access or judge failure.
[^aix-discrete]: [OWASP AI Exchange — DISCRETE](https://owaspai.org/go/discrete/), retrieved 2026-08-20. The objective of reducing the information available to an attacker for selecting and tailoring an attack, and the three stated examples, of which the third is minimizing technical details in model output.
[^aix-soh]: [OWASP AI Exchange — SENSITIVE OUTPUT HANDLING](https://owaspai.org/go/sensitiveoutputhandling/), retrieved 2026-08-19. The exposure-restricted data classes, the output-time enforcement point, recitation detection against an indexed training set, the Limitations block's four entries on pattern coverage, false positives, subtle or context-dependent disclosures, and output obfuscation, and the separate risk-reduction statement bounding recitation reach to indexed data.
[^aix-modelensemble]: [OWASP AI Exchange — MODEL ENSEMBLE](https://owaspai.org/go/modelensemble/), retrieved 2026-08-20. The category line "development-time AI engineer control - including specific runtime implementation"; deployment as an ensemble over a randomly split training set so a deviating output signals possible manipulation; and the stated effectiveness bound that the approach weakens as the share of poisoned samples rises.
[^aix-piioh]: [OWASP AI Exchange — PROMPT INJECTION I/O HANDLING](https://owaspai.org/go/promptinjectioniohandling/), retrieved 2026-08-18. Unicode NFKC normalization and invisible-character removal, locale-independent case-folding with the Turkish-locale worked example, confusable collapsing under Unicode UTS #39, tool-output sanitization before context injection, and the per-language miss-rate guidance.
[^nand-fg]: [NAND Research — CrowdStrike Falcon Guardian: AI Agent Security at the Endpoint](https://nand-research.com/crowdstrike-falcon-guardian-ai-agent-security-at-the-endpoint/), early September 2026. NAND Research attributes both figures to CrowdStrike ("CrowdStrike claims 99% detection efficacy against prompt attacks with response latency under 100 milliseconds"). Neither figure appears in CrowdStrike's launch press release or product blog, so the claim reaches the reader through analyst coverage and no published benchmark stands behind it; see [[falcon-guardian|Falcon Guardian]].
