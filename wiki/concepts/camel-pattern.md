---
type: concept
title: "CaMeL Pattern (Compartmentalized Machine Learning)"
address: c-000300
created: 2026-05-03
updated: 2026-08-18
origin: aggregated
scope_axis:
  - sec-of-ai
tags:
  - concepts
  - prompt-injection
  - sandboxing
  - runtime
  - agentic-ai
status: stub
aliases:
  - "CaMeL"
  - "Compartmentalized Machine Learning"
  - "Privileged/Quarantined LLM split"
related:
  - "[[lethal-trifecta]]"
  - "[[indirect-prompt-injection]]"
  - "[[agent-sandboxing]]"
  - "[[firecracker]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[owasp-ai-exchange]]"
  - "[[prompt-injection-containment]]"
sources:
  - "https://arxiv.org/abs/2503.12599"
---

# CaMeL Pattern (Compartmentalized Machine Learning)

**CaMeL** (Compartmentalized Machine Learning) is a research-stage architectural pattern from Google DeepMind (March 2025) for defending agentic systems against [[prompt-injection|prompt injection]]. The core idea: **split the agent's LLM into two separate models** with different trust levels and information-flow rules between them.

## The split

```
┌─────────────────────────────┐
│  PRIVILEGED LLM             │  ← Receives user instructions (trusted)
│  (coordinates workflow)     │  ← Has access to high-trust tools
│  Sees: user intent only     │
└──────────────┬──────────────┘
               │ structured, stripped commands only
               ▼
┌─────────────────────────────┐
│  QUARANTINED LLM            │  ← Receives untrusted content (web, docs, email)
│  (executes retrieval tasks) │  ← Can only return structured data, no free text
│  Sees: retrieved content    │
└─────────────────────────────┘
```

The quarantined LLM handles all interaction with untrusted content (web pages, emails, documents, RAG retrievals). Its output channel is **restricted**: it can only return structured data (JSON, typed values) back to the privileged LLM, not free-form text that could carry injected instructions. The privileged LLM never directly processes untrusted content, so injected instructions cannot reach it.

The [[owasp-ai-exchange|OWASP AI Exchange]] names the mechanism rather than the topology: privilege-based data flow control, in which capability metadata is attached to values and the user's intent is converted into sandboxed code steps in place of unconstrained natural-language tool calls.[^aix-pi] Under that reading the control is the capability metadata carried on values, and the two-model split is one implementation of it. That is the distinction this page's research-status section reaches for when it states that two LLMs alone are not sufficient. What travels between the models is a value carrying its own provenance and permitted uses, and the enforcement acts on that metadata rather than on the text of a request. The Exchange states the underlying limit that makes the pattern necessary: prompt injection is not solvable at the model layer alone.[^aix-pi]

## Consequences for the lethal trifecta

The [[lethal-trifecta|Lethal Trifecta]] requires three conditions in one agent: (1) private data access, (2) untrusted content ingest, (3) external communication. CaMeL's split addresses leg 2 by preventing injected content from ever reaching the LLM that holds legs 1 and 3.

Compared to trifecta-splitting strategies (e.g., separate agents per trifecta leg), CaMeL:
- **Keeps the capability in one logical system**: the user gets the full agentic experience
- **Enforces the split at the model level**: not just at the tool-call or prompt-architecture level, which can be bypassed by clever injection
- **Allows formal analysis**: because information flows are typed and restricted, the quarantine boundary can be formally verified

## Research status

CaMeL was published in March 2025 (arXiv 2503.12599) by Google DeepMind. As of May 2026:
- **No shipped production implementation** exists outside Google's internal research environment
- **CMM D4 L5+ and D3 L5+** reference it as a research-stage primitive that qualifies for leading-edge credit only when run as a documented pilot with exit criteria
- The pattern requires significant prompt engineering and data-flow enforcement work; "just use two LLMs" is not sufficient. The structured output channel is the load-bearing control

The Exchange lists the pattern among five structural mitigations it recommends against agentic prompt injection, alongside capping the concurrent risk factors an agent holds and separating instructions from data.[^aix-pi] Recommended in a standards-liaison publication and unshipped in production are both current: the recommendation states the shape of the control, and no vendor supplies it.

## Comparison to alternative containment approaches

| Approach | Where it breaks the trifecta | Shipped? |
|---|---|---|
| Trifecta splitting (per-agent role) | Removes legs from individual agents | Yes (architectural guidance) |
| Egress filtering (Stripe approach) | Removes leg 3 (external comms) at network | Yes (Smokescreen) |
| Capability warrants (Tenuo approach) | Constrains what legs 1+3 can do per task | Yes (Tenuo OSS) |
| CaMeL (DeepMind) | Prevents injected content from reaching legs 1+3 | Research-stage only |
| HITL on sensitive actions | Inserts human into leg 3 path | Yes (architectural primitive) |

## In the RA / CMM

- **RA Runtime Plane:** CaMeL is listed as "Compartmentalized LLMs (CaMeL pattern)," classified as `Research`.
- **[[agentic-ai-security-cmm-d3-control-least-agency|CMM D3]] L5+:** "A CaMeL-style privileged/quarantined LLM split for trifecta-positive workloads (research, no shipping vendor)"; evidence requires a deployed pilot with documented exit criteria.
- **[[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4]] L5+:** "a CaMeL-style privileged/quarantined LLM split in production (research)" is one of three leading-edge primitives that can qualify for D4 L5+.

## See also

- [[lethal-trifecta|Lethal Trifecta]]: the structural condition CaMeL defends against
- [[indirect-prompt-injection|Indirect Prompt Injection]]: the attack that CaMeL constrains
- [[agent-sandboxing|Agent Sandboxing]]: OS-level complement; Firecracker can isolate the quarantined LLM process
- [[firecracker|Firecracker]]: the recommended OSS sandbox for isolating the quarantined LLM
- [[agentic-ai-security-reference-architecture|Agentic AI Security RA]] §Runtime plane
- [[prompt-injection-containment|Prompt Injection Containment]]: the containment stack CaMeL sits inside, and the Exchange's seven layers of protection

## Notes

[^aix-pi]: [OWASP AI Exchange — Prompt injection](https://owaspai.org/go/promptinjection/), retrieved 2026-08-18. Privilege-based data flow control identified with CaMeL — capability metadata attached to values, user intent converted to sandboxed code steps; the five structural mitigations for agentic prompt injection; the statement that prompt injection is not solvable at the model layer alone.

<!-- sources:auto -->
## Sources

- [arxiv.org](https://arxiv.org/abs/2503.12599)
<!-- /sources -->
