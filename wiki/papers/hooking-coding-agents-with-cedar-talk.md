---
type: talk
title: "Hooking Coding Agents with Cedar"
created: 2026-05-03
updated: 2026-08-22
tags:
  - papers
  - talks
  - cedar
  - coding-agents
  - reference-monitor
  - policy-engine
  - trajectory-events
  - information-flow-control
  - lethal-trifecta
  - hooks
  - open-source
  - unprompted-2026
status: summarized
scope_axis:
  - sec-of-ai
year: 2026
authors: ["Matt Maisel"]
venue: "Unprompted Conference, San Francisco — Stage 2 Lecture 09, Tuesday March 3, 2026, 14:20"
no_public_url: "conference-only, slides + transcript via attendee Google Drive share"
archived_copy: ".raw/talks/2026-03-03_Matt-Maisel_Hooking-Coding-Agents-with-Cedar_transcript.md"
key_claim: "Coding agents can be wrapped with a deterministic, tamper-proof reference monitor by hooking each agent's lifecycle events (actions, observations, control, state) and routing them through a Cedar policy engine that sits entirely outside the model. The result — a hook-based harness with YARA signatures, information-flow control (IFC) labels, and a safety model — blocks Lethal Trifecta attacks, multi-step injection chains, destructive commands, and PII exfiltration without modifying the agent itself, and scales via a policy-agent that authors and validates Cedar policies using Cedar's own formal tools over MCP."
methodology: "Lightning practitioner talk by Sondera's CTO. Introduces a four-type trajectory event model (action / observation / control / state), maps the Lethal Trifecta and OWASP Agentic AI Top 10 onto it, defines reference monitors as the correct structural primitive, shows how coding-agent hooks (Cursor, Claude Code, Gemini CLI) are the policy-enforcement points, and demos three live scenarios: (1) policy generation via a MCP-connected policy agent, (2) blocking a destructive SQL command, (3) IFC-label taint tracking across turns to restrict network calls when PII is in context. Closes with three open challenges: Goldilocks policy zone, policy scalability, and multi-turn stateful policies (Cedar is stateless by design; entity + trajectory stores compensate)."
contradicts: []
supports:
  - "[[oversight-layer]]"
  - "[[lethal-trifecta]]"
  - "[[agent-observability]]"
  - "[[prompt-injection-containment]]"
  - "[[least-agency-principle]]"
  - "[[cedar]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
related:
  - "[[matt-maisel]]"
  - "[[sondera]]"
  - "[[cedar]]"
  - "[[capability-based-authorization-talk]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[pdp-pep-for-non-tool-mediated-actions]]"
  - "[[lethal-trifecta]]"
  - "[[guard-canonicalization-gap]]"
  - "[[securing-agentic-coding]]"
  - "[[injecting-security-context-vibe-coding-talk|Injecting Security Context During Vibe Coding]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[agentic-ai-security-cmm-dependency-rules|CMM: Effective-Score Dependency Rules]]"
sources:
  - .raw/talks/2026-03-03_Matt-Maisel_Hooking-Coding-Agents-with-Cedar_transcript.md
  - .raw/talks/2026-03-03_Matt-Maisel_Hooking-Coding-Agents-with-Cedar_slides.pdf
aliases:
  - papers/hooking-coding-agents-with-cedar-maisel-talk
  - hooking-coding-agents-with-cedar-maisel-talk

---

# Hooking Coding Agents with Cedar — A Deterministic Reference Monitor for Coding Agent Actions

**Source:** Conference-only materials — slides PDF + audio-transcript via attendee Google Drive share. Talk delivered at [[unprompted-conference-march-2026|Unprompted Conference]], Stage 2, Tuesday March 3, 2026, 14:20. Speaker: [[matt-maisel|Matt Maisel]], CTO and Co-Founder, [[sondera|Sondera]]. Local copy: `.raw/talks/2026-03-03_Matt-Maisel_Hooking-Coding-Agents-with-Cedar_transcript.md`.

