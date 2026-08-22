---
type: concept
title: "Context-Aware Trimming for Security Continuity"
created: 2026-05-03
updated: 2026-06-23
tags:
  - concepts
  - agentic-ai
  - observability
  - context-window
  - security-continuity
  - log-analysis
status: seed
no_public_url: "wiki synthesis of a non-published talk; no single external canonical source"
complexity: intermediate
domain: agent-runtime-security
aliases:
  - "context-aware trimming"
  - "security-event pinning"
  - "context window security continuity"
  - "priority-preserve context trimming"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agent-observability]]"
  - "[[prompt-injection-containment]]"
  - "[[indirect-prompt-injection]]"
  - "[[memory-poisoning]]"
  - "[[building-secure-agentic-systems-talk]]"
  - "[[nist-ai-800-4|NIST AI 800-4]]"
sources:
  - .raw/talks/2026-03-04_Brooks-McMillin_Building-Secure-Agentic-Systems_transcript.md
---

# Context-Aware Trimming for Security Continuity

## Definition

**Context-Aware Trimming** is the practice of tagging messages in an agent's context window by security significance so that, when context trimming occurs, security-relevant events (blocked attacks, permission denials, anomaly indicators) are preserved rather than dropped along with routine log volume.

## The attack it defends against

An agent performing long-running log analysis accumulates messages until its context window fills. Default trimming strategies (oldest-first, lowest-priority-first) will eventually drop older messages to make room for new ones.

This creates a **temporal blind spot**: an attacker who knows the context window size can send one attack event every `N` tokens. At any moment, the agent's context contains only the most recent attack instance — all prior instances have been trimmed away. The agent cannot detect a pattern across multiple attack attempts; each appears to be an isolated anomaly.

McMillin's framing:
> "I don't want an attacker to be able to just send an attack every 200,000 tokens, and then I only see one at a time."

## The fix

Tag incoming MCP response messages at the time they arrive according to their security significance. Examples of taggable events:
- `SSRF_BLOCKED` — server-side request forgery attempt blocked
- `PERMISSION_DENIED` — agent attempted an unauthorized action
- `INJECTION_DETECTED` — LLM firewall flagged a potential injection
- Custom security event types defined by the operator

The context trimmer then applies a **priority-preserve rule**: tagged security events are never eligible for trimming. General log volume continues to be trimmed on normal age/relevance criteria.

## Implementation notes

- Tagging happens at the MCP server layer, when the response is generated — not by the LLM, which could be manipulated
- Tags are metadata on messages, not injected into the message content (avoids polluting the agent's semantic context)
- The operator defines which event types are "pin-worthy"; this is a policy decision, not a model decision

## Limitations

> [!gap] Long-running scaling limit
> As security-pinned events accumulate over long agent runs, they too will eventually fill the context window. McMillin acknowledges this: "as the logs increase, context gets longer... the context can still fill up." The pattern needs extension for truly long-running agents. Candidate approaches:
> - Summarize older pinned events rather than preserving them verbatim
> - Maintain a queryable external security log store (Redis, ELK) separate from the in-context log, with the agent able to search it via MCP tool
> - Implement a rolling security-event digest that compresses N old events into a summary entry

## Relationship to other wiki concepts

- **[[indirect-prompt-injection|Indirect Prompt Injection]]**: the N-token attack pattern is a specialization of indirect injection — the attacker controls retrieval/log content to influence the agent
- **[[agent-observability|Agent Observability]]**: context-aware trimming is a sub-pattern of observability for long-running agents; security events must survive the agent's own context management to remain visible
- **[[memory-poisoning|Memory Poisoning]]**: distinct from context trimming, but both involve an attacker exploiting the temporal boundaries of the agent's information access
- **[[prompt-injection-containment|Prompt Injection Containment]]**: this pattern is a runtime-layer complement to input-side controls; it doesn't prevent injection but ensures injection attempts remain visible across long runs
- **[[nist-ai-800-4|NIST AI 800-4]]**: keeping security-significant events visible through trimming preserves the post-deployment monitoring signal whose loss the report frames as a barrier; a dropped attack event is a self-inflicted instance of the visibility gap

## Primary source

[[building-secure-agentic-systems-talk|Building Secure Agentic Systems — Brooks McMillin]] (Unprompted, March 2026) — primary practitioner source with live demo and the motivating attack description.
