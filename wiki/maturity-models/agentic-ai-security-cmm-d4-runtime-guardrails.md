---
type: maturity-model
title: "CMM D4: Runtime and Guardrails"
address: c-000126
created: 2026-05-25
updated: 2026-08-31
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
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[prompt-injection]]"
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
---

# Agentic AI Security CMM — D4 Runtime & Guardrails (Deep Dive)

Companion deep-dive to [[agentic-ai-security-cmm-2026|the CMM]]'s D4 domain, written under the [[agentic-ai-security-cmm-recalibration-method-2026|recalibration method]]. D4 is the [[oversight-layer|Policy Enforcement Point]] at runtime: it enforces what [[agentic-ai-security-cmm-d3-control-least-agency|D3]] decides. The runtime threats it answers map to [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]]: Tool Misuse (T2), Intent Breaking and Goal Manipulation (T6), Unexpected RCE and Code Attacks (T11), and Rogue Agents in Multi-Agent Systems (T13), whose playbooks call for execution sandboxing with per-call reset and in-path reasoning-manipulation controls. The recalibration's main move here is to grade maturity accurately. The L2/L3 input-and-output controls are GA and cheap, but the L4 spine the current CMM names as deployable (chain-of-thought auditing, groundedness checking) sits at preview or experimental status, short of GA.

> [!gap] Single-source grounding
> Levels and cost model synthesize the recalibration method against the [[agentic-cmm-regulated-fi-stress-test|regulated-FI stress test]] plus vendor documentation. Tooling status is a May 2026 snapshot.

## Threat coverage

D4 is the primary domain for **ASI01 (Agent Goal Hijack)** and **ASI05 (Unexpected Code Execution)**, and the runtime home of **Class 2 (APT — cross-version eval continuity)**, **Class 3 (collusion — supervisor agents)**, and **Class 4 (model-version regression — continuous red-teaming)**. Its effective score is capped by D3, since a guardrail cannot enforce a decision the policy decision point never makes. See the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix and the [[agentic-ai-threat-classes-2026|threat classes]].

The [[agent-escape|agent escape]] threat entry is graded primarily in [[agentic-ai-security-cmm-d3-control-least-agency|D3]], which holds the capability-based access control that decides a call. D4 holds the boundary that bounds the reach of a decision that was wrong, and the [[owasp-ai-exchange|OWASP AI Exchange]] keeps the two layers distinct: infrastructure-layer enforcement blocks escape even where jailbreak has already succeeded at the reasoning layer.[^aix-escape]

The Exchange cuts the same pair on a different axis. `LEAST MODEL PRIVILEGE` is preventative and `OVERSIGHT` is detective — reactive or gate-based — and the Exchange states that both may apply to the same action tier.[^aix-oversight] A domain boundary drawn on decide-versus-enforce and a control boundary drawn on prevent-versus-detect cross rather than coincide, so a control anchored at D3 can be the detective one and a control anchored here can be the preventative one. `OVERSIGHT` itself is categorised a runtime control. The Exchange divides it into automated and human oversight, and the automated side carries two mechanisms that sit on different planes: recognizing unwanted output and suspicious action sequences in model output, before execution, or after it, which is graded here, and withholding an action until a policy gate returns, which [[agentic-ai-security-cmm-d3-control-least-agency|D3]] grades. The three-domain split is set out in [[agentic-ai-security-cmm-crosswalk|the CMM crosswalk]], and [[oversight-layer|the oversight layer]] states the same automated side as PIP detection and PEP enforcement.[^aix-oversight]

## Control landscape (dated)

