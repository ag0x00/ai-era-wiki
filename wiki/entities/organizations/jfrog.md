---
type: entity
entity_type: organization
org_type: vendor
title: "JFrog"
address: c-000125
created: 2026-05-25
updated: 2026-09-25
tags:
  - entities
  - organization
  - vendor
  - supply-chain
  - artifact-management
  - sec-against-ai
status: developing
scope_axis: [sec-against-ai, ai-in-sec-defense, sec-of-ai]
homepage: https://jfrog.com
related:
  - "[[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Supply Chain and AI-BOM]]"
  - "[[jfrog-ssc-state-of-union-2026|2026 Software Supply Chain Security State of the Union]]"
  - "[[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]]"
  - "[[slopsquatting|Slopsquatting]]"
  - "[[ai-bom|AI-BOM]]"
  - "[[artifactory|JFrog Artifactory]]"
  - "[[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]"
  - "[[supply-chain-security-for-agents|Supply Chain Security for Agentic AI]]"
  - "[[mcp-security|MCP Security]]"
sources:
  - https://jfrog.com
  - https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs
  - https://jfrog.com/press-room/jfrog-embeds-security-into-the-agentic-workforce/
  - https://jfrog.com/blog/agent-guard-control-ai-assets-before-they-become-shadow-ai/
verified: 2026-09-25
verified_against:
  - ".raw/papers/jfrog-ssc-state-of-union-2026.pdf"
verified_findings: 0
verified_note: "Read whole 2026-09-25 (#283 D8 satellite): SSC report p.4 platform line and figure types; incident sentences also against the Black Hat transcript; AgentSecOps release and Agent Guard blog live; BusinessWire 403; 5 low fixed (capability count, Agent Guard and APM attribution, quote comma, workstation scope); none open"
---

# JFrog

**Sources:** [JFrog (homepage)](https://jfrog.com) · [JFrog Security Research](https://jfrog.com/blog/category/security-research/) · [2026 SSC report announcement (BusinessWire)](https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs)

JFrog is a software supply chain platform vendor, built around the Artifactory binary repository and a security suite (Xray, Advanced Security) that scans artifacts, dependencies, and AI/ML model files. Its platform is the system of record for thousands of organizations, including most of the Fortune 100, which gives it telemetry across billions of software artifacts.

On this wiki JFrog appears in three roles. The first is as a measurement source for software-supply-chain risk. Its JFrog Security Research team publishes the annual [[jfrog-ssc-state-of-union-2026|Software Supply Chain Security State of the Union]], the first-party origin of the malicious-npm surge figure, the annual CVE-volume count, the malicious Hugging Face model tally, and the counts of attack surface in agentic developer tooling, cited across [[slopsquatting|Slopsquatting]], the [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] thesis, and [[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]]. The precise figures and their page references live on [[jfrog-ssc-state-of-union-2026|the report summary]].

The second role is as a target. During the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] (May–July 2026), autonomous evaluation agents found and exploited two zero-day chains in an internal [[artifactory|JFrog Artifactory]] deployment: a legacy token-refresh endpoint that accepted a token with an invalid signature and returned a validly signed administrative token, and a deserialization path in which a staged malicious Ruby object cached as repository dependency data reached a JRuby time-of-check/time-of-use flaw, yielding remote code execution and theft of the Artifactory administrative signing key. OpenAI notified JFrog during the remediation that ended on 2026-07-06, and a patched service was redeployed; the source does not state the remediation status of the second chain. Both were described publicly at Black Hat USA 2026 by Michael Dalton and Eric Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]. The product page [[artifactory|JFrog Artifactory]] carries the technical detail. The discovery was fully autonomous, ran against a production instance of a widely deployed enterprise application, and placed a package-manager and caching proxy on the agentic attack surface rather than only on the artifact-integrity side of it. A third role appeared in September 2026: JFrog itself supplies agent-facing supply-chain controls, described below.

## AgentSecOps (September 2026)

At swampUP 2026 on 2026-09-02, JFrog extended [[artifactory|Artifactory]] and Xray to cover the artifacts AI coding agents consume and produce, under its own term **AgentSecOps** ([press release](https://jfrog.com/press-room/jfrog-embeds-security-into-the-agentic-workforce/)). The release names seven capabilities, four protecting what agents consume and three controlling how they build. AI Asset Scanning indexes, scans and blocks risky or malicious models, MCP servers, skills and plugins, and applies semantic analysis to Markdown files, skill scripts and instruction sets, blocking an asset it judges malicious before it reaches a developer's workstation. Agent Guard enforces project-scoped allow and deny policies inside developer tools such as Claude Code, Cursor and VS Code, so an agent consumes only approved AI assets. JFrog's product blog describes it as an approved-only proxy that shows an authenticated developer's agent only the MCP servers a project admin approved, with a hook in VS Code and Cursor that denies an MCP tool call routed around it, and states that it supported MCP servers alone on 2026-07-29 ([JFrog blog](https://jfrog.com/blog/agent-guard-control-ai-assets-before-they-become-shadow-ai/)). An Agent Plugins registry governs coding-agent plugins through Agent Guard, and an Agent Package Manager registry integrates the Microsoft-led APM standard into Artifactory to package and version prompts, skills and MCP servers with pinned versions. On the build side, the JFrog Agent Plugin carries the organization's policies into agent workflows, Agent Package Resolution routes an agent's dependency fetches through Artifactory, and Traffic Controller with SASE partners blocks direct reads of public registries at the network layer.

Yoav Landman, co-founder and CTO, stated the premise: *"AI coding agents not only write software at machine speed, but also consume software at scale."* The release states the capabilities were available to customers on announcement, and it gives no pricing and no detection rate for the semantic analysis. The controls land on [[supply-chain-security-for-agents|supply chain security for agentic AI]] as a resolution-time layer above the install-time scanning that practice already carries, and on [[agentic-ai-security-cmm-d8-supply-chain|CMM D8]]'s MCP server / skill provenance row as a named COTS implementation of the pre-install scan D8-ABILITY-SCAN grades at L3. The Agent Package Manager registry adds no cryptographic name-to-binary signing, which D8-SIGN-MCP asks of an MCP server's author at L5+.
