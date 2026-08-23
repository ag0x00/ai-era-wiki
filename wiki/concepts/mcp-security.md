---
type: concept
title: "MCP Security"
created: 2026-04-30
updated: 2026-08-22
tags:
  - concepts
  - mcp
  - agentic-ai
  - api-security
  - protocols
status: seed
scope_axis:
  - sec-of-ai
complexity: intermediate
domain: agent-integration-security
aliases:
  - "Model Context Protocol security"
  - "MCP proxy"
  - "MCP monitoring"
source_url: "https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices"
related:
  - "[[mcp-exposure-measurements]]"
  - "[[agent-identity-architecture]]"
  - "[[agent-observability]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[gartner-mq-enterprise-ai-coding-agents-2026]]"
  - "[[securing-agentic-coding]]"
sources:
  - "[[.raw/papers/securing-the-autonomous-future.md]]"
---

# MCP Security

## Definition

**MCP Security** covers the controls required to safely operate AI agents that use the **Model Context Protocol (MCP)** — Anthropic's open standard for giving LLM-based agents access to external tools, data sources, and services. As MCP adoption scales across enterprise estates, securing MCP servers and the traffic flowing through them becomes a distinct security domain.

## Aliases / Variants

- MCP proxy security
- Agent integration security
- A2A ([[a2a-protocol|Agent2Agent]]) security — the complementary Google-origin protocol for agent-to-agent communication

## Threat Surface

MCP introduces several novel attack surfaces beyond traditional API security:

| Threat | Description |
|---|---|
| **Rogue MCP server** | A malicious or compromised MCP server that injects instructions or data into an agent's context, causing goal manipulation or data exfiltration |
| **Agent Account Takeover (AATO)** | An attacker hijacks an agent's session or identity to use it as a proxy for malicious actions — analogous to ATTO (Account Takeover) for human accounts |
| **[[prompt-injection\|Prompt injection]] via MCP** | MCP server returns a response containing injected instructions that override the agent's intended behavior |
| **MCP scope creep** | Agents granted overly broad MCP permissions over time; no governance equivalent to OAuth scope auditing |
| **Data exfiltration through MCP** | An agent (or compromised agent) sends sensitive data out via MCP calls to an attacker-controlled server |
| **Unauthenticated network exposure** | Server bound to a public interface with no client authentication and no transport encryption. Exploiting it needs no software defect ([[mcp-exposure-measurements\|measurements]]) |
| **Shared-credential privilege collapse** | One hardcoded data-source credential, so every client inherits the same access and no action is attributable |

## Required Controls

### 1. MCP Proxy Layer
An **MCP proxy** sits between the agent and MCP servers, providing:
- Traffic inspection for AI-generated and AI-bound payloads (semantic context, not just protocol)
- Allow-listing of approved MCP servers and endpoints
- Policy enforcement (block calls to unapproved servers)
- Logging for forensics and compliance

### 2. Behavioral Monitoring
Because MCP traffic is semantically rich (natural language + structured calls), monitoring requires **agent-specific intelligence** overlaid on network/API monitoring. Standard WAF or NGFW rules are insufficient.

### 3. Threat Intelligence for Agent Traffic
As internet traffic increasingly originates from bots and agents, identifying malicious-agent traffic requires:
- Enrichment with agent/bot-specific threat intelligence feeds
- Detection of AATO patterns (session anomalies, unusual call patterns)
- Filtering of traffic from known-bad MCP servers

### 4. Exposure Management and Token Delegation

The proxy, monitoring and threat-intelligence controls above all assume the server is reached through a controlled path. Measurement says that assumption often fails: a scan in April 2026 found 1,467 internet-reachable MCP servers running with neither client authentication nor traffic encryption, up from 492 nine months earlier.[^exposure] Those deployments need no vulnerability to leak their data source, so they sit below the proxy layer rather than behind it.

- Bind to loopback or a private subnet by default, and prefer the stdio transport where network exposure is not required.
- Where network exposure is required, terminate it at a reverse proxy or gateway that authenticates the client, and enforce TLS.
- Replace the server's hardcoded data-source credential with per-request OAuth token delegation, so each call runs in the calling user's permission scope and stays attributable.
- Grant read scopes in preference to write scopes, and audit configurations for embedded secrets.

Token delegation is the control the primary research recommends first, and it is what the [[agentic-ai-security-reference-architecture|AAI-S reference architecture]] egress plane implements through per-tool token exchange.

### 5. MCP Governance at Scale
Enterprise estates will accumulate many MCP server registrations. Required capabilities:
- Inventory and discovery of all MCP servers in use
- Per-server allow/deny policies
- Periodic permission reviews (analogous to OAuth token auditing)
- Incident response playbooks for rogue MCP server scenarios

## Relationship to Existing Controls

MCP security is an extension of — but not a replacement for — API security, network monitoring, and AI firewall capabilities. The key differentiation is understanding **agent intent**: whether a particular MCP call is within the normal operating envelope of the agent, or represents anomalous or malicious behavior.

