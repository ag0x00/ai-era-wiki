---
type: practice
title: "Plan-Validate-Execute Pattern"
created: 2026-05-03
updated: 2026-08-19
tags:
  - practices
  - hitl
  - control-plane
  - agentic-ai
  - structural-defense
status: developing
scope_axis:
  - sec-of-ai
attributed_to: "Google Workspace, presented by Nicolas Lidzborski at Unprompted March 2026"
related:
  - "[[hitl]]"
  - "[[least-agency-principle]]"
  - "[[agency-gap]]"
  - "[[orchestration-hijacking]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[owasp-ai-exchange]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[securing-workspace-genai-at-google-talk]]"
  - "[[guard-canonicalization-gap]]"
  - "[[guardfall-shell-injection-audit]]"
---

# Plan-validate-execute pattern

> Source: [[securing-workspace-genai-at-google-talk|Securing Workspace GenAI at Google]] (Unprompted 2026, Nicolas Lidzborski). [Transcript share](https://drive.google.com/file/d/1OgJBaHE6NfJvnangzg2DwobcQQqB2Zq2/view).

## On this page
- [The three stages](#the-three-stages)
- [Structural distinction from "ask before doing"](#structural-distinction-from-ask-before-doing)
- [Review fatigue](#review-fatigue)
- [Implementation surface across the reference architecture](#implementation-surface-across-the-reference-architecture)
- [CMM positioning](#cmm-positioning)
- [Cross-references](#cross-references)

A structural pattern for handling high-stakes irreversible actions in agentic systems. Instead of letting the agent execute autonomously, the action sequence splits into three stages with a deterministic gate between planning and execution. [[nicolas-lidzborski|Nicolas Lidzborski]] presented Google Workspace's implementation at [[unprompted-conference-march-2026|Unprompted March 2026]].

The pattern operationalizes [[hitl|HITL]] for irreversible writes (moves, deletes, sends, sharing changes), where post-hoc detection arrives too late.

## The three stages

### 1. Plan
The agent enumerates what it intends to do before any execution begins. The plan is a structured artifact: action class, target resource, parameters, source of intent. It is not free-form natural language. It is the input to the validation gate.

The structured-plan requirement matters. Free-form intent ("I'll email John about the contract") is not validatable. A structured plan ("send_email; recipient=user_id_42; subject=...; attached_doc_id=...; intent_source=user_query_at_t1") is validatable against policy and against what the user actually asked for.

### 2. Validate
A gatekeeper evaluates the planned action against:

- **Dynamically generated policy**: derived from session state, accumulated risk level, data sensitivity classifications, and user history
- **User intent**: does the planned action match what the user asked for? Cross-reference against [[agency-gap|agency-gap]] failure modes (wrong recipient, wrong target document, wrong scope)
- **Pre-defined deny rules**: actions unconditionally forbidden in the current context (for example, an outbound web request after accessing high-sensitivity data)
- **Semantic tool validation**: where the tool supports a dry run, a proposed state change computed before execution shows what the call would actually do; deterministic guardrails then evaluate the parsed impact against cumulative session limits rather than per-call limits alone — the check that catches a sequence of individually permissible steps aggregating past a threshold[^aix-oversight]

Three outcomes:

| Outcome | Action |
|---|---|
| Pass | Proceed to Execute |
| Confirm | Block until a human approves; surface the structured plan and reasoning to the user |
| Block | Refuse the action; log the attempt; surface the reason to the user |

The Block path applies when a contextual security framework detects systemic failure, such as rapid-fire sensitive actions or scope-escalation patterns.

### 3. Execute
Only after the gate passes does the agent invoke the underlying tool. Execution is mechanical. The policy decision is already made, and the tool call is constrained to the parameters in the validated plan.

## Structural distinction from "ask before doing"

Naive HITL implementations interleave the model's reasoning with confirmation prompts. The model decides what to do, asks the user, then proceeds on the reply. Plan-validate-execute differs in three load-bearing ways:

1. **The gate is deterministic.** A non-LLM policy engine renders the permit or deny, which is what breaks the [[recursive-prompt-injection|recursive-injection]] failure mode in which a reviewing model inherits the injection that compromised the model it reviews. Model-based checks may feed the gate without becoming it: the [[owasp-ai-exchange|OWASP AI Exchange]] specifies an independent adversarial judge drawn from a different model family, comparing the user's stated intent against the proposed parameters, running alongside deterministic guardrails on parsed impact rather than in place of them.[^aix-oversight] The family separation is the load-bearing part — a judge sharing the primary model's family and context inherits its susceptibilities and reproduces the failure the deterministic gate exists to prevent.
2. **The plan is visible before execution.** When prompted, the user sees the concrete action (recipient, subject, attached resources) rather than a model summary that may not match what the agent will do.
3. **The runtime enforces the gate; the model does not.** The model can propose; only the runtime can act. This is the platform-layer-over-prompt-layer principle in concrete form.

## Review fatigue

Lidzborski is explicit: *"There's still a lot of UX research to deal with review fatigue and rubber stamping. Like, first people will keep verifying things, then they'll just become approval bots."*

Plan-validate-execute does not solve review fatigue; it creates the surface where the problem becomes addressable. Mitigations under research:

- **Adaptive confirmation frequency**: confirm-tier the first time an action class runs in a session, then auto-tier identical actions within the same context
- **Risk-proportional UX**: heavyweight modals for high-impact actions, inline notifications for low-impact ones
- **Structured-plan summaries**: surface the delta from expected behavior rather than the full plan, so anomalies stand out
- **Post-hoc anomaly correlation**: even when users rubber-stamp, drift detection catches sustained anomaly patterns

None of these has a settled best practice as of 2026. The pattern is sound; the UX layer remains open.

The [[owasp-ai-exchange|OWASP AI Exchange]] names two of the same mitigations as requirements rather than research: tier risk and batch low-impact calls to guard against approval fatigue on repetitive tool approvals, and monitor reviewer behaviour with session-level approval rate limits that fire where a session's approval rate significantly exceeds its historical baseline.[^aix-oversight] It also constrains the review interface itself — the action requested, its stated rationale, relevant prior actions in the session, estimated impact, and a reversibility classification, with agent-generated urgency framing sanitised out.[^aix-oversight] The adaptive-confirmation-frequency mitigation listed above sits in tension with that last constraint and with the [[agentic-ai-security-cmm-d7-observability|D7]] requirement to detect silent degradation of human-in-the-loop steps: auto-tiering repeat actions within a session is the documented mechanism by which a low-risk approval later covers a high-impact action.

## Limit: the validator must see what the executor sees

The deterministic gatekeeper is only as sound as the equivalence between the plan it validates and the action that runs. Where the executor transforms its input before acting — a shell that expands and unquotes, a filesystem that resolves a symlink, a tool server that rewrites arguments behind a name — a validator reading the pre-transformation form is deciding about a different action than the one performed. [[guard-canonicalization-gap|The guard canonicalization gap]] names this failure, and [[guardfall-shell-injection-audit|the GuardFall audit]] found ten of eleven surveyed coding agents bypassable on exactly it.

This does not weaken the pattern; it specifies a precondition. Validate canonicalized actions, and place an isolation boundary beneath the gatekeeper so that a validation error is contained rather than executed.

## Implementation surface across the reference architecture

Plan-validate-execute concentrates in the Control plane of the [[agentic-ai-security-reference-architecture|reference architecture]] but touches multiple planes:

| Plane | Role |
|---|---|
| Control | Gatekeeper and policy engine; least-agency tier evaluation |
| Identity | Validates the agent identity issuing the plan; binds the plan to a human principal |
| Runtime | Lifecycle hook intercepts tool calls before execution |
| Egress | Tool-call broker enforces the validated plan parameters |
| Observability | Logs the plan, validation decision, and execution outcome for audit |

A reference implementation pairs:
- [[cedar|Cedar]] or [[opa|OPA]] for the policy engine
- [[hitl|HITL]] confirmation UX for the user-facing gate
- [[tenuo-warrant|Tenuo Warrants]] (where deployed) for the capability-token layer that constrains what the agent can request
- [[agentgateway|AgentGateway]] for tool-call brokering

## CMM positioning

In the [[agentic-ai-security-cmm-2026|CMM]]:

- **[[agentic-ai-security-cmm-d3-control-least-agency|D3 Control & Least-Agency]] L3+**: the pattern that takes a deployment from "guardrails exist" to "guardrails are structurally enforced"
- **[[agentic-ai-security-cmm-d4-runtime-guardrails|D4 Runtime & Guardrails]] L3**: operationalizes HITL for irreversible actions via the deterministic gate
- **[[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] L4**: the semantic-validation clause — proposed state change computed by dry run and the cross-family adversarial judge — that this pattern's Validate stage adds; D4's own maturity caveat records both as specifications with no named implementation, preview or otherwise, so a deployment assembling and evidencing them in-house — rather than a vendor status page — is what discharges this rung
- **[[agentic-ai-security-cmm-d7-observability|D7 Observability & Detection]] L3**: the plan, validation, and execution triple is high-value audit data

## Cross-references

- [[hitl|HITL]]: plan-validate-execute is the HITL implementation for the confirm tier
- [[breaking-the-lethal-trifecta-talk|Bullen's Stripe talk]]: Stripe's three-ring containment uses analogous gate-before-action mechanics on egress and tool policy
- [[agency-gap|Agency gap]]: the validation step catches non-adversarial agency-gap failures such as "wrong John" errors
- [[orchestration-hijacking|Orchestration hijacking]]: the deterministic policy engine in the validation step resists the prompt-injection attacks that compromise LLM-based reviewers

## Notes

[^aix-oversight]: [OWASP AI Exchange — OVERSIGHT](https://owaspai.org/go/oversight/), retrieved 2026-08-19. The simulate-before-execute specification: proposed state change, cross-family adversarial judge, deterministic guardrails with cumulative-session limits, and the approval-fatigue and review-interface requirements.
