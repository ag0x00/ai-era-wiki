---
type: entity
entity_type: organization
org_type: vendor
title: "Sysdig"
created: 2026-05-07
updated: 2026-06-21
tags:
  - organizations
  - sysdig
  - cloud-security
  - threat-research
  - vibehacking
status: seed
homepage: "https://sysdig.com"
related:
  - "[[unprompted-conference-march-2026]]"
sources:
  - "https://sysdig.com"
---

# Sysdig

**Sources:** [Sysdig (homepage)](https://sysdig.com) · [Sysdig Threat Research Team](https://sysdig.com/threat-research/)

Cloud-security and runtime-threat-detection vendor; operates the Sysdig Threat Research Team. In the context of this wiki, Sysdig appears as a primary source for **two AI-assisted attacks caught in the wild** — the headline data point in the [[unprompted-conference-march-2026|Unprompted March 2026]] "VibeHacking" thread.

## "VibeHacking" — caught-in-the-wild attacks

Sergej Epp (CISO) presented "8 Minutes to Admin. We Caught It in the Wild. Welcome to VibeHacking" (Day 2 / Stage 1 / 09:35). Two campaigns:

1. **8-minute AWS escalation** — stolen credentials → full administrator inside eight minutes. Compresses known privilege-escalation primitives to a speed that breaks traditional detection models.
2. **EtherRAT** — fileless Node.js implant that uses **Ethereum smart contracts as the C2 channel**. Framed as "the attacker's resilience play that becomes the defender's greatest forensic gift" because the on-chain artefact is permanent and observable.

The talk argues neither campaign introduced novel attack primitives; what changed is the **speed and scale** at which known techniques get assembled, and proposes a behavioural methodology for attributing AI-assistance when forensic proof is impossible.

## See also

- [[unprompted-conference-march-2026|Unprompted March 2026]] — talk venue
