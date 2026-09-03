---
type: entity
entity_type: organization
title: "Varonis"
address: c-000318
created: 2026-08-20
updated: 2026-08-31
tags:
  - entities
  - organizations
  - vendor
  - dspm
  - vulnerability-research
  - prompt-injection
status: seed
scope_axis:
  - sec-of-ai
origin: aggregated
org_type: vendor
role: "Data security posture management (DSPM) vendor; Threat Labs unit disclosed three 2026 Microsoft Copilot one-click vulnerability findings (Reprompt, SearchLeak, CoSnitch)"
homepage: "https://www.varonis.com"
first_mentioned: "[[oversharing-controls|Oversharing Controls]]"
related:
  - "[[cosnitch-copilot-personal-exfiltration|CoSnitch: Copilot Personal Data Exfiltration]]"
  - "[[oversharing-controls|Oversharing Controls]]"
  - "[[guardian-agent|Guardian Agent]]"
  - "[[knostic|Knostic]]"
  - "[[microsoft]]"
  - "[[cyera|Cyera]]"
sources:
  - "https://www.varonis.com"
  - "https://www.varonis.com/blog/cosnitch"
  - "https://www.varonis.com/blog/searchleak"
  - "https://www.varonis.com/blog/reprompt"
  - ".raw/articles/oecd-aim-copilot-cosnitch-incident-2026-08-20.md"
---

# Varonis

**Sources:** [Varonis (homepage)](https://www.varonis.com) · [CoSnitch: When Your AI Assistant Becomes Its Own Whistleblower](https://www.varonis.com/blog/cosnitch) · [SearchLeak](https://www.varonis.com/blog/searchleak) · [Reprompt](https://www.varonis.com/blog/reprompt)

Data security posture management (DSPM) vendor, listed alongside [[cyera|Cyera]] and BigID as one of the DSPM incumbents extending into AI-feed monitoring ([[oversharing-controls|Oversharing Controls]]) and named in Gartner's "Agent security and risk specialists" segment of the [[guardian-agents-market-guide|Guardian Agents Market Guide]] ([[guardian-agent|Guardian Agent]]).

Varonis Threat Labs is the vendor's offensive-research arm and the credited discoverer of [[cosnitch-copilot-personal-exfiltration|CoSnitch]] (CVE-2026-24301), a one-click Microsoft Copilot Personal vulnerability chain disclosed to Microsoft in December 2025 and patched August 18, 2026. Varonis states CoSnitch is the third Microsoft Copilot flaw the unit found in 2026, after **Reprompt** — Copilot Personal, no disclosed CVE, the `q` URL parameter pre-fills a prompt that still needs the victim to press Enter — and **SearchLeak** (CVE-2026-42824, updated June 15, 2026) — Microsoft 365 Copilot *Enterprise*, chaining the same `q`-parameter injection with an HTML rendering race condition and an SSRF through Bing's CSP-allowlisted image-search endpoint to exfiltrate mailbox, calendar, and file data through image-embedded requests. Varonis states the pattern common to all three is that one click on a legitimate-looking link is enough; the specific mechanism after that click differs in each case (a manual Enter press, an SSRF chain, or CoSnitch's silent `autorun` parameter). Neither Reprompt nor SearchLeak has its own wiki incident page as of this ingest.

Varonis's disclosed discovery method for CoSnitch — repeatedly asking Copilot to justify why an attack should be impossible, and treating each refusal's technical justification as reconnaissance — is documented on [[cosnitch-copilot-personal-exfiltration|the incident page]] rather than here, since it is specific to that one finding.

## See Also

- [[cosnitch-copilot-personal-exfiltration|CoSnitch: Copilot Personal Data Exfiltration]]
- [[oversharing-controls|Oversharing Controls]]
- [[guardian-agent|Guardian Agent]]