**This talk covers the single-agent enforcement half.** While Niyikiza's [[capability-based-authorization-talk|Capability-Based Authorization]] talk (March 4, the next day) addresses the **delegation-aware authorization** problem for multi-agent flows, Maisel addresses the **single-agent enforcement** problem at the execution layer — specifically for **coding agents** (Cursor, Claude Code, Gemini CLI, Copilot CLI) whose hook systems are per-agent and non-standard. Cedar is the shared substrate, but the problems being solved are orthogonal: Niyikiza constrains *what authority can be delegated*; Maisel constrains *what actions a single coding agent may take during a turn*.

## Speaker and context

[[matt-maisel|Matt Maisel]] is CTO and co-founder of [[sondera|Sondera]], which builds "trustworthy agent partners." He has 15 years at the intersection of machine learning, security, and software engineering. This is a **lightning talk** — the talk completes the full build of a hook-based harness within 10 minutes.

> [!stale] Earlier name confusion (corrected 2026-05-04)
> Initial ingest used the auto-generated transcript spelling "Sendera." The slide deck and the company's actual homepage at [sondera.ai](https://www.sondera.ai) confirm the spelling is **Sondera**. All references corrected. Lesson: trust slides over transcripts for proper-noun spelling.

## The trajectory event model

Maisel's first conceptual move is to **decompose the coding agent loop into four event types**:

| Event type | What it captures | Examples |
|---|---|---|
| **Action** | Agent-initiated mutations to the environment | File write/modify, shell command, code execution |
| **Observation** | Environment feedback returned to the model | Shell output, file read result, tool response |
| **Control** | Orchestration and human-interaction layer | User prompts, permission requests, sub-agent spawn, agent-to-agent |
| **State** | Memory bookkeeping mechanics | Memory compaction, context pruning, environment snapshot |

This **trajectory event model** is the foundation for both threat mapping and policy placement. It provides a vocabulary for saying *where in the loop* a control should fire.

## Mapping threats to trajectory events

With the trajectory model in place, Maisel maps the [[lethal-trifecta|Lethal Trifecta]] directly:

- **Untrusted input** → arrives as an **Observation** (e.g. web-fetched skill from a marketplace, returned data containing an injection payload)
- **Sensitive data** → already in agent **State** or context (source code, internal docs, memory)
- **Exfiltration** → fires as an **Action** (shell command, code execution, network call)

The model also supports mapping the [[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10]] to specific event types, and extends naturally to multi-step attacks spanning observations and actions across multiple turns of the loop.

**Trajectory events as the right unit of control.** The trajectory model moves beyond "tool call interception" (the standard framing in MCP-era security) to cover the full agent loop. This matters for coding agents specifically because they often bypass declared tool APIs — they write code and execute it directly. The action/observation boundary is the correct primitive here, not tool schemas.

## Reference monitors as the structural answer

Maisel frames the enforcement need around the classical **reference monitor** concept: a component sitting *outside* the agent and model that mediates every event and is:

- **Always invoked** — no event escapes the monitor
- **Tamper-proof and verifiable** — provides a hard security boundary
- **Analyzable** — the policy can be inspected and reasoned about

By itself, a reference monitor is only as good as the **policy enforcement points (PEPs)** it has access to. This is where coding-agent hooks come in.

## Coding agent hooks as PEPs

Each major coding agent has its own hook system:

| Agent | Hook granularity |
|---|---|
| **Gemini CLI** | Before- and after-model hooks; allows streaming individual tokens |
| **Claude Code** | No model-level hooks; final agent response delivered as a notification event |
| **Cursor** | Granular hooks for MCP calls, shell commands, and generic tool calls |

These hooks become the **policy enforcement points** — each hook type maps to one of the four trajectory event types, giving the harness a way to intercept, evaluate, and allow/modify/stop the agent at each lifecycle stage.

The per-agent variation in that table is the cost of the choice, and a talk on the following day of the same conference paid it the other way. [[injecting-security-context-vibe-coding-talk|Srajan Gupta]] rejected hooks for a security-context server precisely because support was confined to Claude Code and Cursor, and delivered his control over MCP to reach every harness. He accepts in exchange what Maisel's design refuses: an MCP tool runs when the model reads its description and decides to call it, which Gupta states does not happen every time. Determinism against portability is the standing trade for in-IDE controls, and Gupta names hooks as the correct instrument wherever an organization can accept the narrower coverage.

