---
type: concept
title: "Monotonic Attenuation"
created: 2026-05-03
updated: 2026-08-21
tags:
  - concepts
  - capabilities
  - delegation
  - multi-agent-security
  - authorization
status: developing
no_public_url: "Coined formulation from an ingested conference talk; no external canonical publication."
related:
  - "[[lethal-trifecta|Lethal Trifecta]]"
  - "[[capability-based-authorization]]"
  - "[[tenuo-warrant]]"
  - "[[ambient-vs-derived-authority]]"
  - "[[agent-identity-architecture|AI Agent Identity Architecture]]"
  - "[[least-agency-principle]]"
  - "[[capability-based-authorization-talk]]"
sources:
  - "[[capability-based-authorization-talk]]"
---

# Monotonic Attenuation

The protocol-level invariant of capability-based delegation: **a child capability is always a subset of its parent**. Capabilities, constraints, and TTL **can only shrink** at every delegation hop. There is no operation in the protocol that *widens* scope.

```
W₂ ⊆ W₁ ⊆ W₀
```

Synonyms: **subtractive delegation** (slide 7 of [[capability-based-authorization-talk|Niyikiza's talk]]); **caveat-based attenuation** (Macaroons); **delegation chain restriction** (UCAN); **capability narrowing** (object-capability literature).

## Significance

The whole point of capability-based authorization is that authority flows *downstream from a task*, not laterally between identities. If delegation could widen scope, the system would be back to ambient authority by another name — a child agent could turn its narrow grant into something broader. Monotonic attenuation is the invariant that prevents this.

The practical consequence stated by [[capability-based-authorization-talk|Niyikiza]]:

> "Even if a sub-agent is fully compromised, it cannot exceed what it was granted. **The blast radius is frozen.**"

This is the security property that lets a multi-agent flow be reasoned about as a unit. Whoever issues the top warrant `W₀` knows the *whole* downstream tree is bounded by `W₀`, regardless of how many sub-agents spawn or what they get prompted with.

## Guarantees of the invariant

Given a delegation chain `W₀ ▶ W₁ ▶ W₂ ▶ … ▶ Wₙ`, for any action `a`:

```
allowed(Wₙ, a) ⇒ allowed(W₀, a)
```

The contrapositive is the operationally useful form: if the top warrant doesn't permit `a`, no sub-agent in the chain permits `a` either. The verifier checks the whole chain locally; it never has to walk back to the root issuer.

## Limits of the invariant

- **It does not bound which sub-agent acts.** If `Wₙ` is broader than is strictly necessary at hop `n`, the misuse is contained by `W₀` but not by `Wₙ` itself. Sub-agent scope determination is an upstream design problem (orchestrator-driven or approval-gated, per the [[capability-based-authorization-talk|Niyikiza Q&A]]).
- **It does not bound the orchestrator.** If the top-level orchestrator is compromised, it can mint child warrants right up to its own ceiling. Monotonic attenuation contains compromise *below* the orchestrator, not at it. Above the orchestrator: human-in-the-loop or hardware roots of trust.
- **It does not solve constraint design.** A constraint that says "path matches `/data/*`" doesn't actually stop `/data/../etc/passwd` from resolving to `/etc/passwd`. See [[capability-based-authorization-talk|the talk]] §"Map vs Territory".

## Diagram (slide 7)

```
Orchestrator (W₀)
      │
      ▼
   Agent A (W₁ ⊆ W₀)
      │
      ▼
   Agent B (W₂ ⊆ W₁ ⊆ W₀)
      │
      ▼
  Tool / API
   (Money · Data · Infra)
   Tool boundary
```

Delegation history is embedded in the artifact and verified locally. This gives **cryptographic provenance** as a side-effect: any verifier can reconstruct who-delegated-what-to-whom from the warrant alone.

## Prior art (and where Tenuo fits)

| Source | Attenuation operator |
|---|---|
| Macaroons (Google, 2014) | Caveats — append-only restrictions to a bearer token |
| UCAN | Delegation chains where each successor proves derivation from a predecessor |
| Biscuits | Datalog-typed capability tokens with chained restrictions |
| Tenuo Warrants ([[capability-based-authorization-talk\|Niyikiza, March 2026]]) | Six-property warrant where (delegation-aware + holder-bound) compose into the full subtractive-delegation guarantee |

None of these is platform-native. The [[agent-identity-architecture|AI Agent Identity Architecture]] places the capability-token layer at the leading edge of enterprise agent identity: hyperscaler identity products issue per-*resource* (audience) tokens rather than per-*task* holder-bound grants, so a deployment that wants this invariant assembles it from the vendor-neutral warrant layer sitting above the platform identity stack.

## See also

- [[capability-based-authorization|Capability-Based Authorization]]
- [[tenuo-warrant|Tenuo Warrant]]
- [[ambient-vs-derived-authority|Ambient vs Derived Authority]]
- [[least-agency-principle|Least Agency Principle]] — design goal that monotonic attenuation operationalizes for multi-agent systems

[[lethal-trifecta|The lethal trifecta]] treats this as the action-layer answer to the case where an agent must hold all three legs. Attenuation does not remove a leg; it deterministically constrains what the agent can do while holding them, which is why it is listed as a containment control rather than a way of breaking the trifecta.
