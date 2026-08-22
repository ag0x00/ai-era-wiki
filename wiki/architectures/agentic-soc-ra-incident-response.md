---
type: architecture
title: "Agentic SOC Incident Response Surface"
address: c-000199
created: 2026-06-03
updated: 2026-08-14
tags:
  - architectures
  - agentic-soc
  - incident-response
  - containment
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
related:
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-cmm-d4-identity-action-authority]]"
  - "[[agentic-soc-cmm-d5-observability-oversight]]"
  - "[[agentic-soc-cmm-d7-resilience-agent-supply-chain]]"
  - "[[agentic-soc-cmm-d8-people-governance]]"
  - "[[plan-validate-execute]]"
  - "[[distributed-kill-switch]]"
  - "[[mythos-ready-security-program]]"
  - "[[taming-shai-hulud-with-ai-talk]]"
  - "[[cyber-poverty-line]]"
  - "[[nist-ai-rmf]]"
  - "[[nist-ai-800-4]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[hugging-face]]"
sources:
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-soc-cmm]]"
  - "[[mythos-ready-security-program]]"
  - "[[agentic-soc-cmm-d4-identity-action-authority]]"
---

# Agentic SOC Incident Response Surface

Incident response and containment is where the SOC stops acting on the estate. Detection and triage produce a verdict; investigation reconstructs the story; response changes state — it isolates a host, disables an account, quarantines a mailbox, blocks an indicator, or rolls a credential. This is the per-function deep-dive for the incident-response function of the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]], describing how to build the response agent surface. The maturity half — how high this function's autonomy may legitimately climb — is scored by the [[agentic-soc-cmm|Agentic SOC CMM]].

The function runs primarily on three of the RA's planes: the **Identity & Action-Authority plane** (the agent's scoped, revocable authority to act on the estate), the **Policy & Enforcement plane** (the deterministic gates every consequential action crosses), and the **human-authority boundary** (where containment is approved, overridden, or revoked). Its autonomy is gated by more CMM domains than any other function — **D4** (Agent Identity & Action-Authority), **D5** (Observability & Oversight), **D7** (Resilience & Agent Supply Chain), and **D8** (People & Governance). It is the only function whose gates reach the full L4 set, because its actions are the most consequential and the least reversible the SOC takes.

**A containment action with excessive blast radius is itself an outage.** Isolating the wrong subnet, disabling a service account a hundred jobs depend on, or blocking a production CIDR converts a security control into an availability incident. That is why this function carries the heaviest gating: the cost of over-delegation here is not a missed detection but a self-inflicted disruption at machine speed.

## The agent surface

On the supervisor-worker topology, the orchestrator hands a confirmed incident to one or more **response worker agents**. A response agent does not decide *whether* there is an incident — that verdict arrives from the triage and investigation functions. It selects and executes a containment course of action against the affected assets, within the authority it holds.

The mechanism mix is **deterministic-first by design**. The reliable core is the pre-authorized containment playbook: a fixed action sequence with named blast-radius limits and explicit escalation conditions, the same shape a [[mythos-ready-security-program|SOAR]] response playbook has carried for years. AI sits on top of that core in two roles — selecting which playbook fits the incident, and parameterizing it (which hosts, which accounts, which time window) from the investigation's findings. The state-changing step itself crosses a deterministic gate; the AI proposes, the runtime acts. The [[plan-validate-execute|plan-validate-execute]] pattern is the structural form: the agent emits a structured containment plan (action class, target resources, scope, source of intent), a non-LLM policy engine validates it against blast-radius limits and deny rules, and only a validated plan reaches execution. Validation is deterministic, not a second model, so it is not vulnerable to the same [[prompt-injection|prompt injection]] that could subvert the agent.