| Capability | What ships today | Status | Microsoft | AWS | GCP |
|---|---|---|---|---|---|
| Input prompt-injection / jailbreak classifier | Meta PromptGuard 2 (OSS); NVIDIA NemoGuard NIM; Lakera, HiddenLayer | GA | Content Safety Prompt Shields, direct + indirect, GA[^ps] | Bedrock Guardrails prompt-attack filter, GA[^bedrock] | Model Armor, GA[^ma] |
| Chain-of-thought / alignment auditing (goal-hijack) | Meta AlignmentCheck (OSS) | **Experimental**[^pg2] | Content Safety Task Adherence — **public preview**[^ta] | none native | none native |
| Code-gen static safety | Meta CodeShield (OSS) | GA-equivalent (OSS)[^pg2] | none native | none native | none native |
| Output content safety / filtering | NeMo Guardrails; Guardrails AI | GA | Content Safety, GA | Bedrock filters, GA | Model Armor, GA |
| Groundedness / hallucination check | — | **preview / partial** | Groundedness Detection — **public preview, English-only**[^ground] | Bedrock Automated Reasoning checks, GA (US-East)[^ar] | none native |
| Tool-call interception / gating | Microsoft Agent Governance Toolkit (OSS); AgentShield (OSS) | OSS GA-equivalent | Defender agent runtime protection — **preview, GA targeted Q3 2026**[^def] | — | — |
| Sandboxing of high-risk tasks | MiniClaw (OSS reference); [[gke-agent-sandbox\|Agent Sandbox]] (gVisor) | OSS primitive + managed GKE | Foundry hosted-agent microVM sandbox — preview[^foundry] | AgentCore code-interpreter sandbox | [[gke-agent-sandbox\|GKE Agent Sandbox]][^agentsandbox] + Vertex sandboxed execution |

The input/output safety layer (L2/L3) is GA across all three clouds and sits largely inside Azure entitlements. The agentic-reasoning layer (L4) is not: chain-of-thought auditing and groundedness checking ship as preview or experimental components, so an L4 program assembles them rather than buying them.

The [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]] is an operational example of the failure class the jailbreak-classifier row grades against. A framing-based jailbreak ("authorized penetration testing") defeated the underlying models' refusals across a four-day, 12-wave campaign.[^taiwan] The graded capability is holding a refusal against a social-engineering frame that persists for days. A GA classifier in the path is the precondition rather than the evidence.

Cyera's Protect phase is a further vendor example of the tool-call interception row above: Cyera states it blocks a risky tool call during execution ([[cyera-agent-guardian-release|Cyera Agent Guardian Release]]). Cyera names the block and gives no efficacy figure, no bypass rate, and no decision mechanism behind that block, so that row's graded status is unchanged.

The [[microsoft-zt4ai|Microsoft ZT4AI]] Apps & Workloads pillar (assume breach) supplies the Microsoft-native runtime controls behind these rungs — Prompt Shields, Groundedness Detection, and Task Adherence — crosswalked to D4 in [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]], which records the same GA-versus-preview split.

## Capability-decoupled levels

Stated as capabilities per [[agentic-ai-security-cmm-recalibration-method-2026|rule 1]]; a control counts when it operates in production per rule 2.

- **L1 — Initial.** No runtime guardrails, or only system-prompt instructions. No enforcement boundary.
- **L2 — Developing.** A default safety filter runs on input and a content-safety classifier on output. Single-layer, no agentic-reasoning coverage. Universally reachable GA.
- **L3 — Defined.** Input arrives canonicalized and screened in path for direct *and* indirect [[prompt-injection|prompt injection]], output leaves through a content-safety classifier whose data-class scope is recorded, platform lifecycle hooks intercept the agent loop, and high-risk-tier actions run in a per-task sandbox built to the [[owasp-ai-exchange|OWASP AI Exchange]] specification. The classifiers and hooks are GA across the major stacks and the rest is configuration, so an assessor grades this rung off the deployment rather than off a product's status page.
- **L4 — Managed.** Runtime control reaches the agent's reasoning and the semantics of its tool calls — chain-of-thought auditing, code-safety analysis, groundedness checking, framework-enforced trusted/untrusted context boundaries, and semantic validation of high-impact calls — and human approval stays mandatory for configured high-blast-radius operations however clean those checks come back. The load-bearing controls are preview, experimental, or specification-only, so an L4 program assembles this rung and evidences part of it from its own pipeline.
- **L5 — Optimizing.** Every L4 control runs platform-level on every agent surface with no opt-out, and every guardrail carries a measured efficacy figure and an enforced budget — per-language bypass miss rates, encoding-aware response-leak scanning at egress, and latency and cost limits that fail closed on critical paths. The services every sandbox shares are enumerated, and each is either partitioned per agent or accepted in writing with its residual risk stated.
- **L5+ — Leading Edge.** Cryptographic TEE attestation that guardrails executed in an enclave (reference pilots only); a CaMeL-style [[camel-pattern|privileged/quarantined LLM split]] in production (research); measurable bypass-class evidence with vendor-acknowledged remediation cycles.

