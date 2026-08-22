---
type: concept
title: "Agent Message Structure Manipulation"
address: c-000299
created: 2026-08-18
updated: 2026-08-18
tags:
  - concepts
  - agentic-ai
  - multi-agent
  - prompt-injection
  - mcp-security
  - threat-model
status: developing
origin: aggregated
scope_axis:
  - sec-of-ai
source_url: "https://owaspai.org/go/agentmessagestructuremanipulation/"
related:
  - "[[owasp-ai-exchange]]"
  - "[[orchestration-hijacking]]"
  - "[[indirect-prompt-injection]]"
  - "[[multi-agent-runtime-security]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[a2a-protocol]]"
  - "[[prompt-injection]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[prompt-injection-containment]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[csa-maestro]]"
  - "[[mitre-atlas]]"
sources:
  - "[[.raw/papers/owasp-ai-exchange-threats-through-use-2026-08-18.md]]"
  - "https://owaspai.org/go/agentmessagestructuremanipulation/"
---

# Agent Message Structure Manipulation

An attacker forges, replays, or alters the structured messages passing between agents, tools, and orchestration layers, changing task parameters, tool arguments, routing metadata, conversation state, or schema fields, so that a downstream component executes an unintended action.[^aix-amsm] The [[owasp-ai-exchange|OWASP AI Exchange]] gives the threat its own entry and permalink and classifies it as an input threat.[^aix-amsm]

## Distinction from indirect prompt injection

The Exchange draws the separation by target. [[indirect-prompt-injection|Indirect prompt injection]] smuggles instructions inside untrusted *text content* that an application inserts into a prompt. This threat targets the *message fabric* itself — protocol fields, envelopes, and delegation chains — and the Exchange states that the attack rests on those fields rather than on natural-language instructions alone.[^aix-amsm]

That leaves a detection gap. One of the Exchange's four examples carries the attack in poisoned structured metadata with no classic prompt-injection prose in it,[^aix-amsm] and a text-pattern classifier tuned for injection phrasing has nothing to match on in such a payload. A message that passes every content filter and every channel authentication check can still bind a tool call to an attacker-chosen file path.

The Exchange states that the threat reaches **single agentic flows** as well as multi-agent systems.[^aix-amsm] A retrieval-augmented agent treating tool output or its own planner steps as trusted structured input is exposed with no second agent in the system.

## Worked examples

The Exchange gives four, spanning multi-agent and single-agent shapes:[^aix-amsm]

1. **Agent impersonation on a weakly authenticated channel.** The attacker injects forged messages that change the arguments of a downstream tool call.
2. **A malicious tool response.** The response is valid JSON with manipulated field values — task ID, recipient, file path — and the orchestrator treats it as ground truth. The planner functions correctly throughout; the failure is data integrity.
3. **Single-agent RAG pipeline.** Poisoned structured metadata in a retrieved document chunk alters routing and parameter binding, with no second agent and no injection prose involved.
4. **LLM-to-LLM prompt infection.** One corrupted message propagates through a conversation graph over multiple hops.

## Impact

Four stated impacts, reachable even where every visible user prompt is benign:[^aix-amsm]

- Goal hijacking — the agent's objective is redirected.
- Privilege escalation through confused-deputy behaviour, where a component acts on its own authority for a request it should not have honoured.
- Cascading misinformation across agents.
- Unauthorized tool actions.

## Controls

Three of the Exchange's named controls are specific to the message fabric, and one is a framing rule that governs all of them.[^aix-amsm] The entry also routes to general input-threat and least-privilege controls specified in sections this wiki has not yet summarized.

| Control | What it establishes |
|---|---|
| Channel integrity | Message signing, mutual TLS, and replay protection, so a message's origin and freshness are verifiable |
| Signed delegation tokens | Validated across the full chain, with scope non-expansion, so a relayed request cannot widen the authority it started with |
| Deny-by-default schema validation | Applied at tool and message boundaries, so a field outside the declared schema is rejected rather than interpreted |

The framing rule is that peer-agent, tool, and orchestrator messages are untrusted input, **including inside single-agent tool loops**.[^aix-amsm] That extends the untrusted-input treatment past the multi-agent setting where it is usually applied, and it is the rule that makes example 3 above tractable.

Channel integrity and delegation-token validation answer different questions. Signing and mutual TLS establish who sent a message; scope non-expansion establishes whether the sender held the authority the message claims. A signed message from an authenticated peer can still carry a delegated request wider than its originator's rights, which is the case the second control exists for.

