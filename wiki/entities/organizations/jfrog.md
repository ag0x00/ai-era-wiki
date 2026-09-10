---
type: entity
entity_type: organization
org_type: vendor
title: "JFrog"
address: c-000125
created: 2026-05-25
updated: 2026-09-10
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
---

# JFrog

**Sources:** [JFrog (homepage)](https://jfrog.com) · [JFrog Security Research](https://jfrog.com/blog/category/security-research/) · [2026 SSC report announcement (BusinessWire)](https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs)

JFrog is a software supply chain platform vendor, built around the Artifactory binary repository and a security suite (Xray, Advanced Security) that scans artifacts, dependencies, and AI/ML model files. Its platform is the system of record for thousands of organizations, including most of the Fortune 100, which gives it telemetry across billions of software artifacts.

On this wiki JFrog appears in three roles. The first is as a measurement source for software-supply-chain risk. Its JFrog Security Research team publishes the annual [[jfrog-ssc-state-of-union-2026|Software Supply Chain Security State of the Union]], the first-party origin of the malicious-npm surge figure, the annual CVE-volume count, the malicious Hugging Face model tally, and the counts of attack surface in agentic developer tooling, cited across [[slopsquatting|Slopsquatting]], the [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] thesis, and [[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]]. The precise figures and their page references live on [[jfrog-ssc-state-of-union-2026|the report summary]].

The second role is as a target. During the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] (May–July 2026), autonomous evaluation agents found and exploited two zero-day chains in an internal [[artifactory|JFrog Artifactory]] deployment: a legacy token-refresh endpoint that accepted a token with an invalid signature and returned a validly signed administrative token, and a deserialization path in which a staged malicious Ruby object cached as repository dependency data reached a JRuby time-of-check/time-of-use flaw, yielding remote code execution and theft of the Artifactory administrative signing key. OpenAI notified JFrog during the July 2026 remediation and a patched service was redeployed; the source does not state the remediation status of the second chain. Both were described publicly at Black Hat USA 2026 by Michael Dalton and Eric Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]. The product page [[artifactory|JFrog Artifactory]] carries the technical detail. The discovery was fully autonomous, ran against a production instance of a widely deployed enterprise application, and placed a package-manager and caching proxy on the agentic attack surface rather than only on the artifact-integrity side of it. A third role appeared in September 2026: JFrog itself supplies agent-facing supply-chain controls, described below.

## AgentSecOps (September 2026)

At swampUP 2026 on 2026-09-02, JFrog extended [[artifactory|Artifactory]] and Xray to cover the artifacts AI coding agents consume and produce, under its own term **AgentSecOps**. Four capabilities carry it. AI Asset Scanning applies semantic analysis to MCP server code, Markdown files, skill scripts and instruction sets, and blocks an asset it judges malicious before it reaches a developer or an agent. Agent Package Resolution routes an agent's dependency fetches through Artifactory, with traffic-controller partnerships blocking direct reads of public registries at the network layer. Agent Guard is a local proxy that handles authentication transparently and enforces project-scoped permissions on the developer's machine, restricting which MCP servers and tools an agent may consume inside a supported IDE. An Agent Plugins registry and an Agent Package Manager registry hold the curated set, the latter built against Microsoft's open-source Agent Package Manager, a dependency manager that authenticates an AI agent and resolves its packages through Artifactory.

Yoav Landman, co-founder and CTO, stated the premise: *"AI coding agents not only write software at machine speed but also consume software at scale."* JFrog states the capabilities were available to customers on announcement, discloses no pricing, and publishes no detection rate for the semantic analysis. The controls land on [[supply-chain-security-for-agents|supply chain security for agentic AI]] as a resolution-time layer above the install-time scanning that practice already carries, and on [[agentic-ai-security-cmm-d8-supply-chain|CMM D8]]'s MCP server / skill provenance row as the first named COTS implementation of the pre-install scan its L3 rung requires; the registry does not add the cryptographic name-to-binary signing that domain's L5+ rung still lacks.