## Production Detection: Sensor over Gateway

The proxy-layer control above is the gateway pattern. A counter-position has now shipped at scale: the [[adr-agentic-detection-system|ADR system]] deployed at [[uber|Uber]] (ten months, 7,200+ hosts) explicitly evaluated and rejected an LLM/MCP gateway for observability, on the grounds that a gateway requires host changes, breaks on streaming responses, and omits environmental context. ADR instead reconstructs MCP activity from an endpoint sensor that parses the local caches of coding agents, capturing the full prompt → reasoning → tool-call → outcome chain.[^adr] Its two-tier detector then reasons over MCP context — querying tool source code, threat intelligence, and policy over dedicated MCP providers — to close the agent-intent gap this page names. The architectural fork is treated in depth in [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]]; the practical reading is that a gateway is one valid PEP but not the only path to MCP observability.

[[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]] names this surface as Insecure Inter-Agent Protocol Abuse (T16): flaws in MCP and A2A such as consent-flow manipulation, MCP response injection, and tool-description exploitation. It is the first OWASP document to treat protocol-level abuse of MCP and A2A as a distinct threat rather than a general prompt-injection variant, and its tool-execution and authentication playbooks call for message authentication on inter-agent channels and signed agent cards.

> [!gap] Standards gap
> As of *Securing the Autonomous Future* (Oct 2025), no established standard or framework specifically governed MCP security. [[owasp-agentic-ai-top-10|OWASP's Agentic AI]] guidance covers the general threat class; MCP-specific controls were being built by startups ahead of incumbent tools. By 2026 the gap is narrowing from the operations side rather than the standards side — production detection frameworks ([[adr-agentic-detection-system|ADR]]) and an MCP-native benchmark ([[adr-bench|ADR-Bench]]) now exist, but no normative MCP-security standard has landed.

## MCP as a market baseline

MCP support became a market entry condition in 2026: [[gartner-mq-enterprise-ai-coding-agents-2026|Gartner's Magic Quadrant for enterprise AI coding agents]] lists *"native Model Context Protocol (MCP) support"* among its inclusion criteria, so every vendor in the category ships it. That promotes MCP server provenance and authorization from a per-product concern to a property of the whole coding-agent market. Harness-side controls are catalogued in [[securing-agentic-coding|Securing Agentic Coding]].

## Occurrences

- [[securing-the-autonomous-future|Securing the Autonomous Future: Trust, Safety, and Reliability of Agentic AI]] — primary source; introduces MCP proxy, AATO, and governance requirements
- [[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]] — production MCP detection-and-response at Uber; sensor-over-gateway observability; [[adr-bench|ADR-Bench]] MCP-native benchmark
- [[mcp-exposure-measurements|MCP Exposure Measurements]] — the four published measurements of MCP risk, what each supports, and which to cite
- [[agent-identity-architecture|AI Agent Identity Architecture]] — agent identity is exercised at the MCP boundary
- [[agent-observability|Agent Observability]] — MCP traffic is part of the full-stack observability requirement

## Related Concepts

- [[agent-identity-architecture|AI Agent Identity Architecture]] — agents authenticate at MCP boundaries; [[spiffe|SPIFFE]]/NHI applies
- [[agent-observability|Agent Observability]] — MCP call logs feed behavioral monitoring
- [[non-human-identity|Non-Human Identity (NHI)]] — MCP access credentials must be governed as NHIs
- [[agentshield|AgentShield]] — open-source static scanner with 23 MCP-specific rules (high-risk server types, `npx -y` typosquat surface, hardcoded secrets in env config, remote SSE/HTTP transports, shell metacharacters in args, sensitive-file args, `0.0.0.0` binding, missing timeouts, `autoApprove`) and a `--supply-chain[-online]` provenance mode; provenance-aware `runtimeConfidence` separates `mcp.json` / `.claude/mcp.json` / `.claude.json` (active runtime) from `mcp-configs/` / `config/mcp/` (template-example catalogs)

[^exposure]: Alfredo Oliveira and David Fiser, [*Update on Exposed MCP Servers: The Threat Widens to the Cloud*](https://www.trendmicro.com/vinfo/us/security/news/vulnerabilities-and-exploits/update-on-exposed-mcp-servers-the-threat-widens-to-the-cloud), Trend Micro, 2026-04-28, updating the 492-server count from their 2025-07-16 scan. The three measurement families are separated on [[mcp-exposure-measurements|MCP Exposure Measurements]].

[^adr]: §3.1 *Observability: The ADR Sensor*, [arXiv:2605.17380](https://arxiv.org/abs/2605.17380): the rejected LLM/MCP gateway alternative (host changes, streaming incompatibility, partial context) and the cache-parsing sensor that reconstructs the full causal chain.

<!-- sources:auto -->
## Sources

- [MCP Security](https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices)
<!-- /sources -->