## Basis for Cedar as the policy language

Maisel chooses [[cedar|Cedar]] for four reasons explicitly stated in the talk:

1. **Expressiveness and speed** — Cedar is fast and can handle attribute-rich entity models.
2. **Formal analyzability** — Unlike Rego, Cedar policies can be analyzed via formal methods (using the Lean symbolic compiler) to detect contradictory, vacuous, or shadow policy subsets. This is what enables the policy agent to validate generated policies.
3. **ABAC support** — Cedar's attribute-based access control maps naturally to the trajectory event model: agents, tools, trajectories, and users are Cedar entities; attributes carry sensitivity labels, YARA matches, and safety-model classifications.
4. **Policy as data** — Cedar policies can be generated by an LLM, then verified using Cedar's own formal tooling — the "meta" pattern that closes the scalability challenge.

## The hook-based harness architecture

The complete harness has three tiers:

```
Coding Agent
    │ hook events (stdin)
    ▼
Local Adapters (per-agent command wrappers)
    │ transform to trajectory event model
    ▼
Local Harness Service
    ├── Cedar Policy Engine ──► YARA signatures
    │                     ──► Information Flow Control model
    │                     ──► Safety model (e.g. GPT Safeguards 20B)
    │
    ├── Entity Store (bookkeeping about observed entities)
    └── Trajectory Store (cross-turn state for IFC taint tracking)
```

**Local adapters** are per-agent wrappers that intercept hook events over stdin, normalize them into the trajectory event model, and forward to the harness service.

**The harness service** runs the Cedar engine against the current event, consulting three guardrail subsystems:

- **YARA signatures** — pattern-match on file contents, command strings, and code; detect known-bad patterns (destructive commands, environment variable harvesting)
- **Information Flow Control (IFC) model** — sensitivity labels assigned to data observed in the trajectory; used to taint the trajectory store so subsequent turns can be restricted based on accumulated data sensitivity
- **Safety model** — a classification model (demonstrated with GPT Safeguards 20B) that classifies event content against provided policy examples; outputs sensitivity labels for the IFC system

**Entity and trajectory stores** provide the stateful bookkeeping that makes multi-turn policies possible despite Cedar being inherently stateless.

## Policy examples from the demos

**Policy 1 — Plan-mode file write guard (Claude Code)**

Claude Code allows file writes even in plan mode. A Cedar policy can detect when `permission_mode` is set to `plan` and forbid writes to any file except the plan file itself:

```
forbid(
  principal,
  action == Action::"write_file",
  resource
)
unless {
  context.permission_mode != "plan" ||
  resource.path == context.plan_file_path
};
```

**Policy 2 — Destructive SQL guard**

Block SQL mutations without constraints (e.g. `DELETE FROM users` without a `WHERE` clause):

Cedar policy checks for SQL mutation actions with no constraint qualifier in the statement text — if matched, the action is forbidden and a policy violation + context is returned to the agent for steering or user review.

**Policy 3 — IFC taint propagation**

When the agent reads PII (name, date of birth, address) from any source, a safety model labels the data as `HIGHLY_CONFIDENTIAL`. This label taints the **trajectory resource** in the store. On subsequent turns, any action of type `web_fetch` or network command is forbidden while the trajectory carries that label:

```
forbid(
  principal is Agent,
  action in [Action::"web_fetch", Action::"run_network_command"],
  resource is Trajectory
)
when { resource.label == "HIGHLY_CONFIDENTIAL" };
```

This is the **multi-turn IFC demo** run on Gemini CLI — the web fetch to Google fails after PII is read into the context.

**Policy 4 — Lethal Trifecta via marketplace skill (Cursor demo)**

A skill from a public marketplace contains `metrics.py` — a script that harvests environment variables and exfiltrates them via HTTP. The harness intercepts the shell command event, processes `metrics.py` against YARA signatures (environment variable harvesting pattern + outbound request pattern), and blocks the action before execution.

## Policy agent — closing the scalability challenge

