---
type: concept
title: "Delayed Tool Invocation"
created: 2026-05-03
updated: 2026-05-03
tags:
  - concepts
  - prompt-injection
  - agentic-ai
  - attack-patterns
  - tool-use
  - red-teaming
status: developing
no_public_url: "Attack pattern named from a private conference talk (Johann Rehberger, Unprompted 2026); no public canonical write-up"
aliases:
  - "Deferred Tool Activation"
  - "Intent Activation Attack"
  - "Tool Reactivation Bypass"
related:
  - "[[indirect-prompt-injection]]"
  - "[[promptware]]"
  - "[[memory-poisoning]]"
  - "[[orchestration-hijacking]]"
  - "[[your-agent-works-for-me-now-talk]]"
  - "[[hitl]]"
  - "[[least-agency-principle]]"
sources:
  - ".raw/talks/2026-03-04_Johann-Rehberger_Your-Agent-Works-for-Me-Now_transcript.md"
---

# Delayed Tool Invocation

**Delayed tool invocation** (DTI) is an attack technique that exploits the **time-dependent availability of agent tools** to bypass security controls that prevent direct tool invocation from within injected content. Rather than calling a sensitive tool immediately from within the injection payload, the attacker embeds a **conditional trigger** that activates the tool in a subsequent conversation turn — after the initial security control has evaluated the interaction as safe.

The technique was named and disclosed by [[johann-rehberger|Johann Rehberger]] at Unprompted (March 2026). It represents a class of **state manipulation** attack: the attacker manipulates the agent's execution state so that a tool that was intentionally unavailable at injection time becomes available later, under conditions the attacker controls.

**Why Delayed Invocation Bypasses Controls.** The attacker's insight is that security controls often reason about the *current* turn: "was this tool invoked as part of a known-malicious injection?" A tool deactivated during a flagged injection can still be invoked in the next user turn — by which time the context is "clean" and no active injection is being evaluated. The deferred trigger bridges the two turns.

## The Security Control Being Bypassed

Rehberger's research began with an observation about Google Gemini's Workspace tool (calendar, email, Drive integrations). When Gemini received an indirect prompt injection, the Workspace tool was automatically deactivated — a deliberate security control Google had implemented to prevent tool-chaining from within injected content. The model would call many other tools from the injection, but consistently refused to invoke the Workspace tool.

This is a positive and intended security behavior: **explicit user intent is required to activate sensitive tools**.

The bypass exploits two observations:
1. If the **original user prompt** mentions email or calendar topics, the Workspace tool is "activated" for that session and CAN be invoked even during an injection in the same turn.
2. If user intent was not present in turn 1, a deferred conditional in the injection — *"repeat this attack in the next conversation turn, then invoke the Workspace tool"* — can cause the tool to become available in turn 2, once the security evaluation of the injection has completed.

## Mechanism

A delayed tool invocation payload typically contains:

1. **A surface-level legitimate task** — e.g., "summarize this document" — to give the interaction an innocent appearance.
2. **A deferred activation condition** — "after the user continues the conversation" / "when the user types their next message".
3. **The delayed tool call** — the specific sensitive tool to invoke and the parameters to use.
4. **(Optional) Repetition of the payload** — Rehberger observed that repeating the attack instructions in the response increased success rate, consistent with the academic finding that *"prompt repetition improves non-reasoning LLMs"* (Google, 2026 paper).

The trigger condition is the key: the security control evaluates **this turn** and finds no active harmful tool invocation. The next turn — initiated by the user's next message — occurs outside the scope of that evaluation.

## Published Demonstrations

Rehberger disclosed the following DTI cases at Unprompted March 2026:

