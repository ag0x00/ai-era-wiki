---
type: incident
title: "GitSpawn Coding-Agent Git-Config RCE"
created: 2026-09-17
updated: 2026-09-17
tags:
  - incidents
  - agentic-coding
  - coding-agent
  - toolchain-poisoning
  - supply-chain
  - git
  - disclosure
status: developing
scope_axis:
  - sec-of-ai
  - sec-against-ai
origin: aggregated
incident_class: toolchain-poisoning
attack_with_or_on_ai: "on AI"
date_observed: 2026-06-26
date_disclosed: 2026-09-01
target: "Context-gathering git subprocesses in Claude Code, Goose, Qwen Code, Grok Build, Hermes, OpenAI Codex and Cursor"
threat_actor: "research-disclosure"
impact: "Arbitrary code execution as the developer, outside the agent sandbox and before the permission prompt, from a repository delivered as files; four of eight reported findings unpatched at publication"
related:
  - "[[manifold-security|Manifold Security]]"
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
  - "[[claude-code|Claude Code]]"
  - "[[hermes-agent|Hermes]]"
  - "[[cursor-ide|Cursor]]"
  - "[[block|Block]]"
  - "[[vulncheck|VulnCheck]]"
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[agent-sandboxing|Agent Sandboxing]]"
  - "[[guardfall-shell-injection-audit|GuardFall Shell-Injection Audit]]"
  - "[[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[supply-chain-security-for-agents|Supply Chain Security for Agentic AI]]"
  - "[[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Supply Chain and AI-BOM]]"
sources:
  - "https://www.manifold.security/blog/ai-coding-agents-git-hijack"
  - "https://github.com/aaif-goose/goose/security/advisories/GHSA-r5pp-p5r8-466r"
  - "https://www.cve.org/CVERecord?id=CVE-2026-71963"
  - ".raw/articles/ai-coding-agents-git-hijack-2026-09-17.md"
verified: 2026-09-17
verified_against:
  - ".raw/articles/ai-coding-agents-git-hijack-2026-09-17.md"
verified_findings: 0
verified_note: "All claims re-read against the Manifold post; 6 findings fixed on the page (attribution, arithmetic, hedges). Goose GHSA and CVE records not opened."
---

# GitSpawn Coding-Agent Git-Config RCE

## Summary

On 2026-09-01 [[manifold-security|Manifold Security]] published eight findings across seven CLI coding agents under the name **GitSpawn**.[^manifold] Each agent runs `git` as a background subprocess to work out what repository it has been opened in, and each passes the repository's own `.git/config` through to that subprocess unfiltered. Several git configuration settings name a program that git then executes, so the repository chooses what runs. The command runs as the developer, on the host, outside the agent's sandbox, and before the permission model or the workspace-trust prompt is consulted.[^manifold]

