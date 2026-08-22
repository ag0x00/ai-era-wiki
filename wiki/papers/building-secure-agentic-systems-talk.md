---
type: talk
title: "Building Secure Agentic Systems"
created: 2026-05-03
updated: 2026-05-03
tags:
  - papers
  - talks
  - mcp-security
  - memory-isolation
  - capability-bounding
  - agentic-ai
  - observability
  - prompt-injection
  - dropbox
  - unprompted-2026
status: summarized
year: 2026
authors: ["Brooks McMillin"]
venue: "Unprompted Conference, San Francisco — Stage 1 Lecture 06, Wednesday March 4, 2026, 11:45"
source_url: "https://drive.google.com/file/d/1OecS5qHEtghmjBRxzC9JlnmBAUSpAR0x/view"
archived_copy: ".raw/talks/2026-03-04_Brooks-McMillin_Building-Secure-Agentic-Systems_transcript.md"
key_claim: "Running a personal fleet of 19 agents with 73 MCP tools exposes the same failure modes that enterprise deployments face at scale — and fixing them (memory isolation by identity namespace, context-aware security-event pinning, per-agent capability scoping) produces measurable improvements in both security posture and functional correctness, proving that security and functionality are complementary, not opposed."
methodology: "Practitioner live-demo talk. McMillin describes a personal home-lab agentic infrastructure he runs continuously, using it as a test bed for agentic-AI security controls. Evidence is observational (his own production system) rather than empirical. Talk includes three live demos (permission denial, memory isolation, context-aware trimming) with Q&A capturing the MCP sidecar vs. service-mesh architectural fork."
supports:
  - "[[least-agency-principle]]"
  - "[[mcp-security]]"
  - "[[agent-observability]]"
  - "[[prompt-injection-containment]]"
  - "[[memory-poisoning]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
related:
  - "[[brooks-mcmillin]]"
  - "[[dropbox]]"
  - "[[agent-memory-isolation]]"
  - "[[context-aware-trimming]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[securing-your-agents-talk]]"
sources:
  - .raw/talks/2026-03-04_Brooks-McMillin_Building-Secure-Agentic-Systems_slides.pdf
  - .raw/talks/2026-03-04_Brooks-McMillin_Building-Secure-Agentic-Systems_transcript.md
aliases:
  - papers/building-secure-agentic-systems-mcmillin-talk
  - building-secure-agentic-systems-mcmillin-talk

---

# Building Secure Agentic Systems — Brooks McMillin