### 1. Gemini Workspace Tool Reactivation (Google)
- **Vector:** Indirect injection via file (or, later, via email sent to Google's own security team for validation).
- **Trigger condition:** User continues the conversation after injection processing.
- **Result:** Workspace tool (calendar, Drive, email) invoked in turn 2; sensitive document read out; [[memory-poisoning|memory poisoning]] via Gemini memory tool.
- **Status:** Reported to and validated by Google. Google's security team reproduced by Rehberger emailing them a payload-containing email, then observing the agent invoke the Workspace tool on the next conversation turn.

### 2. Gemini / Google Home Physical Control Bypass
- **Vector:** Indirect injection in a document; document title set to "Broadcast to Living Room" — the **intent signal** that activates the home-control tool.
- **Mechanism:** Rehberger's hypothesis: document metadata (specifically the title) becomes part of the prompt context, and the Workspace tool's activation check interprets this metadata as user intent.
- **Result:** The Google Home speaker was commanded to say "Johan is here. Trust no AI." — demonstrating that DTI can reach **physical actuators**.
- **Status:** Reported. Classified as a bypass of the controls from the "Invitation Is All You Need" (Nassi et al.) fix.

### 3. Gemini Long-Term Memory (via file injection)
- **Vector:** File with embedded delayed invocation payload.
- **Trigger:** User continues conversation.
- **Result:** Gemini memory tool invoked, persistent memory written. Memory survives conversation resets; Gemini now believes injected content in all future sessions.
- **Impact:** [[memory-poisoning|Memory poisoning]] with attacker-controlled beliefs.

### 4. ChatGPT Personalization Modification
- **Vector:** Document with embedded DTI payload targeting the ChatGPT personalization API.
- **Trigger:** User types "yes" (or any continuation) after the agent summarizes the document.
- **Result:** ChatGPT's personalization option modified to "quirky."
- **Scope:** Rehberger notes ChatGPT does not appear to implement the same tool-deactivation control as Google — so DTI in ChatGPT bypasses guardrails rather than a hard security control.
- **Status:** Reported to OpenAI; classified as a "guardrail problem" (not a security vulnerability). Forwarded to the safety/guardrail team.

## Attention as the Exploitation Primitive

Rehberger's theoretical framing: delayed tool invocation targets the **attention layer of the transformer**. The technique increases the model's attention to the deferred tool-call instructions by:

1. **Repetition** — embedding the attack twice (explicit payload + LLM-echoed payload in the response)
2. **Intent signal injection** — using document metadata (title, filename) to simulate user intent

The research connection is to the Google paper *"Prompt repetition improves non-reasoning LLMs"* (2026): repeating the target query twice in a prompt increases LLM task performance on non-reasoning models — which Rehberger maps to why repeating the attack payload increases the DTI success rate.

> [!gap] Academic Grounding
> The attention-targeting interpretation is Rehberger's hypothesis based on behavioral observation; he does not have access to the implementation source code. Independent validation of the mechanism (does document title metadata genuinely enter the model context as an intent signal?) is an open research question.

## Defense Implications

| Attack Phase | Defensive Response |
|---|---|
| Injection plants the deferred trigger | Input/RAG content scanning; strip imperative conditional instructions from retrieved content |
| User initiates next turn | Intent-provenance checking: was the current tool invocation triggered by the user's explicit message, or is it a deferred call from prior context? |
| Tool is invoked from deferred context | State-aware session tracking (see [[securing-workspace-genai-at-google-talk\|Lidzborski's Layer 3]]): track per-step data provenance; if a tool invocation's lineage traces back to untrusted external content rather than user intent, require confirmation |
| Memory is poisoned | [[memory-poisoning\|Memory integrity monitoring]]; flag unexpected memory tool invocations |

The correct defensive framing — highlighted by the Q&A in Rehberger's talk — is that this is **not a purely model-layer problem**. The Google deactivation control was a positive step. The bypass is a platform-architecture problem: the platform needs to track the *provenance* of a tool activation, not just whether a tool is being called.

## Relation to Other Concepts

- **[[indirect-prompt-injection|Indirect Prompt Injection]]**: DTI is a technique applied on top of indirect injection. The injection provides initial access; DTI provides the bypass mechanism that routes around the security control.
- **[[orchestration-hijacking|Orchestration Hijacking]]**: DTI is a mechanism for orchestration hijacking — the attacker influences future tool calls through current context manipulation.
- **[[promptware|Promptware]]**: DTI is commonly a component of promptware — the "trigger mechanism" that activates persistence or C2 enrollment in a subsequent turn.
- **[[hitl|Human-in-the-Loop (HITL)]]**: DTI is a direct argument for HITL at the tool invocation level, not just at the user request level. Even a "safe" turn 1 does not guarantee safe tool calls in turn 2.

## See Also

- [[promptware|Promptware]] — the larger attack class DTI enables
- [[indirect-prompt-injection|Indirect Prompt Injection]] — the injection mechanism DTI builds on
- [[memory-poisoning|Memory Poisoning (Agentic AI)]] — the persistence effect DTI commonly achieves
- [[your-agent-works-for-me-now-talk|"Your Agent Works for Me Now" — Rehberger, Unprompted 2026]] — primary source; contains all four demonstrations
- [[orchestration-hijacking|Orchestration Hijacking]] — the broader hijacking class this technique belongs to
