---
type: gap-analysis
title: "CMM Known Limitations (current state)"
created: 2026-05-06
updated: 2026-09-29
tags:
  - gaps
  - cmm
  - known-limitations
status: developing
origin: produced
scope_axis:
  - sec-of-ai
target: "[[agentic-ai-security-cmm-2026]]"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[a2a-protocol]]"
  - "[[aiuc-1]]"
  - "[[securing-agentic-coding]]"
sources:
  - "[[.raw/papers/owasp-ai-exchange-threats-through-use-2026-08-18.md]]"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Reviewed current OWASP input-series control, pinned A2A v1.0.0, AIUC-1 Society, current CMM D1/D3/D4/D5/D7, Handbook, coding catalog, and dated archive; no archived source opened."
---

# CMM Known Limitations (current state)

The [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] defines the domain criteria, and the [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] defines their evidence and verdict rules. This register identifies current limits that can change an assessment or investment decision. It does not change a criterion or a domain level. The assessor records a material limit beside the affected finding, confidence judgment, or target proposal.

## Open limitations

### Cross-session input-series detection

The [OWASP AI Exchange input-series control](https://owaspai.org/go/unwantedinputserieshandling/) addresses suspicious patterns that emerge across multiple inputs, including nonconsecutive probes. [[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]] grades progressive relaxation *within* one session and cross-session attack evaluation. Neither criterion grades production detection of probing spread across sessions. For a deployment open to repeated interaction, the assessor reports this exposure and any existing detector separately; a D7 level does not establish that the detector exists. The control's possible placement between [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]] and D7 remains a model-design decision. **Owner:** CMM domain maintainers.

### Vendor-held authorization evidence

An in-process decision point in a vendor runtime can enforce a customer-administered policy outside the model context. Its observed refusals still depend on the vendor code under assessment. [[agentic-ai-security-cmm-d3-control-least-agency|CMM D3: Control and Least-Agency]] and the Handbook permit customer tests and scoped supplier evidence, but those records may give less independent assurance than a separately operated decision point. The assessor states the evidence limit in confidence. An applicable outcome that neither party can test or substantiate is **unanswerable**, which blocks the cumulative claim. **Owners:** The deployment assessor records the finding. The supplier owns missing implementation evidence.

### Agentic-coding catalog coverage

[[securing-agentic-coding|Securing Agentic Coding]] maps its harness controls to D2 through D8 and has no catalog rows for D1 or D9. A coding-fleet assessment based on that catalog alone would omit governance and operating evidence. The assessor collects those records directly from [[agentic-ai-security-cmm-d1-governance|CMM D1: Governance and Accountability]], [[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]], and the Handbook. The missing coordinates limit the companion catalog, not the CMM grade. **Owner:** agentic-coding catalog maintainer.

## Source and adoption questions

### A2A replay profile

The [[a2a-protocol|A2A Protocol (Agent-to-Agent)]] [v1.0.0 specification](https://a2a-protocol.org/v1.0.0/specification/) makes Send Message idempotency optional and permits agents to use `messageId` to detect duplicates.[^source-date] Protocol support alone does not establish the receiver's duplicate-message behavior. [[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]] therefore grades a documented receiver replay rule and a duplicate-refusal test. The target case includes receiver state, tests, and any integration needed with external peers. This is a CMM security choice, not a claim of A2A protocol conformance.

### Societal misuse boundary

The [[aiuc-1|AIUC-1 AI Agent Certification Standard]] [Society requirements](https://www.aiuc-1.com/society) address AI-enabled cyber misuse and catastrophic misuse.[^source-date] The CMM's nine-domain profile does not separately grade those outcomes. A deployment with credible misuse consequences needs a separate misuse assessment and decision owner; a CMM L5 domain result does not establish Society-control coverage. An AIUC-1 certificate used as D1 governance-assurance evidence must be read within its own scope.

## History

The [dated limitations archive](../../docs/aai-s-cmm-known-limitations-archive-2026-09.md) preserves superseded criteria, resolved findings, and earlier source readings. It is historical evidence, not a current scoring instruction. The [CMM stress-test issue #168](https://github.com/ag0x00/ai-era/issues/168) records the issue trail.

[^source-date]: A2A v1.0.0 and AIUC-1 Society pages checked 2026-09-29.
