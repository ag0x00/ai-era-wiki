---
type: concept
title: "Orchestration Hijacking"
created: 2026-05-03
updated: 2026-08-20
origin: aggregated
tags:
  - concepts
  - agentic-ai
  - prompt-injection
  - mcp-security
  - threat-model
status: developing
scope_axis:
  - sec-of-ai
no_public_url: "Named formulation from an ingested Google/Unprompted talk; no stable primary-publisher URL (third-party recaps only)."
attributed_to: "Nicolas Lidzborski (Google), Unprompted March 2026"
related:
  - "[[indirect-prompt-injection]]"
  - "[[agency-gap]]"
  - "[[mcp-security]]"
  - "[[tool-poisoning]]"
  - "[[memory-poisoning]]"
  - "[[lethal-trifecta]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[securing-workspace-genai-at-google-talk]]"
  - "[[owasp-ai-exchange]]"
  - "[[agent-message-structure-manipulation]]"
  - "[[precize-agentic-ai-top10]]"
coined_by:
  - "[[google]]"
---

# Orchestration Hijacking

A class of attack against agentic systems in which the **orchestration layer** — the LLM (or LLM-driven planner) responsible for sequencing tool calls — is manipulated by adversarial content to plan attacker-favorable actions. Named in this form by [[nicolas-lidzborski|Nicolas Lidzborski]] (Google Workspace) at [[unprompted-conference-march-2026|Unprompted March 2026]].

The orchestration layer is the natural target for a sophisticated indirect-injection attack because manipulating *what the agent decides to do* is more powerful than manipulating *what a single tool call returns*. A successful orchestration hijack converts the LLM from a useful planner into a tool-call generator under attacker control — without compromising any individual tool.

## Distinction from primary-prompt injection

A typical [[indirect-prompt-injection|indirect prompt injection]] targets the model's response to the *current* prompt — for example, causing the model to leak data in its output, or to call a tool with attacker-chosen parameters in the same turn.

Orchestration hijacking is broader and often **delayed**:

- The injection enters the agent's context (memory, retrieved document, tool output) but does not act immediately
- It plants instructions that influence *future* planning decisions
- The actual malicious action may occur many turns later, after the user has long forgotten the entry point
- Triggers can be **time-based** (act on the first call after midnight UTC), **event-based** (act when a specific user query is observed), or **cascade-based** (act when another agent invokes this one)

This is why Lidzborski warns: *"You can have that injection inserted in the database, and a layer later, it's being executed."* The temporal decoupling makes attribution and incident response materially harder.

## Sub-patterns

### Planner manipulation
The injected content alters the agent's choice of tool, the sequence of calls, or the parameters passed. An agent that "should" call `search → summarize` is nudged into `search → exfiltrate → summarize`. The high-level task still appears to complete, masking the inserted step.

### Inter-agent communication hijacking
In multi-agent systems, a compromised agent can send adversarial messages to peer agents. The receiving agent — now planning based on adversarially shaped peer output — may take actions its principal would not have authorized. (Cross-reference: [[multi-agent-runtime-security|Multi-Agent Runtime Security]] cascade detection.)

### Privilege-mediated delegation

The [[owasp-ai-exchange|OWASP AI Exchange]] names a second multi-agent path in which no planner is subverted: a low-privileged agent is tricked into requesting a higher-privileged agent to act on its behalf.[^aix-pi] The receiving agent's reasoning is intact and its compliance is correct — it honours a well-formed request from a peer it has no reason to distrust — and the escalation comes from the privilege difference between the two. This is the confused-deputy pattern, and it differs from the sub-pattern above in what has to go wrong: there, the receiving agent's planning is shaped by adversarial content; here, only the requesting agent is compromised and the privilege boundary does the rest.

The distinction decides the control. Content-side defenses on the receiving agent see nothing, because nothing about the request is anomalous from its side. The Exchange's control is a signed delegation token validated across the full chain, with scope non-expansion, so a delegated request cannot carry authority its originator did not hold.[^aix-amsm] See [[agent-message-structure-manipulation|Agent Message Structure Manipulation]], which carries the full control set for the message fabric this request crosses. The same control follows from the Exchange's stated principle of no transitive trust between agents: A trusting B and B trusting C does not make C trusted by A.[^aix-agentic]

### Dormant trigger insertion
The attacker plants instructions that lie inert until a specific condition is met. The classic example: a malicious row inserted into a vector store that is retrieved (and therefore interpreted) only when a specific class of query is asked. The agent at retrieval time has no signal that this content is older or more suspicious than other context.

