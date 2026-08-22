---
type: framework
title: "OWASP SAMM: Software Assurance Maturity Model"
address: c-000111
created: 2026-05-24
updated: 2026-08-20
tags:
  - framework
  - owasp
  - secure-sdlc
  - maturity-model
  - samm
status: seed
scope_axis: [sec-of-ai, sec-against-ai]
framework_class: secure-sdlc
issuer: "[[owasp|OWASP]]"
homepage: https://owaspsamm.org
current_version: "2.0"
aliases:
  - "SAMM"
  - "SAMM v2"
  - "OWASP Software Assurance Maturity Model"
related:
  - "[[nist-ssdf|NIST SSDF]]"
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
  - "[[microsoft-sdl|Microsoft SDL]]"
  - "[[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack 2026]]"
sources:
  - https://owaspsamm.org
---

# OWASP SAMM — Software Assurance Maturity Model (v2)

**Source:** [OWASP SAMM](https://owaspsamm.org)

OWASP SAMM is a vendor-neutral maturity model for measuring and improving a software assurance program. Version 2 organizes practice across five business functions (Governance, Design, Implementation, Verification, Operations), each with three security practices, scored against three maturity levels. It ships an assessment toolkit so a team can self-score and plan increments.

On this wiki SAMM is the peer maturity model to [[microsoft-sdl|Microsoft SDL]] and a precursor referenced by [[nist-ssdf|NIST SSDF]]. It is one input to the [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack]] synthesis.

The [[owasp-ai-exchange|OWASP AI Exchange]] names SAMM alongside NIST SSDF in the references of its `SEC DEV PROGRAM` control, which requires AI secure-development practice to extend an existing secure-development program rather than stand up an isolated AI-specific one ([`/go/secdevprogram/`](https://owaspai.org/go/secdevprogram/)). That places SAMM as a base program an AI assurance effort adds to, and it is why the Exchange's AI-specific engineering particularities are stated as additions to a development program instead of as a separate model.

> [!gap] Seed page
> Created to resolve dead links from [[nist-ssdf|NIST SSDF]] and [[microsoft-sdl|Microsoft SDL]]. A full treatment would map SAMM's five functions to the wiki's CMM domains and note where it does and does not address agentic-AI assurance.

<!-- sources:auto -->
## Sources

- [OWASP SAMM: Software Assurance Maturity Model](https://owaspsamm.org)
<!-- /sources -->
