---
type: framework
title: "SOC-CMM Security Operations Maturity Model"
address: c-000210
created: 2026-06-11
updated: 2026-06-11
tags:
  - framework
  - maturity-model
  - soc
  - cmm
  - security-operations
status: seed
origin: aggregated
scope_axis:
  - ai-in-sec-defense
framework_class: security-operations-maturity
issuer: "Rob van Os"
homepage: https://soc-cmm.com
current_version: "2.3"
first_published: 2017
adoption_signal: maintained
last_substantive_update: 2026-06-11
aliases:
  - "SOC-CMM"
  - "SOC Capability Maturity Model"
related:
  - "[[agentic-soc-cmm]]"
  - "[[ai-augmented-soc-survey]]"
  - "[[cybersecurity-cmms-exemplars]]"
  - "[[agentic-soc-autonomy-ladders]]"
sources:
  - https://soc-cmm.com
---

# SOC-CMM Security Operations Maturity Model

**Source:** [soc-cmm.com](https://soc-cmm.com)

SOC-CMM is a free, open self-assessment model for measuring the maturity and capability of a Security Operations Center. Rob van Os built it from a 2016 MSc thesis and released the first version in 2017. The deliverable is a spreadsheet tool: an organization scores itself against a fixed question set and reads back per-domain maturity and capability levels. It is the incumbent SOC maturity standard, and the model the wiki's [[agentic-soc-cmm|Agentic SOC Capability Maturity Model]] is written to extend rather than replace.

## Structure

SOC-CMM decomposes a SOC into five domains, themselves subdivided into roughly 26 aspects and more than a thousand scored elements.

| Domain | Covers |
|---|---|
| Business | SOC mandate, drivers, charter, governance, customer scope |
| People | Staffing, roles, training, certification, retention |
| Process | Operational, analytical, and management procedures |
| Technology | SIEM, analytics, telemetry collection, and supporting tooling |
| Services | The detection, response, and adjacent services the SOC delivers |

The first three rows expand the classic people–process–technology triad; Business and Services were added to capture the SOC's mandate and its output.

## Maturity and capability scales

SOC-CMM scores two distinct axes, a separation the wiki's autonomy-versus-maturity distinction borrows.

- **Maturity** (all five domains), a 0–5 scale derived from CMMI and ISO/IEC 15504: 0 non-existent, 1 initial, 2 limited, 3 defined, 4 managed, 5 optimizing. Maturity asks how well a thing is organized and repeatable.
- **Capability** (Technology and Services only), a 0–3 scale. Capability asks what the SOC can actually deliver, independent of how maturely it is run.

Plotting both axes per domain is the assessment's main output, distinguishing a SOC that runs immature processes around strong tooling from one with disciplined processes and thin capability.

## Adoption

SOC-CMM is a free SOC maturity self-assessment with a third-party ecosystem around it. The tool is downloaded from soc-cmm.com under a permissive license, and a partner program certifies assessors. Commercial platforms wrap the assessment, for example SOCSCOPE, and a CERT-specific variant (SOC-CMM for CERTs) extends the structure to incident-response teams. The academic prior art the [[agentic-soc-cmm|Agentic SOC CMM]] builds on treats it as the incumbent baseline. The latest documented tool release is version 2.3 (June 2023); the author continues to publish an annual SOC maturity report.

> [!gap] Reach is asserted, not measured
> No independent adoption metric was located for SOC-CMM: no download counts, SOC-survey share, or published-assessment count. The "de facto standard" framing traces to soc-cmm.com's own description, so the reach claim here is an inference from the surrounding ecosystem (partner program, third-party tooling, academic baseline use), not a measured figure. The official downloads page was unreachable at fetch time (HTTP 403), so a release newer than v2.3 could not be ruled out.

## The AI gap

As of its 2025 self-assessment report, SOC-CMM does not yet model AI, autonomy, or agentic automation. That absence is the opening the wiki's [[agentic-soc-cmm|Agentic SOC CMM]] occupies: it adds a per-function autonomy ladder and AI-specific maturity domains while crosswalking back to SOC-CMM's maturity scale. The [[ai-augmented-soc-survey|MDPI AI-Augmented SOC survey]] supplies the worked correspondence between the two, mapping its five autonomy levels onto SOC-CMM maturity 1–5.

## Relations

- [[agentic-soc-cmm|Agentic SOC Capability Maturity Model]] — extends SOC-CMM into the autonomy era; reuses its maturity-versus-capability split and crosswalks against its 0–5 scale.
- [[ai-augmented-soc-survey|AI-Augmented SOC Survey]] — the academic prior art that maps an AI autonomy ladder onto SOC-CMM maturity levels.
- [[cybersecurity-cmms-exemplars|Cybersecurity CMM Exemplars and Design Lessons]] — the broader CMM design-lesson catalog; SOC-CMM is the SOC-specific incumbent alongside CMMI, BSIMM, SAMM, CMMC, and NIST CSF.

<!-- sources:auto -->
## Sources

- [SOC-CMM Security Operations Maturity Model](https://soc-cmm.com)
<!-- /sources -->