The affected products are [[claude-code|Claude Code]] (two separate findings), Goose, Qwen Code, Grok Build, [[hermes-agent|Hermes]], OpenAI Codex and [[cursor-ide|Cursor]]. Manifold states it found the same pattern in further agents it does not name. Four of the eight findings were unpatched at publication, each re-confirmed against a current release beforehand. Two carry CVE identifiers: CVE-2026-72718 against Goose, scored [7.0 by the maintainers](https://github.com/aaif-goose/goose/security/advisories/GHSA-r5pp-p5r8-466r),[^goose] and [CVE-2026-71963](https://www.cve.org/CVERecord?id=CVE-2026-71963) against Hermes, assigned by [[vulncheck|VulnCheck]] as an independent CVE Numbering Authority after the vendor failed to triage the report.[^manifold]

> [!gap] Single-source disclosure
> Every technical detail on this page is Manifold's account. Only the Goose finding carries a vendor advisory of its own; the remaining seven are documented through Manifold's write-up and its screen recordings, and the two CVE records were not opened for this page. Manifold withholds the configuration key behind the second Claude Code finding while it remains unpatched and publishes no proof-of-concept repository, so that finding is documented at the level of mechanism rather than of a reproducible payload. Patch status for the four unpatched findings has not been re-checked here since 2026-09-01.

## Attack Vector

### Index refresh is the execution sink

A coding agent asks git for the current branch, the modified and staged files, the paths a change touched, or the full tracked file list. Manifold gives two of the calls it observed, `git status --porcelain=2 --branch` and `git diff --name-only HEAD`, and notes that neither is unusual.[^manifold] Both commands touch the working tree, and a git command that touches the working tree refreshes the index first.

`core.fsmonitor` is a performance setting for large repositories: rather than stat every file, git asks a helper program what changed, and runs that program during the index refresh. Git reads the setting from the repository's own `.git/config`, so a repository that ships

```
[core]
    fsmonitor = <command>
```

executes `<command>` on any index-refreshing git call.[^manifold] Which command the agent chose does not matter, because the refresh is common to all of them. Manifold states that `core.fsmonitor` is one of several settings of this kind, and that one of its eight findings turns on a different key, which it leaves unnamed.[^manifold]

### Delivery moves a directory rather than cloning one

Manifold states that git does not carry the payload: cloning a hostile URL runs nothing, and neither does `fetch` or `pull`. The repository has to arrive as files with its `.git` directory already inside it, which makes the vector anything that copies a directory: a shared `.zip`, a shared drive, a sync folder, a USB stick.[^manifold] Manifold used a `.zip` for every proof of concept. Projects passed between colleagues and handed from consultants to clients travel this way, and no registry, package manager or clone-time control sits on that path.

### The subprocess precedes every control the agent has

The git call belongs to the agent's own code rather than to a tool the model asked for, so it runs outside the sandbox and raises no approval prompt.[^manifold] Manifold's Claude Code recording shows the payload dropping a marker file while the workspace-trust prompt is still on screen waiting to be accepted, and its Qwen Code case executes before the user has authenticated.[^manifold] The permission model, the sandbox and the trust prompt are all downstream of a subprocess that has already run. This is the ordering property the [[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]] established for a harness configuration directory, reached here through an artifact the agent never reads and never authored.

## Findings by agent

Sinks, reporting dates, vendor responses and status are Manifold's, taken from its published timeline.[^manifold]

| Agent | Sink | Reported | Vendor response | Status at publication | CVE |
|---|---|---|---|---|---|
| Claude Code (startup) | `core.fsmonitor` | 2026-06-26 | Closed as duplicate of a same-day report | Patched in 2.1.196 | none |
| Qwen Code | `core.fsmonitor` | 2026-07-07 | Accepted by Alibaba's security response centre | Unpatched at 0.22.3 | none |
| Cursor | not published | 2026-07-08 | Closed as duplicate of an earlier report | Patched | none |
| Goose | `core.fsmonitor` | 2026-07-13 | Acknowledged, CVE assigned | Patched in 1.44.0 | CVE-2026-72718 |
| Grok Build | `core.fsmonitor` | 2026-07-14 | Closed as duplicate of a 1 July report xAI had closed as informative | Unpatched at 1.0.13 | none |
| Claude Code (`ultrareview`) | undisclosed second key | 2026-07-15 | Closed as duplicate of an internal ticket | Unpatched at 2.1.252 | none |
| Hermes | `core.fsmonitor` | 2026-07-20 | No triage after six contacts across five channels | Unpatched at 0.21.0 | CVE-2026-71963 |
| OpenAI Codex | not published | 2026-07-20 | Closed as duplicate of an earlier report | Patched | none |

### Trigger and confirmed version, by case study

- **Claude Code, startup.** `git status` gathers repository context as an internal subprocess when a folder is opened with `claude`. Confirmed on 2.1.193.
- **Claude Code, `ultrareview`.** The review path does not strip the second configuration key, and `claude ultrareview` runs the repository's command at start-up, before the workspace-trust prompt is shown and before the requested review begins. Reported against 2.1.210, re-confirmed unpatched on 2.1.252 on 2026-09-01.
- **Goose.** `goose review` builds the working diff with `git diff`, passing one configuration flag, `core.quotePath=off`, and stripping none. The payload runs before goose contacts the model. Affected 1.41.0, fixed in 1.44.0.
- **Hermes.** `git status` in the session directory gathers repository context, and the payload runs when the user sends the first message. Confirmed on 0.18.2 on 2026-07-19 and again on 0.21.0 on 2026-09-01.
- **Qwen Code.** `git status` runs at start-up, before authentication. Confirmed on 0.19.6, and again on 0.22.3 on 2026-09-01.
- **Grok Build.** The context-gathering call fires on the first keystroke of a prompt, before any message is sent. Confirmed on 0.2.93, and again on 1.0.13 on 2026-09-01.

OpenAI Codex and Cursor were added in a 2026-09-01 update to the post. Manifold states the Codex variant differs in mechanism while belonging to the same class, and publishes no case study for either, because both came back as duplicates of reports other researchers had already filed and both had been patched by then.[^manifold]

## Disclosure record

Manifold states that five of its eight reports came back as duplicates of findings other researchers had filed independently, one of them on the same day as its own.[^manifold] Its timeline records one of those five, the `ultrareview` finding, as a duplicate of an internal vendor ticket rather than of another researcher's report. The Grok Build duplicate points at a 2026-07-01 report that xAI had already closed as informative, so the class had been reported to that vendor and dismissed before Manifold arrived. Manifold reads the duplicate rate as evidence that the pattern is being found from several directions at once, which is the argument it gives for publishing while four findings remain live.

Two responses stand apart. Alibaba's security response centre accepted the Qwen Code report and the finding is still unpatched at 0.22.3, three minor releases after the version it was reported against. The Hermes project received six contact attempts across five channels and never triaged the private GitHub advisory, and [[vulncheck|VulnCheck]] assigned CVE-2026-71963 as an independent CNA in the vendor's place.

Manifold states it kept the published detail deliberately minimal: a sink, a trigger and a recording per finding, the mechanism in general terms, no ready-made repository, and the second configuration key withheld while the finding it turns on stays unpatched.

## Mitigations

**Receiving a repository as files.** Inspect `.git/config` before the directory is opened with an agent, and treat any setting that names a program as executable content.[^manifold] The check is cheap, and on the recipient's side it is the control the delivery path leaves available, because the payload never passes through a clone, a registry or a package manager.

**Shipping a coding agent.** Sanitize the git configuration on every context-gathering call the product makes in the background. Manifold's worked form is `git -c core.fsmonitor=false status`.[^manifold] The instruction generalizes past the one key: a background subprocess a harness spawns for its own bookkeeping needs an explicit configuration allowlist, because the repository supplies the configuration and the repository is the untrusted input.

## Defensive Lessons

**A subprocess the harness spawns for itself sits outside every control the harness advertises.** The sandbox, the permission prompt and the workspace-trust gate all govern actions the model asks for, and context gathering is not one of those. The question to ask of a coding agent is which processes it starts before its first model call, and under what configuration; [[agent-sandboxing|Agent Sandboxing]] carries that limit and [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4]] the runtime-control ladder it sits under.

