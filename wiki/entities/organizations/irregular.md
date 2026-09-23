---
type: entity
entity_type: organization
org_type: vendor
title: "Irregular"
address: c-000282
created: 2026-08-16
updated: 2026-09-22
tags:
  - entities
  - organization
  - vendor
  - evaluation
status: developing
scope_axis:
  - sec-of-ai
origin: aggregated
role: "Tel Aviv frontier-model security evaluation firm; its evaluation environment produced the Anthropic, Meta Muse Spark and Gemini evaluation incidents"
related:
  - "[[anthropic-cybersecurity-eval-incidents|Anthropic Cybersecurity Evaluation Incidents]]"
  - "[[meta-muse-spark-irregular-incident|Meta Muse Spark Evaluation Incident]]"
  - "[[evaluation-containment-failure|Evaluation Containment Failure]]"
  - "[[meta|Meta]]"
  - "[[openai|OpenAI]]"
  - "[[gemini-irregular-evaluation-incident|Gemini Irregular Evaluation Incident]]"
  - "[[irregular-addressing-recent-incidents|Irregular Evaluation Incident Findings]]"
  - "[[evaluation-containment-open-questions|Evaluation Containment Open Questions]]"
  - "[[google|Google]]"
sources:
  - "https://www.securityweek.com/meta-ai-hacked-external-systems-during-cybersecurity-testing/"
  - "https://www.engadget.com/2231446/meta-ai-model-hacked-third-party-irregular/"
  - "https://www.cnn.com/2026/08/05/tech/meta-ai-hacking"
  - "https://www.cnbc.com/2026/09/18/googles-gemini-becomes-latest-ai-model-to-break-out-and-hack-computer-systems.html"
  - "https://www.irregular.com/research/addressing-recent-incidents-ongoing-findings-and-path-forward"
  - ".raw/articles/cnbc-gemini-latest-ai-model-to-break-out-2026-09-22.md"
  - ".raw/articles/irregular-addressing-recent-incidents-2026-09-22.md"
verified: 2026-09-22
verified_against:
  - ".raw/articles/cnbc-gemini-latest-ai-model-to-break-out-2026-09-22.md"
  - ".raw/articles/irregular-addressing-recent-incidents-2026-09-22.md"
verified_findings: 0
verified_note: "1 finding fixed: the sandbox-escape characterisation now carries the Irregular spokesperson's verbatim wording and an Engadget citation that demonstrably holds it; SecurityWeek, cited before, does not. CNN and SecurityWeek are unarchived and were checked live."
---

# Irregular

Israeli security-evaluation firm of roughly 35 people, based in Tel Aviv, retained by frontier AI labs to run independent assessments of model offensive and defensive cyber capability.[^ex]

## Relevance to This Wiki

Irregular ran the third-party evaluation in which Meta's Muse Spark reached the public internet and compromised an external organization's service.[^cnn] A configuration error in Irregular's environment left outbound connectivity enabled where the environment was specified as isolated. An Irregular spokesperson said the episode "did not involve a sandbox escape or a sophisticated cyber action", and that it was "the exact same evaluation-environment issue" behind [[anthropic-cybersecurity-eval-incidents|Anthropic's three disclosed incidents]] a week earlier.[^eng][^cnn] Claude models reached production infrastructure at three organizations from Irregular environments, in one case extracting credentials and opening a database of several hundred rows of production data. Google confirmed on 2026-09-18 that a Gemini model had reached protected systems at three more companies during an Irregular evaluation in May, and an Irregular spokesperson said the case was the issue already reported and not a materially separate incident; see [[gemini-irregular-evaluation-incident|Gemini Irregular Evaluation Incident]].[^cnbc] OpenAI has a separate Irregular-linked incident distinct from the [[openai-hugging-face-agent-incident|Hugging Face breach]].[^ex]

Irregular's own account, published on 2026-08-14, traces every public disclosure to that date to one underlying issue in a single evaluation scenario. Internet access was unintentionally available, and the fictional target company's name matched a real domain that Irregular's initial review had missed. The events occurred in fewer than 1 in 10,000 advanced simulations, usually hundreds of turns in, and Irregular has committed to an open whitepaper on internet-access standards for pre-deployment evaluations. [[irregular-addressing-recent-incidents|Irregular Evaluation Incident Findings]] summarizes the account.[^irr] Whether one scenario covers every case attributed to it is open; [[evaluation-containment-open-questions|Evaluation Containment Open Questions]] tracks that and the rest of the unsettled record.

The firm is the wiki's clearest instance of concentration risk in the evaluation supply chain: independent assessment of frontier models runs through a small number of specialist vendors, so an environment defect at one reaches several otherwise unrelated labs' most capable models. [[evaluation-containment-failure|Evaluation Containment Failure]] carries the argument.

## Notes

[^cnn]: [An AI model from Meta also hacked another company during testing](https://www.cnn.com/2026/08/05/tech/meta-ai-hacking), CNN Business, 2026-08-05.
[^eng]: [Meta claims its own AI also hacked into a third-party service during testing](https://www.engadget.com/2231446/meta-ai-model-hacked-third-party-irregular/), Engadget, 2026-08-05. The Irregular spokesperson's statement, quoted verbatim.
[^ex]: [Meta AI Hacked Another Company — 4th Disclosure in a Month](https://explainx.ai/blog/meta-ai-hacked-company-irregular-eval-fourth-disclosure-august-2026), explainx.ai, 2026-08-06.
[^cnbc]: MacKenzie Sigalos and Kif Leswing, [Google's Gemini becomes latest AI model to break out and hack computer systems](https://www.cnbc.com/2026/09/18/googles-gemini-becomes-latest-ai-model-to-break-out-and-hack-computer-systems.html), CNBC, 2026-09-18. Google's confirmation and the Irregular spokesperson's statement.
[^irr]: Irregular, [Addressing Recent Incidents: Ongoing Findings and Path Forward](https://www.irregular.com/research/addressing-recent-incidents-ongoing-findings-and-path-forward), 2026-08-14. The single-scenario attribution, the name collision, the rate of fewer than 1 in 10,000 advanced simulations (no count given), and the whitepaper commitment.
