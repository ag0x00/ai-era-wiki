---
type: concept
title: "Shadow Automation"
created: 2026-04-30
updated: 2026-07-30
tags:
  - concepts
  - governance
  - shadow-it
  - agentic-ai
  - coding-agents
status: developing
scope_axis:
  - sec-of-ai
coined_by: "Knostic (popularized 2025–2026)"
related:
  - "[[ai-coding-agent-governance]]"
  - "[[scaling-agentic-ai-cios-talk]]"
  - "[[non-human-identity]]"
  - "[[decision-rights]]"
  - "[[ai-agent-layered-council]]"
  - "[[agent-catalog]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[nist-ai-rmf]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[microsoft-cli-coding-agent-adoption-study]]"
  - "[[endor-labs-ai-code-governance]]"
sources:
  - "[[ai-coding-agent-governance]]"
  - "[[.raw/talks/scaling-agentic-ai-cios-2026-05-01.md]]"
  - "https://www.ibm.com/reports/threat-intelligence"
---

# Shadow Automation

The agent-era equivalent of shadow IT: developers, engineering teams, or business units spin up AI agents (coding agents, data-science copilots, RAG bots, MCP servers) **without formal security or governance review**, then those agents access code repositories, production systems, or credentials outside policy visibility. Microsoft names the same phenomenon **agent sprawl** — uncontrolled agent expansion without visibility, management, or lifecycle controls — in its [[microsoft-entra-agent-id-security-governance|Entra Agent ID security overview]], and proposes the sponsor-plus-blueprint governance model as the control.

## Significance

The pace mismatch is structural: per Knostic, **63% of organizations deploy code daily or faster**, while governance review cadences are weekly or quarterly. **96% of enterprises plan to expand AI agent use within 12 months** (Cloudera 2025), and **~50% target organization-wide rollout**. Without an inventory and registration step, governance teams lose track of which agent performed what action, under what context, and with what permissions. The IBM X-Force 2025 Threat Intelligence Index cites the same pattern: hidden, unmanaged AI agents introduced without security or compliance review.

Shadow automation is the direct negation of the [[nist-ai-rmf|NIST AI RMF]] GOVERN function: an agent deployed outside review has no documented owner, no assigned roles, and no accountability mapping, which are exactly the organizational structures GOVERN requires before an AI system is put into use. The containment levels below restore those GOVERN preconditions by forcing every agent into an inventory with an attributed owner.

Shadow automation is **operationally distinct from shadow IT**:

| | Shadow IT | Shadow Automation |
|---|---|---|
| Actor | Human user adopting unsanctioned SaaS | Developer / team spinning up an [[shadow-ai\|unsanctioned AI]] agent |
| Detection signal | DNS / SSO logs to unknown SaaS | Network egress to LLM APIs; new agent identities; unregistered MCP servers; Cursor/Copilot rules files committed |
| Governance gap | Data residency / DPA missing | Decision rights / scope / accountability missing |
| Blast radius | Per-user data exposure | Code-base writes, production deploys, credential access at agent speed |

## Containment in the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]

| CMM Level | Shadow-automation containment |
|---|---|
| L1 | None — every agent is shadow |
| L2 | Manual inventory in spreadsheet; reactive |
| L3 | Agent registry; new-agent gate at deployment time |
| L4 | Active discovery (Okta ISPM Agent Discovery, Microsoft Agent 365 Registry); orphan-agent reaper; CI/CD blocks unregistered agents |
| L5 | Closed-loop: every detected unsanctioned agent triggers a governance ticket within an SLA; zero-shadow-agent-quarter as a measurable program metric |

## Defensive primitives

- **Agent inventory + registration** (D2 of the CMM)
- **Shadow-agent discovery** via identity-provider telemetry (Okta ISPM, Microsoft Agent 365 Discovery)
- **Egress filtering** to known LLM endpoints + per-agent token validation (D5)
- **CI/CD policy gate** that blocks unregistered agent identities from pushing code or deploying (D3)
- **Decision-rights matrix** per agent type (see [[decision-rights|Decision Rights for AI Agents]]) so registration is not just "we know it exists" but "we know what it's allowed to do and who approves it"

## Comply-or-explain (vs comply-or-die)

Per [[scaling-agentic-ai-cios-talk|Gartner's Scaling Agentic AI talk (May 2026)]], **67% of employees use a personally obtained AI tool** (ChatGPT, Gemini, etc.) — meaning shadow consumption of AI is already at majority scale even before agentic deployments. The talk argues against a punitive **comply-or-die** posture (block / clamp down), proposing instead a **comply-or-explain** posture:

- *Comply* with the sanctioned [[agent-catalog|agentic AI catalog]] and stack by default.
- *Explain* when a BU has a need the catalog does not satisfy — and use the explanation to expand the catalog rather than to punish the deviation.

The procurement chokepoint (the catalog) becomes the carrot; the comply-or-explain framing reduces the political cost of the [[ai-agent-layered-council|Layered Council]]'s containment story while preserving the inventory discipline. This sharpens the wiki's earlier "block or be circumvented" framing into a finer-grained governance posture.

## Relations

- Coined / popularized by: [[knostic|Knostic]] (see [[ai-coding-agent-governance|AI Coding Agent Governance (Knostic, 2025–2026)]])
- Sibling concept: [[decision-rights|Decision Rights for AI Agents]] — the missing piece without which "we have an agent inventory" is still not governance
- Defensive context: [[non-human-identity|Non-Human Identity (NHI)]], [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] D2 + D3 + D9
- Measured diffusion rate: [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] reads [[microsoft-cli-coding-agent-adoption-study|Microsoft's adoption study]] as putting a coefficient on this concept — adoption spreads along reporting lines faster than an enumeration-first governance program can run, which makes shadow automation the default state rather than a failure mode.

<!-- sources:auto -->
## Sources

- [Scaling Agentic AI: A Leadership Guide for CIOs](https://stream.stream-ext.bizzabo.com/U00liQQ00l2Th5l5Bkc302pY02k01IzHU8P3OqHaCDwYzvxw.m3u8)
- [ibm.com](https://www.ibm.com/reports/threat-intelligence)
<!-- /sources -->
