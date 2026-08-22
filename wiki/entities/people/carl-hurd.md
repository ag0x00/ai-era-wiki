---
type: entity
entity_type: person
title: "Carl Hurd"
created: 2026-05-03
updated: 2026-05-03
tags:
  - people
  - starseer
  - detection-engineering
  - mechanistic-interpretability
  - ics-security
  - unprompted-2026
status: seed
related:
  - "[[starseer]]"
  - "[[glass-box-security-talk]]"
  - "[[unprompted-conference-march-2026]]"
sources:
  - .raw/talks/2026-03-03_Carl-Hurd_Glass-Box-Security_transcript.md
---

# Carl Hurd

Co-founder and CTO of [[starseer|Starseer]]. Detection engineer and security researcher with a background spanning national lab work, ICS/embedded systems vulnerability research at Cisco Talos, and applied machine learning.

## Background

| Period | Role | Notable output |
|---|---|---|
| National labs (early career) | Security researcher | ICS/embedded focus; "diving into the depths of how technology works" |
| Cisco Talos (~7 years) | Zero-day researcher + detection engineer | Public CVEs across ICS/embedded systems; co-developed Badgerboard PLC backplane IDS/IPS (open source on GitHub); contributed to VPNFilter malware reverse engineering |
| Red Balloon Security (short stint) | DARPA-adjacent research | Formal methods (Provers); more DARPA work on firmware/embedded |
| 2025–present | Co-founder + CTO, [[starseer\|Starseer]] | Building next-generation detection engineering tooling for AI using mechanistic interpretability |

Hurd holds a master's degree and used ML coursework to build game-automation projects using convolutional neural networks — his entry point into deep learning before pivoting to AI security.

## Key contribution

Hurd's Unprompted March 2026 talk, **[[glass-box-security-talk|Glass-Box Security: Operationalizing Mechanistic Interpretability for Defending AI Agents]]**, introduced the Glass-Box Security paradigm: using forward-pass hooks into a model's residual stream to capture *intent* (cosine similarity of activation vectors against concept reference directions) and measure its *strength* (scalar projection / dot product), enabling YARA-style behavior-based detection rules that operate on model internals rather than plaintext surfaces.

## Intellectual lineage

Hurd explicitly positions Glass-Box Security as closing the same maturity gap that the move from signature-based AV to behavioral EDR closed in traditional endpoint security. His ICS background shapes the framing: ICS security required understanding niche proprietary protocols at depth to write any useful detection content — the same principle applies to understanding neural network activation geometry to write useful AI detection rules.

## Contact / publications

Referenced Starseer blog at time of talk (no URL in transcript). GitHub: Badgerboard PLC backplane IDS/IPS (open source, published during Cisco Talos tenure).
