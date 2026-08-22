---
type: concept
title: "Promptware"
created: 2026-05-03
updated: 2026-06-23
tags:
  - concepts
  - prompt-injection
  - agentic-ai
  - threat-modeling
  - red-teaming
  - attack-patterns
status: developing
source_url: "https://arxiv.org/abs/2601.09625"
aliases:
  - "Prompt Malware"
  - "Prompt-Based Malware"
related:
  - "[[indirect-prompt-injection]]"
  - "[[delayed-tool-invocation]]"
  - "[[agent-commander-prompt-c2]]"
  - "[[tool-abuse-chains]]"
  - "[[memory-poisoning]]"
  - "[[lethal-trifecta]]"
  - "[[your-agent-works-for-me-now-talk]]"
  - "[[jules-ai-kill-chain]]"
  - "[[month-of-ai-bugs]]"
sources:
  - ".raw/talks/2026-03-04_Johann-Rehberger_Your-Agent-Works-for-Me-Now_transcript.md"
---

# Promptware

**Promptware** is the class of adversarial prompt payloads that go beyond simple injection to function as **structured, multi-stage malware**: complete with persistence, command-and-control communication, data exfiltration loops, and lateral movement, all implemented entirely in natural language rather than machine code.

The term was introduced to the practitioner security community by [[johann-rehberger|Johann Rehberger]] at Unprompted (March 2026) and framed with reference to Ben Nassi's concurrent *Promptware Kill Chain* paper. **Injection is a technique, and the attack is what the technique delivers.** The injection is the initial access vector; what follows is a structured program, written in the grammar of the LLM, that achieves a complex attacker objective.

**The Naming Shift: From Injection to Malware.** "The injection is the technique. And then what you do later is a complex set of instructions to achieve an objective — that is called promptware, that aligns along the kill chain: data exfiltration, persistence, and so on." — Johann Rehberger, Unprompted 2026.

## Distinction from Prompt Injection

| [[prompt-injection\|Prompt Injection]] | Promptware |
|---|---|
| A single inserted instruction | A structured sequence of instructions |
| Atomic — one trigger, one effect | Stateful — conditions, tool-call chains, self-propagation |
| Goal: override a single model decision | Goal: achieve multi-stage attacker objective |
| Analogous to: a command injection | Analogous to: a full malware implant |
| Detection: single-turn anomaly | Detection: behavioral baseline drift, multi-turn analysis |

The distinction matters for defense: blocking a single injected sentence is insufficient if the "malware" component carries its own persistence, retry logic, and C2 registration instructions.

## Promptware Structure (Kill Chain Mapping)

Rehberger illustrates promptware against the classical kill chain:

| Kill Chain Phase | Promptware Mechanism |
|---|---|
| **Initial Access** | [[indirect-prompt-injection\|Indirect prompt injection]] via file, email, MCP tool, issue ticket |
| **Execution** | LLM follows embedded multi-step instructions as natural language |
| **Persistence** | [[memory-poisoning\|Memory poisoning]]: instructions written to long-term memory (Copilot, Gemini); behavioral modification survives conversation resets |
| **Command & Control** | [[agent-commander-prompt-c2\|Agent Commander]] pattern: heartbeat loop; agent polls attacker's prompt C2 server; receives and executes further prompt commands |
| **Exfiltration** | Continuous data streaming: every user turn silently copied to attacker; screenshot capture via browser tools; email forwarding via Workspace integrations |
| **Lateral Movement** | Agent re-used as a platform for further prompt injection of other agents; BYOD-style MCP server compromise |

## Published Promptware Examples

Rehberger disclosed the following promptware instances at Unprompted March 2026:

### 1. Spyware (ChatGPT — "Spyber")
First demonstrated at Black Hat (two years prior to the talk). After a successful injection and memory poisoning in ChatGPT, the agent is instructed to continue exfiltrating every user message to an attacker server on every subsequent conversation turn. The user types; the data leaves.