**Executable configuration is not confined to the harness config tree.** [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] was written around directories the harness itself reads, `.claude/` and its analogues. `.git/config` is read by git, not by the agent, and the agent's only contribution is to hand it to a program that honours it. Any file the agent passes to a third-party tool carries that tool's configuration semantics, and the agent inherits them.

**An artifact that arrives as files defeats acquisition-side supply-chain control.** [[agentic-ai-security-cmm-d8-supply-chain|CMM D8]] grades what an organization pulls from a registry and what its own workloads push to one. A project copied from a shared drive or unzipped from an email attachment passes neither gate, and it is the ordinary way work moves between a consultancy and a client.

**Independent duplicate reports are a disclosure-timing signal.** Five of the eight reports came back as duplicates, most of them of findings other researchers had filed and one of those on the same day, which indicates the class is reachable by anyone who reads a coding agent's process tree. A vendor treating a report as low priority because no public exploit exists is estimating from the wrong population.

**The pattern travels with the artifact class rather than with git.** Manifold's closing generalization is that skills, MCP servers and plugins also arrive as files, carry their own configuration and are trusted on arrival for the same reason a repository is.[^manifold] The claim is consistent with the incident record the wiki already carries: [[guardfall-shell-injection-audit|the GuardFall audit]] drove ten of eleven coding agents into shell execution through READMEs, Makefiles and MCP servers that arrived with the repository, and [[gemini-cli-workspace-trust-rce|the Gemini CLI advisory]] executed from a configuration directory inside a pull request.

