---
type: entity
entity_type: product
title: "Tenuo Warrant"
address: c-000135
created: 2026-05-03
updated: 2026-08-20
tags:
  - products
  - capabilities
  - authorization
  - delegation
  - tenuo
  - warrants
  - cryptographic-tokens
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
parent_org: "[[tenuo]]"
homepage: "https://tenuo.ai"
related:
  - "[[cedar|Cedar]]"
  - "[[capability-based-authorization]]"
  - "[[monotonic-attenuation]]"
  - "[[ambient-vs-derived-authority]]"
  - "[[tenuo]]"
  - "[[capability-based-authorization-talk]]"
  - "[[oversight-layer]]"
  - "[[plan-validate-execute]]"
  - "[[orchestration-hijacking]]"
sources:
  - "[[capability-based-authorization-talk]]"
  - "https://github.com/tenuo-ai/tenuo"
coined_by:
  - "[[tenuo]]"
---

# Tenuo Warrant

**Sources:** [Tenuo (homepage)](https://tenuo.ai) · [Tenuo core (repo)](https://github.com/tenuo-ai/tenuo)

The **Tenuo Warrant** is a cryptographic capability artifact for AI-agent authorization, specified by [[niki-aimable-niyikiza|Niki Aimable Niyikiza]] and shipped as the open-source primitive of [[tenuo|Tenuo]] (Rust core, Python bindings, GitHub `tenuo-ai/tenuo`). It is the first publicly-disclosed vendor-neutral capability primitive specifically targeting agent-runtime delegation. See [[capability-based-authorization-talk|the talk page]] for the full design rationale and demo.

## The six properties

| # | Property | Meaning |
|---|---|---|
| 1 | **Signed** | Issuer's cryptographic signature; can't be forged or modified. |
| 2 | **Scoped** | Specific tool, specific action, explicit constraints (paths, hosts, args). If a task isn't encoded in the warrant, the agent can't do it. |
| 3 | **Ephemeral** | Short TTL by design; expires with the task. The temporal scope of authority does not exceed the temporal scope of the task. |
| 4 | **Holder-bound** | Tied to the holder's key — proof-of-possession, not bearer-style. A stolen warrant without the private key is useless. (Distinguishes from OAuth bearer tokens.) |
| 5 | **Verifiable offline** | Local verification by any verifier; no central authorization-server round-trip. Verification is a function of the artifact + the request, nothing else. |
| 6 | **Delegation-aware** | Embeds the full delegation chain from the original minter through every intermediate agent. Verifier can check the chain locally. |

## Envelope vs body

A persistent point of confusion in Q&A: **the warrant body is not a secret**. It can travel in plain HTTP headers; it is the *artifact that the agent presents to the verifier*. The cryptographic envelope around it is what authenticates the request.

| Layer | What it carries | What the verifier checks |
|---|---|---|
| Body (warrant proper) | Capabilities + constraints + delegation chain | Constraint match against the requested action; chain integrity |
| Envelope | Signatures + holder's proof-of-possession | Signature chain back to the root key; PoP confirms the right agent is presenting it |

## Subtractive delegation

A child warrant `W₁` minted by an agent holding `W₀` must satisfy

```
W₁ ⊆ W₀
```

— capabilities, constraints, and TTL **can only shrink**. There is no way to widen scope at a delegation hop. This is the protocol-level invariant called **[[monotonic-attenuation|Monotonic Attenuation]]**. From the talk:

> "The mathematical principle is that the warrant of the child agent can never exceed the scope of the parent agent. So we, it's the principle commonly known as monotonic attenuation. As you delegate further downstream in your multi-agent workflow, the capabilities of the agent can normally shrink."

The practical consequence: even if a sub-agent is fully compromised — by prompt injection, jailbreak, or supply-chain — it cannot exceed the scope it was granted. **The blast radius is frozen at the top of the chain.**

## Constraint language

Tenuo's constraint language supports:

- **Basic logical operations** (and / or / not / equality / set-membership)
- **Regex** for string matching
- **Glob** for path matching
- **CEL** (Common Expression Language) for arbitrary expressivity when the above don't suffice

Constraints can name MCP tool + argument structure directly when the agent calls through MCP, or can name HTTP host + path + arguments for raw API calls.

**Constraint design > cryptography.** Slide 11 of [[capability-based-authorization-talk|Niyikiza's talk]] reports a real-world rejection-rate fix from **87.5% to 100%** that was *not* a cryptographic fix — it was constraint-design. A regex-based path constraint is bypassed by `..` traversal; a host glob is bypassed by decimal-encoded IPs (`http://2852039166/`, which resolves to `169.254.169.254`) and URL userinfo (`http://127.0.0.1\@evil.com`, which resolves to `evil.com`). Real-world CVEs validate the failure mode: `CVE-2024-3571` (LangChain), `CVE-2025-3046` (LlamaIndex), `CVE-2025-61784` (LlamaFactory), `CVE-2025-66032` (Claude Code allowlist bypasses). The fix is the **annotated map** (constraint + normalization, e.g. `Subpath`, `UrlSafe`) and the **territory** (execution guard at the OS / sandbox level — `path_jail`, `url_jail`, OS sandbox).

## Cryptographic provenance + audit receipts

Two side-effects of the design:

1. **Provenance.** Every action produces a cryptographic receipt. Receipts can be replayed offline; auditors don't have to trust intermediate components.
2. **Tamper-evident logs.** Receipts are sent to a control plane and become cryptographically-verifiable audit logs of authorization events.

> "You don't have to trust anybody in between, because you have the cryptographic receipts of the action that took place and why it was authorized."

## Performance numbers (slide 10)

| Operation | Time | Notes |
|---|---|---|
| End-to-end authorization (constraints check + PoP check) | **~55 μs** | Rust core, CBOR encoding, Mac M3 Max |
| Denial fast-path | **~200 ns** | The common case |
| Constraint + cryptographic-integrity violations rejected | **53 / 53** | In-house violation set, **0 bypasses** across 5,700 fuzz probes |

## Deployment models

The same warrant artifact can be enforced at four points (slide 9):

| Mode | Where | Best for |
|---|---|---|
| **In-process** | Interceptor / middleware / drop-in node inside the framework | LangGraph, CrewAI, Temporal |
| **Sidecar** | Separate process, same host | Polyglot stacks |
| **Gateway** | Proxy between agents and tools | Fleet-wide enforcement |
| **MCP Proxy** | Inside the MCP tool server (client- or server-side) | Any MCP-compatible agent — easiest because MCP already has structured tool + arg schemas |

## Run-time scope determination

The talk's Q&A clarified two patterns for deciding sub-agent scope at run time:

1. **Orchestrator-driven**: the orchestrator agent decides which task to delegate; mints the child warrant scoped to that task. The top warrant scope is the absolute ceiling — sub-agents can never exceed it.
2. **Approval-gated**: certain action types require an external key-holder (real human) to approve at run time. Maps to enterprise SSO (Okta / Google ID) without losing cryptographic purity of the protocol.

## Interaction with existing IAM

Warrants do **not** replace OAuth / AWS IAM / GCP IAM. They sit *on top of* existing IAM:

```
agent action ──▶ Tenuo verifier (warrant check)
                       │ allowed?
                       ▼
                    cloud IAM (workload-identity check)
                       │ allowed?
                       ▼
                    actual API call
```

The warrant either deny-fails before workload-identity is consulted, or passes through and lets the cloud IAM do its existing job. The point is that workload identity is *task-blind* — it's scoped to the workload, which is "way wider than an individual task" — so warrants close the gap between workload-identity and per-task authority.

## YAML representation (illustrative)

The talk shows a YAML representation of a warrant. Indicative shape (extracted from talk + slide narration; not a formal spec):

```yaml
warrant:
  issuer: <root-key-id>
  holder: <agent-public-key>
  ttl_seconds: 600
  delegation_chain:
    - issuer: <root-key-id>
      to: <triage-agent-public-key>
      capabilities: [read_logs, send_http, read_config]
      constraints: { service: auth-svc }
    - issuer: <triage-agent-public-key>
      to: <investigation-agent-public-key>
      capabilities: [read_logs]
      constraints: { service: auth-svc, path_glob: "/var/log/auth-svc/*.log" }
  signatures: [...]
```

A live decoder is hosted at `tenuo.ai`'s playground; the open-source core is at `github.com/tenuo-ai/tenuo`.

## Boundaries

- **Not authentication** — see Envelope vs Body above.
- **Not anti-prompt-injection** — warrants don't stop injection from happening; they bound what the injected agent can do.
- **Not a policy engine** — warrants *carry* the policy, so there's no central PDP to consult; the artifact is self-evaluating.
- **Not a sandbox** — they constrain the *authorization* layer; the sandbox is the territory, the warrant is the map. Both are needed.
- **Not a validation gate** — a warrant answers whether the agent may hold the capability, not whether the specific call it is about to make matches the user's intent. [[plan-validate-execute|Plan-Validate-Execute]] names Tenuo Warrants as the capability-token layer sitting alongside its own deterministic Validate step, which checks the latter question at call time.

## See also

- [[capability-based-authorization|Capability-Based Authorization]] — broader concept and prior-art lineage
- [[monotonic-attenuation|Monotonic Attenuation]]
- [[ambient-vs-derived-authority|Ambient vs Derived Authority]]
- [[tenuo|Tenuo]]
- [[capability-based-authorization-talk|Niyikiza talk summary]]

- [[cedar|Cedar]] — Warrants can use Cedar as the constraint language, which is how the capability-token layer extends a policy engine rather than replacing it.
- [[orchestration-hijacking|Orchestration Hijacking]] — names Warrants as the tool-call-layer defense: a capability token constrains what an agent can request regardless of what a hijacked planner decided