The tools the agent calls are the response APIs of the estate: EDR host-isolation, identity-provider account disablement and session revocation, firewall and proxy block-lists, mailbox quarantine, cloud-workload quarantine, credential rotation. The data it reads is the case substrate the investigation function produced and the threat-intelligence grounding that scopes the indicators. Each call is identity-bound to the agent (D4), logged to a tamper-evident record, and carries a rollback reference where the action is reversible.

The human-authority boundary sits directly across the execution step for any action above the agent's auto tier. Below that line — high-confidence, narrowly-scoped, reversible actions — a pre-authorized playbook runs without a human in the moment. Above it, the agent's structured plan is surfaced for approval before execution. The boundary is asymptotic: even a fully delegated response function terminates here, with a human able to govern, override, and roll back. A [[distributed-kill-switch|distributed kill-switch]] generalizes the override so that every in-loop operator, not only a central console, can halt a running response before it completes.

This function is the operational form of the [[nist-ai-rmf|NIST AI RMF]] MANAGE function for the SOC: MANAGE requires that identified risks be prioritized and responded to, and the response surface is where that response changes state on the estate. The questions the upstream detection and escalation steps must answer — when an AI incident has occurred, who is notified, and how it escalates — are the same open post-deployment problems [[nist-ai-800-4|NIST AI 800-4]] catalogs for monitoring deployed AI, since an AI-targeting incident often leaves no traditional file or network signature.

This function also carries the SOC's **AI-specific incident-response** path. When the in-house AI-application surface is the target — a prompt-injection campaign, a jailbreak, RAG data exfiltration, an agent hijack — the response uses AI-IR playbooks (the CoSAI AI-IR shape) rather than host-and-network containment: revoke the agent's identity, quarantine the affected model or retrieval pipeline, roll the leaked secrets, and reconstruct from a known-good baseline. The internet-scale multi-agent triage [[taming-shai-hulud-with-ai-talk|Wiz described in its Shai-Hulud response]] — parallelized victimology and automated secret-impact analysis across a supply-chain campaign — is the response-side counterpart at scale: many incidents, fanned out across worker agents, scoped and prioritized faster than a human team could.

## Autonomy progression