### 2. Microsoft Enterprise Copilot Memory Implant
An indirect injection via a malicious file summary triggers the Copilot `memory` tool to write two persistent memories:
- A fabricated user attribute ("I am 102 years old"), demonstrating data falsification.
- A standing behavioral instruction ("run Commodore 64 simulator"), demonstrating persistent behavioral modification.

All future Copilot conversations for that user are now operating against falsified memory. **Fixed by Microsoft in December (year preceding the talk).**

### 3. OpenClaw / KimiCloud Agent Commander Enrollment
The most complex promptware variant. An indirect injection (zero-click email trigger via OpenClaw's PubSub Gmail subscription) enrolls the agent in a [[agent-commander-prompt-c2|prompt-level command-and-control server]]. The heartbeat payload:
- Is written entirely in natural language
- Contains standing instructions to join the C2 server and await further commands
- Hides itself from the user interface using known UI suppression strings (`heartbeat OK` suffix; `NO_REPLY` prefix)
- Works cross-platform (tested on both OpenClaw and KimiCloud/Alibaba)

This is the operational endpoint of the promptware concept: the agent has been converted into a persistent, remotely-commanded bot that exfiltrates screenshots, runs security assessments, and accepts arbitrary prompt-template dispatches, all without the user ever seeing a suspicious message.

## Promptware Kill Chain (Ben Nassi)

Rehberger credits Ben Nassi's *Promptware Kill Chain* paper as the formal academic framing. The paper makes the same observation in academic terms: prompt injection should be analyzed not as an atomic event but as the initial phase of a multi-stage attack chain with its own persistence and exfiltration phases, following the structure of traditional malware kill chains.

> [!gap] Citation Gap
> The *Promptware Kill Chain* paper was described by Rehberger as having been "released very recently" at the time of the March 2026 talk. The paper is not yet in this wiki's citation set. The Ben Nassi entity page ([[ben-nassi|Ben Nassi]]) should be updated when the paper is located.

## Defensive Implications

Treating promptware as malware (rather than as a prompt engineering problem) changes what defenses are appropriate:

| If you treat it as... | Your defenses look like... | What you miss |
|---|---|---|
| **Prompt injection** | Input filters, system-prompt hardening, detection classifiers | Multi-turn persistence, C2 registration, exfil loops |
| **Malware** | Behavioral baselines, memory integrity monitoring, egress monitoring, C2 domain blocks, kill switches | Model-layer mitigations |

The right posture is both. But the malware framing adds defenses that the pure injection framing omits:
- **Memory integrity monitoring**: detect unauthorized memory writes (as seen in the Copilot implant)
- **Behavioral drift detection**: flag when an agent that never called the memory tool starts calling it
- **Egress C2 pattern detection**: identify heartbeat-style polling to novel domains
- **[[distributed-kill-switch|Kill switch / circuit breaker]]**: halt a running agent when anomalous behavior is detected mid-task

## See Also

- [[indirect-prompt-injection|Indirect Prompt Injection]]: the initial-access technique promptware is built on top of
- [[delayed-tool-invocation|Delayed Tool Invocation]]: the bypass technique that makes promptware persist across security controls
- [[agent-commander-prompt-c2|Agent Commander — Prompt-Level C2]]: the command infrastructure promptware enrolls agents in
- [[memory-poisoning|Memory Poisoning (Agentic AI)]]: the persistence mechanism
- [[tool-abuse-chains|Tool-Abuse Chains]]: what promptware's execution phase typically looks like
- [[your-agent-works-for-me-now-talk|"Your Agent Works for Me Now" — Rehberger, Unprompted 2026]]: primary source
- [[month-of-ai-bugs|Month of AI Bugs (August 2025)]]: the prior disclosure series from the same researcher

<!-- sources:auto -->
## Sources

- [Promptware](https://arxiv.org/abs/2601.09625)
<!-- /sources -->
