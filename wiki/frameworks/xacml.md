---
type: framework
title: "XACML: Access Control Markup Language"
created: 2026-05-05
updated: 2026-05-05
tags:
  - frameworks
  - oasis
  - access-control
  - policy-architecture
  - abac
  - zero-trust
status: developing
scope_axis:
  - sec-of-ai
adoption_signal: dormant
last_substantive_update: 2017-08-23
published_by: "OASIS (Organization for the Advancement of Structured Information Standards)"
current_version: "3.0 (2013); errata through 2017-08-23"
first_published: "2003 (XACML 1.0); 3.0 in 2013"
scope: "Standardized policy language and reference architecture for attribute-based access control (ABAC). The architecture survives in modern usage; the XML policy language has largely been supplanted."
audience: "Architects, identity-and-access-management practitioners, policy-engine implementers"
aliases:
  - "XACML 3.0"
  - "OASIS XACML"
related:
  - "[[oversight-layer]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[nist-sp-800-162]]"
  - "[[cedar]]"
  - "[[opa]]"
  - "[[capability-based-authorization]]"
sources:
  - "https://www.oasis-open.org/committees/xacml/"
  - "http://docs.oasis-open.org/xacml/3.0/xacml-3.0-core-spec-os-en.html"
  - "https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.800-162.pdf"
---

# XACML — eXtensible Access Control Markup Language

**Source:** [OASIS XACML Technical Committee](https://www.oasis-open.org/committees/xacml/) · [XACML 3.0 Core Specification (OASIS, 2013)](http://docs.oasis-open.org/xacml/3.0/xacml-3.0-core-spec-os-en.html) · NIST SP 800-162 (ABAC) builds on XACML.

## Definition

**XACML** (eXtensible Access Control Markup Language) is an OASIS standard that specifies **two artifacts**:

1. **A policy language** — XML-based, declarative, attribute-based, expressing access-control policies as rules over subject / resource / action / environment attributes
2. **A reference architecture** — the four-role split (PEP / PDP / PIP / PAP) that decomposes a policy system into clear responsibilities with defined data flows between them

In modern practice the **architecture is what matters**. The XML policy language itself has been largely superseded by [[cedar|Cedar]] (AWS, Apache 2.0) and [[opa|OPA / Rego]] (CNCF) — both newer, more concise, and easier to author. But Cedar and OPA both deploy as a XACML-shaped architecture: a PDP evaluates policy, a PEP enforces the decision, PIPs supply attributes, a PAP manages the policy lifecycle. The vocabulary is XACML's even when the policy language isn't.

This is why the wiki's [[agentic-ai-security-reference-architecture|Reference Architecture]] is annotated with XACML roles, why the [[oversight-layer|Oversight Layer]] is described in PDP/PEP/PIP/PAP terms, and why the [[agentic-ai-security-cmm-2026|CMM]] uses XACML role coloring across its nine domains.

## Two layers — language vs roles — and where each one lives today

A common mental-model error worth flagging early: when readers see "XACML is dormant," they sometimes infer "the four-role architecture is dormant too." That inference is wrong, because XACML actually defined **two artifacts at two different layers**, and only one of them is dormant.

| Layer | What it specifies | Status today | Living anchor |
|---|---|---|---|
| **Policy-language layer** | The XML syntax for expressing policies — `<Policy>`, `<Rule>`, `<Target>`, `<Condition>`, etc. | **Dormant.** Verbose; rarely written by hand; superseded for new work. | [[cedar\|Cedar]] (AWS, Apache 2.0, 2023); [[opa\|OPA / Rego]] (CNCF). Both are *policy engines* — they replace the language and act as PDPs in a XACML-shaped architecture. |
| **Architectural-role layer** | The four-role decomposition — PEP / PDP / PIP / PAP — and the data flows between them. | **Alive and canonical.** Reaffirmed in NIST SP 800-162 (2014, reaffirmed 2019); carried forward in NIST SP 800-207 Zero Trust (2020, slightly different vocabulary). Used colloquially across vendor docs and practitioner literature. | [[nist-sp-800-162\|NIST SP 800-162 (ABAC)]] §2.2 — same four-role split, NIST-sponsored, currently active. |

**The wiki's role vocabulary anchors at the architectural layer, not the policy-language layer.** When a current authoritative citation is needed for any role-vocabulary claim, **prefer NIST SP 800-162 §2.2 over the OASIS XACML 3.0 spec.** The OASIS spec is the historical lineage; NIST SP 800-162 is the living standard that codifies the same four roles. Cedar / OPA are policy engines — they fit *inside* the four-role architecture as PDP implementations and are not, themselves, the right citation for *role* claims (they don't formally define PIP or PAP).

For absence-claim work under the [[standards-validation-methodology-2026-05|Standards Validation Methodology]]: if the claim is about the *language* (e.g. policy-syntax features), cite XACML 3.0 directly. If the claim is about *roles or architecture*, cite NIST SP 800-162 (or NIST SP 800-207 for the Zero Trust framing). Citing dormant XACML alone for a role-architecture claim is technically valid lineage but weaker than citing the living anchor.

