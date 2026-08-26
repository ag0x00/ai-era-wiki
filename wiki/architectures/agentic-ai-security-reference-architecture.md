---
type: architecture
title: "Agentic AI Security Reference Architecture"
created: 2026-04-30
updated: 2026-08-25
tags:
  - architectures
  - reference-architecture
  - agentic-ai
  - security-architecture
status: developing
origin: produced
address: c-000161
scope_axis: [sec-of-ai, sec-against-ai]
attributed_to: "Anton Goncharov + Claude (synthesis session, 2026-04-30)"
problem_solved: "A vendor-neutral, layered reference architecture for securing agentic AI applications across all common deployment shapes — chatbots, generative coding tools, data-science copilots, RAG, MCP servers, agent skills, multi-agent meshes."
components:
  - identity-plane
  - control-plane
  - runtime-plane
  - data-plane
  - egress-plane
  - observability-plane
related:
  - "[[kimi-k3-sandbox-escape|Kimi K3 Sandbox Escape]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[azure-rag-chatbot-security-profile]]"
  - "[[cybersecurity-cmms-exemplars]]"
  - "[[security-controls-for-ai-stacks]]"
  - "[[agent-identity-architecture]]"
  - "[[mcp-security]]"
  - "[[mcp-exposure-measurements]]"
  - "[[mcp-cves-q1-2026]]"
  - "[[agent-observability]]"
  - "[[agent-sandboxing]]"
  - "[[gke-agent-sandbox]]"
  - "[[credential-proxy-pattern]]"
  - "[[lethal-trifecta]]"
  - "[[least-agency-principle]]"
  - "[[emerging-cybersecurity-practices-for-agentic-ai-applications]]"
  - "[[owasp-ai-exchange]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[agent-message-structure-manipulation]]"
  - "[[agent-escape]]"
  - "[[agent-sandbox-isolation-landscape]]"
  - "[[threat-modeling-for-ai]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[owasp-asi-aiuc1-crosswalk]]"
  - "[[standards-review-csa-maestro-atf-2026-Q2]]"
  - "[[multi-agent-runtime-security]]"
  - "[[agentic-soc-reference-architecture]]"
  - "[[capability-based-authorization]]"
  - "[[plan-validate-execute]]"
  - "[[distributed-kill-switch]]"
  - "[[network-layer-prompt-injection-containment]]"
  - "[[behavioral-anomaly-detection-for-agents]]"
  - "[[smokescreen]]"
  - "[[toolshed]]"
  - "[[agentcordon]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[securing-agentic-coding]]"
  - "[[guard-canonicalization-gap]]"
  - "[[agents-rule-of-two]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[offensive-agent-collective]]"
  - "[[artifactory]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[openclaw]]"
  - "[[hermes-agent]]"
  - "[[gemini-cli-workspace-trust-rce]]"
  - "[[harness-config-as-supply-chain-artifact]]"
sources:
  - "[[ai-security-standards-in-q1-2026]]"
  - "[[emerging-cybersecurity-practices-for-agentic-ai-applications]]"
  - "[[securing-the-autonomous-future]]"
primary_documents:
  - "[[.raw/papers/owasp-ai-exchange-development-time-threats-2026-08-19.md]]"
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
---

# Agentic AI Security Reference Architecture (AAI-S RA)

The Agentic AI Security Reference Architecture (**AAI-S RA**) secures agentic AI applications under a vendor-neutral trust model. A single layered design covers every common deployment shape: web and desktop chatbots, generative coding tools, data-science copilots, RAG systems, MCP servers, agent skills, and multi-agent meshes.

## On this page

