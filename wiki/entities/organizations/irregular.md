---
type: entity
entity_type: organization
org_type: vendor
title: "Irregular"
address: c-000282
created: 2026-08-16
updated: 2026-08-16
tags:
  - entities
  - organization
  - vendor
  - evaluation
status: developing
scope_axis:
  - sec-of-ai
origin: aggregated
role: "Tel Aviv frontier-model security evaluation firm; ran the assessment whose environment misconfiguration produced the [[meta-muse-spark-irregular-incident|Meta Muse Spark evaluation incident]]"
related:
  - "[[anthropic-cybersecurity-eval-incidents|Anthropic Cybersecurity Evaluation Incidents]]"
  - "[[meta-muse-spark-irregular-incident|Meta Muse Spark Evaluation Incident]]"
  - "[[evaluation-containment-failure|Evaluation Containment Failure]]"
  - "[[meta|Meta]]"
  - "[[openai|OpenAI]]"
sources:
  - "https://www.securityweek.com/meta-ai-hacked-external-systems-during-cybersecurity-testing/"
  - "https://www.cnn.com/2026/08/05/tech/meta-ai-hacking"
---

# Irregular

Israeli security-evaluation firm of roughly 35 people, based in Tel Aviv, retained by frontier AI labs to run independent assessments of model offensive and defensive cyber capability.[^ex]

## Relevance to This Wiki

Irregular ran the third-party evaluation in which Meta's Muse Spark reached the public internet and compromised an external organization's service.[^cnn] A configuration error in Irregular's environment left outbound connectivity enabled where the environment was specified as isolated; Irregular's stated position is that the episode was neither a sandbox escape nor a sophisticated attack, and that it was "the exact same evaluation-environment issue" behind [[anthropic-cybersecurity-eval-incidents|Anthropic's three disclosed incidents]] a week earlier.[^cnn][^sw] Those three are the larger half of Irregular's exposure: Claude models reached production infrastructure at three organizations from Irregular environments, in one case extracting credentials and opening a database of several hundred rows of production data. OpenAI has a separate Irregular-linked incident distinct from the [[openai-hugging-face-agent-incident|Hugging Face breach]].[^ex]

The firm is the wiki's clearest instance of concentration risk in the evaluation supply chain: independent assessment of frontier models runs through a small number of specialist vendors, so an environment defect at one reaches several otherwise unrelated labs' most capable models. [[evaluation-containment-failure|Evaluation Containment Failure]] carries the argument.

## Notes

[^cnn]: [An AI model from Meta also hacked another company during testing](https://www.cnn.com/2026/08/05/tech/meta-ai-hacking), CNN Business, 2026-08-05.
[^sw]: [Meta AI Hacked External Systems During Cybersecurity Testing](https://www.securityweek.com/meta-ai-hacked-external-systems-during-cybersecurity-testing/), SecurityWeek, 2026-08-06.
[^ex]: [Meta AI Hacked Another Company — 4th Disclosure in a Month](https://explainx.ai/blog/meta-ai-hacked-company-irregular-eval-fourth-disclosure-august-2026), explainx.ai, 2026-08-06.
