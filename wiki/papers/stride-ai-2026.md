---
type: paper
title: "STRIDE-AI Threat Modeling Framework"
created: 2026-06-23
updated: 2026-08-21
tags:
  - papers
  - threat-modeling
  - secure-sdlc
  - generative-ai
status: developing
scope_axis:
  - sec-of-ai
origin: aggregated
address: c-000234
source_url: "https://arxiv.org/abs/2605.17163"
related:
  - "[[threat-modeling-for-ai]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[nist-ai-rmf]]"
  - "[[owasp-llm-top-10]]"
  - "[[prompt-injection]]"
sources:
  - "https://arxiv.org/abs/2605.17163"
  - "[[.raw/papers/stride-ai-arxiv-2605-17163-2026-06-23.md]]"
---

# STRIDE-AI Threat Modeling Framework

Academic proposal ([arXiv:2605.17163](https://arxiv.org/abs/2605.17163), CIIT 2026; Tsafac Nkombong Regine Cyrille, Franziska Schwarz) that adapts Microsoft's STRIDE threat-modeling methodology to generative-AI systems. Its stated contribution is to bridge a high-level governance standard ([[nist-ai-rmf|NIST AI RMF]]) with a technical vulnerability taxonomy ([[owasp-llm-top-10|OWASP LLM Top 10]]), the layer [[threat-modeling-for-ai|threat modeling for AI]] occupies between policy and concrete attack catalogs.

The paper argues that deterministic-system threat modeling does not account for the probabilistic behavior of models, which leaves attack vectors such as model inversion, data poisoning, and [[prompt-injection|prompt injection]] unaddressed.

## Proposal

| Element | Description |
|---|---|
| Six-phase lifecycle | An assessment workflow from scoping through validation |
| Adapted STRIDE | The six STRIDE categories (Spoofing, Tampering, Repudiation, Information disclosure, Denial of service, Elevation of privilege) re-mapped onto AI assets — training data, model weights, the prompt/context window, inference endpoints |
| Web-based tool | A purpose-built application to operationalize the assessment |
| Black-box validation | A worked case study against a deployed LLM chatbot |

## Reported result

In a sandbox case study the framework reduced the chatbot's attack success rate from 80% to 15%.[^stride] This is a single self-reported case study, not an independent benchmark; the wiki treats the figure as illustrative rather than a general efficacy claim.

[^stride]: §case study, [arXiv:2605.17163](https://arxiv.org/abs/2605.17163). Single deployed-chatbot sandbox; no independent replication.

## Placement

STRIDE-AI is the *elicitation* method in the [[threat-modeling-for-ai|threat-modeling spine]]'s five-step pass, and one concrete answer to its question of how to extend classical STRIDE-style modeling to model and agent assets. The [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix records which catalog categories each STRIDE-AI category tends to surface. It maps to the same design-stage practice the wiki anchors at CMM domain D4 (Runtime and Guardrails), where the crosswalk files build-time threat modeling for want of a development-time domain. It is narrower than [[csa-maestro|CSA MAESTRO]]'s seven-layer agentic decomposition, since it targets generative-AI applications generally rather than multi-agent ecosystems. It complements the attack catalogs ([[owasp-llm-top-10|OWASP LLM Top 10]], [[mitre-atlas|MITRE ATLAS]]) by supplying the elicitation method that walks an architecture against them.

## See also

- [[threat-modeling-for-ai|Threat Modeling for AI]] — the concept this operationalizes
- [[nist-ai-rmf|NIST AI RMF]] · [[owasp-llm-top-10|OWASP LLM Top 10]] — the two standards it bridges
- [[csa-maestro|CSA MAESTRO]] — the layered agentic alternative