The level structure survives the recalibration largely intact. Product names move to the tooling map, and L4 gains an explicit maturity grade. The current text presents Azure Groundedness and AlignmentCheck as deployable. Both are preview or experimental, so a regulated buyer should not be told to deploy a preview control to claim a GA-grade L4.

Sandbox strength is graded from L3 rather than assumed, and the control's own limits bound what the grade asserts. The [[owasp-ai-exchange|OWASP AI Exchange]] states that container or hypervisor escape undermines containment and that OS-specific behaviour can weaken the same configuration on a different host.[^aix-sandbox] An L3 sandbox therefore asserts containment on the integrity of a boundary the buyer does not test, which is the same evidence problem the coverage callout below raises for scope and start order. Shared inference, credential, and policy services sit above every boundary in the deployment and reach across all of them, which the L5 criterion grades.

Two adjacent controls fall outside this domain. Exfiltration through legitimately permitted APIs is out of segmentation's reach and is graded in [[agentic-ai-security-cmm-d5-egress-network|D5]], which also takes the call-volume and tool-invocation halves of the Exchange's per-agent quota control, leaving D4 the compute and wall-clock ceilings the sandbox platform imposes. [[agent-sandbox-isolation-landscape|The sandbox and isolation landscape]] compares what each shipping sandbox partitions.

Tool-output sanitization before content enters the agent's context — stripping role-change patterns, validating against schema, labelling trust by source, and scanning for exfiltration-oriented encoding — is specified by the Exchange alongside these controls and is graded nowhere.[^aix-piioh] That control sits on the tool-call path, which [[agentic-ai-security-cmm-d5-egress-network|D5]] owns, and D5's nearest criterion is A2A content scanning at L4, scoped to inter-agent traffic and stopping short of a tool response returning to a single agent. This page records it as an ungraded control with an open domain assignment rather than as a D4 rung.

Recitation detection is a further output-side mechanism the Exchange specifies and this ladder does not grade. It matches output against an indexed set of the training data to catch the model reproducing training content verbatim, and the Exchange states its reach as limited to what is indexed, with shorter and paraphrased disclosures escaping it.[^aix-soh] The index is a development-time artifact of the party that trained or fine-tuned the model, so a deployment consuming a third-party model cannot construct it, and a rung graded against it would grade the model provider rather than the buyer. This page records it as an ungraded control whose owner sits upstream of the assessed organization, alongside the tool-output sanitization case above.

Ensemble deviation detection is a third such mechanism, and the Exchange files it as a runtime implementation of a development-time decision. `MODEL ENSEMBLE` deploys a model as several models trained on a randomly split training set, so that an output deviating from its peers is ignored as possible evidence of a poisoned training set, and the Exchange states its effectiveness falls as the poisoned share of the dataset rises (§3.1).[^aix-modelensemble] The detection runs at inference and the decision that makes it possible — how the training set is split and how many models are trained — is taken by whoever trains the model, so a deployment consuming a third-party model has no instance of it. This page records it with recitation detection, as an ungraded control owned upstream. The L4 semantic-validation criterion below draws on the same diversity argument for a different purpose: a judge from a different model family is independent of the model it checks, and an ensemble's members are independent of one another; both hold only as far as the shared training corpus lets them.

## Assessor detail per level

L1, L2, and L5+ are graded from their statements above. The three rungs below carry criteria an assessor checks item by item, each list stating what its own rung adds.

Grading is cumulative: Level N requires every Level N–1 control plus the new criteria at Level N ([[agentic-ai-security-cmm-2026|the CMM]]), so a rung is met only where every rung below it is met.

Each criterion takes one of four verdicts. **Met** and **not met** are read from the evidence the criterion names. **Not applicable** is recorded where the deployment holds no instance of what the criterion governs, and the reduced scope is recorded as an intentional trade-off in the [[agentic-ai-security-cmm-dependency-rules|effective-score]] strategic-rationale field. **Unanswerable** is recorded where the instance exists and no available evidence settles the question; the rung stays open and the assessment names what would close it. A criterion that can be not applicable carries that condition beside itself. The lists below hold criteria only; a paragraph after a list carries maturity or market commentary and states no criterion.