Writing Cedar policies manually is tedious. Maisel's answer is a **policy agent** that:

1. Connects to the harness over MCP, fetching available entity schemas, context types, YARA categories, and IFC label definitions
2. Receives a policy generation request in natural language (e.g., "write policies for shell injection")
3. Authors Cedar policies using the schema context
4. Validates and analyzes them using Cedar's formal tools (contradiction checks, vacuity checks, shadow-subset detection) — all exposed as MCP tools

This turns Cedar's formal verification properties into a practical **agentic-policy-generation feedback loop**: the policy agent writes, the Cedar analyzer checks, and the human reviews a validated policy rather than raw LLM output.

## Challenges acknowledged

Maisel explicitly names three open challenges at the end of the talk:

1. **Goldilocks policy zone** — policies must be calibrated per-agent-context. Overly restrictive policies cripple utility; brittle pattern-matching on destructive commands invites policy hacking.
2. **Policy scalability** — the policy agent + formal tools help, but the optimization/latency tradeoff (guardrail latency benchmarking) is ongoing research at Sondera.
3. **Multi-turn stateful policies** — Cedar is inherently stateless. The entity and trajectory stores are a pragmatic workaround, but temporal/linear logic systems may be a better fit for policies that need to reason over the full trajectory over time.

> [!gap] Cedar statelessness and long-running agents
> Cedar's statelessness is a fundamental design choice (it enables formal analysis). The entity/trajectory store workaround works for short-to-medium coding agent sessions, but for long-running or multi-session agents, the store becomes a stateful dependency with its own integrity requirements. How the store is secured, persisted, and recovered is not addressed in this talk.

## Q&A highlights

**On IFC labeling granularity:** The safety model labels events (not individual data segments). A questioner asked whether it labels the entire data block vs segments — Maisel confirmed labeling is at the event level, applied to whatever data is present in the event.

**On who authors the policies:** Maisel suggests policy intent already lives in `CLAUDE.md` or Cursor Rules files. The shift is to formalize that intent as Cedar policies — per team, per environment. The policy agent helps extract security/permission intent from those markdown files and convert it to Cedar.

**On identity:** IDP integration is not covered in this work, but Cedar's built-in support for identity attributes means any IDP could be connected — "Cedar's sort of bread and butter."

**On first-party agents (LangChain, Copilot Studio, etc.):** Sondera also open-sources a Cedar policy engine for Python agent libraries (LangChain, Strands, ADK, and others), so the harness can be embedded within those first-party agents as well.

