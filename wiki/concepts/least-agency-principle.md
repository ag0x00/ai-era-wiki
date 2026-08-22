---
type: concept
title: "Least Agency Principle"
created: 2026-04-30
updated: 2026-08-21
tags:
  - concepts
  - agentic-ai
  - access-control
  - autonomy-governance
status: developing
scope_axis:
  - sec-of-ai
aliases:
  - "Least Agency"
  - "Minimum Autonomy Principle"
related:
  - "[[owasp-agentic-ai-top-10]]"
  - "[[emerging-cybersecurity-practices-for-agentic-ai-applications]]"
  - "[[owasp-ai-exchange]]"
  - "[[agent-escape]]"
  - "[[security-controls-for-ai-stacks]]"
  - "[[agent-sandboxing]]"
  - "[[non-human-identity]]"
  - "[[lethal-trifecta]]"
  - "[[lethal-bifecta|Lethal Bifecta]]"
  - "[[tool-abuse-chains]]"
  - "[[prompt-injection-containment]]"
  - "[[distributed-kill-switch]]"
  - "[[decision-rights]]"
  - "[[securing-your-agents-talk]]"
  - "[[scaling-agentic-ai-cios-talk]]"
  - "[[aws-agentic-ai-security-scoping-matrix]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agents-rule-of-two]]"
  - "[[accidental-meltdown|Accidental Meltdown]]"
  - "[[evaluation-containment-failure|Evaluation Containment Failure]]"
sources:
  - "[[.raw/papers/emerging-cybersecurity-practices-for-agentic-ai-applications.md]]"
  - "[[.raw/talks/securing-your-agents-2026-04-30.md]]"
  - "[[.raw/talks/scaling-agentic-ai-cios-2026-05-01.md]]"
  - "[[.raw/articles/aws-agentic-ai-security-scoping-matrix-2026-05-07.md]]"
---

# Least Agency Principle

## Definition

The **Least Agency Principle** extends the classical principle of least privilege to include **autonomy as an independently governable security dimension**. It states:

> Only grant agents the minimum autonomy required to perform safe, bounded tasks. It is no longer sufficient to control what an agent *can* access; organizations must also control how much freedom it has to *act* on that access without human oversight.

