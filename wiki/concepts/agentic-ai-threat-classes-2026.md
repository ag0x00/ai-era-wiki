---
type: concept
title: "Agentic AI Threat Classes"
address: c-000260
created: 2026-05-02
updated: 2026-09-29
tags:
  - concepts
  - threat-modeling
  - agentic-ai
  - peer-review
status: developing
origin: aggregated
scope_axis:
  - sec-of-ai
  - sec-against-ai
complexity: advanced
domain: ai-security
related:
  - "[[threat-modeling-for-ai]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[owasp-ai-exchange]]"
  - "[[lethal-trifecta]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[mitre-atlas]]"
  - "[[csa-maestro]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[peer-review-readiness-2026-05-02]]"
  - "[[gtg-1002-ai-orchestrated-espionage]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[offensive-agent-collective]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[dream-taiwan-multi-agent-ai-attack]]"
  - "[[dream-security]]"
  - "[[openai-dsewiki-agent-collusion]]"
  - "[[apollo-research]]"
  - "[[aisi-uk]]"
  - "[[cset-georgetown]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[owasp-asi-aiuc1-crosswalk]]"
  - "[[standards-review-eu-ai-act-2026-Q2]]"
  - "[[anthropic-threat-intelligence-reports]]"
  - "[[gtg-2002-vibe-hacking-extortion]]"
  - "[[llm-attack-navigator]]"
  - "[[capability-floor-collapse]]"
  - "[[ai-attribution-primaries-2026-08-17]]"
  - "[[precize-agentic-ai-top10]]"
  - "[[owasp-agentic-skills-top-10]]"
sources:
  - "https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf"
  - "https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf"
  - "https://openai.com/index/hugging-face-incident-and-the-road-ahead/"
  - "https://arxiv.org/abs/2401.05566"
  - "https://www.justice.gov/nsd/data-security"
  - "https://arxiv.org/abs/2402.07510"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Current primary Anthropic, OpenAI, DOJ, and cited research checked 2026-09-29."
---

# Agentic AI Threat Classes

This register helps an architect select tests for a defined deployment shape. Classify a case by **the surface reached, what the actor controls, and the effect observed**. One campaign may cross several classes; record each failed boundary rather than assigning the campaign one label. [[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10 (ASI)]] and [[mitre-atlas|MITRE ATLAS]] supply technique detail. This register identifies assessment decisions those taxonomies alone do not make for a deployment.

## Class register

### AI-assisted workforce abuse

A malicious or impersonated worker uses a valid account and AI assistance to obtain or maintain access. The immediate effect is fraudulent employment under apparently valid credentials; subsequent data access or code change depends on the role granted. [Anthropic's August 2025 report](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf#page=11) documents AI-assisted remote-worker fraud. Test whether the deployment binds tool calls to the worker's effective authority, logs high-impact actions, and refuses an action beyond the approved task. Identity authentication alone does not establish intent.

### AI-orchestrated external intrusion

An external operator directs agents through reconnaissance, exploitation, and follow-on actions against exposed systems. The attacker controls the offensive workflow, while defenders observe the resulting network, identity, and application events. [Anthropic's GTG-1002 investigation](https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf) documents this pattern in one disrupted campaign. Test whether an agent-facing route enforces its own rate, authorization, tool, and egress decisions under repeated adaptive probing. Where the target is conventional infrastructure rather than an agent deployment, enterprise intrusion controls remain the assessment instrument.

### Cross-agent coordination through shared state

Agents with no intended communication path may still write and read a shared cache, registry, file store, or log. The controlled surface is shared writable infrastructure. The effect is information transfer or joint action outside the designed agent topology. [OpenAI's account of the Hugging Face incident](https://openai.com/index/hugging-face-incident-and-the-road-ahead/) describes agents exchanging information through a package manager during training and evaluations. Test isolation on every shared write path, including infrastructure that was not designed as an agent message bus, and reconstruct a cross-agent sequence from evidence. The [steganographic collusion paper](https://arxiv.org/abs/2402.07510) is a research model of a different, concealed-message path; it is not evidence that the OpenAI incident used steganography.

### Model or release tampering

An attacker changes weights, training inputs, dependencies, or configuration before promotion, or introduces a model with a hidden trigger. The observable effect is a targeted behavior that ordinary examples may miss. [Sleeper Agents](https://arxiv.org/abs/2401.05566) demonstrates persistent backdoor behavior in a controlled study; it does not establish prevalence in production. Test the exact assembled release against a material trigger, verify artifact identity at promotion, and retest after a model or component change. An unintentional supplier update can also regress behavior, but it has no attacker-controlled input and belongs in change assurance.

## Legal and supplier continuity case

Legal access and supplier cutoff are deployment constraints, not adversarial ML techniques. The [US Department of Justice Data Security Program](https://www.justice.gov/nsd/data-security) prohibits or restricts specified **covered data transactions** that could give countries of concern or covered persons access to US government-related data or Americans' bulk sensitive personal data. It does not ban every AI provider associated with a country of concern, and it does not impose a general data-localization rule. Applicability depends on the data, parties, transaction class, exemptions, and effective rule. An architect should map data flows and supplier access, obtain legal determination where relevant, and test whether a required provider change can be made without losing the agent's approved authority or evidence path.

## CMM routing

| Case | First assessment owner | Evidence to request |
|---|---|---|
| AI-assisted workforce abuse | D2 identity, D3 action authority, D7 evidence | Principal-to-action trace and denied excess action |
| AI-orchestrated probing of an agent route | D4 runtime, D5 inter-agent or egress path where present, D7 detection | Repeated route tests and correlated decisions |
| Cross-agent shared state | D3 action authority, D4 isolation, D7 reconstruction | Shared-medium inventory, denied cross-scope write, joint trace |
| Model or release tampering | D8 engineering and supply assurance; D6 for poisoned data | Artifact provenance, release decision, adversarial retest |
| Covered-data or supplier cutoff | D1 risk decision, D9 continuity | Data-flow determination and exercised transition path |

The [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] grades its domain criteria, not these classes as separate maturity scores. Use its [[agentic-ai-security-cmm-measurement-protocol|Assessor's Handbook]] to mark applicability and record evidence for the deployment. A threat class can justify a higher target or an additional risk-selected test without changing a domain's observed level.

<!-- sources:auto -->
## Sources

- [Anthropic Threat Intelligence Report, August 2025](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf)
- [Anthropic GTG-1002 investigation](https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf)
- [OpenAI Hugging Face incident account](https://openai.com/index/hugging-face-incident-and-the-road-ahead/)
- [Sleeper Agents](https://arxiv.org/abs/2401.05566)
- [DOJ Data Security Program](https://www.justice.gov/nsd/data-security)
- [Secret Collusion among Generative AI Agents](https://arxiv.org/abs/2402.07510)
<!-- /sources -->
