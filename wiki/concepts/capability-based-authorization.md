---
type: concept
title: "Capability-Based Authorization"
created: 2026-05-03
updated: 2026-07-30
tags:
  - concepts
  - authorization
  - capabilities
  - delegation
  - multi-agent-security
  - prompt-injection-containment
status: developing
no_public_url: "classic CS concept (Dennis & Van Horn 1966) synthesized across many works; no single canonical external source for this page"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[tenuo-warrant]]"
  - "[[monotonic-attenuation]]"
  - "[[ambient-vs-derived-authority]]"
  - "[[oversight-layer]]"
  - "[[least-agency-principle]]"
  - "[[mcp-security]]"
  - "[[lethal-trifecta]]"
  - "[[capability-based-authorization-talk]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[guard-canonicalization-gap]]"
sources:
  - "[[capability-based-authorization-talk]]"
---

# Capability-Based Authorization

A family of authorization primitives in which **the artifact being passed around is the policy itself**, rather than a credential that names an identity whose policy lives elsewhere. A capability is an **unforgeable token** that names a specific resource + a specific set of permitted operations on that resource; possession of the token is what authorizes the action. There is no separate "look up this user's role and decide" step — the token *is* the decision.

In agentic AI, the relevant property is that capabilities **encode the task, not the agent**. When Agent A delegates work to Agent B, A mints a *narrower* capability for B and hands it over. B's authority is **derived** from the task, not from B's identity. This is the structural opposite of the **ambient authority** model that every major agent framework ships today (see [[ambient-vs-derived-authority|Ambient vs Derived Authority]]).

## Specific need for agents

Microservices have predictable call graphs and fixed identities; identity-based authorization composes cleanly because each service knows what it will and won't do. Agents are different: they reason at run time, take untrusted input as a normal part of operation, and can spawn sub-agents in ways unknown at deploy time. With identity-based auth, this produces the **AI confused deputy**: the agent has more authority than the current task needs, the task isn't expressed anywhere the system can check, so the system has no basis to refuse — *whether the action was intended, hallucinated, or injected* (see [[capability-based-authorization-talk|Niyikiza, March 2026]]).

Capabilities cut the knot: the *task* gets a token, the agent only ever holds task-tokens, and a sub-agent's token is *necessarily a subset* of its parent's (see [[monotonic-attenuation|Monotonic Attenuation]]).

## Sixty years of prior art

| Era | Work | Contribution |
|---|---|---|
| 1966 | Dennis & Van Horn — *Programming Semantics for Multiprogrammed Computations* | Original capability formulation; named the confused-deputy problem |
| 1970s–1990s | KeyKOS, EROS, Capability OS lineage | Operating-system-level capabilities; influenced sandbox design |
| 2014 | **Google Macaroons** | Bearer tokens with **caveats** — the attenuation primitive that everything since builds on |
| Late 2010s | **UCAN** (User-Controlled Authorization Networks) | Decentralized capability tokens with cryptographic delegation chains |
| Late 2010s | **Biscuits** | Cryptographic capability tokens with attenuation; CRDT-friendly |
| March 2025 | **Google DeepMind, *Defeating Prompt Injection by Design* ([[camel-pattern\|CaMeL]] paper)** | First published case for capability-based authorization as the model that addresses prompt injection. Privileged + Quarantined LLM (P-Q) split. |
| Feb 2026 | **DeepMind, *Intentional AI Design*** *(title best-effort from transcript; needs verification)* | Reiterates capabilities as the auth framework for multi-agent flows |
| March 2026 | [[tenuo-warrant\|Tenuo Warrant]] | First publicly-shipped vendor-neutral capability primitive specifically for AI agents — Rust core + Python bindings + 4 deployment models |

## Family resemblance — the six properties of an agent-grade capability

Capabilities used in agent contexts converge on roughly the same property set. The [[tenuo-warrant|Tenuo Warrant]] specification names six explicitly:

1. **Signed** by the issuer — cryptographic provenance, can't be forged.
2. **Scoped** to a specific tool / action / argument constraints.
3. **Ephemeral** — short TTL, expires with the task.
4. **Holder-bound** — proof-of-possession; theft of the token alone is not enough.
5. **Verifiable offline** — no central authorization server in the request path.
6. **Delegation-aware** — embeds the chain so attenuation is visible to verifiers.

Macaroons cover (1)–(3) and (6) with caveats but not (4) (originally bearer-style). UCAN and Biscuits add (4). Tenuo Warrants synthesize all six and are explicit about agent-runtime concerns the prior work didn't have to think about (LangGraph integration, MCP proxy mode, CEL constraint expressions).

## Placement in the agentic-AI security stack

Compared against the four-layer stack Niyikiza names (Identity / Policy Engines / Model Guardrails / Sandbox), capabilities are **the missing fifth layer**: delegation-aware authorization at execution time, between the policy engine (which can see identities but not delegation chains) and the sandbox (which is bounded but content-blind).