The principle is introduced in the **OWASP Agentic AI Top 10 (2026)** as a core design principle for agentic systems. The risk-tier model below is its operationalization in [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]] (§3.2), which presents the tiers as how least agency manifests in practice; the ASI Top 10 names the principle and supplies no tier scheme, an absence [[owasp-agentic-ai-top-10|that framework's page]] records.

### Anchor terms — agency vs autonomy

The two terms have distinct definitions, formalized in the [[aws-agentic-ai-security-scoping-matrix|AWS Agentic AI Security Scoping Matrix]] (Brown & Saner, 2025-11-21):

- **Agency** — the scope of actions an AI system is permitted and enabled to take within the operating environment, and how much a human bounds an agent's actions or capabilities.
- **Autonomy** — the degree of independent decision-making and action the system can take without human intervention.

The wiki uses the terms with these definitions throughout. Agency is the *what* (allowed actions); autonomy is the *how independently* (decision-making latitude). A system can have high agency and low autonomy (broad permissions but every action approved) or low agency and high autonomy (read-only but decides freely within that scope). Because the two axes are independent, "least agency" — the principle of this page — implicitly governs both.

## Autonomy as a distinct dimension

Traditional access control (RBAC, ABAC, least privilege) governs **what** an entity can access. For deterministic systems, access scope and action scope are essentially the same: a user with read access to a file can read it; no more.

For AI agents:
- Access scope and action scope **decouple**. An agent with read access to all email can autonomously summarize, forward, draft replies, archive, and flag — none of which requires additional access permissions but all of which can cause harm.
- The agent's action selection is influenced by its inputs — emails, documents, web content — which may be adversarially crafted. Even well-scoped access becomes a risk when action selection is non-deterministic.
- **Autonomy amplifies access**. An agent with read-only access to financial data can cause significant harm by autonomously sending that data as email attachments, even if it cannot write to financial systems.

## The action-risk tiers

Operationalized in practice as a four-tier action-risk classification:

| Tier | Wiki short name | Approval Required | Examples |
|---|---|---|---|
| Low risk    | `auto`    | Auto-execute      | Read files, search web, enrich data, look up records          |
| Medium risk | `notify`  | Notify human      | Scale cloud resources, send messages, modify configurations    |
| High risk   | `confirm` | Explicit human approval | Delete data, financial transactions, modify security groups |
| Prohibited  | `block`   | Never execute     | Credential exfiltration, recursive self-modification, unauthorized replication |

The source names the tiers by risk level and by behaviour. The short names in the second column are the wiki's own labels for the same four tiers, and they are the form every other page uses.

The tier assignment is fixed at **tool annotation time** (CI/CD), ahead of runtime; the model does not set its own tier. This is the CI-time tool annotation primitive — equivalent to the containment layer's human-in-the-loop control.

### The oversight-requirement axis

Risk tier answers which actions are dangerous. It does not answer what oversight a dangerous action receives, which is a second axis the [[owasp-ai-exchange|OWASP AI Exchange]] states separately under `OVERSIGHT`: classify actions by risk tier before deployment, then assign each tier one of four oversight requirements.[^aix-oversight]

| Oversight requirement | What the human does | Position relative to execution |
|---|---|---|
| Fully autonomous execution | Nothing | No gate |
| Soft confirmation | Is notified, with the option to cancel | Before execution, on a cancellation window |
| Hard confirmation | Actively approves, with context supplied | Before execution, blocking |
| Mandatory human initiation | Triggers the run; the agent cannot start without it | Before the task, not before the action |

The two axes compose rather than compete. Mandatory human initiation has no counterpart in the risk tiers above, because it gates the agent's start rather than one of its actions, and it is not the prohibited tier: a prohibited action is refused whoever asks, while a human-initiated action is permitted only when a human asked. Soft confirmation and the notify tier differ on the same boundary — soft confirmation opens a cancellation window before the action runs, and notify reports an action that has already run. The Exchange requires each checkpoint to be an infrastructure-layer gate rather than an instruction, with the agent technically incapable of proceeding without a verified human approval signal.[^aix-oversight]

[[prompt-injection-containment|Prompt injection containment]] treats this tier model as one of two execution-layer controls, alongside the [[credential-proxy-pattern|credential proxy pattern]] and [[agent-sandboxing|sandboxing]], for bounding what an injection can do once input-layer detection has failed: the High-risk tier's human-approval requirement is what stops an injected instruction from completing an irreversible action.

## Relationship to Traditional Security Controls

| Traditional Concept      | Least Agency Extension                                   |
|--------------------------|----------------------------------------------------------|
| Least privilege (access) | Least agency (autonomy over actions with that access)    |
| Role-based access control | Risk-tier-based action control                           |
| PAM / human approval flows | High-risk tier requiring human confirmation             |
| Blocked operations list   | Prohibited tier — hard block regardless of model output  |

The prohibited tier corresponds to the **platform-level vs. prompt-level security** principle: prohibited actions must be blocked at the platform/runtime layer, not by prompt instruction. A model instructed to never exfiltrate credentials can be injected to do so; a platform-level block cannot be bypassed by the model. The block is exactly as narrow as the granted scope. The [[owasp-ai-exchange|OWASP AI Exchange]] states the residual directly: an agent with broad authorised scope can cause harm through jailbreak without ever crossing a boundary, so a platform-level block bounds the damage at the edge of what was granted and does nothing inside it.[^aix-escape] That makes the width of the grant, rather than the strength of the block, the quantity a least-agency review measures.

Least agency retains its force where the adversary-based controls do not. In an [[accidental-meltdown|accidental meltdown]] there is no injected input and no attacker, so content filters and injection classifiers never fire, while a capability the agent does not hold remains one it cannot use. That property is what the four 2026 [[evaluation-containment-failure|evaluation containment failures]] test: in each, every primitive the agent used was one it had been granted.

[[prompt-injection-containment|Prompt injection containment]] places the tier model in layer 2 of its three-layer stack, the execution-containment layer that runs on the assumption that input detection has already failed. Layer-1 classifiers such as [[llamafirewall|LlamaFirewall]] and PromptGuard 2 reduce the attack success rate and leave a residual, so tier assignments are sized for the case where every tool call the agent emits is the attacker's choice. A tier assignment therefore holds independent of measured detection performance: a high-risk action stays behind explicit human approval, and the notify tier contains an injected action only where a human reads the notification before that action completes.

The [[microsoft-zt4ai|Microsoft ZT4AI]] framework instantiates this principle as its least-privilege pillar: agents receive scoped, time-bounded, revocable capabilities, which ZT4AI documents as the least-agency principle applied to non-human identities (see [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]]).

## Implementation Patterns

1. **Tool annotation at CI/CD**: annotate each tool the agent can invoke with its risk tier before deployment. This is static analysis; runtime policy enforcement is the separate pattern below.
2. **Runtime tier enforcement**: when the agent selects a tool call, the risk tier determines the execution path (auto / notify / confirm / block).
3. **Scope narrowing for sub-agents**: in multi-agent systems, child agents should inherit a tier threshold no lower than their parent's. A high-risk action cannot be delegated to a sub-agent to bypass human approval.
4. **Human-in-the-loop flows**: the high-risk tier requires a working HITL mechanism — not a logging call. The primitive is: pause execution, present the intended action to a human, resume only on explicit approval.
5. **Prohibited tier is absolute**: no LLM reasoning or tool instruction can override a prohibited action. If the runtime receives a tool call for a prohibited action, it blocks and alerts — it does not ask the model to reconsider.
6. **Scope binding at invocation**: authorize each tool call against the invoking agent's current task scope and role in addition to the tool's own annotation. The [[owasp-ai-exchange|OWASP AI Exchange]] treats a call to an individually authorised tool made while the agent is performing an out-of-scope task as an escape event, which means the authorization decision must read task state that the CI-time annotation cannot carry.[^aix-escape] This is the capability that separates holding a tool from being permitted to use it now, and it is graded at [[agentic-ai-security-cmm-d3-control-least-agency|CMM D3]] L4.
7. **Dynamic narrowing on risk elevation**: reduce the permission set automatically at the moment untrusted external content enters the flow, excluding write, sensitive-read, and outbound tools. The [[owasp-ai-exchange|OWASP AI Exchange]] states this as the required answer where the person or agent selecting an agent may pick one carrying more permissions than the task needs, since a static assignment made at selection time cannot account for what the task later ingests.[^aix-leastmodelpriv] The same entry treats permission escalation as an explicit logged event requiring human approval or policy-engine criteria, never self-escalation through the agent's own reasoning.[^aix-leastmodelpriv] This pattern has no home in the CMM's D3 domain today: [[agentic-ai-security-cmm-d3-control-least-agency|D3]] grades four of the Exchange's seven agentic-authorization-framework parts and leaves this one ungraded, and [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] L4 names it as the specification its own post-hoc read-only narrowing is the runtime counterpart to, without a rung of its own.

## Connection to OWASP ASI

The least agency principle directly addresses multiple OWASP Agentic AI Top 10 categories:

- **ASI02** (Tool Misuse): tier classification limits the damage scope of compromised tool selection.
- **ASI08** (Cascading Failures): auto-execute tier bounding limits how far a single bad decision propagates.
- **ASI03** (Identity and Privilege Abuse): tier classification caps the privilege and autonomy an agent holds, limiting escalation scope.
- **ASI10** (Rogue Agents): high-risk tier requiring human approval prevents a rogue agent from taking irreversible actions autonomously.

The companion [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]] guide is the operational counterpart: its playbooks for tool execution and reasoning manipulation specify least-privilege and just-in-time tool access, execution sandboxing, and scoped memory as the controls that implement least agency against Tool Misuse (T2), Privilege Compromise (T3), and Intent Breaking and Goal Manipulation (T6).