### L3 detail

- **Input canonicalization ahead of the classifier.** Unicode NFKC normalization with zero-width and invisible characters removed, locale-independent case-folding rather than locale-sensitive lowercasing, and confusable-character collapsing under Unicode UTS #39 with mixed-script runs flagged.[^aix-piioh] The criterion is read off configuration rather than off a product's GA status, because the Exchange demonstrates that normalization and case-folding are separate and both required — `NFKC("İGNORE")` is unchanged and Cyrillic `а` (U+0430) survives NFKC — and that a locale-sensitive lowercase on a Turkish-locale host maps ASCII `I` to dotless `ı` and breaks a blocklist match.[^aix-piioh] A classifier graded GA does not answer whether the text reaching it was canonicalized first.
- **In-path injection detection and agent-loop interception.** An input classifier detects direct *and* indirect/document [[prompt-injection|prompt injection]], and platform lifecycle hooks intercept the agent loop. Both capabilities are fully GA across the major stacks (Prompt Shields indirect mode, Bedrock prompt-attack, Model Armor). The indirect half is evidenced by presenting attack inputs through the same insertion path untrusted data takes — a retrieved document, a tool output — rather than through the user channel, so the filtering, detection, and insertion mechanisms in that path are all in scope.[^aix-testing] A vendor's indirect-mode flag states that the capability exists and not that the deployment's augmentation route reaches it.
- **Output content-safety classifier with its data-class scope recorded.** Content safety and exposure-restricted data are separate detections, and the [[owasp-ai-exchange|OWASP AI Exchange]] specifies the second as a final safeguard on personal data, confidential identifiers, passwords, and tokens before output reaches a user or a downstream system, applying wherever the model has reached such data and the output can reach an unauthorized actor.[^aix-soh]
- **Per-task sandbox on high-risk-tier actions, meeting the Exchange specification.** Process spawning, out-of-workspace filesystem access, agent self-configuration, and host interaction confined; a mandatory access control profile with unneeded kernel capabilities disabled; transient state, cached data, and in-sandbox credential material destroyed at task completion or forced stop; per-agent compute and wall-clock ceilings set by the platform rather than by agent self-management.[^aix-sandbox] These criteria are configuration of the platform sandboxes in the tooling map, so an assessor reads them off that configuration rather than off a product's GA status.

### L4 detail

- **Chain-of-thought / alignment auditing** on agentic workloads (goal-hijack detection).
- **Code-safety static analysis** on code-gen agents.
- **Output groundedness / hallucination checking.**
- **Injection-resistant context boundaries.** Trusted/untrusted segmentation enforced at the framework layer rather than in prompt text.
- **Semantic tool validation on high-impact calls**, checking what a call will do as well as who may make it: a proposed state change computed by dry-run where the tool supports one, an independent adversarial judge drawn from a different model family comparing the user's stated intent against the proposed parameters, and deterministic guardrails evaluating the parsed impact against cumulative session limits as well as per-call limits.[^aix-oversight]
- **Human approval on configured high-blast-radius operations, mandatory even where every automated check passes.** The judge informs that decision and does not make it.[^aix-oversight] The approval mechanism is graded outside this domain — the tier assignment at [[agentic-ai-security-cmm-d3-control-least-agency|D3]] L3 and the approval record at [[agentic-ai-security-cmm-d9-operations|D9]] L3 — so what this rung adds is that a clean semantic-validation result does not discharge the requirement, and the evidence is an approval on a call every automated check passed.
- **Optional post-hoc narrowing, named by the Exchange alongside the validation controls.** Read-only tool restriction after risky web access or after a judge failure, held until task end or human clearance, is the runtime counterpart to the dynamic narrowing on risk elevation that [[least-agency-principle|least agency]] carries as an implementation pattern and no [[agentic-ai-security-cmm-d3-control-least-agency|D3]] rung yet grades.[^aix-oversight]