| Layer | Question it answers | Capability primitive's role |
|---|---|---|
| Identity | "Who is this agent?" | Capabilities sit *on top of* identity — they don't replace it. The agent still has a [[non-human-identity\|workload identity]]; the capability narrows what that identity can do *for this task*. |
| Policy engine | "Is this role allowed?" | Capabilities make the *task* explicit so the policy decision is local to the artifact, not a query to a central PDP. |
| Model guardrails | "Does this output look safe?" | Capabilities don't help here — they are deterministic execution-time controls, not content controls. |
| Sandbox | "Is this runtime isolated?" | Capabilities provide the *policy* that the sandbox can enforce; sandbox is the territory, capabilities are the map (see [[capability-based-authorization-talk\|Niyikiza]] §"Map vs Territory"). |
| **Capability layer** | **"Did this authority come from somewhere upstream, and has it narrowed at every hop?"** | The new layer. |

## Containment, not prevention

A persistent confusion in the discourse: capabilities are *not* an anti-[[prompt-injection|Prompt Injection]] measure. They do not stop a prompt injection from being injected. Their job is to **constrain what the agent can do at execution time, even when prompt-injected**. From [[capability-based-authorization-talk|Niyikiza]]:

> "We are not trying to be, like, there's not gonna be prompt injection… we are making sure that the authorization is frozen, even if your agent is prompt injected."

This is the same posture as [[breaking-the-lethal-trifecta-talk|Bullen's containment thesis]]: assume prompt injection will happen, constrain the blast radius. The two approaches stack — Bullen contains via egress and tool-annotation; Niyikiza contains via delegation-aware capability attenuation. A defense-in-depth program runs both.

## Relationship to other concepts

- **[[ambient-vs-derived-authority|Ambient vs Derived Authority]]** — the structural distinction capabilities operationalize. Ambient = identity-based. Derived = capability-based.
- **[[monotonic-attenuation|Monotonic Attenuation]]** — the mathematical invariant: W2 ⊆ W1 ⊆ W0 across delegation hops.
- **[[least-agency-principle|Least Agency Principle]]** — the design goal capabilities operationalize. Least agency *as a principle* + capabilities *as the mechanism*.
- **[[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]]** — capabilities can be the PDP+PEP at the same time, since the artifact carries the policy. Distinguishes Niyikiza-style architectures from [[toolshed|Toolshed]]-style architectures where the PDP is centralized.
- **[[mcp-security|MCP Security]]** — capabilities deploy naturally as an MCP-proxy interceptor (Tenuo's "MCP Proxy" deployment mode); MCP's structured tool + arg schema makes constraint expression easy.
- **[[lethal-trifecta|Lethal Trifecta]]** / **[[lethal-bifecta|Lethal Bifecta]]** — capabilities provide deterministic gates on the third leg (external comms / sensitive write). Strong fit for the trifecta-containment problem.

A capability system inherits the [[guard-canonicalization-gap|canonicalization]] precondition: an attenuated capability is only as narrow as the equivalence between the action it names and the action the executor performs. Where a capability authorizes a command string that a shell rewrites, or a tool name whose server rewrites its arguments, attenuation is nominal. This does not undercut the model — capabilities constrain what can be *requested*, which is a stronger position than inspecting what was requested — but the binding must be to a canonical action for monotonic attenuation to mean anything.

## Limitations and open questions

1. **Constraint-language design is the hard part, not cryptography.** [[capability-based-authorization-talk|Niyikiza's slide 11]] makes this load-bearing: a regex-based path constraint is bypassed by `..`, decimal-IP encoding, URL userinfo tricks. The fix is path/URL normalization (the "annotated map") and execution-guard sandboxes (the "territory"). This is the dominant failure mode in real CVE data — see CVE-2024-3571, CVE-2025-3046, CVE-2025-61784, CVE-2025-66032.
2. **Run-time scope determination.** When the orchestrator decides what task to delegate, the orchestrator itself becomes a trust point. If the orchestrator is compromised, it can mint child warrants right up to its own ceiling. Approval-gating (human-in-the-loop) at sensitive action types compensates but reintroduces UX friction.

[[standards-review-microsoft-zt4ai-2026-Q2|The 2026-Q2 ZT4AI review]] records per-task capability tokens as a falsifiable absence in the [[microsoft-zt4ai|Microsoft ZT4AI]] stack: Entra issues tokens scoped per agent identity (per resource/audience), not per task, leaving [[tenuo-warrant|Tenuo Warrant]] as the only shipping holder-bound primitive at the D2/D3 leading edge.
3. **No public benchmark.** The strongest validation result published — Tenuo's 90%→0% multi-agent ASR — is on a custom in-house harness. There is no shared benchmark for delegation-aware authorization. Open research community problem.
4. **Composability with model-layer defenses.** CaMeL's privileged/quarantined split is *model-layer*. Capabilities are *execution-layer*. They are not redundant in theory, but the engineering of the composition has not been published.

## See also

- [[capability-based-authorization-talk|Capability-Based Authorization for AI Agents (Niyikiza, Unprompted March 2026)]]
- [[tenuo-warrant|Tenuo Warrant]]
- [[tenuo|Tenuo]]
- [[pdp-pep-for-non-tool-mediated-actions|PDP/PEP for non-tool-mediated agent actions]]
- [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]]
