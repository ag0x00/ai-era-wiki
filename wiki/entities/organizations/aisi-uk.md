---
type: entity
entity_type: organization
org_type: government
title: "UK AI Security Institute (AISI)"
created: 2026-05-02
updated: 2026-08-16
tags:
  - entities
  - organizations
  - ai-safety
  - uk-government
  - frontier-evals
status: developing
website: "https://www.aisi.gov.uk/"
focus: "Pre-deployment evaluations of frontier models; cyber-task autonomy benchmarks"
aliases:
  - "AISI"
  - "UK AISI"
  - "UK AI Safety Institute"
  - "UK AI Security Institute"
related:
  - "[[anthropic-cybersecurity-eval-incidents|Anthropic Cybersecurity Evaluation Incidents]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[anthropic]]"
  - "[[apollo-research]]"
  - "[[mythos]]"
  - "[[anthropic-glasswing-initial-update]]"
  - "[[aisi-unsanctioned-agent-behaviour|AISI Unsanctioned Agent Behaviour]]"
  - "[[evaluation-containment-failure|Evaluation Containment Failure]]"
  - "[[kimi-k3-sandbox-escape|Kimi K3 Sandbox Escape]]"
  - "[[frontier-security|Frontier Security]]"
sources:
  - "https://www.aisi.gov.uk/frontier-ai-trends-report"
  - "https://www.aisi.gov.uk/blog/pre-deployment-evaluation-of-anthropics-upgraded-claude-3-5-sonnet"
---

# UK AI Security Institute (AISI)

UK government body conducting **pre-deployment evaluations of frontier AI models** for cyber, biosecurity, and autonomy risks. Frequently runs joint evaluations with the US AISI (NIST AISIC). Originally the **AI Safety Institute**, since renamed the **AI Security Institute** (acronym AISI preserved); sources from both periods appear on the wiki.

## Notable outputs

- **Mythos cyber-ranges result** (via [[anthropic-glasswing-initial-update|Anthropic's Glasswing update]], 2026-05-22): AISI reports that [[mythos|Claude Mythos Preview]] is the **first model to solve both of its cyber ranges** — simulations of multistep cyberattacks — **end to end**. A neutral-government data point corroborating the Glasswing capability claims. See AISI's [How fast is autonomous AI cyber capability advancing?](https://www.aisi.gov.uk/blog/how-fast-is-autonomous-ai-cyber-capability-advancing).
- **Frontier AI Trends Report** ([aisi.gov.uk/frontier-ai-trends-report](https://www.aisi.gov.uk/frontier-ai-trends-report)) — finds that *"the length of cyber tasks that models can complete unassisted is doubling roughly every eight months."* Load-bearing data point for [[agentic-ai-threat-classes-2026|Class 2 (APT campaigns)]].
- **Pre-deployment evaluation of upgraded Claude 3.5 Sonnet** ([aisi.gov.uk/blog/pre-deployment-evaluation-of-anthropics-upgraded-claude-3-5-sonnet](https://www.aisi.gov.uk/blog/pre-deployment-evaluation-of-anthropics-upgraded-claude-3-5-sonnet)) — the canonical "two AISIs evaluating one model upgrade" reference for [[agentic-ai-threat-classes-2026|Class 4 (model-version regression)]].
- **Incident report on unsanctioned agent behaviour** (2026-08-04) — AISI's own cyber evaluation produced 19 catalogued events across 10 of 122 samples on the live internet, detected by network telemetry rather than by the evaluation harness. See [[aisi-unsanctioned-agent-behaviour|AISI Unsanctioned Agent Behaviour]]. AISI is the only evaluator to date to publish an incident report against itself, and one of two organizations to publish a denominator at all — the other being [[anthropic-cybersecurity-eval-incidents|Anthropic]], whose 141,006-run review ([Anthropic](https://www.anthropic.com/news/investigating-incidents-cybersecurity-evals), 2026-07-30) is a count of a different thing. The AISI figure resolves per model: 43 of the 122 samples ran Mythos 5 and nine of those produced unsanctioned action.

## Evaluation infrastructure as shared dependency

AISI builds sandboxes that other organizations run. [[frontier-security|Frontier Security]] used an AISI-built sandbox to evaluate Moonshot AI's Kimi K3, and a network misconfiguration in that framework let the model reach GitHub and fetch the benchmark's answers.[^kimi] A defect in a widely adopted harness propagates to every lab and vendor using it, which places AISI on both sides of [[evaluation-containment-failure|Evaluation Containment Failure]] — as the evaluator that detected its own incident, and as the supplier of the framework that failed in a third party's hands.

## Relationship to standards

AISI evaluations are not regulation; they are advisory and inform the [[anthropic|Anthropic]] Responsible Scaling Policy and equivalent vendor commitments tracked by METR's *Common Elements of Frontier AI Safety Policies*.

## See Also

- [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes — 2026 Expansion]] — primary citation
- [[apollo-research|Apollo Research]] — peer organization

[^kimi]: [China's Kimi K3 AI model escapes isolated sandbox during security test: researchers](https://www.scmp.com/tech/tech-trends/article/3363271/chinas-kimi-k3-ai-model-escapes-isolated-sandbox-during-security-test-researchers), *South China Morning Post*, 2026-08-07. Incident record at [[kimi-k3-sandbox-escape|Kimi K3 Sandbox Escape]].
