---
type: concept
title: "Ambient vs Derived Authority"
created: 2026-05-03
updated: 2026-06-23
tags:
  - concepts
  - authorization
  - capabilities
  - identity
  - confused-deputy
  - multi-agent-security
status: developing
no_public_url: "wiki analytical framing synthesized from a non-published talk; no single external canonical source"
related:
  - "[[capability-based-authorization]]"
  - "[[tenuo-warrant]]"
  - "[[monotonic-attenuation]]"
  - "[[least-agency-principle]]"
  - "[[oversight-layer]]"
  - "[[capability-based-authorization-talk]]"
sources:
  - "[[capability-based-authorization-talk]]"
---

# Ambient vs Derived Authority

The structural distinction at the root of [[capability-based-authorization|capability-based authorization]]. Names two different answers to the question *"why is this agent allowed to do this thing?"*

| | Ambient Authority | Derived Authority |
|---|---|---|
| Source of permission | Identity (who is the agent) | Task (what is being done, by whom delegated) |
| Where the policy lives | External — in a PDP, IAM, role/group | In the artifact itself — the capability |
| Decision shape | "Lookup this principal's role; check role-policy" | "Verify the capability presented; check it permits this action" |
| Composes through delegation? | No — when A hands work to B, B operates with B's identity, not A's narrowed authority | Yes — B receives a *narrower-than-A* capability and is bounded by it |
| Failure mode | **Confused deputy** — agent has more authority than current task needs | Constrained by the artifact, even when the agent is wrong about what it should do |

## Consequences for agents

Microservices have predictable identity-and-call-graph: when a service starts, you know what it will do, and authority can be inferred from identity. **Agents are different.** They reason at run time, take untrusted inputs as a normal part of operation, and can spawn sub-agents in ways unknowable at deploy time.

Identity-based (ambient) authority gives the agent its full set of credentials at deploy time and trusts run-time decision-making to stay in the lines. From [[capability-based-authorization-talk|Niyikiza, March 2026]]:

> "Agents act with full authority whether the action was *intended, hallucinated, or injected*. Identity-based auth treats all three the same."

That's the AI confused deputy: the agent has the keys, the task hasn't been expressed anywhere, so the system has no basis to refuse.

## The valet-key analogy

Slide 3 of [[capability-based-authorization-talk|Niyikiza's talk]] frames the structural choice as a hand-off problem:

| Option A — The Master Key (ambient) | Option B — The Valet Key (derived) |
|---|---|
| Check the badge. Hand over your full key. | Hand over a key that starts one car, caps speed, disables the trunk and glove box, and dies in 2 hours. |
| Starts the engine, opens trunk + glove box, works until the shift ends. *Hope the valet only uses what they need.* | You don't need to trust the valet. **The key is the policy.** |
| Identity determines access. The key doesn't know what the task is. | Task determines access. Each handoff can only narrow what was given. |

The talk's load-bearing claim: every major agent framework today ships Option A. Agents deserve a valet key.

## Placement of each model in the stack

| Layer | Ambient model | Derived model |
|---|---|---|
| Identity | [[non-human-identity\|Workload identity]] (k8s ServiceAccount, OAuth client, AWS IAM Role) — the *who* | Same workload identity continues to authenticate the agent for transport — but the *authorization decision* shifts to the warrant |
| Policy | Centralized PDP queried per request (Cedar, OPA, Kyverno) | The capability artifact is self-evaluating; verifier is local |
| Delegation | Implicit (agent inherits its workload's full perm set) | Explicit (parent mints child capability; chain visible in artifact) |
| Audit | Inferred from logs across services | Cryptographic receipts emitted at every enforcement point |

Both models can coexist in the same architecture; warrants typically sit *on top of* existing IAM (see [[tenuo-warrant|Tenuo Warrant]] §"How it stacks with existing IAM").

## Boundaries of the distinction

- **Not anti-IAM.** Derived-authority architectures keep workload identity for authentication and use it as the substrate for *who can mint a warrant*. The shift is in *what gates the action*, not who the agent is.
- **Not deny-by-default vs allow-by-default.** Both models can be deny-by-default. Ambient-deny-by-default still suffers the confused-deputy problem because the question being decided is the wrong one.
- **Not a model-layer guarantee.** Derived authority binds run-time *actions*, not the model's internal reasoning. The model can still be jailbroken; the warrant just stops the *consequence* from landing.

## Basis for the "confused deputy" name

The classic confused-deputy problem (Hardy 1988) is a process invoked with high authority that performs an action requested by a less-authorized caller, treating the caller's request as if it were the process's own. AI agents reproduce this exactly: the agent has high authority (its workload identity), receives a request that may be from a user, may be from injected content, may be hallucinated, and acts with its own authority on whichever interpretation it landed on. **Ambient authority is what makes that mismatch unrecoverable.** Derived authority makes it recoverable: the warrant says what the *current task* is; if the action doesn't match, the verifier denies, regardless of how the agent got the idea.

## See also

- [[capability-based-authorization|Capability-Based Authorization]] — the family of primitives that operationalize derived authority
- [[tenuo-warrant|Tenuo Warrant]] — concrete six-property primitive
- [[monotonic-attenuation|Monotonic Attenuation]] — the invariant that makes derived authority survive multi-hop delegation
- [[least-agency-principle|Least Agency Principle]] — design principle aligned with derived authority
- [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] — when to centralize the decision (ambient + PDP) vs distribute it (derived in artifact)
