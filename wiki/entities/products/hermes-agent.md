---
type: entity
entity_type: product
title: "Hermes"
address: c-000267
created: 2026-08-15
updated: 2026-09-17
tags:
  - entities
  - products
  - agentic-framework
  - coding-agent
  - open-source
status: developing
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
  - sec-of-ai
origin: aggregated
homepage: "https://hermes-agent.org/"
aliases:
  - "Hermes Agent"
related:
  - "[[openclaw|OpenClaw]]"
  - "[[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]]"
  - "[[dream-taiwan-multi-agent-ai-attack|Taiwan Multi-Agent Attack Reconstruction]]"
  - "[[offensive-agent-collective|Offensive Agent Collective]]"
  - "[[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]]"
  - "[[manifold-security|Manifold Security]]"
  - "[[vulncheck|VulnCheck]]"
sources:
  - "https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia"
  - "https://www.manifold.security/blog/ai-coding-agents-git-hijack"
  - "https://www.cve.org/CVERecord?id=CVE-2026-71963"
  - ".raw/articles/ai-coding-agents-git-hijack-2026-09-17.md"
verified: 2026-09-17
verified_against:
  - ".raw/articles/ai-coding-agents-git-hijack-2026-09-17.md"
verified_findings: 0
verified_note: "GitSpawn section verified; project identification still rests on a name match, stated in the page's gap callout."
---

# Hermes

Open-source agentic orchestration framework ([hermes-agent.org](https://hermes-agent.org/)) that turns an underlying LLM into a task-executing agent capable of multistep, tool-using operation. It entered this wiki through the [[dream-taiwan-multi-agent-ai-attack|Dream Security reconstruction]], which found it deployed alongside [[openclaw|OpenClaw]] as one of two orchestration platforms underlying the [[taiwan-ai-agent-government-intrusion|Taiwan multi-agent AI intrusion]], operating under the workspace identifier `.hermes`.

A second research account arrived on 2026-09-01 and describes the ordinary developer use of the same project: a CLI agent opened on a repository, which runs `git status` in the session directory to gather repository context when the user sends a first message.[^manifold] [[manifold-security|Manifold Security]] reports over 237,000 GitHub stars for the project, the largest count in its five-agent set.[^manifold]

> [!gap] Project identification rests on the name
> Manifold links no repository and names no maintainer, so treating its Hermes as Dream Security's Hermes rests on the shared project name and on both accounts describing an open-source agent run from a working directory. Neither source is the project's own documentation. The install-base figure is a star count, which measures account-level interest rather than deployment, and the offensive-use characterization remains Dream's framing.

## Security record

Hermes carries the least responsive disclosure outcome in the [[gitspawn-coding-agent-git-config-rce|GitSpawn]] set. Its context-gathering `git status` passes the repository's own `.git/config` through untouched, so a repository delivered as files can name a `core.fsmonitor` helper that runs on the host as the user. Manifold confirmed the finding on 0.18.2 on 2026-07-19, reported it the next day, and confirmed it again on 0.21.0 on 2026-09-01 after six contact attempts across five channels left the private advisory untriaged.[^manifold] [[vulncheck|VulnCheck]] assigned [CVE-2026-71963](https://www.cve.org/CVERecord?id=CVE-2026-71963) as an independent CVE Numbering Authority in the vendor's place. The finding was unpatched at publication.

An unresponsive maintainer is the operationally relevant fact for a project already documented as offensive infrastructure: no fix had been acknowledged as of 2026-09-01, and the CVE exists because a third party issued it.

## Notes

[^manifold]: [Manifold Security — GitSpawn: A Single Flaw Lets Untrusted Repos Run Code in Claude Code, Codex, Cursor, and Grok](https://www.manifold.security/blog/ai-coding-agents-git-hijack), Francisco Rosales, 2026-09-01. Source for the Hermes sink, trigger, affected versions, disclosure record and star count; the star total is stated in the post's TL;DR without a repository link or retrieval date. Summarized at [[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]].

Source for the offensive deployment: [Taiwan Multi-Agent Attack Reconstruction](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia), Dream Research Labs, 2026-08-12.