## The four roles

| Role | Full name | Responsibility |
|---|---|---|
| **PEP** | Policy Enforcement Point | Sits in the action path. Intercepts a request, asks the PDP for a decision, applies the result (allow / deny / modify / quarantine). |
| **PDP** | Policy Decision Point | Evaluates policy against the context the PEP supplies and the attributes the PIPs supply. Produces a decision. Stateless by design. |
| **PIP** | Policy Information Point | Provides context the PDP needs that isn't in the request itself — subject attributes, resource posture, environmental signals (time, threat intel, behavioral baselines). |
| **PAP** | Policy Administration Point | Manages the policy lifecycle: authoring, versioning, deployment, retirement. Operates offline / upstream of the request path. |

A PEP is at the boundary; a PDP makes the call; PIPs feed in horizontally; the PAP configures from above. They are **collaborators with different responsibilities**, not vertically stacked layers — a common mental-model error worth flagging on first introduction.

The wiki's preferred collective noun for the four is **"roles"** — matching XACML's own terminology and avoiding the false-stacking implication of "layers." See [[wiki/meta/conventions|Conventions]] §Standards Provenance for the falsifiability discipline applied to absence claims about XACML or any standard.

## Basis for the `dormant` status

The OASIS XACML Technical Committee has not produced a substantive 4.0 revision. The 3.0 spec (2013) is the current version; the most recent committee output is errata from 2017-08-23. The committee is not abandoned — XACML 3.0 is still cited and implemented — but new policy-engine work has migrated to Cedar / OPA / capability tokens. XACML's enduring contribution is the architectural decomposition; the language itself is on a slow glide.

`adoption_signal: dormant` reflects this: still referenced in current literature, no version drift to track, role decomposition is the load-bearing artifact. NIST SP 800-162 (ABAC, 2014) codifies the same decomposition without invoking the XML language.

## Relation to adjacent standards

| Adjacent | Relationship |
|---|---|
| **[[nist-sp-800-162\|NIST SP 800-162 (ABAC)]]** | Generalizes XACML's role split into a standards-agnostic ABAC reference. The four functional points are the same; the language is unconstrained. **The wiki's preferred citation for any role-vocabulary claim** — it is the living anchor where XACML is dormant. |
| **Zero Trust (NIST SP 800-207)** | Adopts the PDP/PEP separation as one of its core architectural commitments. Zero-trust's "Policy Engine" + "Policy Administrator" + "Policy Enforcement Point" map onto XACML's PDP + PAP + PEP. |
| **[[cedar\|Cedar]]** | AWS-led, Apache-2.0 OSS policy language (2023). Replaces XACML's XML with a SQL-like declarative syntax; deploys as a XACML-shaped architecture (Cedar engine = PDP). Used in [[hooking-coding-agents-with-cedar-talk\|Sondera coding-agent harness]] and [[agentcordon\|AgentCordon credential broker]]. |
| **[[opa\|OPA / Rego]]** | CNCF-graduated, Apache-2.0 OSS policy engine. Datalog-inspired Rego language. Same architecture pattern as XACML; different policy language. |
| **[[capability-based-authorization\|Capability-based authorization]]** ([[tenuo-warrant\|Tenuo Warrants]], Macaroons, UCAN, Biscuits) | Alternative model: policy is bound *into the credential*, evaluated implicitly when the credential is presented. Reduces the PDP-call frequency at the cost of needing strong attenuation and revocation primitives. Niyikiza's Unprompted talk frames this as the principal alternative to XACML's request-time PDP model. |

## In the wiki

- [[oversight-layer|Oversight Layer]] — wiki's architectural primary term for the PDP+PEP+PIP+PAP system applied to AI agents
- [[agentic-ai-security-reference-architecture|Reference Architecture]] §"XACML roles colored across planes" — visual mapping of the four roles onto the six RA planes
- [[agentic-ai-security-cmm-2026|CMM]] — uses XACML-role coloring across the nine domains (PIP blue, PDP yellow, PEP red, mixed purple, cross-cutting green)
- [[guardian-agent|Guardian Agent]] — procurement-language synonym for the oversight-layer; cross-walk includes XACML lineage
- [[capability-based-authorization|Capability-Based Authorization]] — the principal alternative model

## Citations

- [OASIS XACML 3.0 Core Specification](http://docs.oasis-open.org/xacml/3.0/xacml-3.0-core-spec-os-en.html) (2013, OASIS Standard)
- [NIST SP 800-162 — Guide to Attribute Based Access Control (ABAC) Definition and Considerations](https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.800-162.pdf) (2014; reaffirmed 2019)
- [NIST SP 800-207 — Zero Trust Architecture](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207.pdf) (2020)

<!-- sources:auto -->
## Sources

- [oasis-open.org](https://www.oasis-open.org/committees/xacml/)
- [docs.oasis-open.org](http://docs.oasis-open.org/xacml/3.0/xacml-3.0-core-spec-os-en.html)
- [nvlpubs.nist.gov](https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.800-162.pdf)
<!-- /sources -->
