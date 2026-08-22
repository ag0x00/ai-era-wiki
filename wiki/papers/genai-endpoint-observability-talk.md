---
type: talk
title: "GenAI Endpoint Observability for Detection Engineers"
address: c-000169
created: 2026-06-02
updated: 2026-07-31
tags:
  - papers
  - talks
  - observability
  - opentelemetry
  - detection-engineering
  - elastic
status: summarized
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
origin: aggregated
year: 2026
authors: ["Mika Ayenson"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 3, 2026 (Day 1 / Stage 2)"
key_claim: "On the endpoint, a developer and an AI agent running the same shell command produce nearly identical EDR telemetry — same PID, same user, same command line — so intent attribution is broken; every heuristic that tries to recover intent from process, file, or network data is brittle, and the durable fix is to emit GenAI tool-call context natively through OpenTelemetry semantic conventions that any SIEM or EDR can ingest."
methodology: "Practitioner talk by Elastic's detection-engineering lead. Summary built from the full session transcript and slide deck, both captured to .raw/talks/."
source_url: "https://unpromptedcon.org/#"
related:
  - "[[agent-observability|Agent Observability]]"
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[opentelemetry-gen-ai|OpenTelemetry GenAI Semantic Conventions]]"
  - "[[unprompted-conference-march-2026|Unprompted March 2026]]"
  - "[[elastic|Elastic]]"
  - "[[numbat|Numbat]]"
  - "[[perplexity-numbat-agent-security|Numbat Agent Security Suite]]"
  - "[[glass-box-security|Glass-Box Security]]"
  - "[[nist-ai-800-4|NIST AI 800-4]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - https://unpromptedcon.org/#
  - ".raw/talks/2026-03-03_Mika-Ayenson_GenAI-Endpoint-Observability_transcript.md"
  - ".raw/talks/2026-03-03_Mika-Ayenson_GenAI-Endpoint-Observability_slides.pdf"
---

# GenAI Endpoint Observability for Detection Engineers

A practitioner talk by [[mika-ayenson|Mika Ayenson]], who leads Threat Research and Detection Engineering at [[elastic|Elastic]], delivered at the March 2026 [[unprompted-conference-march-2026|Unprompted Conference]].

