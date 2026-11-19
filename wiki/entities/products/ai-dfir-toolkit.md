---
type: entity
entity_type: product
title: "depalmar/ai-dfir-toolkit"
created: 2026-10-21
updated: 2026-10-21
tags:
  - entities
  - products
  - dfir
  - detection-engineering
  - sigma
  - forensic-artifacts
  - mcp
  - coding-agent
  - open-source
status: seed
scope_axis:
  - sec-of-ai
  - sec-against-ai
origin: aggregated
vendor: "Raymond DePalma (independent maintainer)"
license: "Apache-2.0 (code); CC BY 4.0 (catalog data)"
role: "Open-source AI/ML DFIR repository: Sigma, YARA and Suricata rules for attacks on and through AI systems, plus a machine-readable catalog of the endpoint artifacts AI agents, model runtimes and MCP components leave, with forensic-tool exports."
homepage: "https://github.com/depalmar/ai-dfir-toolkit"
website: "https://depalmar.github.io/ai-dfir-toolkit/"
first_mentioned: "[[following-the-ai-audit-trail-talk]]"
related:
  - "[[coding-agent-investigation-evidence|Coding-Agent Investigation Evidence]]"
  - "[[following-the-ai-audit-trail-talk|Following the AI Audit Trail]]"
  - "[[mitre-atlas|MITRE ATLAS]]"
  - "[[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications]]"
  - "[[agentic-soc-ra-detection-engineering|Agentic SOC RA: Detection Engineering]]"
  - "[[shadow-ai|Shadow AI]]"
  - "[[agent-observability|Agent Observability]]"
sources:
  - ".raw/articles/ai-dfir-toolkit-2026-10-21.md"
  - ".raw/articles/ai-dfir-toolkit-artifact-catalog-2026-10-21.md"
  - "https://github.com/depalmar/ai-dfir-toolkit"
  - "https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/README.md"
  - "https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/artifacts/README.md"
  - "https://depalmar.github.io/ai-dfir-toolkit/"
verified: 2026-10-21
verified_against:
  - ".raw/articles/ai-dfir-toolkit-2026-10-21.md"
  - ".raw/articles/ai-dfir-toolkit-artifact-catalog-2026-10-21.md"
verified_findings: 0
verified_note: "Full read vs both READMEs at 1a5963d plus talk slide text (slides 5, 17); 4 fixes; deep-link paths confirmed at commit"
---

# depalmar/ai-dfir-toolkit

