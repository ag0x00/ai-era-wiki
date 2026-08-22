---
type: entity
entity_type: organization
org_type: vendor
title: "JFrog"
address: c-000125
created: 2026-05-25
updated: 2026-08-14
tags:
  - entities
  - organization
  - vendor
  - supply-chain
  - artifact-management
  - sec-against-ai
status: developing
scope_axis: [sec-against-ai, ai-in-sec-defense]
homepage: https://jfrog.com
related:
  - "[[jfrog-ssc-state-of-union-2026|2026 Software Supply Chain Security State of the Union]]"
  - "[[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]]"
  - "[[slopsquatting|Slopsquatting]]"
  - "[[ai-bom|AI-BOM]]"
  - "[[artifactory|JFrog Artifactory]]"
  - "[[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]"
sources:
  - https://jfrog.com
  - https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs
---

# JFrog

**Sources:** [JFrog (homepage)](https://jfrog.com) · [JFrog Security Research](https://jfrog.com/blog/category/security-research/) · [2026 SSC report announcement (BusinessWire)](https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs)

JFrog is a software supply chain platform vendor, built around the Artifactory binary repository and a security suite (Xray, Advanced Security) that scans artifacts, dependencies, and AI/ML model files. Its platform is the system of record for thousands of organizations, including most of the Fortune 100, which gives it telemetry across billions of software artifacts.

On this wiki JFrog appears in two roles. The first is as a measurement source for software-supply-chain risk. Its JFrog Security Research team publishes the annual [[jfrog-ssc-state-of-union-2026|Software Supply Chain Security State of the Union]], the first-party origin of the malicious-npm surge figure, the annual CVE-volume count, the malicious Hugging Face model tally, and the agentic developer-tooling attack-surface counts cited across [[slopsquatting|Slopsquatting]], the [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] thesis, and [[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]]. The precise figures and their page references live on [[jfrog-ssc-state-of-union-2026|the report summary]].

The second role is as a target. During the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] (May–July 2026), autonomous evaluation agents found and exploited two zero-day chains in an internal [[artifactory|JFrog Artifactory]] deployment: a legacy token-refresh endpoint that accepted a token with an invalid signature and returned a validly signed administrative token, and a deserialization path in which a staged malicious Ruby object cached as repository dependency data reached a JRuby time-of-check/time-of-use flaw, yielding remote code execution and theft of the Artifactory administrative signing key. OpenAI notified JFrog during the July 2026 remediation and a patched service was redeployed; the source does not state the remediation status of the second chain. Both were described publicly at Black Hat USA 2026 by Michael Dalton and Eric Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]. The product page [[artifactory|JFrog Artifactory]] carries the technical detail. The case is relevant to the wiki because the discovery was fully autonomous, against a production instance of a widely deployed enterprise application, and because it places a package-manager and caching proxy on the agentic attack surface rather than only on the artifact-integrity side of it.
