---
type: paper
title: "Agentic Threat Hunting Framework (ATHF)"
created: 2026-09-22
updated: 2026-09-22
tags:
  - papers
  - agentic-soc
  - threat-hunting
  - levels-of-autonomy
  - agent-memory
  - ai-in-sec-defense
status: summarized
scope_axis:
  - ai-in-sec-defense
origin: aggregated
year: 2026
authors: ["Sydney Marrone"]
venue: "Nebulock Blog"
publication_date: "2026-03-18"
source_url: "https://nebulock.io/blog/agentic-threat-hunting-framework"
archived_copy: ".raw/articles/agentic-threat-hunting-framework-2026-09-22.md"
no_public_url: ""
key_claim: "An AI assistant hunts well only over a structured record of past hunts; ATHF supplies that record as LOCK-format markdown (Learn, Observe, Check, Keep) managed by a CLI, and grades a hunting program on five levels that climb by memory and tool reach, from manual hunts in ticket queues (Level 0) to multiple agents with shared memory whose drafted hunts a human approves (Level 4)."
methodology: "Vendor blog post announcing an open-source repository. Describes the framework, the CLI workflow and the five levels, with one illustrative hunt (macOS AppleScript data collection) as the worked example. Reports no deployment, user study or evaluation. The LOCK diagram, the five-levels chart and the CLI animations are images and were not transcribed."
contradicts: []
supports:
  - "[[agentic-soc-ra-threat-hunting]]"
related:
  - "[[agentic-soc-ra-threat-hunting]]"
  - "[[agentic-soc-autonomy-ladders]]"
  - "[[ai-automation-boundary-threat-hunting-talk]]"
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-cmm-d2-threat-intel-knowledge]]"
  - "[[agentic-soc-state-of-the-field]]"
  - "[[mcp-security]]"
  - "[[agent-memory-isolation]]"
  - "[[memory-poisoning]]"
  - "[[sift-claude-code-dfir-talk]]"
sources:
  - "[[.raw/articles/agentic-threat-hunting-framework-2026-09-22.md]]"
  - "https://nebulock.io/blog/agentic-threat-hunting-framework"
  - "https://github.com/Nebulock-Inc/agentic-threat-hunting-framework"
verified: 2026-09-22
verified_against:
  - ".raw/articles/agentic-threat-hunting-framework-2026-09-22.md"
verified_findings: 0
verified_note: "ATHF post read whole; 1 medium (traceability argument attributed to post) and 2 low fixed"
---

# Agentic Threat Hunting Framework (ATHF)

