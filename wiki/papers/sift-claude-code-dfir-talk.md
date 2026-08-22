---
type: talk
title: "Claude Code on the DFIR SIFT Workstation"
address: c-000151
created: 2026-05-26
updated: 2026-05-26
tags:
  - papers
  - talks
  - agentic-soc
  - dfir
  - mcp
  - autonomous-adversaries
  - ai-in-sec-defense
status: stub-summary
scope_axis: [ai-in-sec-defense, sec-against-ai]
year: 2026
authors: ["Rob T. Lee"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 3, 2026"
key_claim: "Wiring Claude Code into the SANS SIFT digital-forensics workstation over MCP lets a single natural-language prompt orchestrate the workstation's deterministic forensic tools — deliberately mirroring the agentic-AI-over-MCP architecture that the Anthropic GTG-1002 espionage operation used offensively, repurposed for defensive DFIR."
methodology: "Practitioner talk and demo by SANS Chief AI Officer Rob T. Lee on Protocol SIFT, an experimental research initiative. The published abstract plus the SANS Protocol SIFT blog post are the primary sources captured here; slides and video are not yet ingested."
source_url: "https://www.sans.org/blog/protocol-sift-experimental-research-initiative-ai-assisted-dfir"
related:
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[mcp-security|MCP Security]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[unprompted-conference-march-2026|Unprompted March 2026]]"
  - "[[sans-institute|SANS Institute]]"
  - "[[anthropic|Anthropic]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - "https://www.sans.org/blog/protocol-sift-experimental-research-initiative-ai-assisted-dfir"
  - "https://unpromptedcon.org/#"
---

# SIFT — FIND EVIL!! I Gave Claude Code R00t on the DFIR SIFT Workstation

A practitioner talk by Rob T. Lee ([[sans-institute|SANS Institute]]; Chief AI Officer and Chief of Research) at the [[unprompted-conference-march-2026|Unprompted Conference (March 2026)]] on **Protocol SIFT** — wiring Claude Code into the SANS SIFT forensic workstation. Abstract plus the SANS blog post are the primary sources; slides and video are not yet captured.

## The Argument

The SANS SIFT (SANS Investigative Forensic Toolkit) workstation bundles a large set of deterministic digital-forensics and incident-response utilities. Protocol SIFT connects Claude Code to those utilities over the [[mcp-security|Model Context Protocol]] so that an analyst states intent in natural language — "find evil" — and the agent sequences the underlying tools: timeline generation, memory analysis, malware sweeps, and triage across a disk image.

The framing is deliberately symmetrical. The same agentic-AI-over-MCP pattern that an attacker can point at a target, a defender can point at an investigation. SANS frames Protocol SIFT as an **experimental, community-driven research initiative**: the deterministic utilities remain the sole source of analytical output, and the project is explicitly **not forensically validated and not admissible in court**.[^sans]

The talk anchors the threat side on Anthropic's **GTG-1002** report. Anthropic attributes that operation to a Chinese state-sponsored group detected in mid-September 2025 that manipulated Claude Code into an autonomous attack agent across roughly 30 targeted entities, and assesses that the AI executed **approximately 80–90% of tactical operations independently**, with humans in a supervisory role.[^gtg1002] Protocol SIFT is the defender's answer to that same capability.

## Placement

This is direct evidence for the **DFIR-automation** capability in the [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] thesis: an agent orchestrating forensic tooling over MCP on a real workstation, not a lab demo. The GTG-1002 anchor also ties it to [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] and the broader autonomous-adversary narrative — the same agent architecture cuts both ways.
## Notes

[^sans]: SANS, "Protocol SIFT: An Experimental Research Initiative for AI-Assisted DFIR." [sans.org/blog/protocol-sift-experimental-research-initiative-ai-assisted-dfir](https://www.sans.org/blog/protocol-sift-experimental-research-initiative-ai-assisted-dfir). The blog frames the project as experimental and states it is not forensically validated or court-admissible.
[^gtg1002]: Anthropic, "Disrupting the first reported AI-orchestrated cyber espionage campaign" (GTG-1002). The report states the AI "executed approximately 80 to 90 percent of all tactical work independently." Report PDF: [assets.anthropic.com/.../Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf](https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf); announcement: [anthropic.com/news/disrupting-AI-espionage](https://www.anthropic.com/news/disrupting-AI-espionage). The "GTG-1002" designation appears in the PDF, not the announcement page.