## Assessment

The mechanism is ordinary and verifiable: `core.fsmonitor` is documented git behaviour, the index-refresh path is public, and the delivery constraint Manifold states, that clone and fetch do not carry the payload, follows from where git reads local configuration. Two findings carry identifiers assigned outside Manifold, one by the Goose maintainers and one by VulnCheck as an independent CNA, which puts the class on record in two places Manifold does not control. The remaining findings rest on Manifold's recordings.

Three limits bound what this page supports. The version and date evidence is entirely Manifold's, and no vendor besides the Goose maintainers has published a corroborating advisory. Star counts and download volume are Manifold's aggregation, given without per-repository links or a retrieval date: over 77 million monthly npm downloads for Claude Code, [measured over 2026-07-28 to 2026-08-27](https://api.npmjs.org/downloads/point/2026-07-28:2026-08-27/@anthropic-ai/claude-code),[^npm] and GitHub star totals of over 237,000 for Hermes, 143,000 or more for Claude Code, 54,000 or more for Goose, 27,000 or more for Qwen Code and 26,000 for Grok Build, which Manifold adds to close to half a million.[^stars] Neither figure measures exposure, because the vector needs a repository that arrived as files. And Manifold sells runtime observation of agent behaviour, so its framing of the finding as a gap that endpoint detection and gateways cannot see, discussed at [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]], is also its product pitch.

## Notes

[^manifold]: [Manifold Security — GitSpawn: A Single Flaw Lets Untrusted Repos Run Code in Claude Code, Codex, Cursor, and Grok](https://www.manifold.security/blog/ai-coding-agents-git-hijack), Francisco Rosales, 2026-09-01. Source for the mechanism, the delivery constraint, the per-agent sinks and triggers, the affected and re-confirmed versions, the disclosure timeline, the mitigations and the closing generalization. Local copy at `.raw/articles/ai-coding-agents-git-hijack-2026-09-17.md`.
[^goose]: [GitHub Advisory Database — GHSA-r5pp-p5r8-466r](https://github.com/aaif-goose/goose/security/advisories/GHSA-r5pp-p5r8-466r). The advisory Manifold cites for CVE-2026-72718 and for the severity score of 7.0 the maintainers assigned after its report. The advisory's own contents have not been read here; the affected and fixed versions 1.41.0 and 1.44.0 are Manifold's.
[^npm]: [npm registry downloads API, `@anthropic-ai/claude-code`, 2026-07-28 to 2026-08-27](https://api.npmjs.org/downloads/point/2026-07-28:2026-08-27/@anthropic-ai/claude-code). Package downloads over a 31-day window, the endpoint Manifold cites for the figure. Download counts include mirrors, continuous-integration installs and re-installs, so they bound the package's distribution rather than its installed base.
[^stars]: Star totals as stated in the TL;DR of [Manifold's post](https://www.manifold.security/blog/ai-coding-agents-git-hijack), 2026-09-01. Manifold gives no repository URLs and no retrieval date, and a GitHub star count measures account-level interest rather than installation.