## The multi-agent layer

The Exchange attaches a separate control note at the multi-agent layer, and it carries a claim made nowhere else in the document's prompt-injection material. Individual agent controls such as access control are named necessary and not sufficient, on the stated ground that emergent collective behaviour can violate policy even where each agent complies in isolation.[^aix-amsm]

Per-agent compliance is therefore not evidence of system compliance. The wiki's [[agentic-ai-threat-classes-2026|Threat Classes 2026]] Class 3 argues the same property from one reconstructed incident and from reasoning about shared media; the Exchange states it as a general position.

## Ownership across neighbouring pages

Four wiki pages hold adjacent material, and the division is deliberate.

| Page | What it owns |
|---|---|
| [[orchestration-hijacking\|Orchestration Hijacking]] | Planner subversion through content, and the privilege-mediated delegation confused deputy |
| This page | Message-fabric integrity: forged, replayed, and altered protocol fields |
| [[multi-agent-runtime-security\|Multi-Agent Runtime Security]] | Detection, containment doctrine, and cross-agent forensics |
| [[prompt-injection-containment\|Prompt Injection Containment]] | The containment stack and the Exchange's seven layers of protection |

The [[agentic-ai-security-reference-architecture|reference architecture]]'s Egress plane holds the channel-securing controls — A2A signed Agent Cards, inter-agent path blocking, MCP runtime authorization — and records the content-validation half as an unfilled gap, since no listed reference implementation emits evidence for full-chain delegation scope or boundary schema validation.

## Limits

- **Schema validation bounds the shape of a field and not its truth.** Example 2 above passes schema validation, because a manipulated recipient or file path is a well-formed value of the declared type. Deny-by-default schema validation removes the unexpected-field class and leaves the plausible-wrong-value class.
- **Replay protection depends on the orchestrator holding message state.** An orchestrator that treats each tool response independently has no window against which to judge freshness.
- **The Exchange states no detection method for the single-agent case.** Example 3 arrives through the same retrieval path the application is built on, and the controls named above sit at message boundaries an in-process RAG pipeline may not have.

> [!gap] Single-source extraction
> This page rests on one primary source: the OWASP AI Exchange's threat entry.[^aix-amsm] No other taxonomy the wiki tracks names message-structure manipulation as a distinct threat category. The nearest is [[owasp-agentic-ai-top-10|ASI07]], insecure inter-agent communication, which covers interception, spoofing, replay, and downgrade on agent-to-agent channels and is scoped to those channels, so it reaches neither the orchestrator's routing metadata nor the single-agent tool loop this entry includes. [[csa-maestro|CSA MAESTRO]] and [[mitre-atlas|MITRE ATLAS]] carry no named equivalent. The examples, impacts, and controls above are the Exchange's, and the corroboration a second primary would supply does not yet exist. Independent evidence of exploitation in the wild is also absent: the Exchange's four examples are constructed rather than incident-derived.

## Cross-references

- [[owasp-ai-exchange|OWASP AI Exchange]] — the source taxonomy; §Document 2 carries the surrounding prompt-injection material
- [[indirect-prompt-injection|Indirect prompt injection]] — the sibling threat, separated by target
- [[orchestration-hijacking|Orchestration hijacking]] — planner-side manipulation and the delegation confused deputy
- [[multi-agent-runtime-security|Multi-Agent Runtime Security]] — detection and forensics, where message authenticity is a distinct requirement from log integrity
- [[agentic-ai-security-reference-architecture|Agentic AI Security RA]] §Egress plane — the controls and gap 9
- [[a2a-protocol|A2A Protocol]] — the inter-agent transport whose signing covers the channel and not the content
- [[prompt-injection|Prompt injection]] — the parent concept

## Notes

[^aix-amsm]: [OWASP AI Exchange — Agent message structure manipulation](https://owaspai.org/go/agentmessagestructuremanipulation/), retrieved 2026-08-18. The threat definition and manipulated field set; the input-threat category line; the stated distinction from indirect prompt injection by target; the statement that the threat applies to single agentic flows as well as multi-agent systems; the four examples; the impact set; channel integrity through signing, mutual TLS, and replay protection; signed delegation tokens with full-chain validation and scope non-expansion; deny-by-default schema validation at tool and message boundaries; the untrusted-input framing including single-agent tool loops; and the multi-agent layer note on emergent collective behaviour.