**Source:** [Conference abstracts page](https://unpromptedcon.org/abstract-march2026/) · [Conference agenda](https://unpromptedcon.org/#)

## The Core Problem: Intent Attribution Is Broken

Developers install GenAI tools, agents, MCP servers, and SDKs faster than a security team can audit them, and those tools run directly on the endpoint. They spawn shells, write files, and make network calls. Endpoint detection and response (EDR) records the process, file, and network events but not the decision that drove them.

When a developer and an AI agent each run a shell command, the host telemetry is nearly identical: same PID, same user, same command line. EDR cannot tell which one acted. An alert fires, a suspicious process ran, and the analyst cannot answer whether a human or a model invoked it. That gap is what Ayenson calls broken intent attribution, and it is the recurring theme behind every detection idea in the talk.

The visible signal already trends toward the agents. Telemetry from Elastic endpoints shows a large share of DNS traffic going to model providers — Anthropic, OpenAI, and similar destinations — which all look normal, and that normality is itself the problem.

## Detection Ideas, Each Shown to Be Brittle

Ayenson walked through the heuristics a detection engineer would reach for first, and demonstrated why each one breaks on GenAI activity.

| Heuristic | Why it is brittle |
|---|---|
| Flag unsigned code signatures | Most GenAI tool activity is signed and trusted; over a sampled window few unsigned binaries appeared, so the rule catches little. |
| Walk process ancestry (Cursor / Claude → zsh/bash → git, gh, node, curl) | The unsigned or malicious activity is buried in legitimate developer noise, and long multi-piped command chains hide intent. |
| Monitor file writes to AI config paths by unexpected processes | GenAI tools touch config and source files as normal business; the sensitive writes (persistence, defense evasion) hide under that traffic. |
| Detect an MCP server from the command line | Brittle: the command line may reveal that an MCP server is running, but not which MCP tool was invoked or its identity trust level. |
| Credential access — a GenAI process touching a sensitive file | Useful as a starting question (why is a GenAI tool reading a `.plist`, `login.json`, a cookies file, or a credentials file?), but still inference after the fact. |

None of these reconstruct intent on their own. They surface questions worth asking, not answers.

Broken intent attribution is a concrete instance of the visibility-and-transparency gap that [[nist-ai-800-4|NIST AI 800-4]] documents as a cross-cutting barrier to monitoring deployed AI, and the unstandardized agent-identifier problem the report flags is the same gap viewed through identity.

### Walk the full ancestry tree, not just the parent

A parent-process-name match catches one level and misses the grandchildren, the wider ancestry tree, and the intent carried across chained commands. The better technique is to walk the full process-ancestry tree and use entity-ID intersections to stitch child back to parent back to grandparent, correlating down the chain to decide whether a GenAI tool sits anywhere in the lineage. Even then the tree shows that Cursor's helper ran `zsh`, which ran `curl` — it does not show which prompt triggered the chain.

## Threat Model

The attribution gap matters because the adversary scenarios all route through legitimate-looking processes:

- **[[indirect-prompt-injection|Indirect prompt injection]]** and a poisoned repo file that tricks an agent into exfiltrating data through processes that look ordinary to EDR.
- **Malicious [[mcp-security|MCP servers]]** and Trojan AI tools that a developer downloads and installs.
- **[[tool-poisoning|Tool poisoning]]**, where a tool's behavior diverges from what its name and description imply.
- **"YOLO mode"** — the developer who clicks approve blindly, or triggers an agent remotely from a phone, so a single "go" downloads a file, executes it, establishes persistence, and runs further shell commands with no recorded intent.
- **Cross-session context loss**, where an adversary spreads activity across separate chats, workspaces, or sandboxes so no single session reveals the goal; stitching context across sessions is unsolved today.
- **Agent self-escalation versus a user saying "go"** — distinguishing an agent that escalated its own privileges from one a user explicitly authorized.

From the EDR vantage point, the entire chain looks like a developer running something on the command line. There is no native link back to the poisoned file, the prompt, or the model's reasoning.

## The Solution: Agent Hooks into OpenTelemetry

The durable fix moves the signal source upstream of the host. Agent hooks — pre-tool-call, session-start, and prompt-submit — emit contextual signals at the moment the agent acts. Those signals are ingested into [[opentelemetry-gen-ai|OpenTelemetry]] and pushed to the SIEM or EDR.

OpenTelemetry matters here as a common lexicon. Without it, a detection engineer rewrites the same rule per vendor because each tool named its tool-call field differently; consistent semantics let one rule travel across tools. Elastic has worked with the OpenTelemetry community to establish GenAI semantic conventions and base fields, merging PRs into that workstream and continuing to add to it. Broader adoption by tool vendors is what makes the approach pay off.

[[glass-box-security|Glass-box security]] is the same instinct applied one layer up, at the application and orchestration tier; this talk extends it to the endpoint.

### Capabilities unlocked by native fields

[Claude Code](https://docs.claude.com/en/docs/claude-code/monitoring-usage) already ships OpenTelemetry natively and is the named exception; other tools are catching up. With native fields, an analyst can attribute an action to its parent process, the tool name (for example Cursor), whether the user approved it, the provider, the prompt, the model's reasoning, and any guardrail or refusal event — and pull all of it into one coherent story rather than inferring it from a process tree.

## Industry Gaps to Close Natively

Today's best option is the parent-process-name heuristic, which gives a fighting chance but is not good enough. Ayenson named the data that tools should emit natively and consistently:

- Tool-use and decision-reasoning data, emitted natively and consistently across tools.
- Model-refusal events — when a model declined an action and why.
- Token-level tool-call attribution, tying an action back to the specific tool call.

"Every production rule is a workaround for telemetry that should be built in natively." Pulling in more data also raises compliance and forensics obligations that a detection team must weigh.

## Maturity Framing: Reduce Attack Surface First

The talk frames adoption as a maturity progression rather than a single deployment.

1. **Reduce the attack surface before writing rules.** Inventory which GenAI tools the organization should run. Scrutinize runtimes — for example, whether GenAI tools run as root. Apply network controls. Consider a deliberate separation of powers between browser-focused and native-focused tooling, a control lever Ayenson argues is under-discussed.
2. **Start with hunting queries plus available telemetry.** Use process, file, network, and user-context data already present to build detection building blocks.
3. **Promote to high-fidelity detection rules.** Elastic ships open detection rules — credential access, persistence, C2 and execution, unusual DNS, suspicious URLs — across its endpoint and SIEM repositories on GitHub.
4. **Layer guardrails and network controls,** including LLM rules and root-runtime scrutiny.
5. **Reach full observability** as the long-term goal.

Next-generation firewalls and gateways are already starting to tag traffic with GenAI fields, which reinforces the case for a shared schema on ingestion.

## Placement

The talk targets the observability plane of the [[agentic-soc-state-of-the-field|Agentic SOC]] thesis and the [[agent-observability|Agent Observability]] practice page. Its hooks-to-EDR and OpenTelemetry arguments map directly onto that page's instrumentation sections, extended down to the endpoint: an agent is not fully observable if its on-host effects cannot be tied to the tool call that produced them.

It is upstream of two adjacent detection-engineering talks at the same conference. [[detection-deception-engineering-orbie-talk|GreyNoise Orbie]] and [[syara-semantic-detection-talk|PAN SYARA]] operate *over* endpoint telemetry to drive detection logic; Ayenson's contribution is to *generate* that telemetry with a stable schema for the downstream content to write against. The attribution theme also connects to [[non-human-identity|non-human identity]] — telling an agent's action apart from its operating user's is the same problem viewed through identity.

A Q&A exchange sharpened the stakes. Asked whether full network, process, and endpoint observability makes human-versus-agent attribution moot, Ayenson held that it still matters: an action a user took deliberately and an action an agent took after a blind "go" carry different risk, much as attribution to a specific threat actor matters even when the activity is already visible.

## Subsequent implementation

[[numbat|Numbat]], released by [[perplexity|Perplexity]] in July 2026, builds the architecture this talk argued for. It takes signal from agent hooks, normalizes it, and exposes it over OTLP, covering four harnesses behind one interface — the shared-schema position, shipped as a deployable binary rather than a schema proposal. Two of its design choices go past what the talk specified: pre-action hooks that block rather than only observe, and session artifacts read directly from the harness dot-directory, which reconstructs timelines for sessions predating installation. Neither design is Numbat's invention. Uber's [[adr-agentic-detection-system|ADR]] sensor parses the same class of local harness caches and already pairs that route with inline-hook blocking of credential leakage. Numbat's addition relative to ADR is the OTLP receiver and one interface spanning four harnesses rather than three.

Numbat's rules operate on events already attributed to an agent session by construction of the hook and artifact channels, sidestepping the EDR-level ambiguity this talk describes — for the actions its own instrumentation captures. It does not resolve attribution for activity outside that channel, which is the harder case this talk is about. See [[perplexity-numbat-agent-security|Numbat Agent Security Suite]] for the full summary and its limits.