- [[#Deliverables]]
- [[#Design principles]]
- [[#The six planes]]
- [[#Mapping to deployment shapes]]
- [[#Threat-control matrix (OWASP Agentic AI Top 10 to planes)]]
- [[#Trade-offs]]
- [[#Gaps in the architecture]]
- [[#Prior work and comparison]]
- [[#Relations]]

```mermaid
block-beta
  columns 2
  
  User(["Human user"]):2
  
  Identity["Identity plane"]:2
  
  Control["Control plane"]:2
  
  Runtime["Runtime plane"]:2
  
  Egress["Egress plane"]
  Data["Data plane"]
  
  Obs["Observability plane"]:2

  classDef pip stroke:#0d6efd
  classDef pdp stroke:#fd7e14
  classDef pep stroke:#dc3545
  classDef mixed stroke:#6f42c1
  classDef user stroke:#198754
  
  class User user
  class Identity pip
  class Control pdp
  class Runtime pep
  class Egress pep
  class Data mixed
  class Obs pip
```

## Deliverables

The architecture below is the **implementation surface** of an [[oversight-layer|oversight layer]]: the system that monitors, evaluates, and intervenes in AI agent behavior in production. The six planes decompose the layer into the four roles codified in [[nist-sp-800-162|NIST SP 800-162]] §2.2: **PDPs** (Policy Decision Points, concentrated in Control), **PEPs** (Policy Enforcement Points, distributed across Runtime / Egress / Data), **PIPs** (Policy Information Points, spanning Identity / Data / Observability), and **PAP** (Policy Administration Point, cross-cutting policy lifecycle). The [[sentinels-and-operatives|Sentinels and Operatives]] split (PIPs feed PDP+PEP) is the runtime decomposition. The goal is **verified accountable autonomy**: agents act independently, but every action is verifiable, auditable, and bounded by enforced policy.

[[gartner|Gartner]] calls this layer a [[guardian-agent|guardian agent]] in its procurement taxonomy. The architectural primary term (oversight layer / PDP+PEP) applies when discussing components; the procurement synonym (guardian agent) applies when discussing vendor categories. See [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] §Cross-walk for the full term comparison.

## Design principles

Six principles anchor the architecture, drawn from Q1 2026 incident data and practitioner consensus.

1. **Enforcement runs below the model.** Every control that matters runs in the runtime/platform, below the model, because prompt-level guardrails are bypassable by definition. (Consensus across [[ai-security-standards-in-q1-2026|AI Security Standards in Q1 2026: Agentic Threats Outpace Frameworks]], [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]], [[securing-the-autonomous-future|Securing the Autonomous Future: Trust, Safety, and Reliability of Agentic AI]].)
2. **Least agency extends least privilege to autonomy.** Autonomy and agency are distinct governable dimensions: agency is the *scope* of permitted actions; autonomy is the *degree of independent decision-making* within that scope (definitions per the [[aws-agentic-ai-security-scoping-matrix|AWS Agentic AI Security Scoping Matrix]]). An agent's allowed tier (auto / notify / confirm / block) is assigned per action, so the same agent can carry different tiers across its own action set. ([[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]] §3.2, via [[least-agency-principle|Least Agency Principle]]; the OWASP ASI Top 10 names the principle and supplies no tiers.)
3. **Verifiable identity for every actor.** Every agent has a cryptographic identity that traces back to a human. Action-to-identity binding is the foundation for audit, revocation, and compliance. ([[agent-identity-architecture|AI Agent Identity Architecture]], NIST CAISI Concept Paper, Feb 2026.)
4. **No agent owns credentials.** The credential proxy pattern is load-bearing; 5+ tools converged on it independently. Even successful prompt injection cannot extract credentials that never enter context. ([[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]].)
5. **The "Lethal Trifecta" is a structural test.** Any deployment combining private data, untrusted content, and external comms is unconditionally vulnerable. The architecture must break the trifecta at the platform layer. ([[lethal-trifecta|Lethal Trifecta]], Simon Willison.)
6. **The workflow space is not enumerable at design time.** An agent holding many tools and chaining multiple steps produces a set of possible workflows too large to pre-specify, so purpose, boundaries, and side effects cannot all be fixed before deployment. Threat modeling and governance specification remain necessary, and runtime guardrails carry the enforcement that a design-time workflow list cannot ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/agenticaioverview/`](https://owaspai.org/go/agenticaioverview/)). The principle carries a regulatory consequence the other five do not: where a regime assumes every workflow is describable before deployment, a compositional agentic system needs an explicit handling program in place of a workflow list. That consequence lands on [[eu-ai-act|the EU AI Act]]'s Annex IV technical-documentation duty, which the [[agentic-ai-security-cmm-crosswalk|CMM crosswalk]] maps to D8.

> [!contradiction] Competing evidence on principles 1 and 5
> See [[wiki-novelty-and-counterarguments-2026|Wiki Novelty and Counter-Arguments]] §Thesis 1 (platform vs prompt) and §Thesis 3 (Lethal Trifecta).
>
> **Principle 1**: the framing is *hierarchy, not exclusivity*. Prompt-layer guardrails reduce ASR materially ([[llamafirewall|LlamaFirewall]] PromptGuard 2 on [[agentdojo|AgentDojo]]: 17.6%→7.5%, combined with AlignmentCheck to 1.75%; Anthropic Constitutional Classifiers: 86%→4.4% jailbreak success).[^asr-evals] Platform-layer is primary because injection cannot bypass it; prompt-layer is residual-risk reduction. [[rag-hardening|RAG hardening]] + [[system-prompt-architecture|system-prompt architecture]] pages already carry residual-risk callouts in this spirit.
>
> **Principle 5**: "unconditionally vulnerable" is design-time pedagogy. In production, [[breaking-the-lethal-trifecta-talk|Stripe's containment architecture]] runs trifecta agents and reports 1.5–6.7% ASR depending on model (single-source practitioner data, not independently replicated): probabilistically exploitable, not unconditional. The Lethal Trifecta is **necessary** for natural-language exfil at scale and **sufficient given current defense maturity** to require platform-layer containment. Containment can drive ASR very low but not to zero, which remains unacceptable for high-risk-tier actions.

## The six planes

The architecture decomposes into six logical planes. Multiple planes may be implemented by a single product, but the controls must be addressable independently.

Plane order follows the action flow: User, then Identity, Control, Runtime, and Egress/Data. Each plane is annotated with its [[xacml|XACML]] role (PIP / PDP / PEP / PAP). Observability spans the bottom as a cross-cutting plane consuming signals from all five above.

Each plane table carries a **Type** column classifying its reference implementations. **OSS** is open-source software and **COTS** a commercial off-the-shelf vendor product or SaaS. **Std** is a formally governed standard or specification from IETF, CNCF, OWASP, or NIST, and **Infra** a generic infrastructure primitive such as a cloud VPC or Docker networking. **Research** is an academic prototype with no shipped production implementation, and **Concept** an architectural concept with no canonical implementation yet. **Exploratory** covers a forward-looking prototype or ecosystem project such as OpenClaw, which indicates where agentic security is heading and stands as an emerging indicator rather than a foundational control. Many rows combine types, as in "OSS + COTS", where a capability has both free and commercial implementations in common use.

```mermaid
block-beta
  columns 2
  
  User(["Human user"]):2
  
  Identity["Identity plane · PIP-side<br/>Workload identity · Agent lifecycle<br/>NHI governance · Credential proxy"]:2
  
  Control["Control plane · PDP + PAP<br/>Policy evaluation · Capability tokens<br/>Least-agency tiers · HITL"]:2
  
  Runtime["Runtime plane · PEP (in-process)<br/>Lifecycle hooks · Input filtering<br/>CoT auditing · Code scanning<br/>Sandboxing"]:2
  
  Egress["Egress plane · PEP (broker)<br/>Agent/MCP proxy · Tool authorization<br/>Tool integrity · Egress filtering"]
  Data["Data plane · PIP + PEP<br/>AI-BOM · RAG provenance<br/>Memory integrity · State rollback<br/>Supply-chain scanning"]
  
  Obs["Observability plane<br/>PIP (cross-cutting)<br/>Distributed tracing<br/>Behavioral monitoring · AI-SPM<br/>Red-team integration"]:2

  classDef pip fill:#cfe2ff,stroke:#0d6efd,color:#000
  classDef pdp fill:#fff3cd,stroke:#fd7e14,color:#000
  classDef pep fill:#f8d7da,stroke:#dc3545,color:#000
  classDef mixed fill:#e2d5f3,stroke:#6f42c1,color:#000
  classDef user fill:#d1e7dd,stroke:#198754,color:#000
  
  class User user
  class Identity pip
  class Control pdp
  class Runtime pep
  class Egress pep
  class Data mixed
  class Obs pip
```

The six planes map one-to-one onto the CMM's per-plane domains: Identity ↔ [[agentic-ai-security-cmm-d2-identity|D2]], Control ↔ [[agentic-ai-security-cmm-d3-control-least-agency|D3]], Runtime ↔ [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]], Egress ↔ [[agentic-ai-security-cmm-d5-egress-network|D5]], Data ↔ [[agentic-ai-security-cmm-d6-data-rag|D6]], Observability ↔ [[agentic-ai-security-cmm-d7-observability|D7]].

An agentic penetration test is scoped along a different partition. Document 5 of the [[owasp-ai-exchange|OWASP AI Exchange]] sorts a test into four layers — LLM reasoning, tool execution, infrastructure, and inter-agent communication — which cut through the planes instead of sitting inside one and reach five of the six at once: Runtime, Control, Egress, Identity and Observability. The Data plane — the corpus, the augmentation store, agent memory — has no layer reaching it, because none of the four names an asset this plane holds; the credentials and keys its infrastructure layer does name sit on the Identity plane. This architecture reads the four layers as a way to evidence gap 8 and leaves its own six-plane partition as the only one.[^aix-testing-ra]

The [[agentic-ai-security-cmm-recalibration-method-2026|domain recalibration]] carries six calibration points into this architecture:

1. **Identity-first sequencing.** Per-agent identity is the prerequisite for per-agent egress and observability (the D2→D5 and D2→D7 caps), so build the Identity plane before investing in Egress or Observability.
2. **Per-task capability tokens remain leading-edge.** As of 2026-Q2 no hyperscaler platform ships them natively; the only implementation is the early-stage [[tenuo-warrant|Tenuo]] OSS primitive (see [[agentic-ai-security-cmm-d2-identity|D2]], which places them at L5+).
3. **The Control plane now has platform-native PDPs:** AWS AgentCore Policy (GA) and the Microsoft Agent Governance Toolkit (OSS).
4. **Runtime's reasoning-layer controls sit at preview status.** Chain-of-thought auditing and groundedness checks have not reached GA.
5. **The Data plane's load-bearing control for a closed-corpus bot is answer-time entitlement enforcement** against oversharing / [[inference-exposure|inference exposure]], not corpus attestation; the [[azure-rag-chatbot-security-profile|Azure RAG chatbot security profile]] works this case end to end.
6. **Licensing is near-zero for an E5 incumbent** across most planes; the real cost is labor and SIEM run-rate.

For an all-Microsoft deployment, [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]] maps named, deep-linked controls to each plane: Entra Agent ID and Conditional Access (Identity), least-action design plus the Agent Governance Toolkit PDP (Control), Prompt Shields with preview Groundedness / Task Adherence (Runtime), Entra Internet Access and the APIM AI Gateway (Egress), Purview answer-time entitlement and DSPM (Data), Defender XDR and Sentinel agentic-SOC tooling (Observability). The recalibration's grading reflects the preview status of the adaptive Runtime and Observability controls.

### 1. Identity plane

The Identity plane binds every agent to a verifiable identity that traces back to a human principal. It covers workload identity, agent and [[non-human-identity|Non-Human Identity (NHI)]] lifecycle governance, the credential-proxy primitive, action-to-identity tracing, and OAuth 2.1 / OIDC delegation extensions for agents.

| Capability                    | Reference implementation                                                                                      | Type       | Status                          |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------- | ---------- | ------------------------------- |
| Workload identity             | [[spiffe\|SPIFFE / SPIRE]]                                                                                    | OSS + Std  | Mature                          |
| Agent identity & lifecycle    | [[microsoft-entra-agent-id\|Microsoft Entra Agent ID]] + Agent 365 Registry (GA May 1, 2026); AWS Bedrock AgentCore identities (GA); GCP Agent Identity (GA, SPIFFE-based); [[okta-for-ai-agents\|Okta for AI Agents]] (Early Access; GA expected FY27) | COTS       | GA platform-native on all three hyperscalers |
| [[nhi-governance-for-agents\|Non-Human Identity governance]] | Aembit, Astrix Security, [[cyberark-conjur\|CyberArk Conjur]], Okta NHI, [[oasis-security\|Oasis Security]]  | COTS       | Developing                      |
| Credential proxy              | AgentKeys, Keychains.dev, Aegis (local), OneCLI, AgentSecrets                                                 | OSS / COTS | Developing — 5-tool convergence |
| Action-to-identity tracing    | Microsoft Agent 365, Anthropic Compliance API (Mar 24, 2026)                                                  | COTS       | Mature (vendor-stack-locked)    |
| OAuth 2.1 / OIDC for agents   | Standard OIDC + NIST CAISI Concept Paper extensions (Feb 2026)                                                | Std        | Developing                      |

**The credential proxy is the load-bearing control of this plane.** Five OSS and commercial tools converged on the same architecture independently. A credential that never enters the model's context cannot be extracted by prompt injection. See [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]].

### 2. Control plane

The Control plane adjudicates policy and issues capability tokens **before** an agent's tool call reaches the runtime. It evaluates Cedar / OPA Policy Decision Points, issues task-scoped Warrants, runs the least-agency action-risk tier engine, gates high-impact actions through HITL, and applies CSA ATF risk-based step-up. The tier engine sorts actions by risk into auto, notify, confirm and block, the scheme [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]] §3.2 uses to operationalize the OWASP ASI least-agency principle. The tiers are separate from the [[owasp-ai-exchange|OWASP AI Exchange]] oversight requirements, which assign each tier a human-involvement requirement; the two axes compose, and [[least-agency-principle|Least Agency Principle]] carries both tables.

| Capability | Reference implementation | Type | Status |
|---|---|---|---|
| Policy language / PDP engine | [[cedar\|Cedar]], [[opa\|OPA/Rego]]; **platform-native PDPs:** AWS Bedrock AgentCore Policy (Cedar, GA Mar 2026), Microsoft Agent Governance Toolkit (OSS, sub-0.1 ms) | OSS + COTS | GA platform-native |
| Capability tokens / Warrants | [[tenuo-warrant\|Tenuo Warrants]] — task-scoped, signed, ephemeral, holder-bound [[capability-based-authorization\|capability authorizations]] with [[monotonic-attenuation\|monotonic attenuation]] (the [[ambient-vs-derived-authority\|ambient-to-derived authority]] shift) | OSS | Leading-edge — no platform-native implementation; single early-stage OSS primitive |
| Least-agency tier engine | Action-risk tiers (auto / notify / confirm / block), [[emerging-cybersecurity-practices-for-agentic-ai-applications\|Emerging Practices]] §3.2 | Std | Conceptual; needs reference implementation |
| Task-scope binding at invocation | Verify the invoking agent's current task scope and authorized role at every tool call, so an authorized tool called during an out-of-scope task is denied ([[agent-escape\|agent escape]]) | Std | Conceptual — no product evaluates task scope as a policy attribute[^aix-escape-ra] |
| Orchestrator hardening | Coordination-only permission set holding no direct high-impact tool credentials; a tamper-evident workflow log kept outside orchestrator memory; reconciliation of executed agent actions against that log to surface phantom steps; human approval ahead of irreversible or cross-system workflows ([[owasp-ai-exchange\|OWASP AI Exchange]] `OVERSIGHT`) | Std | Conceptual — no listed implementation constrains the orchestrator's own credential set or externalizes its workflow log[^aix-oversight-ra] |
| Tool annotation enforcement | Anthropic tool annotations, OpenAI function-call schemas; [[toolshed\|Stripe Toolshed]] annotation PEP | COTS | Mature |
| HITL primitive | [[hitl\|Human-in-the-loop confirmation gate]] before high-impact tool calls, carrying a per-request approval token that binds approver identity, action parameters, and expiry, and presenting authorised scope from a static capability manifest rather than from consent text the agent composes ([[hitl\|HITL]] holds both specifications); [[plan-validate-execute\|Plan-Validate-Execute]] deterministic gate (Google Workspace); [[distributed-kill-switch\|distributed kill switch]] for halt authority | Concept + Practice | Developing |
| Risk-based step-up | [[csa-maestro\|CSA Agentic Trust Framework]] v1.0 (Feb 2, 2026) — 4 maturity levels gated by 5 promotion gates[^atf-ra] | Std | Developing |

The control plane breaks the Lethal Trifecta. If a tool call combines private-data scope, untrusted-content provenance, and external-comms reach, the PDP downgrades to a safer tier (notify or confirm) or denies the call.

**Every inter-agent path transits the orchestrator, which the [[owasp-ai-exchange|OWASP AI Exchange]] names the highest-leverage target in the system.** The Exchange derives a posture from that: coordination-only permissions, no direct egress, and a workflow log held outside the memory the orchestrator itself writes.[^aix-oversight-ra] This architecture concentrates inter-agent traffic at the orchestrator by design, for the reasons the Egress plane gives, and that concentration turns the constraint into a requirement. Everything the orchestrator consumes is validated and segregated as untrusted — sub-agent responses, tool outputs, external content — which is the rule gap 9 states for structured messages, applied to the component that receives the most of them. Reconciling executed agent actions against the external workflow log detects phantom steps: actions present in the record of what ran and absent from the record of what was planned. No row in the Observability plane emits that comparison, and it is recorded as gap 11.

### 3. Runtime plane

The Runtime plane defends the agent process itself. It installs lifecycle hooks (Google ADK, Anthropic), filters input for [[prompt-injection|prompt injection]], audits chain-of-thought alignment, scans code output statically, classifies content safety, and sandboxes each task. At research stage, it also separates the privileged and quarantined LLMs.

| Capability | Reference implementation | Type | Status |
|---|---|---|---|
| Lifecycle hooks | Google ADK `before_model_callback` / `before_tool_callback`; Anthropic tool-use hooks | OSS + COTS | Mature |
| [[prompt-injection-containment\|Input filtering / prompt-injection detection]] | [[llamafirewall\|LlamaFirewall]] PromptGuard 2, NVIDIA NeMo Jailbreak Detection NIM, [[palo-alto-prisma-airs\|Palo Alto Prisma AIRS]], [[onyx-platform\|Onyx Platform]] runtime protection | OSS + COTS | Mature |
| Chain-of-thought alignment auditing | LlamaFirewall AlignmentCheck | OSS | Developing — novel primitive |
| Code-output static analysis | LlamaFirewall CodeShield, GitHub Copilot for Security | OSS + COTS | Developing |
| Topic / content safety | NVIDIA NeMo Content Safety NIM, [[lakera-guard\|Lakera Guard]], Lasso, Microsoft Prompt Shields | COTS | Mature |
| Sandbox / containment | [[firecracker\|Firecracker]] (per-task VM), [[gvisor\|gVisor]] container, WebAssembly sandbox; [[gke-agent-sandbox\|Agent Sandbox]] (Kubernetes-native CRDs: SandboxTemplate blueprint + SandboxClaim, gVisor default) | OSS | Mature |
| Per-agent resource quotas | Platform-enforced CPU time, memory, disk I/O, network egress, tool-invocation, and wall-clock limits ([[owasp-ai-exchange\|OWASP AI Exchange]] `LIMIT RESOURCES`); container and Kubernetes limits | Std + Infra | Mature for compute and wall-clock; the API-volume and tool-invocation ceilings are enforced at the Egress plane gateway[^aix-limitresources-ra] |
| Compartmentalized LLMs (CaMeL pattern) | [[camel-pattern\|Privileged + quarantined LLM split]] (Google DeepMind); privilege-based data flow control in the [[owasp-ai-exchange\|OWASP AI Exchange]]'s vocabulary[^aix-pi-ra] | Research | Research-stage |
| Tool-boundary firewall | Paired input/output inspection at the agent-to-tool boundary, which the [[owasp-ai-exchange\|OWASP AI Exchange]] positions as lighter than a dual-LLM architecture[^aix-pi-ra] | Concept | Conceptual — no listed implementation inspects tool output before it re-enters context |
| Proof-of-Guardrail attestation | AWS Nitro Enclaves + [[miggo-security\|Miggo Security]] | COTS | Research-stage |

**A quota bounds an agent's cost and its availability impact, and nothing beyond those two.** The Exchange states that resource limits leave every harm inside the allocated budget untouched.[^aix-limitresources-ra] The actions an agent takes below its ceiling proceed as if the ceiling were absent. Two requirements travel with the row. Containers, API gateways, or orchestration enforce the caps and the agent must not, which is the same infrastructure-layer rule the Observability plane note below states for containment. A breached limit also terminates cleanly and writes an audit event, which makes the breach an event this architecture's Observability plane can consume.[^aix-limitresources-ra] The same entry names fleet-wide consumption monitoring for correlated spikes and slow exhaustion attacks, for which no row in any plane of this architecture names a reference implementation; it is recorded as gap 10.

**Defense-in-depth across all six planes is required, because each guardrail here has a demonstrated bypass.** The Trendyol Tech LlamaFirewall bypass used non-English and leetspeak prompt injections.

**Every capability in this table is a property of a running system, and the table says nothing about when each one begins to hold.** [[gemini-cli-workspace-trust-rce|GHSA-wpqr-6v78-jr5g]] shows what that omission costs: headless Gemini CLI read a workspace-supplied `.gemini/` configuration tree and executed from it *before its sandbox initialized*, so the isolation capability was correctly implemented and never in the path.[^gemini-init] A control that starts after the harness has read attacker-reachable configuration is absent for that window, and no plane below covers the window. Two assessment questions follow, and they are independent: which subsystems the boundary contains, and at what point in startup the boundary exists. The second is currently unanswerable from vendor documentation for every harness the wiki tracks, which is recorded in [Gaps in the architecture](#gaps-in-the-architecture).

**The [[owasp-ai-exchange|OWASP AI Exchange]] states five limitations of agent sandboxing, four of which bound what this table's containment rows assert.**[^aix-sandbox-ra] Container or hypervisor escape undermines containment, so the Sandbox row's Mature grade is a claim about the isolation mechanism it is configured with, compared across mechanisms in [[agent-sandbox-isolation-landscape|the isolation landscape]]. Shared inference, credential, and policy services create implicit cross-agent channels, recorded as gap 8. Network segmentation cannot stop exfiltration through legitimately permitted APIs, which the Egress plane and gap 5 carry, since a legitimately permitted API is an allowlisted destination. Host-OS behaviour differs across hosts, which makes the same Mature grade a property of a verified host image that this table does not record. The fifth limitation is that sandbox overhead scales with concurrent agents, which is a cost rather than a bound on reach.

The Exchange requires quota enforcement at the platform rather than through agent self-management, and the reason this table grades it that way is that an agent holding its own quota can revise it.[^aix-sandbox-ra] Sandboxing reaches [[agent-escape|agent escape]] only along the paths that cross the boundary it enforces. An agent reaching a filesystem path or a network destination outside its scope is stopped by the Runtime plane, which is the Exchange's own worked example; an agent invoking an unauthorized tool stays inside its process and crosses a policy boundary the Control plane owns, and the Control plane's task-scope binding row is the criterion for it.[^aix-escape-ra]

**The Exchange describes the CaMeL pattern by what enforcement acts on rather than by its topology:** capability metadata is attached to values, and the user's intent is converted into sandboxed code steps in place of unconstrained natural-language tool calls.[^aix-pi-ra] The two-model split named in that row implements the control. The Exchange also lists the pattern among five structural mitigations it recommends against agentic prompt injection, a standing above the Research-stage grade the row records; recommended in a standards-liaison publication and unshipped in production are both current, and [[camel-pattern|CaMeL]] carries the reconciliation.

**The Exchange ranks three detection layers and puts execution-level detection — observing the tool calls and side effects an agent actually produces — above text-level and model-level detection for reliability.**[^aix-pi-ra] This table's grades run the other way for a defensible reason: the text-layer row is Mature because classifiers ship as products, and behavioural monitoring in the Observability plane is Developing because it is assembled. Maturity and reliability are separate axes, and the ordering means this plane's most purchasable detection row and the layer the Exchange calls most reliable are different controls. The precedence claim agrees with design principle 1, because the Exchange states that its own I/O-handling layer raises the bar so later layers become more effective — the hierarchy-not-exclusivity reading the callout under that principle already carries.

### 4. Egress plane

The Egress plane mediates an agent's reach to tools, MCP servers, peer agents, and the open internet. It proxies MCP / A2A / LLM traffic through a gateway, authorizes MCP calls at runtime, defends against tool poisoning and rug-pulls, enforces agent-to-agent cryptographic identity, filters egress destinations, and segments the network per agent.

| Capability                            | Reference implementation                                                                       | Type       | Status       |
| ------------------------------------- | ---------------------------------------------------------------------------------------------- | ---------- | ------------ |
| MCP / A2A / LLM proxy                 | [[agentgateway\|AgentGateway]] (Linux Foundation, July 2025), Solo Enterprise for agentgateway | OSS + COTS | Mature (OSS) |
| MCP runtime authorization             | Operant MCP Gateway, Natoma; [[toolshed\|Stripe Toolshed]] (annotation PEP); [[agentcordon\|AgentCordon]] (OSS Cedar PDP + MCP gateway) | OSS + COTS | Developing   |
| [[tool-poisoning\|Tool poisoning / rug-pull defense]]     | Solo Enterprise tool-server fingerprinting, versioning, runtime policy                         | COTS       | Developing   |
| Agent-to-agent cryptographic identity | [[a2a-protocol\|A2A v1.0]] §8.4 signed Agent Cards (Std baseline); Oktsec (Ed25519 sigs, 268 detection rules, content scanning) | Std + COTS | Developing   |
| [[network-layer-prompt-injection-containment\|Network-layer prompt-injection containment]] | Microsoft Entra Internet Access prompt-injection protection (GA Mar 2026); Layer-0 control outside the agent process | COTS | GA |
| Egress destination filtering          | Credential proxy destination allowlists, [[smokescreen\|Smokescreen]] SSRF egress proxy (Stripe OSS), Docker DOCKER-USER chain | OSS        | Mature       |
| Network segmentation                  | Per-agent subnet, agent VPC isolation                                                          | Infra      | Mature       |
| Inter-agent path blocking | No direct agent-to-agent network path; traffic transits an orchestration layer or message bus that authenticates and validates it ([[owasp-ai-exchange\|OWASP AI Exchange]]) | Infra + Std | Mature — ordinary network segmentation, with no AI-specific product dependency[^aix-sandbox-ra] |
| Orchestrator egress blocking | No direct orchestrator egress; external access routed through segmented sub-agents, with sub-agent responses, tool outputs, and external content validated and segregated as untrusted before the orchestrator consumes them ([[owasp-ai-exchange\|OWASP AI Exchange]] `OVERSIGHT`) | Std + Infra | Conceptual — the path-blocking row above routes traffic through the orchestrator and states no constraint on the orchestrator's own reach[^aix-oversight-ra] |

The [[openai-hugging-face-agent-incident|OpenAI–Hugging Face incident]] exercised three rows of this table and none of them held. Destination filtering was enforced correctly on the agent's own interface and still yielded internet egress: the single allowlisted destination, an internal [[artifactory|JFrog Artifactory]] package manager and caching proxy, held broad internet access of its own, and an SSRF against it relayed the request.[^bhusa-ra] Agent-to-agent cryptographic identity was never in the path at all: the agents' inter-run message board ran over authorized package-manager writes to a fleet-shared repository, so no A2A link existed to sign and the traffic read as ordinary artifact publishing. Destination filtering therefore requires a transitive property — every allowlisted internal service is itself egress-constrained — and the inter-agent rows require a channel definition wider than the A2A protocol, covering shared writable build and cache infrastructure. Per-run write scoping on that infrastructure belongs in the network-segmentation row. A second variant needs no relay at all: in the [[kimi-k3-sandbox-escape|Kimi K3 benchmark escape]], the allowlist was enforced correctly and the permitted destination served the sought content itself — `github.com`, admitted for package maintenance, also hosted the evaluation's published answers. Destination filtering therefore has two failure modes, and only one is transitive. What an allowlisted host can *relay* and what it can *serve* are separate properties, and both belong in the evidence a destination-filtering control emits.

The path-blocking row addresses the topology behind those channels. Blocking direct agent-to-agent connections makes the broker the only inter-agent path, where the signing and content-scanning rows secure a path that already exists; the Exchange pairs it with segmenting agents that process untrusted content away from sensitive internal services.[^aix-sandbox-ra] Neither criterion reaches a shared writable registry, which is read and written over an authorized client protocol.

Making the broker the only inter-agent path concentrates authority in it, and the orchestrator-egress row keeps that concentration defensible. The orchestrator holds no direct high-impact tool credentials and no direct egress path, and it reaches external resources through segmented sub-agents.[^aix-oversight-ra] A message bus meets the path-blocking row while holding neither; an LLM-driven orchestrator meets the same row and routinely holds both, which is why the two rows are assessed together. The Control plane carries the permission and workflow-log half of the same posture.

The rows above secure a channel and do not authenticate what crosses it. The Exchange gives [[agent-message-structure-manipulation|agent message structure manipulation]] its own threat entry: forged, replayed, or altered structured messages between agents, tools, and orchestration layers, where the manipulated field is a task parameter, a tool argument, routing metadata, conversation state, or a schema field rather than natural-language content.[^aix-amsm-ra] Two consequences reach this plane. The controls are signed delegation tokens validated across the full chain with scope non-expansion, and deny-by-default schema validation at tool and message boundaries, neither of which any listed reference implementation emits evidence for. And the threat reaches single agentic flows, where a poisoned metadata field in a retrieved chunk alters parameter binding with no inter-agent link involved,[^aix-amsm-ra] which is outside the scope this plane's rows assume.

The egress plane contains the [[mcp-security|MCP attack surface]]. Q1 2026 produced 30+ MCP CVEs (see [[mcp-cves-q1-2026|MCP CVEs Q1 2026]]), revealing systematic path traversal and injection vulnerabilities across server implementations; AgentGateway's automatic token exchange limits each tool's permissions to exactly what it needs. Token exchange also answers the failure mode that needs no CVE: a scan in April 2026 found 1,467 internet-reachable MCP servers with no client authentication and no transport encryption, each holding a single hardcoded credential that every caller inherits.[^mcp-exposure-ra] That population sits outside any gateway, so the plane's control depends on an exposure-management precondition it does not itself enforce — the measurements and their methods are separated on [[mcp-exposure-measurements|MCP Exposure Measurements]].

### 5. Data plane

The Data plane limits the data attack surface, attributes trust, and enforces integrity across training data, retrieval corpora, knowledge bases, per-session memory, and the AI Bill of Materials. It generates and reconciles the [[ai-bom|AI/ML-BOM]], attests [[rag-hardening|RAG provenance]], defends against memory poisoning, maintains [[cognitive-file-integrity|cognitive file integrity]] for agent identity files, rolls back corrupted state, and scans the [[supply-chain-security-for-agents|supply chain]].

**OpenClaw provenance.** Five rows in this table (RAGShield, TrustRAG, SecureClaw / cognitive file integrity, Brain Git, Aguara Watch) come from the [[openclaw|OpenClaw]] ecosystem, which indicates where agentic data-plane security is heading and ships no production-proven control. They are retained because they identify real capability gaps and likely future directions. Their **Exploratory** classification puts them a tier below OWASP AIBOM, sigstore, and Miggo Security as evidence: treat them as emerging indicators and validate independently before production deployment.

The ecosystem's dual-use status now has a sourced case. The [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]] ran unmodified OpenClaw, alongside [[hermes-agent|Hermes]], as one of two orchestration platforms in an attack framework used against government infrastructure.[^dream-taiwan] This does not change the Exploratory classification of the *defensive* tooling built on OpenClaw above; those five rows remain unvalidated either way. It does mean that an organization evaluating OpenClaw-ecosystem tooling for its own defensive stack is simultaneously evaluating a platform independently confirmed to run as offensive infrastructure elsewhere. Provenance and threat-model review apply to the platform choice as well as to the specific tool built on it.

| Capability | Reference implementation | Type | Status |
|---|---|---|---|
| Answer-time entitlement enforcement / [[oversharing-controls\|oversharing remediation]] | Microsoft Purview [[dspm\|DSPM for AI]] + oversharing assessments; label-aware DLP for M365 Copilot; Restricted SharePoint Search (site-capped stopgap); generalizes to the [[ai-usage-control\|usage-control (UCON) model]] of continuous authorization | COTS | GA — the load-bearing control against [[inference-exposure\|inference exposure]] for closed-corpus member/customer bots (see [[agentic-ai-security-cmm-d6-data-rag\|D6 deep dive]]) |
| AI/ML-BOM generation | OWASP AIBOM Generator (CycloneDX ML-BOM, v1.7 current), SPDX 3.0 AI extensions, IBM Granite 4.0 disclosures | OSS + Std | Developing |
| Runtime AI-BOM | Miggo Security DeepTracing (behavioral baseline) | COTS | Developing — novel |
| RAG provenance / attestation | RAGShield (cryptographic doc attestation), TrustRAG | Exploratory | Exploratory — OpenClaw ecosystem; not production-validated |
| [[memory-poisoning\|Memory poisoning defense]] | Microsoft Defender for Cloud Apps memory-injection detector (50+ examples found, March 2026); [[agent-memory-isolation\|namespace isolation]] (structural — memory keyed by code, not LLM-assertable) | COTS + Concept | Developing |
| Cognitive file integrity | SHA-256 monitoring of SOUL.md, IDENTITY.md; SecureClaw | Exploratory | Exploratory — OpenClaw ecosystem; emerging indicator, not foundational control |
| State rollback | Brain Git (SlowMist) | Exploratory | Exploratory — OpenClaw ecosystem; no production adoption evidence |
| Skill / model registry signing | sigstore / cosign, OWASP AIBOM | OSS | Developing |
| Supply-chain scanning | [[jfrog\|JFrog]] ML scan, ReversingLabs; [[agentshield\|AgentShield]] (OSS — scans the agent-harness config tree: secrets, hooks, MCP, permissions) | OSS + COTS | Developing |
| Supply-chain scanning (emerging) | Aguara Watch (5 registries daily, SlowMist) | Exploratory | Exploratory — OpenClaw ecosystem; forward indicator for registry hygiene direction |

Q1 2026's three largest agentic incidents all landed on the data plane: [[clawhavoc|ClawHavoc]] (1,184+ malicious skills), SANDWORM_MODE (npm worm into MCP), and the LiteLLM compromise. Defense requires registry, pre-install, checksum, and cognitive-file integrity controls layered together.

Limiting the data attack surface sits upstream of every row in this table. The [[owasp-ai-exchange|OWASP AI Exchange]] groups five controls under sensitive data limitation, whose stated purpose is to reduce the impact of confidentiality and integrity threats by cutting the amount and variety of data held and the duration it is kept ([`/go/datalimit/`](https://owaspai.org/go/datalimit/)). A field that was never collected and a record deleted on schedule are unreachable by every path this plane mediates ([`/go/dataminimize/`](https://owaspai.org/go/dataminimize/), [`/go/shortretain/`](https://owaspai.org/go/shortretain/)). The entitlement row above governs reach into data that exists; this group governs whether the data is there to reach. It adds no row of its own because no entry in it names a reference implementation, and grading sits at [[agentic-ai-security-cmm-d6-data-rag|D6]].

Training and fine-tuning data integrity appears in this plane's scope sentence and in no row of the table above; every other asset that sentence names has one. The [[owasp-ai-exchange|OWASP AI Exchange]] specifies the control set at §3.1: quality control that detects poisoned samples by statistical deviation, spectral signatures, activation clustering, Reject on Negative Impact, or gradient fingerprinting, under separate filtering and alerting thresholds; distortion of untrusted training data to break inserted triggers; benign-data volume to outnumber poisoned samples; ensemble deployment so a deviating member is identifiable; and pruning or clean-data fine-tuning of a model already poisoned ([`/go/datapoison/`](https://owaspai.org/go/datapoison/), [`/go/dataqualitycontrol/`](https://owaspai.org/go/dataqualitycontrol/)).[^aix-datapoison-ra][^aix-dqc-ra] The group adds no row because no entry in it names a reference implementation, the same reason the sensitive-data-limitation group above adds none, and grading sits at [[agentic-ai-security-cmm-d6-data-rag|D6]] with the acquisition half at [[agentic-ai-security-cmm-d8-supply-chain|D8]]. One integrity problem in the group is structural rather than detective: a dataset composed of pointers rather than content — the Exchange's example is LAION-400M, whose entries are image URLs — depends on data that is neither permanent nor under the consumer's control, which is answered by hashing dataset entries at acquisition ([`/go/devsecurity/`](https://owaspai.org/go/devsecurity/)).[^aix-devsec-ra] No reference implementation listed in this plane emits that hash.

### 6. Observability plane

The Observability plane provides full-stack visibility across the five upstream planes. It implements OpenTelemetry `gen_ai.*` semantic conventions, agent-aware tracing, AI-SPM, agent behavioral monitoring, identity multiplexing in logs, and SIEM/SOAR with agent playbooks. It integrates AI red-teaming as a continuous feed rather than a point-in-time exercise.

| Capability | Reference implementation | Type | Status |
|---|---|---|---|
| OpenTelemetry `gen_ai.*` semantic conventions | [[opentelemetry-gen-ai\|OTel SemConv v1.37+]] (CNCF standard; SIG contributors: Amazon, Elastic, Google, IBM, Langtrace, Microsoft, OpenLIT, Scorecard, Traceloop) | Std | Mature (experimental status, broad adoption) |
| Agent-aware tracing | LangSmith, Langtrace, Traceloop, Helicone | OSS + COTS | Mature |
| [[ai-spm\|AI-SPM (Security Posture Management)]] | [[wiz-ai-spm\|Wiz AI-SPM]], [[palo-alto-prisma-airs\|Palo Alto Prisma AIRS]], Orca AI-SPM, Reco, [[onyx-platform\|Onyx Platform]], Google Agent Security dashboard (Security Command Center, preview) | COTS | Mature |
| Agent behavioral monitoring (anomaly detection on agent activity) | Vectra AI, [[miggo-security\|Miggo]] behavioral drift, [[google-agentic-soc\|Google SecOps]] Agent Anomaly Detection ([[llm-as-a-judge\|LLM-as-a-judge]] on agent reasoning, preview); pattern: [[behavioral-anomaly-detection-for-agents\|three-level behavioral anomaly detection]] over a [[tiered-detection-cascade\|cost-ordered detection cascade]] | COTS | Developing |
| Agent behavioral monitoring (emerging) | SecureClaw nightly audits | Exploratory | Exploratory — OpenClaw ecosystem; forward indicator for agent audit direction |
| Trip-wire / canary detection | [[canary-tokens-for-llms\|Canary tokens]] planted in system prompts and RAG wrappers; fires on injection / system-prompt exfil at the post-LLM hook | Concept | Developing |
| Security-event preservation | [[context-aware-trimming\|Context-aware trimming]] — pin tagged security events so a multi-attempt injection cannot hide across the context-window trim boundary | Concept | Developing |
| Identity multiplexing in logs | [[agent-observability\|Agent Observability]] §3 | Concept | Developing |
| SIEM / SOAR with agent playbooks | Splunk + agent IOCs, Sentinel + Defender for Cloud, [[crowdstrike\|Falcon AIDR]] + NeMo Guardrails | COTS | Developing |
| AI red-teaming integration | [[promptfoo\|Promptfoo]], [[mindgard-cart\|Mindgard CART]], [[pyrit\|PyRIT]] (Microsoft), [[garak\|Garak]] (NVIDIA), [[palo-alto-prisma-airs\|Palo Alto Prisma AIRS]] (continuous CART) | OSS + COTS | Mature |

**This plane is classified above as a PIP, so its output is an input to the Control plane's decisions.** The [[owasp-ai-exchange|OWASP AI Exchange]] states that a defensive monitoring agent is itself part of the attack surface, and that agentic containment must operate at the infrastructure layer without depending on the agent cooperating.[^aix-monitoruse-ra] Three rows above are exposed by construction: behavioral monitoring includes an [[llm-as-a-judge|LLM-as-a-judge]] pass over agent reasoning, trip-wire detection fires on content the attacker supplies, and SIEM playbooks act on records the monitored fleet produced. None of the listed reference implementations states whether its detection logic runs outside the trust boundary of the agents it watches. The Exchange's containment position gives the design rule this plane can act on: enforcement triggered by a detection here executes at the infrastructure layer rather than by instructing the agent to stop.

Observability scaling is a primary engineering constraint for agentic workloads. At production volume, pre-aggregation at the runtime hook is required: raw per-span emission overruns the SIEM. The [[openai-hugging-face-agent-incident|OpenAI–Hugging Face investigation]] is the volume datapoint: reconstructing a compromise that ran from 2026-05-08 to at least 2026-07-19 meant reviewing more than 7 billion log entries and agent trajectories at a cost of millions of GPU hours, work carried out with Codex and other agents because it was not tractable by hand. The investigation was still incomplete when the reconstruction was presented on 2026-08-06.[^bhusa-ra] Where retrospective search is itself an agentic workload, what the runtime hook chose to aggregate and retain determines whether the question can be answered at all.

**The retention decision this table prices in dollars carries a second consequence in exposure.** Agent telemetry carries prompts, tool results, and retrieved content, and the [[owasp-ai-exchange|OWASP AI Exchange]] names runtime logging among the activities data minimization applies to, with retention limits stated as a special form of the same control ([`/go/dataminimize/`](https://owaspai.org/go/dataminimize/), [`/go/shortretain/`](https://owaspai.org/go/shortretain/)).[^aix-datalimit-ra] What this plane retains for detection is therefore a copy of the material the Data plane's limitation controls act on, held under a different access model and for a duration set by investigation need. The Exchange supplies no retention period and names no artifact for logging specifically, so this plane records the coupling rather than a threshold: the aggregation policy set at the runtime hook decides both the SIEM bill and the size of the second copy.

## Mapping to deployment shapes

The six-plane structure applies to every deployment shape; the active controls and their relative weight differ by shape. The table below specifies which planes are exercised and which controls are load-bearing for each shape.

Shape granularity is itself revisable. A shape earns its own row where the load-bearing controls change; a product rename alone earns none. Generative coding was one row until July 2026 and is now five, because *where the agent process runs and whether a human is positioned to see an action before it executes* changes which plane carries the weight — from the control plane's approval prompt to the runtime plane's isolation boundary. The per-variant analysis is in [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]; the plane-by-plane control catalog with availability grades is in [[securing-agentic-coding|Securing Agentic Coding]].

| Deployment shape                                          | Identity                                                                                          | Control                                                                                                                                                                                                                                          | Runtime                                                                                                             | Egress                                                                                                                                                   | Data                                                                                          | Observability                                                                                                                                                                                                             |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Web/desktop chatbot (no tools)**                        | OIDC for human; agent runs as bot identity                                                        | Topic-control, content-safety policies                                                                                                                                                                                                           | NeMo Content Safety, Lakera Guard                                                                                   | None (no tools)                                                                                                                                          | RAG corpus only                                                                               | OTel `gen_ai.*`                                                                                                                                                                                                           |
| **Generative coding — interactive local** (IDE or terminal, developer present) | Workspace identity + per-repo scope; agent rules files (`.cursorrules` / `CLAUDE.md`) baselined | Per-action approval prompts; explicit deny rules; org-managed settings local config cannot override; destructive-action classification routes force-push / mass refactor / prod-config writes to `confirm` or `block` | Authoring-time dangerous-pattern warnings; [[agent-sandboxing\|OS sandbox]] used to cut prompt volume rather than as the primary boundary | Source-control + LSP only; **typosquat-aware dependency install gate** | Repo content + skill registry; **cognitive file integrity for rules files** | Tool-call audit; **MCP usage + rule changes + policy-violations dashboard** ([[kirin\|Kirin]]-class) |
| **Generative coding — unattended local** (prompts suppressed) | As above + per-agent identity | Prompts are absent by construction: enforcement moves to the OS boundary; `ConfigChange` auditing on in-session policy edits | **Whole-process** OS sandbox ([[anthropic-sandbox-runtime\|sandbox runtime]], container, or VM) — a Bash-only sandbox leaves file tools, MCP servers, and hooks on the host; hard-fail when isolation is unavailable | Default-deny domain allowlist; TLS-terminating proxy where the threat model includes exfiltration rather than misconfiguration | Credential-file and environment-variable denial (nothing is denied by default); settings-file write protection | Session-correlated OTel to SIEM (prompts, tool results, permission decisions) |
| **Generative coding — delegated cloud** (vendor-managed session) | Real repository token held outside the sandbox; scoped credential issued inside it ([[credential-proxy-pattern\|credential proxy]]) | Push restricted to the working branch; vendor-side policy, customer-side unenforceable | Isolated per-session VM with automatic reclamation | Default-allowlist egress proxy | Repository clone only; no reach to host credentials | Vendor audit log; **first-party analytics do not follow sessions routed through a gateway or non-vendor model provider** |
| **Generative coding — CI-runner agent** (event-triggered, no human in-loop) | One token per workflow and environment at minimum scope, excluding `actions:write` unless workflow dispatch is intended — the authority to start a workflow is that workflow's authority | Untrusted-input exclusion covering the trigger as well as the payload: no fork-PR execution, no issue or comment bodies as instructions, and no workspace-supplied harness configuration ([[harness-config-as-supply-chain-artifact\|config as artifact]]) trusted by default; autonomy flags verified not to suppress the tool allowlist; the removed [[agents-rule-of-two\|Rule-of-Two]] property is declared and recorded | Runner isolation with no host credentials in the process environment, established **before** workspace configuration is read ([[gemini-cli-workspace-trust-rce\|GHSA-wpqr-6v78-jr5g]]) | Egress allowlist on the runner | [[credential-proxy-pattern\|Credential proxy]] rather than environment-variable delivery | Workflow logs + provider-side usage anomaly detection (new source addresses, endpoint changes) |
| **Generative coding — fleet / parallel** (many sessions, aggregate scale) | Per-agent identity; every action attributable to an agent *and* a human | Organization-wide managed policy; harness configuration under review, not per-developer discretion | Isolation enforced through image and device management, since no harness enforces containers itself | Gateway-mediated egress across the fleet | Harness-config tree scanned as a supply-chain artifact ([[harness-config-as-supply-chain-artifact\|config as artifact]]) | Inventory of harnesses, versions, MCP servers, skills, and hooks; cross-vendor attribution |
| **Data-science copilot (Jupyter, notebooks)**             | User identity passthrough                                                                         | Sandbox-resource policy; HITL on dataset writes                                                                                                                                                                                                  | Per-notebook sandbox (Firecracker)                                                                                  | Internal data warehouse via credential proxy                                                                                                             | Dataset lineage + AI-BOM                                                                      | OTel + behavioral drift                                                                                                                                                                                                   |
| **RAG application** ([[azure-rag-chatbot-security-profile\|Azure worked example]]) | Per-tenant agent identity                                                                         | Source-trust attribution; lethal-trifecta breaker                                                                                                                                                                                                | Input filter on prompts; output classifier + groundedness check                                                     | Vector store + per-source allowlist                                                                                                                      | **Closed corpus: answer-time entitlement enforcement + oversharing remediation** (the live risk); open/multi-writer corpus: document attestation + PoisonedRAG defense | Retrieval-pattern behavioral monitoring                                                                                                                                                                                   |
| **MCP server (consumed by agents)**                       | mTLS + workload identity (SPIFFE)                                                                 | OAuth 2.1 token exchange ([[cosai\|CoSAI]] / NIST CAISI)                                                                                                                                                                                                    | Tool-fingerprinting, rug-pull detection (Solo Enterprise)                                                           | n/a (server-side)                                                                                                                                        | Skill/server signing, version pinning                                                         | MCP CVE feed integration                                                                                                                                                                                                  |
| **Agent skill (e.g., Claude skill, Anthropic plugin)**    | Skill author identity (signed)                                                                    | Skill-scoped Cedar policy                                                                                                                                                                                                                        | Skill manifest validation; runtime sandbox                                                                          | Per-skill egress allowlist                                                                                                                               | Skill registry signing (sigstore); cognitive file integrity                                   | Skill execution telemetry                                                                                                                                                                                                 |
| **Multi-agent mesh ([[a2a-protocol\|A2A]] v1.0)**         | Per-agent Ed25519 identity (Oktsec-side; not in spec); SPIFFE/SPIRE workload identity             | Cross-agent ACL (Oktsec default-deny); [[a2a-protocol\|A2A]] opacity principle; per-skill authorization advertised in Agent Card; **stop-mesh-vs-isolate containment doctrine** ([[multi-agent-runtime-security\|Multi-Agent Runtime Security]]) | Per-agent runtime sandbox; AgentGateway broker between                                                              | A2A v1.0 over HTTPS / TLS 1.3 + signed Agent Cards (§8.4) + content scanning (Oktsec 268 rules); replay protection layered by impl (timestamps + nonces) | Shared memory / blackboard with provenance; cross-agent OTel `gen_ai.*` trace propagation     | Pairwise/triadic traffic baselines; **graph-walk anomaly detection** (SentinelAgent / TraceAegis-class, prototype); cross-agent drift correlation; cascade detection per [[multi-agent-runtime-security\|ASI08 doctrine]] |

### Recommended stacks by org profile

**Opinionated tool selections per plane** for two common org profiles. Neither stack is exhaustive — treat each as a starting point extensible per deployment shape and risk profile.

#### Enterprise stack (COTS-heavy)

Suited for large organizations with existing vendor relationships, centralized IAM, and SOC/SIEM infrastructure. Prioritizes vendor SLAs, support contracts, and integrations with Microsoft 365 / AWS / GCP environments.

| Plane | Primary choices | Notes |
|---|---|---|
| **Identity** | Microsoft Entra Agent ID + Microsoft Agent 365 Registry (GA), AWS Bedrock AgentCore, or GCP Agent Identity (all GA); Okta for AI Agents (Early Access, GA expected FY27); CyberArk Conjur or Aembit for NHI governance | Per-agent identity is GA platform-native on all three hyperscalers; **build this plane first** (it gates Egress and Observability); per-task capability tokens remain off-stack / leading-edge |
| **Control** | AWS Cedar managed policy service (March 2026 AI governance release); Anthropic Compliance API or Microsoft Agent Governance Toolkit (Apr 2026); Permit.io for RBAC UI | Cedar is the enterprise-grade choice; COTS wrappers add audit + workflow tooling |
| **Runtime** | LlamaFirewall (Meta OSS — no license cost) + NVIDIA NeMo NIMs (commercial inference); Microsoft Prompt Shields for content safety; per-task Firecracker VM or Hyper-V sandbox | Mix OSS guardrail (LlamaFirewall) with COTS NIM delivery for SLA coverage |
| **Egress** | **Azure API Management AI Gateway** (Microsoft stack: LLM token governance + inline Content Safety + MCP brokering with Entra/OAuth authorization, GA) or Entra Internet Access for network-layer PI / [[shadow-ai\|Shadow-AI]] filtering; Solo Enterprise for AgentGateway, Kong AI Gateway, or [[cloudflare\|Cloudflare]] AI Gateway; Operant MCP Gateway for MCP-specific authorization; mTLS via Istio / Linkerd | All-Microsoft shops have a native LLM gateway (APIM); MCP tool-integrity / rug-pull defense and per-task capability tokens still require an off-stack tool (Solo / Operant / Tenuo) |
| **Data** | Microsoft Purview AI (M365 environments); Wiz AI-SPM or Palo Alto Prisma AIRS; JFrog ML Catalog for AI-BOM; ReversingLabs for supply-chain scanning | Stack assumes M365 + cloud environment; swap Purview for CASB equivalent if GCP/AWS-native |
| **Observability** | [[datadog\|DataDog]] AI Monitoring or New Relic AI Monitoring (OTel-native); LangSmith for agent-specific tracing; Mindgard CART for continuous red-teaming; Vectra AI or Palo Alto Cortex XSIAM for behavioral monitoring | OTel gen_ai.* spans feed into existing SIEM; Mindgard replaces point-in-time red-team for CARTS programs |

#### FOSS / small-team stack

Suited for research teams, security teams running internal agent experiments, startups, or orgs with open-source mandates. Prioritizes zero licensing cost, community support, and composability. Requires more operational ownership.

| Plane | Primary choices | Notes |
|---|---|---|
| **Identity** | [[spiffe\|SPIFFE / SPIRE]] for workload identity; standard OAuth 2.1 + OIDC (any provider) for delegation; Aegis or AgentKeys for credential proxy | SPIFFE is the vendor-neutral workload-identity standard; pairs with any OIDC-compatible IdP |
| **Control** | [[opa\|OPA/Rego]] or [[cedar\|Cedar]] (open-source distribution); [[tenuo-warrant\|Tenuo Warrants]] (Rust OSS) for task-scoped capability tokens; the least-agency action-risk tiers for tier design | Both Cedar and OPA are fully OSS; Tenuo adds cryptographic delegation without a license |
| **Runtime** | [[llamafirewall\|LlamaFirewall]] PromptGuard 2 + AlignmentCheck + CodeShield (Meta OSS); NVIDIA NeMo Guardrails (OSS portion); [[firecracker\|Firecracker]] or [[gvisor\|gVisor]] for per-task sandboxing | Full LlamaFirewall stack is zero-cost; Firecracker is AWS OSS (Apache 2.0); gVisor is Google OSS |
| **Egress** | [[agentgateway\|AgentGateway]] (Linux Foundation, Apache 2.0); mTLS via Istio (CNCF OSS) or Linkerd (CNCF OSS); Docker `DOCKER-USER` iptables chain for egress filtering | AgentGateway is the canonical OSS agent proxy; Istio adds mTLS with near-zero operational overhead vs DIY |
| **Data** | OWASP AIBOM Generator (CycloneDX ML-BOM, OSS); sigstore / cosign for artifact signing; lockfile enforcement + SCA for [[slopsquatting\|slopsquatting]]; RAGShield or TrustRAG for RAG attestation; Brain Git (SlowMist) for state rollback | All zero-cost; for a closed-corpus bot the load-bearing control is answer-time entitlement enforcement (no strong FOSS option — this is a COTS/platform gap); RAGShield/TrustRAG are research-grade |
| **Observability** | [[opentelemetry-gen-ai\|OpenTelemetry gen_ai.* SemConv]] (CNCF standard, v1.37+); Langtrace or Traceloop (OSS) for agent-aware tracing; [[pyrit\|PyRIT]] (Microsoft OSS) + [[garak\|Garak]] (NVIDIA OSS) + [[promptfoo\|Promptfoo]] (OSS) for red-team coverage | OTel is the zero-cost observability foundation; the three red-team tools cover orchestration / probe / regression — all open-source |

The enterprise stack cuts operational overhead through vendor support and pre-built integrations, which is its advantage over the FOSS stack. Security capability is comparable: at L4 CMM a well-operated FOSS stack reaches equivalent controls. The FOSS stack is production-viable at D3–D4 of the CMM for most deployment shapes. The Research-type items (CaMeL, RAGShield at scale) are not yet production-ready for either stack.

## Threat-control matrix (OWASP Agentic AI Top 10 to planes)

Maps OWASP Agentic AI Top 10 (`ASI01`–`ASI10`) risk categories to the planes that primarily mitigate them. Most categories have controls in multiple planes; the following table identifies the *primary* control surface and lists reference controls for each.

```mermaid
flowchart LR
    subgraph Threats[Threats]
        ASI01[ASI01: Goal Hijack]
        ASI02[ASI02: Tool Misuse]
        ASI03[ASI03: Identity & Privilege]
        ASI04[ASI04: Supply Chain]
        ASI05[ASI05: Unexpected Code Execution]
        ASI06[ASI06: Memory Poisoning]
        ASI07[ASI07: Inter-Agent Comms]
        ASI08[ASI08: Cascading Failures]
        ASI09[ASI09: Human-Agent Trust]
        ASI10[ASI10: Rogue Agents]
    end
    subgraph Planes[Planes]
        ID[Identity]
        CTL[Control]
        RT[Runtime]
        EG[Egress]
        DT[Data]
        OBS[Observability]
    end
    ASI01 --> RT & CTL
    ASI02 --> CTL & EG
    ASI03 --> ID
    ASI04 --> DT
    ASI05 --> RT & CTL
    ASI06 --> DT
    ASI07 --> EG
    ASI08 --> CTL & OBS
    ASI09 --> OBS & CTL
    ASI10 --> ID & OBS
```

| OWASP ASI | Primary plane | Reference controls |
|---|---|---|
| ASI01 Goal Hijack | Runtime + Control | LlamaFirewall AlignmentCheck; HITL on goal-changing actions |
| ASI02 Tool Misuse | Control + Egress | Cedar/OPA tool-call policy; AgentGateway runtime authz |
| ASI03 Identity & Privilege | Identity | Okta for AI Agents; Microsoft Entra Agent ID; credential proxy |
| ASI04 Supply Chain | Data | OWASP AIBOM Generator; sigstore; Aguara Watch |
| ASI05 Unexpected Code Execution (RCE) | Runtime + Control | Sandboxed execution (e.g. mcp-run-python); ban `eval` / safe interpreters; code-generation/execution separation with validation gates; LlamaFirewall CodeShield |
| ASI06 Memory Poisoning | Data | RAGShield; cognitive file integrity; M365 memory-injection detector |
| ASI07 Insecure Inter-Agent | Egress | [[a2a-protocol\|A2A v1.0]] over HTTPS + signed Agent Cards (§8.4); Oktsec Ed25519 message signing + content scanning (268 rules) |
| ASI08 Cascading Failures | Control + Observability | Step-up gates (CSA ATF); pairwise/triadic baselines; graph-walk anomaly detection; stop-mesh-vs-isolate doctrine ([[multi-agent-runtime-security\|Multi-Agent Runtime Security]]) |
| ASI09 Human-Agent Trust Exploitation | Observability + Control | Plan-divergence detection; provenance UI with risk-differentiated prompts; HITL confirmation on sensitive actions; oversight-personnel training |
| ASI10 Rogue Agents | Identity + Observability | Behavioral drift detection; Okta Agent Discovery; [[distributed-kill-switch\|distributed kill switch]] |

The ASI categories above are the awareness-level labels; the underlying reference threat model the RA defends against is the T1–T17 catalog in [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]], which places each threat on a single-agent and multi-agent reference architecture and resolves them into six defensive playbooks that map cleanly onto these planes. For the certification view of where these controls are required, the [[owasp-asi-aiuc1-crosswalk|OWASP ASI to AIUC-1 crosswalk]] maps each ASI category to specific [[aiuc-1|AIUC-1]] requirements and records eight areas — inter-agent authentication, agent identity attestation, cascading-failure containment, tool-call observability, runtime monitoring, resource-abuse controls, supply-chain attestation, and I/O schema controls — where AIUC-1 has no requirement the plane controls here address.

### Threat classes beyond the ASI list

The ASI categories are per-component risks. The [[agentic-ai-threat-classes-2026|five threat classes]] are cross-cutting adversary models that no single plane absorbs. The table below places each class against the planes that carry its controls; the full plane-and-domain mapping for every taxonomy is consolidated in the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix.

| Class | Planes that carry the control | Note |
|---|---|---|
| 1 — AI-aware insider | Identity, Control, Data, Observability | Absorbed by the AI-BOM plus continuously-executed customer eval harness on the Data plane |
| 2 — Long-running APT campaign | Runtime, Egress, Observability | Cross-version eval continuity and sustained threat hunting; an operations function, not a single control |
| 3 — Collusion | Control, Runtime, Observability | Requires monitor isolation and output canonicalization; multi-agent cascade detection is research-stage (see Gaps) |
| 4 — Model-version degradation | Runtime, Data | Customer eval suite versioned independently of the vendor; pin-by-hash |
| 5 — Jurisdictional adversary | Governance only | No technical plane control mitigates a legal cutoff; the response is governance and vendor abstraction, and this architecture records the gap rather than filling it |

Two coverage limits are deliberate. Multi-agent **cascade containment** (ASI08) and **agent collusion** (Class 3) rest on research-stage detectors: the wiki tracks the stop-mesh-versus-isolate doctrine in [[multi-agent-runtime-security|Multi-Agent Runtime Security]], and no integrated product ships with documented thresholds. **[[model-layer-attacks|Model-layer attacks]]** carry thinner controls than the prompt- and tool-layer risks, and they do not share a plane. Membership inference and trojaned weights sit on the Data plane and in Class 4, where the customer-side defense is the eval-harness delta. Query-based extraction reaches the model through the permitted query interface, so its controls sit on the Egress plane as per-agent volume ceilings at the gateway; the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix carries the per-threat placement. Long-window query-pattern monitoring is the paired detection and belongs on the Observability plane, where no row above names it. The Exchange states that where an attacker can reach the model and the model allows intensive use, extraction is typically hard to protect against, so the thinness here is a property of the threat.[^aix-exfil-ra] Both limits are recorded in Gaps below and on the [[agentic-ai-threat-classes-2026|threat-classes page]].

## Trade-offs

Architectural trade-offs that vary with deployment scale, latency tolerance, and risk profile. Each row names the decision axis and the recommended default; a deviation carries a strategic-rationale field per the [[agentic-ai-security-cmm-2026|CMM]] reporting convention.

- **Single broker vs mesh.** AgentGateway-as-broker is simpler and provides a single policy chokepoint. Mesh (per-service AgentGateway sidecar) scales better but multiplies the policy surface. Default to broker for up to roughly 50 agents; move to mesh above that.
- **PDP location.** Inline (in-process with the agent) has the lowest latency but couples policy to runtime. Sidecar gives clean separation. External service is the standard zero-trust answer but adds a network round-trip, typically single-digit to low-tens of milliseconds per call. Default to sidecar.
- **Sandbox grain.** Per-call sandbox is safest but expensive; per-task sandbox is the practical default; per-agent sandbox is too coarse for high-risk-tier actions.
- **Fail-closed vs fail-open.** Default to fail-closed for high-risk-tier actions, fail-open for read-only / informational tier. CSA ATF Promotion Gates encode this directly.

## Gaps in the architecture

> [!gap] Known unfilled spots
> 1. **Compartmentalized LLM (CaMeL) reference pattern.** Privileged-LLM-coordinates-quarantined-LLM is theoretically sound but lacks a vendor-neutral reference implementation. (Google DeepMind research-stage.)
> 2. **Cross-tenant MCP server signing.** MCP CVE rate (30+ in Q1 2026) suggests the ecosystem is pre-supply-chain-hardening. sigstore-for-MCP-servers is needed but not standardized.
> 3. **Multi-agent failure containment.** ASI08 (Cascading Failures) and ASI10 (Rogue Agents) have no traditional cybersecurity equivalent. [[multi-agent-runtime-security|Multi-Agent Runtime Security]] covers the cascade-detection / behavioral-baseline / inter-agent IR depth, but 2026 remains the academic-prototype era: graph-walk monitors (SentinelAgent, TraceAegis) ship as papers, and vendor primitives exist (Oktsec rate limits + ACLs) without an integrated cascade-detection product shipping documented thresholds.
> 4. **AI-BOM operationalization gap.** The CycloneDX ML-BOM is the format; the operational workflow (CI/CD integration, vendor disclosure norm, AI-VEX equivalent) is thin.
> 5. **Transitive egress constraint on allowlisted destinations.** The Egress plane specifies filtering at the agent's own interface and says nothing about what an allowlisted destination can reach. Internal package proxies, artifact caches, and build services routinely hold broad internet access, which turns an allowlist entry into a relay reachable by SSRF ([[openai-hugging-face-agent-incident|OpenAI–Hugging Face incident]], 2026). No listed reference implementation emits evidence that an allowed service is itself constrained, and the plane's inter-agent controls assume an A2A protocol, leaving shared writable build infrastructure unmodeled as an agent-to-agent channel. The gap is wider than relaying: a permitted general-purpose host serves content directly, so an allowlist entry for `github.com` or `pypi.org` admits whatever those hosts publish ([[kimi-k3-sandbox-escape|Kimi K3]], 2026). Cataloguing what each allowlisted destination serves is unaddressed by every reference implementation listed.
> 6. **Control initialization order.** Every plane specifies *what* is enforced and none specifies *when* enforcement begins relative to the harness's own startup. Agentic coding harnesses read workspace-supplied configuration during startup, and [[gemini-cli-workspace-trust-rce|GHSA-wpqr-6v78-jr5g]] shows that step preceding sandbox initialization, which puts the resulting execution outside all six planes.[^gemini-init] No vendor documents the ordering for any harness the wiki tracks, so the property cannot currently be assessed even where an operator wants to.
> 7. **Identity binding when humans are decommissioned.** When the human owner of an agent leaves, the agent must be rotated or revoked. Okta and Microsoft Agent 365 cover this for managed agents; orphaned shadow agents are still discoverable but not always governable.
> 8. **Shared services above the isolation boundary.** Runtime isolation partitions agent execution and leaves every service the agents call undivided. The Exchange names shared inference, credential, and policy services as implicit cross-agent channels that sandboxing does not reach.[^aix-sandbox-ra] This architecture creates two of the three by design: the Control plane concentrates policy in a single PDP, and the Identity plane's credential proxy is one service holding credentials for many agents. Both choices are correct for the reasons those planes give, and both mean an agent's behaviour is observable to, and influenceable through, a component every other agent shares. The observability pipeline is a fourth such component, and this architecture creates it deliberately: one trace backend, one detection stack, and one SIEM serve the whole fleet, and the Exchange separately states that a defensive monitoring agent is part of the attack surface.[^aix-monitoruse-ra] No plane above holds a control for any of these four shared components, and no listed reference implementation partitions inference, credential issuance, or policy evaluation per agent. Gap 5 concerns what an allowlisted destination can reach; this gap concerns what a mandatory shared component can carry between agents that never address each other. Document 5 of the Exchange reaches one of these four shared components with a test rather than a control: at the infrastructure layer of an agentic penetration test, verify that the agent cannot suppress or alter its own logs under adversarial conditions, cited to `MONITOR USE`.[^aix-testing-ra] Failing that test demonstrates the gap on the observability component. The inference endpoint, the credential proxy and the policy decision point have no equivalent test named. That test's own four-layer scope, and its relation to this architecture's partition, are stated under [The six planes](#the-six-planes).
> 9. **Message-fabric integrity below the transport.** The Egress plane authenticates agents and blocks direct inter-agent paths; no plane validates the *content* of a structured message against a schema and a delegation scope once the transport is trusted. The Exchange states that peer-agent, tool, and orchestrator messages are untrusted input including inside single-agent tool loops, and that emergent collective behaviour can violate policy even where each agent complies in isolation.[^aix-amsm-ra] Signed delegation tokens with full-chain validation and scope non-expansion are the named control and have no reference implementation in any plane above. See [[agent-message-structure-manipulation|Agent Message Structure Manipulation]].
> 10. **Fleet-wide consumption correlation.** Per-agent quotas are graded on the Runtime plane and per-agent volume ceilings on the Egress plane; both bound one agent at a time. The Exchange requires monitoring consumption across the fleet for correlated spikes and slow exhaustion attacks, which detects a campaign spread thinly enough that no single agent breaches its own cap.[^aix-limitresources-ra] No plane holds a row for it and no listed reference implementation emits fleet-level consumption as a detection signal. A denial-of-wallet campaign shows what the omission costs: the service stays available and every response is correct, so an availability monitor stays silent and the only signal is cost per unit of completed work.[^aix-exhaustion-ra]
> 11. **Phantom-step detection on the orchestrator.** The Control plane's orchestrator-hardening row requires a tamper-evident workflow log held outside orchestrator memory and the reconciliation of executed agent actions against it, which surfaces steps that ran and were never planned.[^aix-oversight-ra] The Observability plane collects traces, behavioural baselines, and per-agent action history, and no row in it compares an execution record against an independent plan record. The comparison is what makes the log tamper-evidence useful: a log the orchestrator alone writes and alone reads records a compromised orchestrator's version of events. No listed reference implementation externalizes the workflow log, so the gap is the artifact as much as the detection built on it.
> 12. **The development environment is outside all six planes.** This architecture is the implementation surface of an oversight layer in production, and its Data plane nonetheless claims training data in scope. The [[owasp-ai-exchange|OWASP AI Exchange]] §3.0 states seven particularities of the AI development environment; no plane above accounts for these five: it holds real sensitive data rather than test data; its data, code, configuration and parameters are targets for behaviour manipulation; source code, configuration and parameters are critical intellectual property; external software components run inside it and can reach training data or model parameters; and collaborative training across trust boundaries — federated learning, merged PEFT modules, model conversion services — extends the attack surface further.[^aix-devtime-ra] The Exchange's answering controls are `DEV SECURITY`, `SEGREGATE DATA`, `CONF COMPUTE`, `FEDERATED LEARNING` and `SUPPLY CHAIN MANAGE`, of which only the last has a home here, in the Data plane's supply-chain-scanning row. Whether this architecture gains a development-time plane or declares the surface out of scope is unresolved; the current position claims part of it in a scope sentence and models none of it. §3.2 puts three named threats against that surface rather than properties alone — development-time data leak, direct development-time model leak, and source code/configuration leak, all three keyed to a confidentiality impact on assets held in that environment ([`/go/devleak/`](https://owaspai.org/go/devleak/)).

## Prior work and comparison

None of the six comparison frameworks below combines a vendor-neutral stance with a control-architecture specification for agentic deployments. Prior work falls into two categories: **cloud-specific operational guidance** (hyperscaler "how to secure AI on our platform" documents) and **vendor-neutral threat taxonomies** (threat enumeration without control specification). The table summarizes each and the gap it leaves.

| Framework                                   | Published                 | Vendor neutral?              | Agentic-specific?                                                         | What it covers                                                                                                                                                        | Key gap                                                                                                                                                                          |
| ------------------------------------------- | ------------------------- | ---------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Microsoft [[microsoft-zt4ai\|ZT4AI]]**                     | March 2026                | No (Azure / Microsoft stack) | Strong yes — agent identity, MCP governance, multi-agent verification     | Seven-pillar Zero Trust applied to AI (the "700 controls / 116 groups" figure is the whole Zero Trust Workshop, not an AI-pillar count — see [[standards-review-microsoft-zt4ai-2026-Q2\|ZT4AI review]]); Entra Agent ID, Agent 365 registry, Purview DSPM, Content Safety, APIM AI Gateway, Sentinel/Defender agentic SOC                                       | Azure-tooling-locked; no per-task capability tokens; MCP tool-integrity is guidance-only (no single Azure service); much of the adaptive agent-runtime layer is preview as of May 2026                                          |
| **Azure OpenAI Reference Architecture**     | Ongoing (Feb 2026 update) | No (Azure)                   | Partial — adds Entra Agent ID, scoped tokens, HITL via Logic Apps         | 5-layer: Network isolation · Identity/access · Content/prompt · Data protection · Monitoring                                                                          | Assumes static LLM API invocations; no multi-agent orchestration architecture; no MCP, A2A, or memory/state security                                                             |
| **Microsoft MCRA**                          | April 2025                | No (Microsoft ecosystem)     | Minimal                                                                   | Enterprise security architecture diagrams across ZT pillars; AI section = Microsoft Security Copilot as a *security tool*                                             | Not a defense-of-AI architecture; treats AI as defender-assistant, not as a class of workloads to secure                                                                         |
| **AWS Well-Architected Generative AI Lens** | November 2025             | No (AWS)                     | Partial — names "excessive agency" as a risk; adds an agentic AI preamble | 6 WAF pillars applied to GenAI lifecycle; security pillar covers: endpoint protection, output risk, prompt security, monitoring, model integrity                      | "Excessive agency" named but mitigations unspecified; no agent identity, tool sandboxing, agent-to-agent trust, or MCP coverage                                                  |
| **Google Cloud AI Security Foundations**    | 2025 (ongoing)            | No (GCP)                     | Partial — 6-layer model includes "Agents and Applications" layer          | 6 architectural layers: foundation → infrastructure → models → data → tools → agents; Model Armor for runtime guardrails; VPC/IAM/CMEK controls                       | "Agents and Applications" layer exists as a named category but is underspecified; no agent identity governance, tool execution sandboxing, inter-agent trust, or memory security |
| **[[csa-maestro\|CSA MAESTRO]]**            | February 2025             | Yes                          | Full — built exclusively for agentic AI systems                           | 7-layer threat taxonomy: Foundation Models → Data Operations → Agent Frameworks → Deployment/Infra → Evaluation/Observability → Security/Compliance → Agent Ecosystem[^maestro-ra] | Taxonomy only — identifies threats but specifies no controls, no implementation guidance, and no compliance mapping                                                              |

### Contribution beyond prior work

The six-plane model occupies the gap between cloud-specific operational guides and vendor-neutral threat taxonomies. Four contributions distinguish it:

**Vendor-neutral control architecture.** Unlike ZT4AI, the Azure RA, the AWS GenAI Lens, and the Google Cloud guidance, this RA specifies *how* to implement controls without mandating a particular hyperscaler. Reference implementations are labeled OSS / COTS / Std / Exploratory so organizations can substitute equivalent tools per plane.

**Agentic-throughout.** Unlike MCRA (AI = security tool) and the AWS / Google guidance (agentic = a paragraph), all six planes assume autonomous multi-step agent loops: credential proxy in the identity plane, per-task sandboxing in the runtime plane, A2A v1.0 in the egress plane, Warrant-based delegation in the control plane, memory-poisoning defense in the data plane, and behavioral-drift detection in the observability plane.

**MCP-specific surface.** The MCP attack surface produced 30+ disclosed CVEs in Q1 2026 (see [[mcp-cves-q1-2026|MCP CVEs Q1 2026]]), and every comparison framework above leaves it absent or underspecified. The egress plane addresses it through MCP runtime authorization, tool fingerprinting, and rug-pull defense.

**Tool supply chain.** The data plane covers AI-BOM generation, skill / model registry signing, and supply-chain scanning. This surface is absent in the cloud-specific guides and only named (not addressed) in MAESTRO's Agent Frameworks layer.

Beyond those four contributions, the RA synthesizes rather than originates. It inherits from:
- ZT4AI's principle of per-agent scoped identity with cryptographic binding
- CSA MAESTRO's 7-layer threat taxonomy as the threat-model input
- CSA ATF's promotion gates as the basis for risk-based step-up and progressive-autonomy promotion
- OWASP ASI Top 10 as the explicit control-to-threat mapping
- NIST CAISI Concept Paper's OAuth 2.1 extensions for agent delegation

What the RA contributes above those inheritances is one chain in one vendor-neutral document: a threat taxonomy (MAESTRO + OWASP ASI Top 10), a plane-by-plane control library (this RA), and a maturity model ([[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]). None of the six comparison frameworks provides that chain end-to-end.

## Relations

- Implemented by: [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — the CMM measures organizational maturity in operating this architecture.
- Defender-operations counterpart: [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] (with the [[agentic-soc-cmm|Agentic SOC CMM]]) — the architecture of an agentic Security Operations Center. This RA secures agentic-AI *applications*; the SOC RA structures the SOC that monitors and responds. They share only the securing-the-agents layer (this RA's Identity / Control / Observability planes ↔ the SOC RA's Identity & Action-Authority / Policy & Enforcement / Observability & Evaluation planes), and the SOC RA treats in-house AI applications as a monitored surface that this RA secures by design.
- Built on: [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] (six-layer inventory), [[agent-identity-architecture|AI Agent Identity Architecture]].
- Validated against: [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]], [[mitre-atlas|MITRE ATLAS]], [[csa-maestro|CSA MAESTRO / CSA Agentic Trust Framework]] (layer/element/gate names verified in [[standards-review-csa-maestro-atf-2026-Q2|the 2026-Q2 review]]), [[cosai|CoSAI — Coalition for Secure AI]].
- Compared with (§Prior Work): Microsoft ZT4AI (March 2026), Azure OpenAI Reference Architecture, Microsoft MCRA, AWS Well-Architected Generative AI Lens (Nov 2025), Google Cloud AI Security Foundations, CSA MAESTRO 7-layer model.
- Threat-anchored to: [[clawhavoc|ClawHavoc — Agentic Skill Marketplace Supply Chain Attack]], [[sandworm-mode-npm-worm|SANDWORM_MODE npm worm — AI Toolchain Poisoning]], [[meta-sev-1-agent-breach|Meta Sev 1 AI Agent Breach]], [[mcp-cves-q1-2026|MCP CVEs Q1 2026]], [[gtg-1002-ai-orchestrated-espionage|GTG-1002 — First Reported AI-Orchestrated Cyber Espionage Campaign]], [[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]] (the Egress and Observability evidence above).
- Stress-tested against: [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes — 2026 Expansion]] — the five threat classes beyond the OWASP ASI / MITRE ATLAS / Lethal Trifecta baseline (insider, APT campaign, collusion, model-version regression, jurisdictional adversary).

[^maestro-ra]: [CSA — Agentic AI Threat Modeling Framework: MAESTRO](https://cloudsecurityalliance.org/blog/2025/02/06/agentic-ai-threat-modeling-framework-maestro), retrieved 2026-06-22. Seven layers L1–L7, verified in [[standards-review-csa-maestro-atf-2026-Q2|the 2026-Q2 standards review]].
[^atf-ra]: [CSA — The Agentic Trust Framework: Zero Trust Governance for AI Agents](https://cloudsecurityalliance.org/blog/2026/02/02/the-agentic-trust-framework-zero-trust-governance-for-ai-agents), retrieved 2026-06-22. Four maturity levels (Intern → Junior → Senior → Principal) advanced through five promotion gates; see [[standards-review-csa-maestro-atf-2026-Q2|the 2026-Q2 review]].
[^bhusa-ra]: Dalton and Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026 (2026-08-06); summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]].
[^asr-evals]: LlamaFirewall PromptGuard 2 / AlignmentCheck attack-success-rate reductions come from Meta's evaluation on the [[agentdojo|AgentDojo]] benchmark ([arXiv:2406.13352](https://arxiv.org/abs/2406.13352)); the Constitutional Classifiers jailbreak-success figure is Anthropic's reported result (2025). Both reductions are restated and contextualized in [[wiki-novelty-and-counterarguments-2026|Wiki Novelty and Counter-Arguments]] §Thesis 1.
[^gemini-init]: [GitHub Advisory Database — GHSA-wpqr-6v78-jr5g](https://github.com/advisories/GHSA-wpqr-6v78-jr5g) (2026-04-24, CVSS 10.0); the pre-sandbox ordering is stated by the reporting researcher at [Novee Security](https://novee.security/blog/google-gemini-cli-rce-vulnerability-cvss-10-critical-security-advisory/) (2026-04-30). See [[gemini-cli-workspace-trust-rce|the incident record]].
[^aix-sandbox-ra]: [OWASP AI Exchange — Agent sandboxing and isolation](https://owaspai.org/go/agentsandboxing/), retrieved 2026-08-18. Implementation criteria (namespaces, mandatory access control, ephemeral layers, default-deny egress, inter-agent path blocking, credential placement, clean termination, platform-enforced quotas) and the four stated limitations.
[^aix-datalimit-ra]: [OWASP AI Exchange — DATA MINIMIZE](https://owaspai.org/go/dataminimize/) and [SHORT RETAIN](https://owaspai.org/go/shortretain/), retrieved 2026-08-20. The applicability statement covering data collection, preparation, training, evaluation and runtime logging; the requirement to remove or anonymize data once it is no longer needed or when legally required; and the statement that limiting a retention period can be seen as a special form of data minimization.
[^aix-monitoruse-ra]: [OWASP AI Exchange — MONITOR USE](https://owaspai.org/go/monitoruse/), retrieved 2026-08-18.
[^aix-testing-ra]: [OWASP AI Exchange — AI security testing](https://owaspai.org/go/testing/), retrieved 2026-08-19. Document 5, the four-layer agentic penetration-test model; its infrastructure layer covers API gateway controls, credential exposure, key management, and `MONITOR USE` log integrity, stated as verifying that the agent cannot suppress or alter logs under adversarial conditions.
[^aix-oversight-ra]: [OWASP AI Exchange — OVERSIGHT](https://owaspai.org/go/oversight/), retrieved 2026-08-19. The secure-orchestration requirements (orchestrator as highest-leverage target, untrusted validation and segregation of all inputs, coordination-only permissions with no direct high-impact tool credentials, tamper-evident workflow log external to orchestrator memory, human approval before irreversible or cross-system workflows, no direct orchestrator egress with external access routed through segmented sub-agents, action-to-log reconciliation for phantom steps) and the high-risk approval workflow's approval token and static capability manifest.
[^aix-limitresources-ra]: [OWASP AI Exchange — LIMIT RESOURCES](https://owaspai.org/go/limitresources/), retrieved 2026-08-19. The six per-agent resource dimensions (CPU time, memory, disk I/O, network egress, tool invocations, wall-clock execution time), the rule that containers, API gateways, or orchestration enforce the caps and the agent does not, clean termination with an audit event on breach, tighter tiers for low-trust and untrusted-content workloads, fleet-wide consumption monitoring, and the stated bound that resource limits bound cost and availability impact without preventing all harm within the allocated budget.
[^aix-exhaustion-ra]: [OWASP AI Exchange — AI resource exhaustion](https://owaspai.org/go/airesourceexhaustion/), retrieved 2026-08-19. The two attacker goals, depletion of funds and system unavailability, the MITRE ATLAS `AML.T0029` anchor, and the sponge / energy-latency attack framed as a denial-of-wallet attack.
[^aix-exfil-ra]: [OWASP AI Exchange — Model exfiltration](https://owaspai.org/go/modelexfiltration/), retrieved 2026-08-19. The query-based route to a replica, and the statement that the threat is typically hard to protect against where an attacker can reach the model and the model allows intensive use.
[^aix-escape-ra]: [OWASP AI Exchange — Agent escape](https://owaspai.org/go/agentescape/), retrieved 2026-08-18. Role and scope boundary verification at every tool invocation; the layer distinction from jailbreak.
[^aix-pi-ra]: [OWASP AI Exchange — Prompt injection](https://owaspai.org/go/promptinjection/), retrieved 2026-08-18. Privilege-based data flow control identified with CaMeL; the five structural mitigations, including tool-boundary firewalls positioned as lighter than dual-LLM architectures; the ranking of execution-level detection above text-level and model-level detection.
[^aix-amsm-ra]: [OWASP AI Exchange — Agent message structure manipulation](https://owaspai.org/go/agentmessagestructuremanipulation/), retrieved 2026-08-18. The threat definition and manipulated field set; the statement that it reaches single agentic flows; signed delegation tokens with full-chain validation and scope non-expansion; deny-by-default schema validation at tool and message boundaries; the multi-agent emergence note.
[^aix-datapoison-ra]: [OWASP AI Exchange — Data poisoning](https://owaspai.org/go/datapoison/), retrieved 2026-08-20. The manipulation routes, the targeted/backdoor versus sabotage split, and the control list for the threat.
[^aix-dqc-ra]: [OWASP AI Exchange — DATA QUALITY CONTROL](https://owaspai.org/go/dataqualitycontrol/), retrieved 2026-08-20. The five named detection methods and the filter-versus-alert threshold split.
[^aix-devsec-ra]: [OWASP AI Exchange — DEV SECURITY](https://owaspai.org/go/devsecurity/), retrieved 2026-08-20. The dataset-by-reference integrity problem, with LAION-400M's URL entries as the example and dataset-entry hashing as the answer.
[^aix-devtime-ra]: [OWASP AI Exchange — Development-time threats](https://owaspai.org/go/developmenttime/), retrieved 2026-08-20. The seven stated particularities of the AI development environment and the five controls the section routes to.
[^dream-taiwan]: Dream Research Labs, [Taiwan Multi-Agent Attack Reconstruction](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia) (2026-08-12); corroborated by Taiwan's Ministry of Digital Affairs, which independently named "Open Claw" in its own statement (Reuters, 2026-08-13). See [[taiwan-ai-agent-government-intrusion|the incident record]].

[^mcp-exposure-ra]: Alfredo Oliveira and David Fiser, [*Update on Exposed MCP Servers: The Threat Widens to the Cloud*](https://www.trendmicro.com/vinfo/us/security/news/vulnerabilities-and-exploits/update-on-exposed-mcp-servers-the-threat-widens-to-the-cloud), Trend Micro, 2026-04-28. The same scan disclosed CVE-2026-5058 and CVE-2026-5059 against `aws-mcp-server`, both command injection at CVSS 9.8.
