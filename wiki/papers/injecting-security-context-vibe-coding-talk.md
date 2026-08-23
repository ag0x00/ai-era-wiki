---
type: talk
title: "Injecting Security Context During Vibe Coding"
address: c-000320
created: 2026-08-22
updated: 2026-08-22
tags:
  - papers
  - talks
  - mcp
  - vibe-coding
  - appsec
  - secure-by-design
  - threat-modeling
  - owasp
status: summarized
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
origin: aggregated
year: 2026
authors: ["Srajan Gupta"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 4, 2026 (Day 2 / Stage 2)"
key_claim: "Security requirements can be supplied to a coding agent as retrievable context before it writes code, and the generated code checked against those same requirements immediately afterwards, both through one MCP server the agent calls from inside the IDE. Gupta's argument for the placement is that an AppSec finding delivered by CI arrives after the intent that produced the code has left the developer's chat window, and that a centralized server is the only place org-wide guidance can live when the alternative is copying it into every repository."
methodology: "Practitioner talk with recorded demo; transcript and slide deck captured to .raw/talks/. The demo is a single paired run — one webhook-endpoint task on the open-source Obot codebase, generated once with the pre-coding review disabled and once with it enabled. Gupta reports no measurement of residual defect rates and, asked for one during Q&A, described qualitative benefit on business-logic issues instead."
source_url: "https://unpromptedcon.org/abstract-march2026/"
related:
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[mcp-security|MCP Security]]"
  - "[[security-guidance-plugin|Security Guidance Plugin]]"
  - "[[vibe-coding|Vibe Coding]]"
  - "[[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar]]"
  - "[[guardrails-beyond-vibes-talk|Guardrails Beyond Vibes]]"
  - "[[unprompted-conference-march-2026|Unprompted Conference I]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - https://unpromptedcon.org/abstract-march2026/
  - .raw/talks/2026-03-04_Srajan-Gupta_Injecting-Security-Context-During-Vibe-Coding_transcript.md
  - .raw/talks/2026-03-04_Srajan-Gupta_Injecting-Security-Context-During-Vibe-Coding_slides.pdf
---

# Injecting Security Context During Vibe Coding

