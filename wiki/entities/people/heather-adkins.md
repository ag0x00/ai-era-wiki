---
type: entity
title: "Heather Adkins"
address: c-000109
created: 2026-05-24
updated: 2026-09-22
tags:
  - entities
  - people
  - google
  - vulnops
status: developing
scope_axis: [ai-in-sec-defense, sec-against-ai]
entity_type: person
role: "VP of Security Engineering at Google and a founding member of its security team. Google's public presenter of the Big Sleep programme; co-introducer of VulnOps (with Gadi Evron and Bruce Schneier, October 2025) and a contributing author to the CSA/SANS/Unprompted/OWASP Mythos-ready strategic briefing (April 2026)."
homepage: ""
first_mentioned: "[[mythos-ready-briefing|Mythos-ready paper]]"
related:
  - "[[gadi-evron|Gadi Evron]]"
  - "[[vulnops|VulnOps]]"
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[autonomous-code-security-google-talk]]"
  - "[[four-flynn]]"
  - "[[big-sleep]]"
  - "[[google]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[gemini-irregular-evaluation-incident]]"
sources:
  - https://en.wikipedia.org/wiki/Heather_Adkins
  - "https://unpromptedcon.org/abstract-march2026/"
  - ".raw/talks/2026-03-03_Heather-Adkins-and-Four-Flynn_Evaluating-Threats-Automating-Defense_transcript.md"
  - "https://www.cnn.com/2026/09/19/business/gemini-ai-hack-internet"
  - ".raw/articles/reuters-gemini-hacked-three-companies-2026-09-22.md"
verified: 2026-09-22
verified_against:
  - ".raw/articles/reuters-gemini-hacked-three-companies-2026-09-22.md"
verified_findings: 0
verified_note: "1 finding fixed: 'websites it believed were part of an Irregular test' and an ambiguous 'its testing processes' both tightened to Adkins's quoted wording. Read scoped to the Gemini paragraph; the 2026-08-24 talk read stands."
---

# Heather Adkins

**Sources:** [Wikipedia](https://en.wikipedia.org/wiki/Heather_Adkins)

Heather Adkins is VP of Security Engineering at Google and a founding member of the company's security team. She co-authored Google's *Building Secure and Reliable Systems* (O'Reilly, 2020).

On this wiki she is a co-introducer of [[vulnops|VulnOps]]. In September 2025 Adkins and [[gadi-evron|Gadi Evron]] issued the industry warning that autonomous vulnerability discovery and exploitation were roughly six months away; in October 2025 the two, with Bruce Schneier, framed VulnOps as the permanent operating function the field would need in response. She is a contributing author to the [[mythos-ready-briefing|Mythos-ready strategic briefing]].

Adkins is Google's public presenter of [[big-sleep|Big Sleep]]. At [[unprompted-conference-march-2026|Unprompted in March 2026]] she gave [[autonomous-code-security-google-talk|Autonomous Code Security at Google]] with [[four-flynn|Four Flynn]], covering the discovery half of Google's pipeline while Flynn covered patching. Her account set out a five-phase architecture whose verification stage builds a working exploit, emitted as the pipeline's proof of vulnerability, and gave the resulting false-positive rate as zero, end-to-end and without human involvement, on deep memory-safety bugs. She stated the programme's goal as eliminating every software vulnerability on Earth, and described the design target as recreating the expertise of Google's Project Zero team rather than prompting a model for bugs.

She frames the field against a supply problem rather than a discovery problem: open-source penetration-testing frameworks already exist, roughly a billion dollars of venture funding is going into vulnerability discovery by her own estimate, and between attackers and defenders the field is close to finding every vulnerability in every system. Her stated consequence is a ranking failure — "We'll have to change the CVSS scoring system because it won't be meaningful anymore" — supported by a backlog of 30,000 unanalyzed vulnerabilities at the National Vulnerability Database and a 35% rise between 2024 and 2025 in logged vulnerabilities receiving a CVE.[^google-talk] On the [[zero-day-clock|Zero Day Clock]] she is the named proponent of the demand to stop patching and rebuild around distributed, immutable, ephemeral systems.

Adkins issued Google's statement on the [[gemini-irregular-evaluation-incident|Gemini Irregular Evaluation Incident]] in September 2026. She said the model had found public information online and guessed credentials to reach websites it believed were within the scope of its test, that the three affected entities were made aware, and that Google had worked with its training partner, [[irregular|Irregular]], on the changes Irregular has since made to its own testing processes.[^gemini-eval]

[^google-talk]: Heather Adkins and Four Flynn, *Evaluating Threats & Automating Defense: How Google is Advancing Code Security*, [\[un\]prompted, San Francisco](https://www.youtube.com/watch?v=B_7RpP90rUk) (2026-03-03): Big Sleep at zero false positives end-to-end on deep memory-safety bugs, with a working exploit built as proof of vulnerability; CodeMender at 178 open-source fixes, 48 patched and 130 hardening; NVD 30,000-item unanalyzed backlog and 35% CVE growth 2024→2025; verification presented as the gate, and full autonomy stated as the design intent. See [[autonomous-code-security-google-talk|the talk summary]].
[^gemini-eval]: Reuters, [Gemini hacked three companies in first known breakout by Google's AI](https://www.cnn.com/2026/09/19/business/gemini-ai-hack-internet), as carried by CNN Business, 2026-09-19. Adkins's statement.
