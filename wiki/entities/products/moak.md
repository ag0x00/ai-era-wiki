---
type: entity
entity_type: product
title: "MOAK (Mother of All KEVs)"
address: c-000086
created: 2026-05-19
updated: 2026-06-21
tags:
  - entities
  - products
  - offensive-ai
  - exploit-generation
  - kev
  - stealth-startup
status: stub
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
homepage: "https://moak.ai/"
team: ["Yuval Zadok", "Karen Katz", "Itay Chechik", "Niv Hoffman", "Ofir Eisenberg", "Yair Saban"]
related:
  - "[[moak-how-it-works|MOAK How It Works]]"
  - "[[moak-mother-of-all-kevs|MOAK Origin Story]]"
  - "[[autonomous-exploit-generation|Autonomous Exploit Generation]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[vulnops|VulnOps]]"
  - "[[xbow|XBOW]]"
  - "[[mythos|Mythos]]"
  - "[[codex-security|Codex Security]]"
  - "[[openant|OpenAnt]]"
---

# MOAK — Mother of All KEVs

**Sources:** [MOAK (homepage)](https://moak.ai/)

**MOAK** ("Mother of All KEVs") is a security research initiative from an unnamed stealth cybersecurity startup. Its core product is the **MOAK Engine**: a five-agent autonomous pipeline that converts a bare CVE number into a validated working exploit, operating without human intervention and without access to prior POCs or exploitation details.

## Function

MOAK automatically exploits newly disclosed CVEs at the moment of disclosure. Given only a CVE number, the pipeline produces a weaponized, CTF-validated exploit within hours. It runs continuously against the CISA KEV list and offers a **live dashboard** where practitioners can request exploitability assessments for specific CVEs relevant to their environment.

## Performance

- **200+ CISA KEVs exploited at 98% success rate** (public-facing headline; the technical deep-dive reports 174/178 = 97.8% — same run, different framing).
- **82% exploited in less than one hour.**
- **Same success rate on post-knowledge-cutoff KEVs** (CVEs disclosed after Sep '25 — the models' training cutoff baseline).
- Best model: **Claude Opus 4.6 at 98% autonomous exploitation rate** on the post-cutoff benchmark.
- No human in the loop; no external POCs or exploitation training data.
- Primary languages tested: Python, JavaScript, Java, PHP, and native C.

See [[moak-how-it-works|the technical deep-dive]] for the full benchmark table and pipeline architecture.

> [!contradiction] Contradicts Anthropic Red Team public statement
> Anthropic's Red Team stated: *"Opus 4.6 is currently far better at identifying and fixing vulnerabilities than at exploiting them. This gives defenders the advantage."*
>
> MOAK demonstrates 98% autonomous exploitation of CISA KEVs using Claude Opus 4.6 as a primary model inside a five-agent agentic pipeline. The Anthropic statement assesses the model in isolation; MOAK demonstrates that the same model in an orchestrated framework crosses the operational exploitation threshold. The statement may be narrowly accurate for bare-model usage while being operationally misleading for agentic deployment. See [[moak-mother-of-all-kevs|MOAK origin story]] and [[anthropic|Anthropic]] for the complementary callout.

## The MOAK Engine

Five agents in sequence — see [[moak-how-it-works|MOAK How It Works]] for full detail:

| Agent | Role |
|---|---|
| **Collector** | Gathers CVE description + vulnerable/patched code; guardrailed to exclude POCs |
| **Researcher** | Builds primitive-chain graph; spawns multi-model swarm (Claude + GPT + Gemini) with rotating Prioritizer / Lead Researchers / Contrarian / Verifier roles |
| **Builder** | Provisions Docker CTF environment (vulnerable + patched pair, secret flag) |
| **Exploiter** | Builds and iteratively refines exploit until flag captured and patched env resists |
| **Judge** | Verifies exploit is genuine, environment realistic, no workarounds used; produces exploitability score |

## Team

Six published authors across two blog posts (all from the same stealth startup):
- **[[moak-how-it-works|"How Does MOAK Work?"]]** (technical): Yuval Zadok, Karen Katz, Itay Chechik
- **[[moak-mother-of-all-kevs|"Mother of All KEVs"]]** (origin story): Niv Hoffman, Ofir Eisenberg, Yair Saban

## Safety Position

Closed-source. Operates in isolated environments only. No exploit artifacts published. Academic/research access available on request.

## Significance

MOAK is the wiki's primary documented instance of [[autonomous-exploit-generation|Autonomous Exploit Generation]] at production scale. It provides the mechanistic explanation for how [[zero-day-clock|Time-to-Exploit]] collapses to hours in 2026: autonomous pipeline + frontier-model reasoning + no human bottleneck = exploit-on-drop.