**Source:** [Nebulock — Agentic Threat Hunting Framework](https://nebulock.io/blog/agentic-threat-hunting-framework) (Sydney Marrone, 2026-03-18). Repository: [Nebulock-Inc/agentic-threat-hunting-framework](https://github.com/Nebulock-Inc/agentic-threat-hunting-framework). Local copy: `.raw/articles/agentic-threat-hunting-framework-2026-09-22.md`.

The Agentic Threat Hunting Framework (ATHF) is an open-source repository from Nebulock that gives a threat hunting team a fixed record format for every hunt, a command-line tool that enforces it, and a five-level scale for how far AI takes part in the work. The post argues that an AI assistant hunts badly when it has no access to the team's hunting history, and that the fix is structure before agency. For the agentic SOC, the framework's position carries more weight than its tooling: it puts **hunt memory** beneath every level of AI involvement, as a precondition of the hunting agent on [[agentic-soc-ra-threat-hunting|Agentic SOC Threat Hunting Surface]].

## Summary

### Structure and memory as the precondition

The post opens on a diagnosis. Hunt notes sit in documents, tickets and chat threads, analysts forget what they hunted last quarter, and an analyst who pastes a question into a general chatbot gets an answer with no knowledge of the environment. ATHF answers with a repository in which every hunt follows the same pattern, every lesson is recorded, and every hypothesis is searchable, so that an AI tool reasons over the team's hunting history instead of guessing.

ATHF places itself on top of Splunk's [PEAK threat hunting framework](https://www.splunk.com/en_us/blog/security/peak-threat-hunting-framework.html). In the post's division of labour, PEAK tells a hunter how to hunt and ATHF captures, organizes and reuses what the hunt produced.

### The LOCK loop

LOCK is the four-step record every hunt follows, and the post presents it as a loop that humans and AI both follow without drifting:

- **Learn.** Gather context from threat intelligence, alerts and anomalies.
- **Observe.** State a hypothesis about adversary behavior and describe what normal and suspicious activity look like.
- **Check.** Run and test the hunting queries.
- **Keep.** Record what happened, what was found and what comes next.

The worked example hunts macOS information stealers that use AppleScript to collect local data, mapped to ATT&CK T1005. The Check step is an SPL-syntax query over EDR process events for `osascript` commands touching Safari cookies or the Notes database. The Keep entry reports a seven-day hunt over 500,000 process events and 2 million file operations that confirmed one instance of information-stealing behavior on an executive's system ([Nebulock — LOCK Keep example](https://nebulock.io/blog/agentic-threat-hunting-framework)); the full hunt is file [H-0001 in the repository](https://github.com/Nebulock-Inc/agentic-threat-hunting-framework/blob/main/hunts/H-0001.md). The post offers the entry as the example for the Keep step and ties it to no named organization or deployment, so it illustrates the record format and is no evidence of results.

### The CLI

A CLI turns the markdown convention into a managed workflow. It creates hunts, validates their structure and metadata, tracks ATT&CK coverage across the hunt library, and is the interface the post expects AI assistants to call. The five steps it lists are initialize a workspace, create a hunt, validate hunts, track ATT&CK coverage, and use AI assistants with the repository.

### Five levels of agentic threat hunting

The five levels climb by what the AI can read and reach. The post calls advancement optional and says each level is useful on its own.

| Level | What the AI has | What the human does |
|---|---|---|
| 0 | Nothing; hunts live in ticket queues or spreadsheets | All of the hunt |
| 1 | Nothing yet; hunts are recorded as LOCK markdown | All of the hunt, now with a reusable history |
| 2 | Read access to the hunt repository and its context files | Works with an assistant that searches past hunts, recalls patterns and recommends next steps |
| 3 | Tool access through MCP servers to SIEM, EDR and ticketing | Asks and validates |
| 4 | Several agents with shared memory that watch threat-intel feeds, draft hunts, test hypotheses and run queries | Approves and refines |

Level 2 depends on two context files: `AGENTS.md`, which holds details of the environment, and `knowledge/hunting-knowledge.md`, which holds hunting expertise. At Level 3 the MCP examples name Splunk, Elastic and Chronicle for SIEM search, CrowdStrike, SentinelOne and Microsoft Defender for EDR telemetry, and Jira, ServiceNow and GitHub for tickets; the agent also writes findings back into the hunt files. The post claims a team reaches Level 1 in a day and Level 2 in a week ([Nebulock — Getting Started](https://nebulock.io/blog/agentic-threat-hunting-framework)), a vendor estimate with no measurement behind it.

## Assessment

### Placement against the SOC autonomy ladders

**ATHF's levels grade the substrate an agent works from, while the SOC autonomy ladders grade the authority a human hands the agent.** The ladders on [[agentic-soc-autonomy-ladders|Agentic SOC Autonomy Ladders]] and the [[agentic-soc-cmm|Agentic SOC Capability Maturity Model]] climb from assisted to approval-gated to conditional to delegated work. ATHF climbs from no record, to a record, to an agent that reads the record, to an agent that reaches the tools, to several agents sharing the record. Read on the authority axis, ATHF's Level 4 still ends in a human approving and refining the drafted hunts, which places it near the semi-autonomous rung (L2) of the hunting ladder on [[agentic-soc-ra-threat-hunting|Agentic SOC Threat Hunting Surface]]. Its top level therefore agrees with that page's position that hunting should rarely reach delegated operation.

The two axes are independent. A team at ATHF Level 3 can run its hunt agent strictly as a query drafter, and a team at CMM L3 can hold its agent to conditional autonomy over a thin hunt history. The ATHF ladder adds a dimension the autonomy ladders leave implicit: an agent granted more authority over an unrecorded hunting history reasons from what the model already knows.

### Security posture the post leaves open

The post describes capability and leaves every control to the adopter.

- **MCP tool reach.** Level 3 gives a hunting agent query access to the SIEM and EDR and write access to ticketing and the hunt files. The post names no read-only scoping, credential handling or approval step for the writes. [[mcp-security|MCP Security]] governs this configuration unchanged.
- **Shared memory.** Level 4 agents share one memory and draft hunts from threat-intel feeds, which carry attacker-influenced text. The post specifies no partition, write provenance or integrity check on that memory, the invariants [[agent-memory-isolation|Agent Memory Isolation]] requires, so a poisoned feed item can reach every agent's context.
- **Context files.** `AGENTS.md` concentrates environment detail in a file every assistant reads. The file is useful for grounding and is also a sensitive inventory and an instruction channel into the agent.

### Evidence quality

The post is a single-vendor announcement. It reports no evaluation of hunt quality with and without the repository, no adoption figures, and no comparison with an assistant given the same history by other means. The claim that AI assistants use the `athf` commands "automatically" is the vendor's own. The argument is the strongest part: an assistant that reads the team's past hypotheses, queries and outcomes reasons from them instead of guessing. Grounding of that kind also lets an analyst trace a conclusion back to the recorded hunts behind it, the argument [[agentic-soc-cmm-d2-threat-intel-knowledge|Agentic SOC CMM D2 Threat Intelligence and Knowledge]] makes for threat intelligence; the post itself does not make the traceability point.

## Relations

- Adds hunt memory to the tools and data of [[agentic-soc-ra-threat-hunting|Agentic SOC Threat Hunting Surface]], and supports its position that hunting keeps a human approval step.
- Adds a ladder graded on memory and tool reach to [[agentic-soc-autonomy-ladders|Agentic SOC Autonomy Ladders]].
- Complements [[ai-automation-boundary-threat-hunting-talk|Exploring the AI Automation Boundary for Threat Hunting]]: Datadog's orchestrator-subagent system automates query generation and narrowing, and ATHF supplies the hunt record such a system reads and writes.
- Level 3's MCP tool access is the pattern [[sift-claude-code-dfir-talk|Claude Code on the DFIR SIFT Workstation]] demonstrates on a forensic workstation.