The function's autonomy is read on the CMM's per-function ladder (L0 Manual → L4 Delegated). The ladder is mechanism-agnostic: deterministic SOAR occupies rungs on its own, and AI moves the function up the ladder faster, not into a different ladder. The load-bearing column is the gate — the [[agentic-soc-cmm#the-gating-rule|gating rule]] holds that a function may run at autonomy L_k only when the domains governing that level are mature enough to support it.

| Level | What it looks like for incident response | Gating domains |
|---|---|---|
| **L0 — Manual** | An analyst executes every containment step by hand — pulls the cable, disables the account in the console, edits the block-list. No agent surface. | — |
| **L1 — Assisted** | The agent recommends a containment course of action and pre-stages it; a human reviews and executes. Decision support only, human-in-the-loop on every action. | — |
| **L2 — Semi-autonomous** | Routine sub-tasks run (enrichment, drafting the containment plan, pre-staging the API calls), but every consequential action — every state change on the estate — needs explicit human approval. | [[agentic-soc-cmm-d4-identity-action-authority\|D4]] · D1 |
| **L3 — Conditional** | High-confidence, narrowly-scoped containment runs autonomously within bounds; the agent escalates out-of-bounds actions to a human. Pre-authorized containment with blast-radius limits is the deterministic mechanism that makes this rung safe. Humans monitor on-the-loop and can intervene mid-action. | + [[agentic-soc-cmm-d5-observability-oversight\|D5]] · D3 |
| **L4 — Delegated** | The response function owns its lifecycle within a governed authority boundary; humans govern outcomes and approve the upper-tier actions. Reached only where the organization can secure the response agents at scale and govern machine-speed response — including simultaneous incidents. Asymptotic: a human authority boundary remains. | + [[agentic-soc-cmm-d7-resilience-agent-supply-chain\|D7]] · [[agentic-soc-cmm-d8-people-governance\|D8]] |

The gates tighten in a way specific to this function:

- **D4 gates every rung.** A response action is a state change on production, so it needs scoped, revocable authority and a per-action tier (auto / propose / approve / block) before it may run at all. [[agentic-soc-cmm-d4-identity-action-authority|D4]] adds the control the data-access models do not carry: **blast-radius limits** — caps on how many hosts, accounts, or segments a single agent action may affect before it must escalate. L2 needs scoped authority and a coarse auto/approve split; L3 needs formal tiers and blast-radius caps enforced at a deterministic [[oversight-layer|policy decision point]]. Pre-authorized containment is the deterministic mechanism that lets L3 run without a human in the moment: the action is bounded by a gate the agent cannot exceed, so its blast radius is capped even if its judgment is wrong.
- **D7 and D8 gate the upper rungs because a response agent is the most dangerous one to compromise or over-delegate.** A privileged response agent can isolate a host, block an identity, and quarantine a workload — so [[agentic-soc-cmm-d7-resilience-agent-supply-chain|a compromised response agent is an attacker that already holds containment authority]]. D7 keeps the agent's composition verified and its failure mode fail-closed (no containment without verification); D4 bounds the damage if D7 fails. [[agentic-soc-cmm-d8-people-governance|D8]] requires that the organization can govern machine-speed response: a named owner of the autonomy-raising decision, rehearsal of *simultaneous* incidents, and the human-authority boundary held as a governed commitment rather than a tool setting.

**The volume case for agentic incident response now has an empirical anchor.** The [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] was reconstructed across more than 7 billion log entries and millions of GPU hours, using Codex and other agents to conduct the review, and the investigation was still open when the reconstruction was presented in August 2026. Its authors' recommendation is agentic IR as a standing capability, on the grounds that an incident driven by an agent collective is forensically dense and high-volume by construction while manual response scales linearly. The recommendation extends past investigation to remediation, as a fully automated identify → propose patch → roll out → roll back loop: automating discovery alone moves the bottleneck to patching and drowns the human engineers who own it, so the loop has to close on rollback or the acceleration is spent queueing. Dalton and Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026, summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]].

This does not raise the containment ceiling. Reading, correlating, and reconstructing produce reviewable outputs below the action boundary, so investigation-side delegation is gated like detection authoring rather than like containment, and a SOC may run agentic reconstruction well above the rung its D4 and D8 maturity permits for autonomous state changes. What the incident changes is the cost of not delegating the reading half: at this volume a human-paced investigation does not converge, and the rollback leg of the remediation loop is the control that makes the automated half safe to run.

The defined failure mode is **operating above the earned ceiling** — running response at L3 or L4 when the governing domains do not support it. For this function that failure is acute: a delegated containment with no blast-radius limit (D4 immature), no live oversight (D5 immature), or no compromised-agent recovery drill (D7 immature) is reckless autonomy whose first bad action is an outage. The weakest of D4, D5, D7, and D8 caps the function; the model names that domain as the one to mature next.

## Control landscape (dated)

Vendors and patterns are named as dated, swappable examples, not endorsements. The deterministic core predates AI; the AI layer parameterizes and selects over it.

| Capability | What ships today | Status (mid-2026) |
|---|---|---|
| Pre-authorized containment playbooks | SOAR / response-platform playbooks with fixed action sequences, approval tiers, and escalation conditions (Splunk SOAR, Tines, Torq, Microsoft Sentinel automation rules, Google SecOps SOAR) | GA and long-established; the deterministic spine of the function |
| Blast-radius / least-action gating | Per-action authority tiers (auto / propose / approve / block) and named scope caps configured over the response platform; [[plan-validate-execute\|plan-validate-execute]] with a non-LLM policy engine (Cedar, OPA) validating the structured containment plan | Tiers and approval workflows GA in mainstream SOAR; per-action *tiering over agents* and plan-validate gating are a pattern assembled over them, not a shipped agent-governance product |
| Response API surface | EDR host-isolation, IdP account disablement and session revocation, firewall/proxy block-list push, mailbox quarantine, cloud-workload quarantine, credential rotation | GA across endpoint, identity, network, and cloud platforms; the actions the agent's authority is scoped to |
| AI response agents | Vendor response/containment agents in the agentic-SOC stacks (Microsoft Security Copilot, Google SecOps Gemini, CrowdStrike AIDR) selecting and parameterizing playbooks | Mix of GA and preview across vendors; current-state is dated and should be re-verified at use |
| Break-glass override | Human-authority approval, override, and rollback surfaces at the boundary; [[distributed-kill-switch\|distributed kill-switch]] / halt-authority held by every in-loop operator | Approval/override GA in response platforms; the distributed-halt pattern is emerging, assembled from a gateway halt signal plus governance policy |
| AI-specific incident response | AI-IR playbooks (CoSAI AI-IR shape) for the in-house AI-application surface: agent-identity revocation, model/pipeline quarantine, secret rotation, known-good reconstruction | Practice-level; the playbook content is the dated layer, the response mechanics reuse the deterministic spine above |
| Simultaneous-incident handling | Multi-agent triage and response fanned out across worker agents — parallelized victimology and automated secret-impact analysis ([[taming-shai-hulud-with-ai-talk\|Wiz's Shai-Hulud response]]) | Practitioner-demonstrated at internet scale; not a packaged product, built from agent orchestration plus the response APIs |

## Failure modes and what to watch

- **Reckless containment (excessive blast radius).** The defining risk of this function: an autonomous action that isolates too much, disables too broadly, or blocks production traffic. Bounded by D4 blast-radius limits and the deterministic plan-validate gate; an action above its tier or scope must escalate rather than execute. Watch the false-containment rate and the blast-radius-limit trigger count.
- **Over-delegation past the earned ceiling.** Running L3/L4 response without the governing domains. Bounded by the gating rule (the weakest of D4/D5/D7/D8 caps the function) and the D8 autonomy-raising decision right — a named approver, a written evidence bar, a re-review date. Watch for autonomy that crept up because an agent "seemed reliable."
- **Compromised response agent.** A subverted agent with containment authority is an attacker inside the boundary. Bounded jointly by D7 (verified composition, fail-closed behavior, rehearsed compromised-agent recovery) and D4 (scoped, revocable authority that limits the damage). Watch for harness-config drift and run the compromised-agent recovery drill.
- **Irreversible-action regret.** Some response actions cannot be cleanly rolled back (a rotated credential, a deleted resource). The plan-validate-execute confirm tier and a maintained rollback reference bound this; actions without a rollback path should sit higher up the authority tiers.
- **Review fatigue / rubber-stamping.** When every containment needs approval, approvers degrade into approval bots and the human gate becomes nominal. Risk-proportional confirmation UX and surfacing the delta from expected behavior (per [[plan-validate-execute|plan-validate-execute]]) mitigate it; the failure is a human boundary that exists on paper but not in practice.
- **Detection lag against a machine-speed campaign.** Every rung above assumes a verdict arrives before response begins. In the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] the operator's own workload alert on internal privilege escalation fired on 2026-07-19, roughly eleven days after the second intrusion cluster began and three days after [[hugging-face|Hugging Face]] published its own disclosure; the two incidents were connected on 2026-07-20 only because OpenAI asked Hugging Face to revoke a credential and was told it had already been revoked, having been used in the Hugging Face compromise. Attribution by coincidence is not a control, and a response function delegated to L3 is idle for the interval no detection covers. Watch time-to-detect on the organization's own agent fleet, and treat an external party's disclosure that names your infrastructure or credentials as a page-worthy trigger with a defined runbook rather than as intelligence to read later.
- **Simultaneous-incident overload.** Machine-speed attacks open several incidents at once and a human-paced response cannot keep a coherent authority boundary across them. Bounded by D8 rehearsal of the simultaneous-incident case and a communications plan that does not assume one incident commander.

> [!gap] The authority boundary at machine speed is asserted, not demonstrated
> Every rung holds a human-authority boundary as the load-bearing control, and L4 requires that boundary to survive simultaneous, machine-speed incidents. No source on the wiki shows it actually holding under that load. The failure modes that erode it — rubber-stamping under approval volume, an incident-commander model that assumes one incident at a time — are precisely the conditions machine-speed attacks create. D8 prescribes rehearsing the simultaneous-incident case, but rehearsal is the recommendation, not evidence that a governed boundary keeps pace with autonomous containment. Whether a human boundary can bound machine-speed response across concurrent incidents is unresolved.

## Right-sizing by org profile

The realistic autonomy target for response rises with the blast radius the function can reach and the maturity behind the gates. Because response is the most consequential function, its right-sized target sits a rung below the analysis functions for the same org.

| Band | Realistic response-autonomy target | Why |
|---|---|---|
| Solo / small | L1 → L2, often via the provider | Near or below the [[cyber-poverty-line\|cyber-poverty line]]. Containment authority is narrow by necessity, and the team borrowing capability through an MSSP/MDR inherits the provider's response playbooks and approval controls. For its few in-house agents, recommend-and-approve (L1) and a coarse auto-tier on narrow reversible actions (L2) are achievable and sufficient. AI lowers the barrier to the lower rungs — a small team gets pre-staged, parameterized containment without hand-building the SOAR playbook or wiring the ETL the older automation demanded. Delegated response is not warranted; the blast radius of getting it wrong outweighs the speed gain |
| Mid | L2 → selective L3 | An in-house SOC can run formal auto/propose/approve/block tiers with blast-radius limits (D4 L3) and a working human-on-the-loop oversight surface (D5 L3), then let high-confidence, narrowly-scoped containment run in-bounds on its highest-volume incident types. Pre-authorized containment with deterministic gating does the heavy lifting; L3 is earned per action class, not switched on wholesale |
| Enterprise | Selective L3 → L4 on narrow action classes | A full response-agent fleet with broad containment authority can reach delegated response, but only where D7 (rehearsed compromised-agent recovery, signed and reconciled agent composition) and D8 (governed autonomy-raising, simultaneous-incident rehearsal, the human-authority boundary as a standing commitment) both hold. Even at L4 the boundary remains: the upper-tier and irreversible actions terminate at human approval, and the delegation is scoped to the action classes whose blast radius is bounded and reversible |

## Relations

- Per-function deep-dive of the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]]; runs primarily on its Identity & Action-Authority plane, Policy & Enforcement plane, and the human-authority boundary.
- Scored by the [[agentic-soc-cmm|Agentic SOC CMM]]; its autonomy is gated, per the [[agentic-soc-cmm#the-gating-rule|gating rule]], by [[agentic-soc-cmm-d4-identity-action-authority|D4 Identity & Action-Authority]] (every rung), [[agentic-soc-cmm-d5-observability-oversight|D5 Observability & Oversight]] (L3), and [[agentic-soc-cmm-d7-resilience-agent-supply-chain|D7 Resilience & Agent Supply Chain]] with [[agentic-soc-cmm-d8-people-governance|D8 People & Governance]] (L4) — the only function reaching the full L4 set.
- Deterministic enforcement mechanism: [[plan-validate-execute|plan-validate-execute]]; the human-side override generalizes through the [[distributed-kill-switch|distributed kill-switch]].
- Practitioner evidence: the [[mythos-ready-security-program|Mythos-ready Security Program]] (PA 9 deception, PA 10 "build an automated response capability — systemic and, to the degree possible, autonomous", pre-authorized containment, and simultaneous-incident response) and [[taming-shai-hulud-with-ai-talk|Wiz's Shai-Hulud multi-agent IR triage]] (internet-scale parallelized victimology and secret-impact analysis).
