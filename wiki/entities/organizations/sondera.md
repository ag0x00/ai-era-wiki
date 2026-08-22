---
type: entity
entity_type: organization
org_type: vendor
title: "Sondera"
created: 2026-05-03
updated: 2026-05-23
tags:
  - organizations
  - coding-agents
  - cedar
  - reference-monitor
  - policy-engine
  - open-source
status: seed
homepage: "https://www.sondera.ai"
related:
  - "[[matt-maisel]]"
  - "[[hooking-coding-agents-with-cedar-talk]]"
  - "[[cedar]]"
  - "[[oversight-layer]]"
sources:
  - "https://www.sondera.ai"
  - .raw/talks/2026-03-03_Matt-Maisel_Hooking-Coding-Agents-with-Cedar_slides.pdf
  - .raw/talks/2026-03-03_Matt-Maisel_Hooking-Coding-Agents-with-Cedar_transcript.md
---

# Sondera

**Sources:** [Homepage](https://www.sondera.ai) · [[matt-maisel|Matt Maisel]] talk at Unprompted March 2026; Sondera research blog mentioned in talk (released research the day before the talk, March 2, 2026).

Sondera is a startup building "trustworthy agent partners." Co-founded by [[matt-maisel|Matt Maisel]] (CTO). Focus: deterministic runtime policy enforcement for coding agents via a hook-based Cedar harness.

> [!stale] Earlier name confusion (corrected 2026-05-04)
> The auto-generated transcript spelled the org name as "Sendera." The slide deck (and the company's actual homepage at sondera.ai) confirm the spelling is **Sondera**. The wiki page was renamed `sendera.md` → `sondera.md` and all cross-references updated. Lesson recorded: trust slides over transcripts for proper-noun spelling.

## Products

An **open-source Cedar policy engine integrated with coding-agent hooks** — a deterministic reference monitor that sits outside the model and mediates every trajectory event (action, observation, control, state) for coding agents including Cursor, Claude Code, and Gemini CLI.

The harness includes:
- Per-agent **local adapters** (command wrappers intercepting hook events)
- A **harness service** running Cedar with YARA signatures, IFC taint tracking, and a safety model
- A **policy agent** that authors and validates Cedar policies using Cedar's formal tools over MCP
- A parallel **Cedar policy engine for Python agent libraries** (LangChain, Strands, AWS ADK, and others)

## Relationship to Tenuo

[[tenuo|Tenuo]] (Niyikiza) and Sondera (Maisel) both use [[cedar|Cedar]] and both address the agent authorization problem — but at different layers. Tenuo's warrants address **delegation-time authorization** (what scope can be granted to a sub-agent). Sondera's harness addresses **run-time per-action enforcement** (what can a single coding agent do in a given turn). The two are complementary and were presented on consecutive days at Unprompted March 2026.

## See also

- [[matt-maisel|Matt Maisel]] — CTO and co-founder
- [[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar (talk)]]
- [[cedar|Cedar]] — the policy language used
- [[tenuo|Tenuo]] — complementary company using Cedar for delegation-aware authorization
