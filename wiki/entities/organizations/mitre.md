---
type: entity
title: "MITRE Corporation"
created: 2026-04-30
updated: 2026-06-21
tags:
  - entities
  - organizations
  - standards-body
  - threat-intelligence
status: active
entity_type: organization
org_type: advisory
homepage: "https://www.mitre.org"
role: "Federally funded R&D center; publishers of MITRE ATT&CK and MITRE ATLAS; rapid threat intelligence publications"
related:
  - "[[mitre-atlas]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# MITRE Corporation

**Sources:** [MITRE (homepage)](https://www.mitre.org) · [MITRE ATLAS](https://atlas.mitre.org) · [MITRE ATT&CK](https://attack.mitre.org)

**MITRE** is a federally funded research and development center (FFRDC) operating multiple FFRDCs sponsored by the U.S. government. In AI security, MITRE is the publisher of **[[mitre-atlas|MITRE ATLAS]]** (Adversarial Threat Landscape for Artificial-Intelligence Systems), the primary adversarial technique knowledge base for AI/ML systems.

## AI Security Role

MITRE ATLAS extends the ATT&CK methodology to AI/ML systems, documenting adversary techniques, sub-techniques, mitigations, and real-world case studies. ATLAS is exclusively adversary-centric — it catalogs attacks but does not provide defensive control specifications.

## Q1 2026 Activity

MITRE ATLAS had the most rapid agentic threat coverage expansion of any framework in Q1 2026:
- **v5.3.0** (January 2026) — 18+ new techniques; Zenity Labs contributions; SesameOp case study (AML.CS0042)
- **v5.4.0** (February 2026) — "Publish Poisoned AI Agent Tool," "Escape to Host"
- **OpenClaw Investigation** (February 9, 2026) — 7 new techniques; CVE-2026-25253; case study AML.CS0050

ATLAS now covers **84 techniques, 16 tactics, 56 sub-techniques, 32 mitigations, and 42 case studies**.

## Key Publications

- [[mitre-atlas|MITRE ATLAS]] — adversarial AI threat knowledge base
- Arsenal CALDERA plugin — automated red team integration (limited 2026 updates)
- OpenClaw Investigation (February 9, 2026) — rapid response threat report