## Organizational counterpart: distributed kill switch

[[distributed-kill-switch|Distributed Kill Switch]] is the organizational counterpart to the technical block tier. The technical layer enforces blocks based on annotation; the organizational layer empowers any human in the loop to *invoke* the block at runtime, downgrading the agent mid-run. Both are needed: the annotation is preventive (the action class can never happen), the kill switch is reactive (this specific run goes wrong, anyone can stop it). Per [[scaling-agentic-ai-cios-talk|Gartner's Scaling Agentic AI talk]], distributing halt-authority across the workforce is the difference between governance and a single CIO standing alone with a kill switch.

## Restatement as a design constraint

[[agents-rule-of-two|The Agents Rule of Two]] (Meta, 2025) narrows least agency to a bounded rule an architect can apply without a full capability inventory: hold no more than two of untrusted input, sensitive access, and state change or egress. Least agency says strip every capability you can; the Rule of Two says which one to strip first and names supervision as the fallback when none can be removed. The two are compatible, and the Rule of Two is the more actionable of the pair at design review.

[[lethal-bifecta|The Lethal Bifecta]] narrows the principle on the action axis. Untrusted input combined with a sensitive write is sufficient for a damaging action, and egress removal — the move that contains the read side — leaves that pair intact, so the write side carries a structural test of its own. Least agency states the direction of the reduction across every capability; the bifecta names the two-ingredient combination that decides which writes belong behind the confirm and block tiers.

## See Also

- [[agent-sandboxing|Agent Sandboxing]] — OS-level enforcement of the prohibited tier (syscall blocking)
- [[prompt-injection-containment|Prompt Injection Containment]] — uses the risk-tier model as an execution-layer containment control
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] — containment layer where this principle lives
- [[distributed-kill-switch|Distributed Kill Switch]] — organizational counterpart at the block tier
- [[decision-rights|Decision Rights for AI Agents]] — the justification record for tier assignments
- [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]] — source framework for the principle; it supplies no tier scheme
- [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]] — §3.2, source of the action-risk tiers

