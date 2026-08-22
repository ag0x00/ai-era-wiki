---
type: entity
title: "Insight Partners"
created: 2026-04-30
updated: 2026-06-21
tags:
  - entities
  - organizations
  - venture-capital
status: developing
entity_type: organization
org_type:  # unset — venture-capital firm; no applicable value in the closed vocabulary
homepage: "https://www.insightpartners.com"
role: "Venture capital firm publishing agentic-AI security market analysis"
first_mentioned: "[[securing-the-autonomous-future]]"
related:
  - "[[securing-the-autonomous-future]]"
sources:
  - "[[.raw/papers/securing-the-autonomous-future.md]]"
---

# Insight Partners

**Sources:** [Insight Partners (homepage)](https://www.insightpartners.com) · [Securing the Autonomous Future](https://www.insightpartners.com/ideas/securing-the-autonomous-future/)

## Identity and role

Insight Partners is a global venture capital and private equity firm investing in high-growth software and internet businesses. In the context of this wiki, they are notable for publishing a three-part series on AI and agentic AI security (October–November 2025) that provides a market-map and practitioner framework for enterprise security teams.

## Relevance to This Wiki

Insight Partners is one of the more rigorous VC investors in the AI security space, with a portfolio spanning identity (Delinea, Teleport, Keeper, Keyfactor, Frontegg, Ory, PlainID), data security (Skyflow, Kiteworks, Atlan), AI safety/evaluation (HoneyHive, Fiddler, Weights & Biases, Promptfoo, Trust3 AI), and agent tooling (CrewAI, Sourcegraph, E2B, Docker, Tavily). Their market maps synthesize signals from many portfolio company conversations.

> [!note] Conflict of interest
> All analysis published by Insight Partners should be read with the awareness that they have financial stakes in many companies their posts recommend or describe. Named portfolio companies in the agentic-AI security series: Sourcegraph, Atlan, Bolt (Stackblitz), Databricks, Tavily, Reco, Spur, HoneyHive, Fiddler, Trust3 AI, Weights & Biases, Onetrust, Delinea, Teleport, CrewAI, Promptfoo, Keeper, Keyfactor, E2B, Frontegg, Ory, PlainID, Anjuna, Aviatrix, Skyflow, Kiteworks, Docker.

## Outputs / Products / Frameworks

### Three-Part AI Security Series (2025)

1. **"The Race to Secure Enterprise AI"** — securing AI model development and runtime (Part 1)
2. **"Securing the Autonomous Future"** (Oct 2025) — [[securing-the-autonomous-future|Securing the Autonomous Future: Trust, Safety, and Reliability of Agentic AI]] — five-category framework for agentic AI security with market map (Part 2)
3. Part 3 (not yet ingested)

### Key Authors (Agentic AI Security Series)

- **George Mathew** — Managing Director
- **Hunter Korn** — Associate
- **Ash Tutika**
- **William Blackwell**

## Notable Statements / Positions

- Frames agentic AI security as a **multi-trillion dollar opportunity** alongside an expanding cybercrime threat (projected \$15T by 2030).
- Argues that the greatest innovation opportunity is in **full-stack observability and monitoring** ("UEBA for Agents").
- Explicitly states that **NHI credential governance is primarily a people-and-process problem**, not a technology problem — a nuanced position that distinguishes it from pure vendor pitches.
- Recommends enterprises start with existing IAM/IGA stacks before buying new point solutions for agent identity.