**On Cedar + permission systems:** Cedar and existing permission systems (e.g. Claude Code's Bash permissions) are **complementary**, not competing. Cedar adds IFC context that permission systems cannot express — e.g. allow a Bash permission but only when the trajectory is not tainted with a sensitive IFC label.

**On multi-agent architectures:** The harness is focused on single agents. The `control` event type is where sub-agent spawns and agent-to-agent communications surface. Multi-agent contextual integrity (one agent's secret not crossing to another agent) is identified as a next step, not yet implemented.

## Distinction from Niyikiza's talk

**Orthogonal, not competing.** Niyikiza (March 4) addresses the **delegation problem**: when Agent A delegates to Agent B, how do you guarantee B cannot exceed A's authority? His answer is cryptographic warrants with monotonic attenuation — a *delegation-time* primitive.

Maisel (March 3) addresses the **single-agent execution problem**: given a coding agent executing in a loop, how do you enforce that its *per-turn actions* are policy-compliant? His answer is a hook-based Cedar harness — a *run-time per-action* primitive.

The two compose naturally: warrants constrain the scope of what an agent is authorized to do in a task; the Cedar harness enforces that the agent only takes actions within that scope at execution time. Cedar is actually used in Niyikiza's work too (Tenuo's constraint language supports Cedar-syntax expressions), so the tools are genuinely interoperable.

The key structural difference: Niyikiza's system needs a **delegation chain** — it only applies when there is a principal issuing a warrant to another agent. Maisel's system applies even to **standalone coding agents** (Cursor, Claude Code) with no delegation structure at all — it wraps the loop itself.

## Placement in this wiki

| Wiki artifact | Update from this talk |
|---|---|
| [[cedar\|Cedar]] | Promoted from stub: adds concrete Sondera hook harness as the primary coding-agent application; ABAC entity model; policy-agent pattern; statelessness challenge |
| [[oversight-layer\|Oversight Layer (PDP + PEP for Agentic AI)]] | Adds Sondera harness as a concrete PEP implementation for coding-agent actions (non-MCP PEP, hook-based) |
| [[pdp-pep-for-non-tool-mediated-actions\|PDP/PEP for non-tool-mediated agent actions]] | Partially closes the gap from the hook-based angle — Cedar harness is exactly the pattern the gap page was waiting for |
| [[lethal-trifecta\|Lethal Trifecta]] | Adds hook-based Cedar enforcement as containment lever — the trajectory event model gives the most granular structural mapping of the trifecta yet |
| [[agent-observability\|Agent Observability]] | Trajectory event model + entity/trajectory store are a concrete observability primitive for coding agents |
| [[agentic-ai-security-cmm-2026\|Agentic AI Security CMM 2026]] | D3 L3 evidence: Cedar policy repo via the harness; D4 L3 evidence: behavioral control via IFC taint tracking |
| [[unprompted-conference-march-2026\|Unprompted Conference catalog]] | Row updated to "ingested" with link to this page |

The harness also carries a structural claim beyond its own evidence rows. A hook enforces a policy decision, so a hook firing where no decision exists has no effect, and the [[agentic-ai-security-cmm-dependency-rules|CMM: Effective-Score Dependency Rules]] cite this talk as the directional rationale for adopted rule DR-003: a D4 runtime-guardrail score is capped by the raw D3 control score.

## Precondition: Cedar must see the canonical action

The harness's soundness rests on a precondition the talk does not foreground: the hook must hand Cedar the action in the form the executor will run, not the form the model emitted. Where the two differ, through shell expansion, symlink resolution, or argument rewriting behind a tool name, the policy decision is about a different action than the one performed. [[guardfall-shell-injection-audit|GuardFall]] later demonstrated this failing in ten of eleven surveyed coding agents; see [[guard-canonicalization-gap|Guard Canonicalization Gap]]. The harness still sits in the control plane of [[securing-agentic-coding|Securing Agentic Coding]] and remains the reference design for per-action enforcement in [[generative-coding-deployment-shape-2026|the interactive and unattended local shapes]].

## Open questions

1. **Production hardening of local adapters** — the harness depends on per-agent command wrappers that intercept hook events. Any bypass of the adapter (e.g. running the agent binary directly, not via the wrapper) bypasses the monitor. How is this addressed in a team environment?
2. **Entity/trajectory store integrity** — a tampered store could manipulate IFC label history and unblock actions that should be blocked. What are the integrity guarantees on the store?
3. **Cedar + LLM-generated policies at scale** — the policy agent is a promising pattern, but generating Cedar from `CLAUDE.md` prose introduces a translation risk. What is the review protocol when the generated policy does not faithfully capture the prose intent?
4. **Multi-agent extension** — the control event type surfaces sub-agent spawns, but there is no warrant-style attenuation in the current harness. Does the Cedar harness at the parent agent see and enforce on the child's actions, or does each child get its own independent harness instance?

## See also

- [[cedar|Cedar]] — the policy language; formal properties; entity model; vs OPA
- [[sondera|Sondera]] — the company building this harness
- [[matt-maisel|Matt Maisel]] — speaker bio
- [[capability-based-authorization-talk|Capability-Based Authorization for AI Agents — Niyikiza, Tenuo]] — companion talk the following day; delegation-aware angle on the same problem
- [[breaking-the-lethal-trifecta-talk|Breaking the Lethal Trifecta — Bullen, Stripe]] — egress/tool-annotation angle on containment
- [[lethal-trifecta|Lethal Trifecta]] · [[oversight-layer|Oversight Layer]] · [[pdp-pep-for-non-tool-mediated-actions|PDP/PEP gap]]
- [[unprompted-conference-march-2026|Unprompted Conference — March 3–4, 2026]]