### Tool-call parameter coercion
The injection coerces the planner into calling an authorized tool with attacker-controlled parameters. The tool itself is uncompromised; the *use* of the tool is. This is the structural pattern behind many of the [[mcp-security|MCP CVE class]] exploits in Q1 2026 — the MCP server isn't necessarily vulnerable, but the agent calling it has been subverted into calling it with adversarial inputs.

## Relation to MCP and the [[lethal-trifecta|Lethal Trifecta]]

MCP makes orchestration hijacking more dangerous:

- The MCP context window is *much* larger than a single LLM prompt — more places to plant a dormant trigger
- MCP servers are often discovered dynamically, increasing the attack surface
- Many MCP server implementations process tool descriptions as free text, creating a [[tool-poisoning|tool-poisoning]] surface that compounds with orchestration hijacking

In the Lethal Trifecta framing, orchestration hijacking is the specific mechanism by which the trifecta becomes catastrophic. Without orchestration hijacking, sensitive private data + untrusted content + external comms is *capable* of being exfiltrated; with orchestration hijacking, the agent is *systematically directed* to do so.

## Defenses

The orchestration layer is not defendable from the inside — it shares the prompt-as-code vulnerability of any LLM. Defenses are structural:

| Layer | Defense |
|---|---|
| Input | Strip hidden content; scan for known-injection patterns; tag content with provenance ([[memory-poisoning\|memory poisoning defense]]) |
| Memory | Provenance attestation on retrieved content; integrity monitoring for scratchpad / state |
| Planner | Deterministic policy enforcement at every step (not just on initial prompt); state-aware FSM tracking risk level of accumulated context |
| Tool call | Capability tokens ([[tenuo-warrant\|Tenuo Warrants]]) constrain what the agent can request, regardless of what the planner decided |
| Delegation | Signed delegation tokens validated across the full chain, with scope non-expansion, so a relayed request cannot widen its starting authority ([[owasp-ai-exchange\|OWASP AI Exchange]])[^aix-amsm] |
| Egress | [[agentgateway\|AgentGateway]] / tool-policy enforcement at the broker level |
| Observability | Behavioral drift detection on planner decisions; per-agent baseline of tool-call patterns |

The deepest structural mitigation is **channel separation** ([[camel-pattern|CaMeL]]) — the privileged LLM that does planning never sees untrusted content directly, eliminating the injection path into the planner.

## Cross-references

- [[precize-agentic-ai-top10|Precize Top 10 for Agentic AI Vulnerability]] — names this surface AAI007 (Agent Orchestration and Multi-Agent Exploitation), splitting it into inter-agent communication exploitation, trust-relationship abuse, and coordination-protocol manipulation, a finer three-way split of the same sub-patterns catalogued above
- [[indirect-prompt-injection|Indirect prompt injection]] — the input vector
- [[memory-poisoning|Memory poisoning]] — the durability mechanism that enables delayed/dormant triggers
- [[tool-poisoning|Tool poisoning]] — the supply-chain twin (compromise the tools rather than the planner that calls them)
- [[agency-gap|Agency gap]] — the *non-adversarial* counterpart (the planner picks a wrong action without external manipulation)
- [[multi-agent-runtime-security|Multi-Agent Runtime Security]] — cascade detection across hijacked planners
- [[plan-validate-execute|Plan-Validate-Execute]] — the structural pattern that interposes validation between planning and execution
- [[agent-message-structure-manipulation|Agent Message Structure Manipulation]] — the message-fabric threat that reaches the same systems through protocol fields rather than through the planner

## Notes

[^aix-pi]: [OWASP AI Exchange — Prompt injection](https://owaspai.org/go/promptinjection/), retrieved 2026-08-18. Multi-agent propagation, in which a low-privileged agent is tricked into requesting a higher-privileged agent to perform an action on its behalf.
[^aix-amsm]: [OWASP AI Exchange — Agent message structure manipulation](https://owaspai.org/go/agentmessagestructuremanipulation/), retrieved 2026-08-18. Signed delegation tokens with full-chain validation and scope non-expansion; deny-by-default schema validation at tool and message boundaries.
[^aix-agentic]: [OWASP AI Exchange — Agentic AI overview](https://owaspai.org/go/agenticaioverview/), retrieved 2026-08-17. No transitive trust between agents, and enforcement of inter-agent security at the message bus or orchestrator rather than in agent prompts. Carried on [[owasp-ai-exchange|OWASP AI Exchange]].
