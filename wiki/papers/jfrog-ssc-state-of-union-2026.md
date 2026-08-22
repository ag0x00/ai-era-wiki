---
type: paper
title: "Software Supply Chain Security State of the Union"
address: c-000124
created: 2026-05-25
updated: 2026-05-25
tags:
  - papers
  - supply-chain
  - malicious-packages
  - ai-bom
  - governance
  - sec-against-ai
status: summarized
scope_axis: [sec-against-ai, sec-of-ai]
year: 2026
authors:
  - "JFrog Security Research Team"
venue: "JFrog (vendor report)"
source_url: https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs
archived_copy: ".raw/papers/jfrog-ssc-state-of-union-2026.pdf"
key_claim: "AI has become structural to the software supply chain — artifact volume up 136% to 18.2 billion, malicious npm packages up 451% to 171,592 unique instances, new attack surfaces in AI models and agentic developer tooling — while governance lags, producing a widening gap between reported security confidence and measured exposure that the report calls an 'illusion of mastery'."
related:
  - "[[jfrog|JFrog]]"
  - "[[slopsquatting|Slopsquatting]]"
  - "[[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]]"
  - "[[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]"
  - "[[ai-bom|AI-BOM]]"
  - "[[vulnops|VulnOps]]"
  - "[[first-vulnerability-forecast-2026|FIRST Vulnerability Forecast 2026]]"
  - "[[ai-coding-agent-governance|AI Coding-Agent Governance]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
sources:
  - https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs
---

# 2026 Software Supply Chain Security State of the Union (JFrog)

JFrog's annual report on the state of the software supply chain, built on three inputs: JFrog Platform telemetry (the platform held 18.2 billion artifacts at year-end 2025 across its SaaS customers, more than 80% of the Fortune 100), independent analysis by the JFrog Security Research team, and a commissioned survey of 1,508 security, development, and operations professionals across eight countries (p.3, p.65). Its thesis: AI is now structural to the supply chain, a new wave of risk is forming faster than governance can absorb it, and the gap between reported confidence and measured exposure is widening into what the report calls an "illusion of mastery" (p.4).

## Supply-Chain Scale and Composition

- The JFrog Platform held **18.2 billion artifacts** at year-end 2025, up **136%** from 2024 (p.4).
- **Hugging Face published 1.4 million new packages** in 2025, the second-largest source of new packages behind Docker Hub (p.4).
- **npm overtook Maven** as the most-used package ecosystem by traffic, and **PyPI passed YUM**, as AI/ML and agentic workloads displace legacy infrastructure toward scripting languages (p.4).
- **AI coding-agent extensions on OpenVSX grew 262% year over year**, from roughly 1,000 to 3,803 new packages published (p.4).
- Language usage consolidated sharply: organizations using seven or more languages fell from 64% (2024) to 45% (2025); 10-or-more dropped from 44% to 19% (p.8). Fewer ecosystems concentrate risk: a single vulnerability in a foundational library now reaches further.

## Accelerating Risk

- **Over 48,000 new CVEs were disclosed in 2025, a 20% increase over 2024.** JFrog attributes part of the growth to AI-generated code that does not apply secure coding practices, driving a resurgence of decades-old weakness classes — XSS, SQL injection, and other injection flaws (p.5).
- **Malicious npm packages rose 451%, reaching 171,592 unique instances**, fueled by three major hijack campaigns that produced more than two million compromised downloads (p.5). This is the first-party figure behind the number cited across this wiki; the npm-specific count is 171,592, not a cross-registry total.
- The JFrog Security Research team identified **495 malicious models on Hugging Face** carrying live payloads — reverse shells, credential harvesting, and system-command execution (p.5).
- JFrog identified **969 malicious AI agent skills** across the ClawHub and Skills.sh repositories (p.5).
- **56 malicious extensions were detected on OpenVSX**, the first time that attack surface has been tracked in this report (p.5).
- In early 2026, JFrog's AI-powered RepoHunter bot found **13 critical CI/CD vulnerabilities** across widely-used open-source projects (Ansible, CNCF Telepresence, JavaScript standards repositories) before attackers could exploit them. The attack mechanics matched the Shai-Hulud campaign: CI/CD pipeline misconfigurations exposing secrets, publishing tokens, and cloud credentials (p.5).

## The Triage-Noise Problem

- **66% of analyzed CVEs had a low applicability rate (0–20%)** — the conditions required to exploit them are rarely met in practice — and **only 12% were highly exploitable** in real enterprise environments (p.6). Volume-based triage produces noise, not risk reduction, which is the operating case for an exploitability-first discipline such as [[vulnops|VulnOps]].

## The Governance Gap

The report's central argument is that governance has not kept pace with the AI surfaces it must now cover:

- **Only 40% of organizations have adopted malicious-package detection** (unchanged from 2024), and **secrets detection is in active use at only 28%** — the categories growing fastest in threat volume are the least covered by tooling (p.6).
- **41% of organizations actively use AI/ML libraries**, up from 34% in 2024, and the average organization now manages **47% more** of these packages year over year (p.6).
- **53% self-host AI models**, pulling from Hugging Face and similar registries where the 495 malicious models were found — yet **97% claim certified model governance**, a figure the malicious-model data calls into question (p.6).
- **59% report full production provenance visibility**, but **48% still need a week or more to generate compliance-audit proof** — visibility without accountability is not governance (p.6).
- **23% of developers would treat an AI-suggested security fix as near-definitive** after only a quick review, and **18% of organizations have no active governance for the IDE extensions and MCP servers** inside their developers' environments (p.6).

**The "illusion of mastery."** The fastest-growing threat categories — AI/ML model artifacts, agentic developer tooling (IDE extensions, MCP servers, agent skills), and AI-generated code — are the least covered by security tooling and governance, even as self-reported confidence stays high (97% claim certified model governance against 495 detected malicious models). Reported security maturity and measured exposure are diverging.

## Placement

- **Upgrades a wiki figure to its primary.** The "451% malicious-npm surge" cited on [[slopsquatting|Slopsquatting]] and the [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] thesis was previously sourced through a BusinessWire press release; the report gives the precise first-party count — **171,592 unique malicious npm instances**, npm-specific, with three hijack campaigns and 2M+ compromised downloads.
- **Corroborates CVE volume.** The 48,000 CVEs (+20%) figure is consistent with the [[first-vulnerability-forecast-2026|FIRST Vulnerability Forecast]]'s trajectory toward 50,000+ annual CVEs, from a different measurement base.
- **Quantifies the agentic developer-tooling attack surface** — OpenVSX extensions, MCP servers, and agent skills — that [[ai-coding-agent-governance|coding-agent governance]] and the [[ai-era-supply-chain-hardening|hardening plan]]'s AI-BOM and IDE-extension controls are meant to cover.
- **Adds malicious AI model artifacts** (495 on Hugging Face) to the supply-chain threat model alongside the AI-BOM control category.

## See Also

- [[jfrog|JFrog]] — the vendor and the platform telemetry behind the report
- [[slopsquatting|Slopsquatting]] — the hallucinated-package attack class within the malicious-npm surge
- [[ai-era-supply-chain-hardening|AI-Era Supply Chain Hardening]] — the enterprise action plan whose controls these figures motivate
- [[ai-bom|AI-BOM]] — the inventory control for the malicious-model and AI-library exposure the report quantifies
- [[vulnops|VulnOps]] — the standing function for the triage-at-scale problem the low-applicability data describes