**Source:** Unprompted Conference 2026, Stage 1 Lecture 06 ([[brooks-mcmillin|Brooks McMillin]], [[dropbox|Dropbox]]). Transcript via attendee [Google Drive share](https://drive.google.com/file/d/1OecS5qHEtghmjBRxzC9JlnmBAUSpAR0x/view). Local copies: `.raw/talks/2026-03-04_Brooks-McMillin_Building-Secure-Agentic-Systems_{slides.pdf,transcript.md}`.

> [!note] Source note
> McMillin is an Infrastructure Cloud Security Engineer at Dropbox, but this talk describes a **personal project** — a home-lab agentic infrastructure he built and runs on his own. It is not a Dropbox product or enterprise deployment story; the lessons are presented as practitioner discoveries in a low-stakes environment (self-described: "if I break it, I'm only affecting myself"). Code is open source on his GitHub.

## TL;DR

McMillin runs 19 agents (13+ interactive, a few service/cron agents) with 73 MCP tools on a local stdio MCP server. Three security controls emerged as load-bearing through trial and error:

1. **Per-agent capability scoping** — each agent has an explicit allowlist of tools and a permission level per tool (read / write / admin). Tools are locked-down by default; tools and permissions must be explicitly granted. Prevents context pollution and reduces blast radius.
2. **Memory isolation by identity namespace** — agents have identity derived from their Python class name, mapped to a memory namespace by the MCP server. Agents cannot influence their own namespace key, preventing memory impersonation and cross-agent memory leakage.
3. **Context-aware security-event pinning** — security-relevant MCP events (SSRF blocked, permission denied) are tagged and preserved across context-window trims, preventing attackers from hiding attacks in the "every N tokens" blind spot.

A fourth control — **LLM firewall (prompt injection detection)** — was deployed but required significant tuning to reduce false positives. Out-of-the-box configurations were too aggressive, blocking legitimate tasks.

**Security and functionality are the same problem.** McMillin's most-repeated framing: "that's a great example of how improved security also improves actual functionality." With 73 MCP tools, injecting all of them into every agent's context wastes tokens AND degrades accuracy. Scoping tools to each agent's actual role improves LLM performance **and** reduces attack surface simultaneously.

## The fleet

As of the talk (March 4, 2026 — "I updated these numbers last night and I think it's already out of date"):

| Dimension | Value |
|---|---|
| Total agents | ~19 |
| Interactive agents | ~13 |
| Service / cron agents | a few |
| MCP tools (local stdio server) | 73 |
| Remote MCP servers | GitHub + personal ones |
| Startup interfaces | CLI, Web UI, Voice (web + standalone app), Slack bot |
| Runtime | Python; personal desktop + home-lab Proxmox LXC |
| Auth pattern | OAuth everywhere |

Notable agents: task manager (daily driver for automated coding tasks), security researcher (read-only, questions RAG + web), chatbot (all permissions, for testing), business analyst ("look through my GitHub, see how I can make money").

## Control 1: Per-agent capability scoping

Each agent is configured with:
- `allowed_tools`: explicit list of MCP tools the agent may invoke
- Permission levels per tool: `read`, `write`, `admin` (default for any unconfigured tool is locked-down / admin-required)

The MCP server enforces these at call time. If an agent tries to exercise a permission it doesn't have (e.g., write to memory with only `read` permission), the MCP server returns a verbose enough error message for the agent to understand and report what's missing.

The security benefit is standard [[least-agency-principle|Least Agency]] — deny by default, permit by exception. The functional benefit, at 73-tool scale, is that agents don't receive irrelevant tool descriptions that would dilute their context and confuse their planning.

**Tool allowlisting at 73-tool scale.** McMillin notes that with 73 tools on one local MCP server, injecting all tool descriptions into every agent's context is both a security problem (unnecessary exposure) and a functional problem (context noise degrades task quality). Per-agent tool allowlists solve both at once. This is the same observation behind Stripe's [[toolshed|Toolshed]] central MCP (see [[breaking-the-lethal-trifecta-talk|Bullen's talk]]) — though McMillin's mechanism is a local sidecar, not an enterprise proxy.

## Control 2: Memory isolation by identity namespace

### The failure mode that triggered it

McMillin originally shared a single Postgres table for all agent memories. Two concrete cross-contamination failures:

1. A "business agent" was asked to optimize for \$1,000/month revenue from GitHub projects. After that, every time the **task manager** was asked to break down tasks, it would reference the \$1,000/month goal — the business agent's objective had leaked into the task manager's context.
2. Multiple similar leakage events followed between different agents.

### The fix: class-name-keyed namespaces

The solution is [[agent-memory-isolation|memory isolation by identity namespace]]:

- Every agent is a Python subclass of a base `Agent` class
- The subclass name (e.g., `TaskManagerAgent`) is used as the namespace key
- When the agent starts its local MCP server, the class name is passed to the MCP server and translated into the memory namespace
- The MCP server controls the namespace mapping — **the LLM has no access to its own namespace key and cannot claim a different identity**

This is a minimal but structurally sound identity-for-memory pattern: the control point is outside the LLM, and the identity proof is structural (class instantiation) rather than prompt-asserted.

Memory storage format: key-value with `category`, `importance` score, and free-form metadata. Important memories (above a threshold importance score) are auto-injected into context on startup; other memories are available on-demand via a `search_memory` MCP tool.

> [!gap] Fragile identity anchor
> McMillin acknowledges the class-name anchor is "pretty simple, and probably needs expanding fairly soon." It works for a single-developer fleet where agents can't easily masquerade as other Python subclasses. In a multi-tenant or adversarially hardened deployment, namespace identity needs a cryptographically stronger anchor — see [[capability-based-authorization|Capability-Based Authorization]] and [[tenuo-warrant|Tenuo Warrants]] for the stronger end of this spectrum.

## Control 3: Context-aware security-event pinning

### The failure mode

An agent analyzing security logs will eventually fill its context window. When context is trimmed, the default behavior drops the oldest messages first. An attacker could therefore send an attack every `N` tokens and guarantee that at any given moment, only one attack instance is visible to the agent — past attacks roll off the context window.

### The fix

Tag MCP response messages with their security significance at the time they arrive:
- Events like `SSRF_BLOCKED` or `PERMISSION_DENIED` are tagged as security-relevant
- The context trimmer applies a **priority-preserve** rule: tagged security events are **never dropped**, regardless of context window pressure
- General log volume continues to be trimmed normally

This is [[context-aware-trimming|context-aware trimming for security continuity]] — a named operational pattern in this wiki. McMillin's implementation is the earliest documented home-lab instance of this pattern.

> [!gap] Long-running scaling limit
> McMillin notes that as logs increase and agent run times extend, even security-pinned events will eventually fill the context on their own. The open question is "how to adjust as we go" — the pattern needs extension for truly long-running agents. One direction: summarization of older pinned events; another: external secure log store that is queryable but not in-context.

## Control 4: LLM firewall (prompt injection detection) — lessons learned

McMillin deployed an LLM-based prompt injection detection layer. Key observations:

- **Out-of-the-box configuration was too aggressive.** Dozens of legitimate tasks were blocked silently — discovered weeks later when reviewing firewall logs. Example: a blocked task about "including a server time field in every API response."
- **Blocking is non-obvious.** The memory demo during the talk showed the firewall erroneously blocking a legitimate memory retrieval call in real time.
- **Context sensitivity matters.** A security-research agent asking about attack techniques legitimately triggers injection-detection signals. Such agents may need the firewall disabled or configured with a very permissive profile.
- **Tuning is ongoing.** McMillin frames this as "still figuring out where the accepted risk tolerance is for each setting."

This aligns with the general wiki position on [[prompt-injection-containment|Prompt Injection Containment]]: behavioral/ML-based detection is valuable but unreliable as a primary control; deterministic architectural controls are preferred for hard constraints.

## Control 5: Observability and cost tracking

Tools in use: **LangFuse** (trace inspection, which agents are doing what, model-version debugging — caught accidental Opus-where-Sonnet-was-intended substitutions), **Grafana dashboards** (cost, daily breakdown, agent audit trail).

McMillin's framing: cost tracking was the first thing that revealed functional waste. Two specific examples:
- **Memory injection waste**: auto-injecting all memories into context wasted tokens without improving performance
- **Review agent over-triggering**: coding agents were configured (by the LLM at initialization) to run all 5 review sub-agents on every task; 5 review agents × 250K tokens = runaway cost

Both were visible from cost telemetry before they were visible from functional output. This supports the wiki position in [[agent-observability|Agent Observability]] that cost tracking is an early-signal observability primitive, not just a FinOps concern.

**Max iterations per turn**: configured to 3–4. This is both a cost guard and a safety control — prevents runaway execution loops.

## Multi-agent MCP relay (acknowledged insecure prototype)

McMillin has built a prototype "chat room for LLMs" — an MCP relay server that allows two Claude instances to communicate and exchange messages, useful for client-server debugging without copying and pasting between sessions.

He explicitly acknowledges the security issues:
> "This is not the most secure setup right now. It is running on my home lab, where only I interact with it, but it's definitely not production-ready. We're injecting context from one LLM to another. Capabilities are not being scoped down."

The architecture is bidirectional context injection without capability scoping — a canonical [[lethal-trifecta|Lethal Trifecta]] scenario (LLM A's untrusted output becomes LLM B's input without controls). McMillin identifies this as work-in-progress, with Redis backend planned as next step.

## Delegation chains: the open gap

McMillin explicitly names **delegation chains** as the main unsolved problem in his infrastructure:
> "Delegation chains is something I still need to work on. Identity-type stuff — I'm sure the SANS talk later will help with that — but trying to go figure out the best way to pass the same identity everywhere we can."

This directly maps to [[ambient-vs-derived-authority|Ambient vs Derived Authority]] and the open gap at [[pdp-pep-for-non-tool-mediated-actions|PDP/PEP for Non-Tool-Mediated Agent Actions]]. The home-lab version of this problem is identical to the enterprise version: how do you pass a narrowed, trustworthy identity claim from an orchestrating agent down through sub-agents and tools without either (a) propagating full ambient credentials or (b) losing accountability?

## Q&A: the MCP sidecar vs. service-mesh fork

Two Q&A exchanges are worth extracting:

**Q1 (MCP sidecar architecture):** McMillin runs a local stdio MCP server that acts as a controlled sidecar for each agent. It:
- Limits which MCP tools the agent can see (based on the per-agent allowlist)
- Enforces path and filetype restrictions for filesystem tools at call time
- Is spawned per-agent, not shared

**Q2 (Service mesh alternative — raised by an audience member):** The questioner suggests doing enforcement outside the context window via Envoy + OPA, avoiding context pollution entirely. McMillin's response:
- Agrees this is the right direction for production
- His current infrastructure (personal desktop + home-lab) is too immature for a service mesh
- He is actively moving to a K3s setup (containerized agents), after which service-mesh controls become feasible
- This validates the [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]] architectural fork as a real design choice even at hobbyist scale

## Connections to existing wiki content

| Wiki artifact | McMillin's contribution |
|---|---|
| [[least-agency-principle\|Least Agency Principle]] | Confirms the functional-benefit framing: tool scoping improves output quality, not just security. 73-tool-scale evidence. |
| [[memory-poisoning\|Memory Poisoning]] | The \$1,000/month task-manager contamination is a concrete example of agent-to-agent memory leakage at the architectural level (shared namespace → cross-contamination). |
| [[agent-memory-isolation\|Agent Memory Isolation]] | **Primary source.** Class-name-keyed namespace pattern. The "LLM cannot influence its own namespace key" property is the key design invariant. |
| [[context-aware-trimming\|Context-Aware Trimming for Security Continuity]] | **Primary source.** Tag-and-pin security events to survive context window pressure; defends against the N-token attack-hiding pattern. |
| [[prompt-injection-containment\|Prompt Injection Containment for Agentic Systems]] | LLM firewall over-triggering is direct evidence that behavioral/ML controls need per-agent context sensitivity tuning. |
| [[agent-observability\|Agent Observability]] | Cost tracking as early-signal observability; LangFuse for trace debugging; max iterations as operational control. |
| [[mcp-security\|MCP Security]] | Local stdio sidecar pattern (vs enterprise proxy). Per-tool permission levels (read/write/admin) at the MCP server layer. |
| [[ambient-vs-derived-authority\|Ambient vs Derived Authority]] | Delegation chains named as the open gap in his fleet — practitioner confirmation that this is the hardest unsolved problem. |
| [[inline-gateway-vs-runtime-instrumentation\|Inline Gateway vs Runtime Instrumentation]] | Q&A confirms the architectural choice is real even at home-lab scale: sidecar today, service mesh as the production upgrade path. |
| [[agentic-ai-security-cmm-2026\|Agentic AI Security CMM 2026]] | D1 L2 evidence (basic tool scoping); D3 L2–L3 evidence (per-tool permission levels); D6 L2 evidence (memory namespace isolation); D7 L1–L2 evidence (cost tracking, LangFuse trace). |

## See also

- [[least-agency-principle|Least Agency Principle]] · [[mcp-security|MCP Security]] · [[memory-poisoning|Memory Poisoning]] · [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]]
- [[agent-memory-isolation|Agent Memory Isolation]] · [[context-aware-trimming|Context-Aware Trimming for Security Continuity]] (new concept pages from this talk)
- [[breaking-the-lethal-trifecta-talk|Breaking the Lethal Trifecta (Bullen)]] — enterprise-scale counterpart: same tool-scoping / memory-isolation patterns applied via Toolshed and egress proxy
- [[unprompted-conference-march-2026|Unprompted Conference — March 3–4, 2026]]