## Notes

[^aix-escape]: [OWASP AI Exchange — Agent escape](https://owaspai.org/go/agentescape/), retrieved 2026-08-18.
[^aix-oversight]: [OWASP AI Exchange — OVERSIGHT](https://owaspai.org/go/oversight/), retrieved 2026-08-19. The risk-tier classification and the four oversight requirements: fully autonomous execution, soft confirmation, hard confirmation, and mandatory human initiation.
[^aix-leastmodelpriv]: [OWASP AI Exchange — LEAST MODEL PRIVILEGE](https://owaspai.org/go/leastmodelprivilege/), retrieved 2026-08-19. Dynamic permission scoping on risk elevation and permission escalation as an explicit logged event.

<!-- sources:auto -->
## Sources

- [billdx.github.io](https://billdx.github.io/Presentations/Securing%20Your%20Agents/securing-ai-agentic-apps.html)
- [Scaling Agentic AI: A Leadership Guide for CIOs](https://stream.stream-ext.bizzabo.com/U00liQQ00l2Th5l5Bkc302pY02k01IzHU8P3OqHaCDwYzvxw.m3u8)
- [The Agentic AI Security Scoping Matrix: A Framework for Securing Autonomous AI Systems](https://aws.amazon.com/blogs/security/the-agentic-ai-security-scoping-matrix-a-framework-for-securing-autonomous-ai-systems/)
<!-- /sources -->