**Sources:** [GitHub — depalmar/ai-dfir-toolkit](https://github.com/depalmar/ai-dfir-toolkit) · [Project site](https://depalmar.github.io/ai-dfir-toolkit/) · [Artifact catalog README at 1a5963d](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/artifacts/README.md)

## Identity and role

ai-dfir-toolkit is an open-source repository maintained by Raymond DePalma, an independent security researcher, as a personal project not affiliated with any employer. It holds two products that share one site. The **AI/ML DFIR Detection Pack** is a set of vendor-neutral Sigma, YARA and Suricata rules for compromise of LLM applications, MCP servers, ML supply chains, AI infrastructure, AI-assisted insider activity and RAG stores. The **AI Agent Artifact Catalog** records what AI coding agents, local model runtimes, agent frameworks and MCP components leave on an endpoint, what each trace proves, and the order in which to collect it. The code is Apache-2.0 and the catalog data CC BY 4.0, a split the catalog README states is meant to let other catalogs and vendors ingest the data.

A responder uses the repository in two ways: to load detection content for AI-specific attack classes into an existing SIEM, EDR or network sensor, and to drive endpoint triage collection for hosts that run AI agents. [[following-the-ai-audit-trail-talk|Following the AI Audit Trail]] lists the catalog as further reading for endpoint evidence, which the talk places outside the agent's own telemetry.

Every figure below is the repository's own count at commit `1a5963d`, captured on 2026-10-21. The descriptions are the maintainer's own, and neither README reports an independent test of the rules' detection quality.

## Detection pack

The root README counts [70 rule files containing 161 signatures](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/README.md#coverage-overview) across nine attack-class categories and a cross-tool endpoint set. The signature count includes multiple documents in one Sigma file, multiple YARA rule blocks in one file and multiple Suricata alert lines in one file.

| Category | Files | Signatures | ATLAS techniques |
| --- | --- | --- | --- |
| LLM prompt injection | 8 | 10 | T0051, T0054, T0029 |
| MCP attacks | 5 | 14 | T0010, T0115.002, T0110, T0086 |
| Model supply chain | 8 | 23 | T0010, T0018, T0020 |
| AI infrastructure | 9 | 31 | T0011, T0049, T0029 |
| Copilot and assistant abuse | 8 | 19 | T0086, T0024 |
| RAG and vector database | 5 | 17 | T0020 |
| Runtime AI malware | 8 | 16 | T0096, T0086 |
| Agentic orchestration and AI-service C2 | 3 | 12 | T0096, T0086, T0054 |
| Agent memory and context poisoning | 2 | 5 | T0080, T0080.000, T0086 |
| Endpoint (cross-tool) | 14 | 14 | T0053, T0081, T0083, T0098 |

Source: [ai-dfir-toolkit README, "Coverage overview"](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/README.md#coverage-overview), commit `1a5963d`. Technique identifiers are [[mitre-atlas|MITRE ATLAS]] IDs as the README writes them, without the `AML.` prefix.

The README also maps each category to the OWASP Top 10 for LLM Applications **2026** list and to the [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications]], whose ASI01 to ASI06 identifiers appear in eight of the ten rows. The README warns that eight of the ten LLM identifiers changed meaning between the 2025 and 2026 lists, so an LLM identifier quoted from an older report or from the 2025 list names a different category in this repository. Per-rule mappings are in the repository's `MAPPINGS.md`.

The runtime-malware and agentic-orchestration categories detect AI used as the attacker's tool: malware that calls an LLM API at runtime, and an AI service API used as command and control. The endpoint set lives with the artifact catalog and covers agent behaviour on a host across every catalogued tool, alongside an osquery pack that answers which hosts run a given tool.

### Deployment and tuning

The rules use open formats only. Sigma rules convert to a SIEM query language through pySigma backends, YARA rules scan model directories and MCP configuration files, and Suricata rules load into Suricata, with a compatible subset for Snort. The README records one conversion hazard: the keyword rules rely on case-insensitive `contains` matching, so on Elastic the matched fields must be mapped as analyzed text, and forcing case-insensitivity with a regex modifier produces an invalid Lucene query.

Each rule carries a `falsepositives:` section, a severity `level:` and tunable selectors. The README recommends a seven-day run in a test index, tuning of the selection and filter blocks, and promotion to production starting at `high` severity and above. The test suite in `tests/` holds malicious and benign samples, and the README states an expected result of [15 passes and 0 failures](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/README.md#testing). The suite exits with an error when the `yara` binary is missing, so an absent tool cannot score a negative test as a pass.

## Artifact catalog

The catalog is one YAML file per tool, validated against a JSON Schema with closed vocabularies for artifact type, evidence type, secret type and storage. Each entry records install paths, credential storage, MCP configuration paths, listening ports, process relationships and registry keys, and carries a `collection` block with a triage priority. The [catalog README's generated Contents table](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/artifacts/README.md#contents) reports, at commit `1a5963d`:

- 51 catalogued tools in eleven categories, the largest being local LLM runtimes (11), coding-agent IDEs (10) and agent frameworks (9)
- 622 documented artifacts
- 164 credential locations
- 66 MCP configuration locations
- 14 Sigma detection rules under `detections/sigma/`
- 22 case studies

The root README directs readers to that table for current counts, because a script generates it and CI gates it. The table's 14 Sigma rules and the root README's 161 signatures count different things: the first counts only the Sigma rules under the catalog's `detections/sigma/`, and the second counts every Sigma, YARA and Suricata signature across the nine category directories and the endpoint set.

### Confidence ratings

Every field carries a `confidence` rating tied to how it was sourced. High means verified on a live host or documented by the vendor, medium means several independent third-party sources agree, and low means single-source or inferred and always carries `unverified: true`. A CI validator refuses a high-confidence entry that contains an unmarked low-confidence artifact, and `docs/VERIFICATION.md` records every correction. At commit `1a5963d` the catalog README reports [27 high, 20 medium and 4 low confidence entries](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/artifacts/README.md#contents), and risk ratings of 11 critical, 27 high, 12 medium and 1 low. The README defines `risk` as exposure when a tool is misused or misconfigured, not as a claim that the tool is malicious.

### Exports

The catalog generates feeds so that consumers read stable outputs rather than the YAML on disk:

| Export | Consumed by |
| --- | --- |
| ForensicArtifacts definitions | Plaso, GRR, Timesketch |
| KAPE targets | KAPE triage collections |
| Velociraptor artifacts | Velociraptor hunts |
| Collection-target path list | Triage sweeps |
| CSV and JSON | Other tooling |

The site's collection plan generates a self-contained PowerShell or bash triage script, or the equivalent Velociraptor or KAPE command, for a chosen set of tools.

## Threat-intel layer

A threat-intel layer sits on the catalog. The root README reports [20 actor profiles and 22 case studies carrying 221 indicators](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/README.md#threat-intel), with pinned versions of ATLAS 2026.09, ATT&CK 19.2, CISA KEV and the OWASP LLM and Agentic 2026 lists. Each profile is derived from the cases and sightings that cite it and links the actor's techniques to the repository rule that detects each one and to the catalogued tools whose artifacts to collect first. Case studies record analyst disagreement in a `contested` field without resolving it. The layer publishes a JSON feed, a STIX 2.1 bundle for OpenCTI or MISP, and Maltego import tables. A weekly workflow refreshes the framework pins, and new public reporting arrives as `intel-candidate` issues for a person to research.

## Scope

The catalog README places the catalog against living-off-the-land projects. LOLBAS, LOLRMM, LOOBins and lolai-project record what a tool can be misused to do, and this catalog records what a tool leaves on the host, which investigative question each trace answers, and the order to collect it. The README calls lolai-project complementary and states that the presence of an agent on an endpoint is not an incident, because nearly every catalogued tool is legitimate software.

## Relations

- [[coding-agent-investigation-evidence|Coding-Agent Investigation Evidence]] places the catalog's collection targets in the endpoint step of a coding-agent investigation.
- [[following-the-ai-audit-trail-talk|Following the AI Audit Trail]] maps the agent-side and platform-side records of coding-agent activity and points to this catalog for the endpoint evidence those records omit.
- [[agent-observability|Agent Observability]] covers artifact parsing of agent session files as an instrument, and the catalog lists where those files and the credentials beside them live.
- [[shadow-ai|Shadow AI]] discovery can use the catalog's collection targets and osquery pack to find unsanctioned agents and MCP servers on endpoints.
- [[agentic-soc-ra-detection-engineering|Agentic SOC RA: Detection Engineering]] places Sigma as the portable detection format and ATLAS as the AI-application threat model, both of which this repository uses.
- [[mitre-atlas|MITRE ATLAS]] and [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications]] are the taxonomies the rules and catalog entries map to.
