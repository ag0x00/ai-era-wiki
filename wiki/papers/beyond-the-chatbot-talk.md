---
type: talk
title: "Beyond the Chatbot: Delivering an Agentic SOC"
address: c-000173
created: 2026-05-07
updated: 2026-06-02
tags:
  - papers
  - talks
  - agentic-soc
  - salesforce
  - supervisor-worker
  - digital-teammates
  - defense-in-depth
status: summarized
year: 2026
scope_axis:
  - ai-in-sec-defense
origin: aggregated
authors: ["Peter Smith", "Ravi Kiran Sharma"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 4, 2026 (Day 2 / Stage 2 / 15:10)"
key_claim: "Salesforce runs an Agentic SOC as a team of specialized digital teammates that take a real intel report from alert to containment in under 10 minutes, built as an intelligent layer on top of the existing SOAR (257 Python integrations) and system of records — no MCP, no A2A, no re-platforming."
methodology: "Practitioner talk with a live ~4-minute demo and an architecture walkthrough, presenting Salesforce's production Agentic SOC. Transcript and slide deck captured to .raw/talks/."
source_url: "https://unpromptedcon.org/#"
contradicts: []
supports:
  - "[[salesforce]]"
  - "[[agent-observability]]"
  - "[[agentic-soc-state-of-the-field]]"
related:
  - "[[1-8m-prompts-30-alerts-talk]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[peter-smith]]"
  - "[[ravi-kiran-sharma]]"
  - "[[crowdstrike]]"
  - "[[plan-validate-execute]]"
  - "[[hitl]]"
sources:
  - "[[unprompted-conference-march-2026]]"
  - "https://unpromptedcon.org/#"
  - .raw/talks/2026-03-04_Peter-Smith-and-RK_Beyond-the-Chatbot_transcript.md
  - .raw/talks/2026-03-04_Peter-Smith-and-RK_Beyond-the-Chatbot_slides.pdf
---

# Beyond the Chatbot: Delivering an Agentic SOC

**Source:** [Conference abstracts page](https://unpromptedcon.org/abstract-march2026/) · [Conference agenda](https://unpromptedcon.org/#)

Practitioner talk presented at [[unprompted-conference-march-2026|Unprompted Conference]] by [[peter-smith|Peter Smith]] (Director, Agentic SOC Product Management, [[salesforce|Salesforce]]) and [[ravi-kiran-sharma|Ravi Kiran Sharma]] (Lead Security Engineer, Agentic SOC, [[salesforce|Salesforce]]). The session paired a live end-to-end demo with an architecture walkthrough of the production system. The abstract branded the design "Polyphonic (Supervisor-Worker)"; the speakers' own vocabulary is **digital teammates**, **many specialized agents**, **deterministic orchestration**, the **8 primitives**, and an **AI Constitution**.

## Thesis

A Q&A chatbot over telemetry is a transitional pattern. Salesforce's Agentic SOC is a collection of first-class agents — digital teammates — that hunt, detect, contain, and collaborate alongside human analysts, mostly inside Slack. The stated target echoes Kevin Mandia's "alert to containment in 10 minutes" goal; the demo runs alert to containment to remediation in under 10 minutes.

The agents form a supervisor-plus-specialized-workers shape: a front-door orchestrator delegates well-scoped subtasks to single-purpose worker agents, each with constrained tool access. One agent maps to one workload, which the speakers describe as the antidote to scope creep and context drift.

## Live demo: a real intel report, end to end

The demo ran a real [[crowdstrike|CrowdStrike]] intelligence report (actor "Venomous Bear," report 2026035, roughly 8 IOCs including domains and MD5 hashes) through about nine specialized agents:

| Agent | Role |
|---|---|
| **Kestrel** | Front-door Slack chat agent; recognizes the CrowdStrike report-number format and fetches the report |
| **Normalizer agent** | Extracts the IOCs from the report |
| **Hunt planning agent** | Builds a hypothesis plus a data-source plan; the plan requires human approval and is then locked read-only |
| **IOC investigation agent** | Hunts each IOC across the given data sources (SIEM, threat intel) |
| **Findings review agent** | Produces the verdict on whether IOCs were present |
| **Hunt concluder agent** | Packages results into the Salesforce hunt record |
| **Threat detection engineer agent** | Writes and deploys detection content for the IOCs |
| **Remediation execution agent** | Blocks IOCs and contains hosts via CrowdStrike |
| **Incident communication agent** | Pages an incident commander and opens a Slack channel, adding the agents to continue the response |

A Salesforce hunt record is created at the start and updated continuously through the run. The three response specialists — detection deployment, remediation, and incident communication — are non-read-only actions, each gated by human approval.

## The 8 primitives

The deck's "Foundational Elements of an Agentic SOC" slide is presented as the most critical of the talk: eight primitives every agentic system needs, grouped into four layers.

| # | Primitive | Description | Layer |
|---|---|---|---|
| 1 | Reasoning & Planning | Break down objectives, plan steps | Cognitive (1 + 4) |
| 4 | Knowledge & Context | Domain expertise, personas | Cognitive (1 + 4) |
| 2 | Tool Orchestration | Discover and execute capabilities | Execution / orchestration (2 + 3) |
| 3 | State Management | Remember across sessions | Execution / orchestration (2 + 3) |
| 5 | Error Recovery | Save state; resume next run | Production safety (5 + 6) |
| 6 | Human in the Loop | Escalate when needed | Production safety (5 + 6) |
| 7 | Guardrails | Max iterations, completion signals | Engineering / design (7 + 8) |
| 8 | Observability | Logging, tracking, state files | Engineering / design (7 + 8) |

The recommended build order starts from these eight primitives, then layers technology beneath them.

## Architecture

The system is an intelligent layer built on top of two existing assets: the **security action library** and the **system of records (SoR)**. The speakers reject the premise that an Agentic SOC requires re-platforming. They kept the existing SOAR — **257 Python integrations** — and taught agents to use it, rather than tearing it down or buying a new orchestration platform.

Humans and digital teammates interact with the system through one of two entry modes:

- **Open-ended autonomous chains** — for open-ended work such as threat hunting against an anomalous indicator.
- **Deterministic orchestration** — for straightforward or high-risk workflows that should follow a fixed path.

Execution flow once a goal is accepted:

1. The **Agent Executor** loads context from **agent cards**, **state files**, and **goal files**, then uses the foundation LLMs to reason and plan. It manages the full agent lifecycle.
2. The **Capability Executor** performs actions against the security action library: SIEM searches, threat-vendor report pulls, internal-application actions, and SoR record reads and writes.
3. All execution loops and critical components are persisted to the SoR for audit.

The system was built deliberately without MCP and without A2A. The speakers state the position plainly: agentic software can be built without MCPs or A2As; the protocols are not required, and they avoided the cost of standing up and maintaining that infrastructure.

> [!note] Correction to the earlier stub
> An earlier version of this page speculated that the policy surface was "Cedar/OPA-style declarative rules." That is not the mechanism. Authorization is enforced through agent cards, tool config, bidirectional permission checks, and the choice between deterministic orchestration and autonomous chains — with no MCP or A2A in the stack.

## Agent card

An **agent card** defines an agent's persona, objective, and configuration: model, max iterations, temperature, max tokens, PII masking, and the allowed tool capabilities the agent may call in production. Binding one agent to one workload is how the design prevents scope creep and context drift. The full registry across the nine agents was roughly 600 lines.

The deck shows a real card for the hunt orchestrator agent ("Otter," type "Primary Orchestration Agent," roles "Threat Intelligence Engineer, Incident Responder"). Its visible config includes `temperature: 0.3`, `max_tokens: 4096`, `pii_masking: false`, `use_non_openai_anthropic_model: true`, a `completion_signals` list (`"goal achieved"`), and `domain_context` carrying `current_phase`, `retry_count`, and `max_retries: 3`. Its `allowed_capabilities` are the five hunt sub-agents: IOC normalizer, threat hunt planner, threat hunt executor, threat hunt findings reviewer, and threat hunt concluder. The [[agent-observability|Agent Observability]] page uses this same card as its worked example.

**max iterations.** One iteration is a full execution cycle — input, reason, plan, multiple tool executions, output — not a single LLM call. On reaching the maximum, the governance and orchestrator layer exits the session gracefully, saves the full state, and moves on.

## Tool config and the permission boundary

The **tool config** defines a tool's schema and integration mapping. It abstracts the real API calls and credentials away from the model, so API keys are never exposed to the base LLM. Authorization is checked at both ends: the agent card declares which tools an agent may call, and the tool config declares which agents may call a given tool. The two declarations form a bidirectional permission check.

## Goal file, state, and non-repudiation

The **goal file** and **state tracking** record the conversation, the assigned objective, the integrations used, phase completion, and per-tool execution (which tool ran, with what arguments). For non-repudiation, every risky tool call logs who approved it, the reason given, and the timestamp — full accountability over risky tool executions.

## Human-in-the-loop and the reasoning loop

Risky, non-read-only actions — deploying detections, blocking IOCs, containing hosts, paging an incident commander — require human approval. At each gate the system captures a business justification, used both as memory for future runs and as a regulator-facing audit trail. The interaction is conversational: an analyst can reject a proposed plan and ask for a different one. An **agent reasoning loop** then folds that feedback into a revised plan, modeled on the ask/plan modes found in coding IDEs. This is the approval-gated [[plan-validate-execute|Plan-Validate-Execute]] shape with [[hitl|Human-in-the-Loop]] at the validate gate.

## Digital teammates

Digital teammates are the first-class agents of the Agentic SOC — reasoning, planning teammates treated as part of the team, present in Slack channels and embedded at every stage of the incident-response lifecycle. The deck offers a "Digital Teammate Test" as a counterpart to the Turing Test: the question is not whether the agent can pass as human, but whether it can earn a human's trust as a peer. If the analyst feels they are "using software," the agent fails; if they feel they are "collaborating with a junior analyst," it passes. The speakers describe training a teammate as one would a grade-5 new hire, reaching a working agent in about 90 days.

## AI Constitution

The build started from an **AI Constitution**: a document that codifies the rules for delivering the Agentic SOC, binding for all personnel and superseding prior documents. The deck shows six articles:

1. Primitives of Agentic Security Operations
2. Governance and Oversight Structures
3. The Shared Responsibility Framework
4. Technology Exploration and Integration Management
5. Agentic Asset Lifecycle Management
6. Evangelism and Enablement

Article I is the 8 primitives, positioned as the starting point for any organization building an Agentic SOC.

## Takeaways

- Use deterministic orchestration; keep chain-of-thought inside guardrails.
- Avoid monolithic agents; prefer many small, specialized ones.
- Keep humans in the loop, especially while training the agents.
- Use ask/plan mode and agent reasoning loops; iterate the first plan with human feedback.
- Reuse existing infrastructure rather than re-platforming.
- Let agents earn trust through workflows.
- Maintain an AI Constitution, beginning with the 8 primitives.

In Q&A the speakers declined to quantify engineering hours, run cost, or KPI impact. One detail stood out: "the entire config was written by Claude — I didn't write a single piece of code," though foundational Python knowledge was still needed. Success is framed against mean-time-to-X SOC performance metrics.

## Companion Salesforce talk

Salesforce contributed two talks to the conference. [[1-8m-prompts-30-alerts-talk|1.8M Prompts, 30 Alerts]] is the detection half — behavioral anomaly detection that turns roughly 1.8M daily prompts into fewer than 30 high-fidelity alerts. This talk is the response half: how the Agentic SOC consumes such alerts and acts on them through specialized agents under human approval. Together they describe the upstream and downstream of an end-to-end agentic SOC at production scale.

## Cross-references

- [[salesforce|Salesforce]] — operator entity.
- [[crowdstrike|CrowdStrike]] — the intel source in the demo and the containment endpoint (host isolation).
- [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] — the production instance of the supervisor-plus-specialized-workers convergence.
- [[agent-observability|Agent Observability]] — its agent-card example is this talk's hunt-orchestrator card (agent cards, max iterations, PII masking, allowed capabilities).
- [[agentic-ai-security-reference-architecture|Agentic AI Security RA]] — the identity and observability planes that the agent cards, bidirectional permission checks, and SoR audit map onto.
- [[unprompted-conference-march-2026|Unprompted Conference]] — conference listing.

## Status

`summarized` — based on the transcript and slide deck captured to `.raw/talks/`.