The CoT-audit and groundedness controls are preview (Task Adherence, Groundedness Detection) or experimental (AlignmentCheck), so a defensible L4 today is assembled from preview and OSS components. Per rule 2, a buyer satisfies L4 with a preview control documented in the approved pipeline with a production date.

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
| Copilot / assistant (RAG + light tools) | L3 → L4 | Add groundedness (RAG) and tool-call gating; CoT auditing earns its cost once tools can write |
| MCP / skill provider (real tool reach) | L4 | CoT/alignment auditing, tool-call interception, and sandboxing become first-order |
| Multi-agent mesh | L4 → L5 | Platform-level no-opt-out enforcement and response-leak scanning across every surface |

The [[lethal-trifecta|lethal-trifecta]] test lowers the required level. An agent that reaches no private data, or that holds no exfiltration path, makes a poor high-impact injection target, so the CoT-audit and response-leak stack buys nothing the deployment shape has not already removed. A lower D4 score is then recorded as an intentional trade-off.

> [!check] State the coverage of "sandboxed" for the coding shape
> For agentic coding harnesses the load-bearing D4 control is an OS boundary, and its scope must be recorded rather than assumed. A harness sandbox that covers shell subprocesses leaves in-process file tools, MCP servers, and hooks on the host — the asymmetry through which the [[claude-code-github-action-credential-exposure|June 2026 CI credential exfiltration]] ran while the shell boundary held. Whole-process wrappers such as [[anthropic-sandbox-runtime|`@anthropic-ai/sandbox-runtime`]] close it without requiring containers, at beta-research-preview grade. Authoring-time instruments such as the [[security-guidance-plugin|Security Guidance plugin]] warn without blocking and therefore carry no D4 level on their own. Full catalog with availability grades: [[securing-agentic-coding|Securing Agentic Coding]].
>
> Scope is one of two coverage questions, and the second is when the boundary begins. A sandbox established after the harness has read workspace-supplied configuration is absent for that window, whatever it contains afterwards. [[gemini-cli-workspace-trust-rce|GHSA-wpqr-6v78-jr5g]] is the case on the record: headless Gemini CLI executed from an attacker-supplied `.gemini/` tree before its sandbox initialized, at CVSS 10.0, with the isolation control correctly implemented and never in the path. No vendor documentation states this ordering for any harness the wiki tracks, so an assessor cannot currently verify it from published material — record it as **unanswerable**, an unanswered vendor question rather than a met or unmet criterion, and do not read a documented sandbox scope as covering it.

## Cost model

| Level | Licensing | Operational labor | Run-rate |
|---|---|---|---|
| L2 | ~0 for an E5/Azure incumbent (Prompt Shields, Content Safety are Azure entitlements) | enable defaults | per-call content-safety transaction cost (low) |
| L3 | ~0 incremental (indirect-PI mode, hooks, sandbox are platform features) | hook wiring; sandbox config; tier-to-risk mapping | classifier calls scale with traffic; sandbox compute scales with concurrently sandboxed work |
| L4 | mostly ~0 on entitlements, but the load-bearing controls are preview/OSS | the real spend: integrating and tuning CoT-audit and groundedness, false-positive triage, context-boundary engineering | per-call cost roughly doubles (input + output + grounding + CoT passes); latency-budget engineering |
| L5 | some off-stack (bypass-class eval tooling; egress response-leak scanning overlaps D5) | continuous multi-language eval; classifier-refresh ops; fail-closed runbook | guardrail inference at every surface × every agent; agent log volume hits the SIEM bill here too |

Licensing is near-zero through L3 and largely zero through L4 for an E5 incumbent: Prompt Shields, Content Safety, Defender runtime protection, and Foundry sandboxing are inside existing entitlements. The spend is guardrail-tuning labor and per-call inference run-rate (each added guardrail pass is another model call), which is why D4 scores well at low cost for this buyer.

Sandbox overhead scales with the number of sandboxes held open concurrently.[^aix-sandbox] The L3 criterion sandboxes high-risk-tier actions, so for a deployment that confines only those actions the run-rate tracks the high-risk task rate. A mesh deployment that runs every agent confined for the length of a session pays the sandbox floor for the whole fleet, and reaching L5 across every agent surface converges on that shape. Cost the L3 line against the deployment's own confinement scope.

## Customer critiques folded in