**Source:** [Conference abstracts page](https://unpromptedcon.org/abstract-march2026/) · [Conference agenda](https://unpromptedcon.org/#) · [[unprompted-conference-march-2026|Unprompted Conference I]]

A practitioner talk by Srajan Gupta, a senior security engineer at the fintech company Dave, whose stated focus areas are threat modeling and security-by-design. Gupta demonstrates an MCP server that supplies security requirements to a coding agent before it generates code, then checks the generated code against those same requirements. The agent in the demonstration is [[cursor-ide|Cursor]], the guidance it retrieves is drawn from [[owasp|OWASP]] cheat sheets alongside an organization's internal standards, and the implementation is released as `ai-security-crew` (Python, MIT licence). The dotted tool names and file paths below belong to that server, or to the demo repository it ran against.

## The generation loop as the control point

Gupta's argument is about placement rather than about detection. Conventional AppSec controls fire after the code exists: pull-request review, then CI scanning, then a ticket, then a pentest. Each of those steps returns its finding to a developer who has moved on, and the fix costs more because the feature has hardened around the defect. Under deadline pressure the gates get bypassed, which is how Gupta says AppSec becomes "the 'no' team by accident".

Agentic coding shortens the interval between intent and shipped code enough that this ordering stops working. The relevant window is the one in which the developer's requirement is still in the chat session, because that is when a correction is cheap and the context needed to judge it is still present.

The proposal is a three-step loop the agent runs on every task: retrieve security context before writing, verify the output immediately after generation, and patch inline while the intent is fresh. Gupta claims fewer findings reach CI, fixes land faster, and rework drops for both the developer and the AppSec team, with the developer never leaving the IDE.

## The security context pack

The retrieved bundle is a **security context pack** — a structured set of constraints scoped to the specific change, rather than a general secure-coding preamble. The deck names its contents as data classification, authorization rules, validation requirements, logging redaction, and allowed dependencies. Five sources feed it: Jira for scope, acceptance criteria and the data touched; Confluence or equivalent docs for the PRD, architecture and trust boundaries; OWASP cheat sheets for established safe patterns; internal standards for authentication, authorization, logging, secrets and PII handling; and repository signals for existing patterns, libraries and helpers.

Gupta's worked justification for the internal-standards source is a developer building on a PCI application that handles a card identifier, where compliance policy forbids writing that identifier into error messages. The model cannot infer that rule from the repository or from public training data, so a correct generation depends on the rule being supplied.

Guidelines carry category labels, and the agent retrieves only the categories the task matches — a task involving JWTs pulls the authentication guidance and leaves the rest. Gupta's stated reason for labelling is to keep retrieval from growing into a monolithic prompt, and he instructs anyone loading internal standards into the server to label them the same way.

## The tool surface

| Tool | Supplies |
|---|---|
| `jira.getIssue`, `jira.getAcceptanceCriteria` | Ticket scope and acceptance criteria |
| `confluence.getPage`, `confluence.search` | PRD, architecture, trust boundaries |
| `security.lookupCheatSheet` | OWASP and internal cheat sheets |
| `policy.getGuidelines` | Organization-specific standards |
| `scan.run`, `patch.apply` | Secrets scanning, SAST and tests, then the patch |

That list is the design, and the demonstration does not exercise it directly. What the agent calls on screen is two coarser tools that wrap the same work: `general_lightweight_security_review` before any code is written, then `general_verify_code_security` against the requirements the first call identified, with generation between them. The table above states a target; for what exists, the authority is the released repository.

## Choosing MCP over hooks and skills

The design comparison is the most transferable part of the talk, because it names the cost the pattern leaves unpaid.

Against packaging the guidance as an agent skill, Gupta's objection is distribution. An organization with a thousand repositories would need the skill placed in each one, and every revision to the guidance re-propagated across all of them. A server holds one copy that every repository reaches, which is why he treats centralization as the requirement that rules skills out at organizational scale.

Against hooks, the objection runs the other way, and Gupta concedes it. A hook fires deterministically; an MCP tool runs only when the model reads its description and elects to call it. Asked in Q&A how he ensures the agent consults the server, Gupta answered that invocation depends on the wording of the tool description, and that it does not happen every time. He chose MCP for portability across coding agents, since hooks at the time of the talk were supported only by Claude Code and Cursor, and he names hooks as the way to make the calls deterministic where an organization can accept that narrower support.

Gupta's own phrasing hides a condition on that conclusion. Hooks were unavailable across the harnesses he wanted to reach; an organization running only Claude Code and Cursor has hook support on every seat, so the portability he bought costs it nothing and the determinism he surrendered comes free. Read the exclusion list as a test to apply rather than as a result to adopt: the argument for MCP is strong in proportion to how much of the fleet sits outside those two products.

That leaves the enforcement question open in the version demonstrated: the control is available to the agent rather than imposed on it. [[hooking-coding-agents-with-cedar-talk|Matt Maisel's Cedar talk]], from the previous day of the same conference, takes the other branch — lifecycle hooks routing every agent action through a policy engine outside the model, which buys the determinism Gupta gives up and pays for it in IDE coverage.

## The demo

The target codebase is Obot, an open-source MCP observability tool Gupta cloned for the demonstration and has no affiliation with. The task prompt asks for a `POST /api/webhooks/{agent_id}` endpoint in the gateway server that accepts webhook payloads from external services such as GitHub and triggers agent workflow executions, with the handler in `pkg/gateway/server/webhook.go` and the route registered in `pkg/gateway/server/router.go`.

Generated with the tools disabled, the code drew two findings at the verification step: sensitive data leaking through forwarded headers, and authorization tokens passed through the workflow.

Generated with the tools enabled, the pre-coding call returned HIGH risk against the categories API security, data validation and web security, with guidance to validate all inputs, filter sensitive headers, handle errors properly, cap body size, and apply least privilege. The resulting change was 124 lines in `webhook.go`, three lines in `router.go` and one in `authz.go`, with the sensitive-header blocklist present in the first draft. Post-generation verification reported no critical, high or medium severity issues.

The contrast is one paired run on one task, which establishes that the mechanism operates end to end and does not measure how well it generalizes. The finding it eliminates, header forwarding, belongs to the category the context pack named in advance, so the result is what the design predicts and carries no information about the defect classes the pack fails to name. Gupta presents it as a walkthrough rather than as evidence of a rate.

## Limits Gupta states

- The pattern does not replace threat modeling for new systems.
- Large systems and large changes still need human review.
- Its value is in repetitive work and known patterns, catching common vulnerability classes early.
- Efficacy is unmeasured. Asked directly what residual security bugs survive the process, Gupta reported qualitative benefit on business-logic issues that scanners do not catch, and gave no figure.

## The framing statistics

Three of the deck's opening figures carry attributions:

| Figure | Attribution given on the deck |
|---|---|
| 75% of developers use AI to write code | Accelerate State of DevOps |
| 41% of code on GitHub is AI-generated | Microsoft, at the Morgan Stanley TMT Conference |
| 65% of LLM-generated code judged insecure | "Study of LLM Secure Code Generation" |

A second set carries none: a 100:1 developer-to-security ratio, an average of 569,354 issues, code velocity rising 30–40%, and a failure to detect a bug roughly 40% of the time. The deck names no study for any of the four, and none is traceable to a primary document from the talk materials alone. They are the talk's framing and are recorded here as such rather than as citable measurements.

## State of the implementation

Read on 2026-08-22, the repository is `Srajangpt1/ai-security-crew` — Python under an MIT licence, last pushed 2026-04-26, and reached from the slide's URL through a rename. The package is `mcp_security_review`, and the Jira and Confluence providers the deck describes are present as source rather than as plan.

It has also grown a second delivery path since March. Alongside the MCP server it now ships a Claude Code plugin manifest, three slash commands, and three skills — `security-review`, `threat-model` and `verify-code`. The skills are not thin wrappers that call the server: `security-review` carries its technology-detection list, its risk tiers and its assessment steps inline, so the guidance travels with the skill file. That is the distribution model Gupta argued against on stage, shipped by the same project within two months of it. The argument and the artifact can both stand, since a skill is the ergonomic way to reach one repository and a server is the maintainable way to hold guidance for a thousand, but a reader taking the centralization claim as settled should know the author now ships both.

## Placement

Anthropic's [[security-guidance-plugin|Security Guidance plugin]] is the nearest first-party instrument, and the pair divides along two axes rather than one. The plugin fires on every write because it is a hook, and matches against a fixed catalogue of general dangerous constructs; Gupta's server fires only when the model elects to call it, and carries the organization-specific rules a shipped pattern catalogue cannot contain. An enforcement point needs both properties, and neither product has both.

The talk is the pre-generation half of [[securing-agentic-coding|Securing Agentic Coding]]. Most of the controls collected there act on what an agent has already produced or attempted — scanning the diff, gating the pull request, constraining the agent at execution time. Gupta's server acts earlier, on the prompt, and treats the absence of organizational context in the model as the defect to fix. His closing claim states the position directly: vibe coding fails when the security context is missing.

For [[mcp-security|MCP Security]] the talk supplies a case that runs against the page's dominant framing. MCP appears in this vault mostly as attack surface — a protocol whose servers are exposed, over-permissioned, or hostile. Here it is the delivery mechanism for a security control, and the properties Gupta selects it for are the ones that make it dangerous elsewhere: it reaches into the IDE, composes freely with Jira, Confluence and scanners, and holds a central position that every agent consults. The same central position that makes the pattern scale would make a compromise of that server a supply-chain path into every generation loop it feeds, which the talk does not address.

Against [[vibe-coding|Vibe Coding]], the talk is evidence that the term has stabilized into something an enterprise AppSec function plans around. Gupta opens on the March 2025 case of a developer who publicized a vibe-coded application, was attacked within about a week, and shut it down — and observes that a year later the same context problem is unresolved.
