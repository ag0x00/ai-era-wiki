---
type: talk
title: "Taming Shai-Hulud with AI"
address: c-000148
created: 2026-05-25
updated: 2026-05-25
tags:
  - papers
  - talks
  - agentic-soc
  - incident-response
  - supply-chain
  - ai-in-sec-defense
status: stub-summary
scope_axis: [ai-in-sec-defense, sec-against-ai]
year: 2026
authors: ["Rami McCarthy"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 3, 2026 (Day 1 / Stage 1 / 15:20)"
key_claim: "Responding to the internet-scale Shai-Hulud supply-chain campaigns was scaled by moving from 'vibe-coded' scrapers to multi-agent triage engines that parallelize victimology and automate secret-impact analysis — a practitioner post-mortem on where agentic incident response works and where 'lazy' AI fails."
methodology: "Practitioner post-mortem on Wiz's response to the 2025 Shai-Hulud campaigns. The published abstract is the only primary source captured here; slides and transcript are not yet ingested."
source_url: "https://unpromptedcon.org/#"
related:
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[unprompted-conference-march-2026|Unprompted March 2026]]"
  - "[[wiz|Wiz]]"
  - "[[jfrog-ssc-state-of-union-2026|JFrog 2026 SSC State of the Union]]"
  - "[[prt-scan-supply-chain-campaign|prt-scan CI/CD Supply-Chain Campaign]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - https://unpromptedcon.org/#
---

# Zeal of the Convert — Taming Shai-Hulud with AI

A practitioner post-mortem by Rami McCarthy ([[wiz|Wiz]]) at Unprompted (March 2026) on responding to the 2025 Shai-Hulud campaigns. Abstract-only; slides and video are not yet captured.

## The Argument

Shai-Hulud was a series of supply-chain attacks that leaked large volumes of victim data onto GitHub. As a responder to these internet-scale incidents, McCarthy describes moving from simple "vibe-coded" scrapers to **multi-agent triage engines** that parallelize victimology and automate secret-impact analysis. The talk is framed as a raw account of what worked, where the ground shifted, and how "lazy" AI lets a responder down. It shares prompts, scripts, and skills as takeaways.

## Placement

Direct evidence for the **incident-response / triage** capability in the [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] thesis: agentic IR applied to a real internet-scale event, not a demo. It also connects to the supply-chain coverage. Shai-Hulud is the campaign whose CI/CD mechanics the [[jfrog-ssc-state-of-union-2026|JFrog 2026 report]] says still persist across the ecosystem (its RepoHunter findings matched Shai-Hulud's pipeline-misconfiguration pattern), and it is adjacent to the [[prt-scan-supply-chain-campaign|prt-scan]] CI/CD campaign.