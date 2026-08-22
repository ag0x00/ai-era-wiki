---
type: entity
entity_type: person
title: "Brooks McMillin"
created: 2026-05-03
updated: 2026-05-03
tags:
  - entities
  - people
  - dropbox
  - agentic-ai
  - mcp-security
  - unprompted-2026
status: seed
related:
  - "[[dropbox]]"
  - "[[building-secure-agentic-systems-talk]]"
  - "[[agent-memory-isolation]]"
  - "[[context-aware-trimming]]"
  - "[[mcp-security]]"
sources:
  - .raw/talks/2026-03-04_Brooks-McMillin_Building-Secure-Agentic-Systems_transcript.md
  - .raw/talks/2026-03-04_Brooks-McMillin_Building-Secure-Agentic-Systems_slides.pdf
---

# Brooks McMillin

Infrastructure Cloud Security Engineer at [[dropbox|Dropbox]]. Speaker at [[unprompted-conference-march-2026|Unprompted Conference]], March 4, 2026 (Stage 1, 11:45).

## Context

McMillin's Unprompted talk describes a personal agentic infrastructure project, not Dropbox work. He runs 19 agents with 73 MCP tools continuously on his own home-lab setup (personal desktop + Proxmox LXC), using it as a low-stakes test bed for agentic-AI security patterns. All code is open source on his GitHub.

## Contributions in this wiki

- **[[building-secure-agentic-systems-talk|Building Secure Agentic Systems]]** — Unprompted, March 4, 2026. Practitioner account of discovering and fixing memory isolation failures, context-aware security-event pinning, and per-agent MCP tool scoping at personal-fleet scale. Named delegation chains as his main open problem.

## Key framing

> "That's a great example of how improved security also improves actual functionality."

McMillin's thesis is that scoping MCP tools to each agent's actual role solves both a security problem (reduced blast radius) and a functional problem (reduced context noise at 73-tool scale). Security and functionality are the same optimization.

## Open GitHub project

Personal agent framework available for inspection (not a packaged product — "it does all run for me, so good luck if you want to go try"). Includes: custom task management system, cron-driven code optimization loop, LLM-to-LLM MCP relay (acknowledged insecure prototype), OAuth-based remote agent on Proxmox LXC.

## External

LinkedIn / email: referenced in closing slide (specific handles not transcribed).

> [!gap] Stub
> GitHub username not captured in the transcript. GitHub profile would give direct access to the open-source agent code referenced throughout the talk.