The stress test rated D4 the persona's strongest domain (raw ~L2–L3, lifting to L3), because the defaults are on and entitled: Prompt Shields (direct + indirect) is GA, and groundedness lifts to L3. One correction applies: **Groundedness Detection is public preview and English-only**[^ground]. That is moot for a US English member bot, but a regulated buyer who cannot deploy preview features should treat the L3 lift as resting on a preview control. Two further critiques addressed:

- *"L4 looks GA but isn't."* The recalibration grades L4 at its shipping status, so a regulated buyer is not told to deploy a preview control to hit L4.
- *"Cost was invisible."* Licensing is near-zero for E5; the real cost is per-call inference run-rate and tuning labor.

## Open questions

- Will CoT / alignment auditing reach GA before the next CMM revision? Today the candidates are AlignmentCheck (experimental) and Task Adherence (preview); if it stays preview, L4 cannot require it as GA.
- Groundedness is English-only — a hard limit for non-English member bases, with no verified multilingual GA date.
- GCP has no native groundedness guardrail; AWS Automated Reasoning is GA but US-East-only. Single-stack GCP buyers go off-platform for L4 groundedness.
- Defender runtime protection is preview with GA targeted Q3 2026, which may or may not fall inside a regulated buyer's procurement window. The cadence qualifier says do not make L5 depend on it until it is production-hardened.
- Response-leak scanning at egress (L5) overlaps [[agentic-ai-security-cmm-d5-egress-network|D5]]; score it in one domain to avoid double-counting.
- **One output-side minimization target falls outside every data class the rungs name.** `DISCRETE` is anchored at [[agentic-ai-security-cmm-d1-governance|D1]] as a classification and publication control, and the third of the three examples it gives is minimizing technical details in model output ([`/go/discrete/`](https://owaspai.org/go/discrete/)).[^aix-discrete] The L3 criterion above grades an output classifier and requires its data-class scope to be recorded, and the two classes it names are content safety and the exposure-restricted data `SENSITIVE OUTPUT HANDLING` covers — personal data, confidential identifiers, passwords, and tokens.[^aix-soh] Detail about the system itself is in neither. The enforcement point for it is this domain's output path, and no rung here claims the class, because the Exchange gives the example with no mechanism, artifact, or threshold and a criterion would grade an assertion. An assessor recording the L3 scope states whether the class is inside it; the control's anchor stays at D1, where its method sits.

## D3→D4 dependency cap

D4's effective score is capped at D3's raw score (`effective(D4) ≤ raw(D3)`), because runtime enforcement acts only on decisions the control layer makes, so strong guardrails over a weak PDP buy little. For the persona, D4 raw scores higher than any other domain while D3 sits at L1–L2, so effective D4 is pulled to roughly L1–L2. The headline "strongest domain" reports the raw score; the dependency-resolved score is lower. The cheapest high-leverage move is therefore to firm up [[agentic-ai-security-cmm-d3-control-least-agency|D3]] ahead of buying more guardrails. D4 should be reported as raw + effective. See [[agentic-ai-security-cmm-dependency-rules|the dependency rules]].

## Notes

[^ps]: [Microsoft Learn — Content Safety what's new](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/whats-new), 2024–2026. Prompt Shields GA (Aug 2024); Groundedness detection listed under public preview.
[^ground]: [Microsoft Learn — Groundedness detection](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/concepts/groundedness), 2026. Preview status; correction mode; language coverage (English).
[^ta]: [Microsoft Learn — Task Adherence](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/concepts/task-adherence), 2025. Public preview; detects misaligned tool invocations / off-task behavior.
[^def]: [Microsoft Learn — Real-time agent protection during runtime (preview)](https://learn.microsoft.com/en-us/defender-cloud-apps/real-time-agent-protection-during-runtime), 2026. Webhook block-before-execute; preview, GA targeted Q3 2026.
[^foundry]: [Microsoft Learn — Foundry hosted agents](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/hosted-agents), 2026. Per-session microVM sandbox for untrusted code / computer use (preview).
[^agentsandbox]: [Google Cloud blog — Bringing you Agent Sandbox on GKE](https://cloud.google.com/blog/products/containers-kubernetes/bringing-you-agent-sandbox-on-gke-and-agent-substrate) and [kubernetes-sigs/agent-sandbox](https://github.com/kubernetes-sigs/agent-sandbox), May 2026. gVisor-default sandbox as Kubernetes SIG Apps CRDs (Apache 2.0); runs on any cluster; managed GKE adds warm pools (300 sandboxes/sec). Unlike the AWS/Azure managed sandboxes, this is an open primitive, not a platform-bound feature — see [[gke-agent-sandbox|GKE Agent Sandbox]]. GCP leads this row, and it is the one row where it does.
[^pg2]: [Meta — LlamaFirewall architecture](https://meta-llama.github.io/PurpleLlama/LlamaFirewall/docs/documentation/llamafirewall-architecture/workflow-and-detection-components), 2025. PromptGuard 2; AlignmentCheck (experimental); CodeShield.
[^bedrock]: [AWS — Bedrock Guardrails prompt-attack filter](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-prompt-attack.html), 2026. Jailbreak / injection / leakage filter (Standard tier).
[^ar]: [AWS — Automated Reasoning checks](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-automated-reasoning-checks.html), 2026. Formal-logic factuality checks; GA, US East.
[^ma]: [Google Cloud — Model Armor overview](https://docs.cloud.google.com/model-armor/overview), 2026. Prompt-injection / jailbreak detection; Vertex / Gemini integration.
[^taiwan]: Dream Security, "[Inside a Multi-Agent AI Framework Used to Compromise Government Entities in Asia](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia)," 2026-08-12. See [[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]].
[^aix-sandbox]: [OWASP AI Exchange — Agent sandboxing and isolation](https://owaspai.org/go/agentsandboxing/), retrieved 2026-08-18.
[^aix-testing]: [OWASP AI Exchange — Testing against prompt injection](https://owaspai.org/go/testingpromptinjection/), retrieved 2026-08-19. Step (3)'s direction to present attack inputs to the system API rather than to the model so the production filtering and detection mechanisms are in the path, and step (4)'s direction to present the inputs to the insertion mechanisms untrusted data uses — tool outputs in an agentic system — which may require a dedicated testing API that routes the input through every filtering, detection, and insertion step.
[^aix-escape]: [OWASP AI Exchange — Agent escape](https://owaspai.org/go/agentescape/), retrieved 2026-08-18.
[^aix-oversight]: [OWASP AI Exchange — OVERSIGHT](https://owaspai.org/go/oversight/), retrieved 2026-08-19. Category statement (runtime control), the automated-oversight detection strategies including simulate-before-execute, the preventative/detective distinction against `LEAST MODEL PRIVILEGE`, and the optional read-only restriction after risky web access or judge failure.
[^aix-discrete]: [OWASP AI Exchange — DISCRETE](https://owaspai.org/go/discrete/), retrieved 2026-08-20. The objective of reducing the information available to an attacker for selecting and tailoring an attack, and the three stated examples, of which the third is minimizing technical details in model output.
[^aix-soh]: [OWASP AI Exchange — SENSITIVE OUTPUT HANDLING](https://owaspai.org/go/sensitiveoutputhandling/), retrieved 2026-08-19. The exposure-restricted data classes, the output-time enforcement point, recitation detection against an indexed training set, the Limitations block's four entries on pattern coverage, false positives, subtle or context-dependent disclosures, and output obfuscation, and the separate risk-reduction statement bounding recitation reach to indexed data.
[^aix-modelensemble]: [OWASP AI Exchange — MODEL ENSEMBLE](https://owaspai.org/go/modelensemble/), retrieved 2026-08-20. The category line "development-time AI engineer control - including specific runtime implementation"; deployment as an ensemble over a randomly split training set so a deviating output signals possible manipulation; and the stated effectiveness bound that the approach weakens as the share of poisoned samples rises.
[^aix-piioh]: [OWASP AI Exchange — PROMPT INJECTION I/O HANDLING](https://owaspai.org/go/promptinjectioniohandling/), retrieved 2026-08-18. Unicode NFKC normalization and invisible-character removal, locale-independent case-folding with the Turkish-locale worked example, confusable collapsing under Unicode UTS #39, tool-output sanitization before context injection, and the per-language miss-rate guidance.
