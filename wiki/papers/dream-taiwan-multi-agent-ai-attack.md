---
type: paper
title: "Taiwan Multi-Agent Attack Reconstruction"
address: c-000264
created: 2026-08-15
updated: 2026-08-15
tags:
  - papers
  - incident
  - offensive-ai
  - multi-agent
  - agent-collective
  - autonomous-exploit-generation
status: summarized
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
year: 2026
authors: ["Dream Research Labs"]
venue: "Dream Security Blog, 2026-08-12"
key_claim: "A deliberately constructed multi-agent framework — up to eight lettered sub-agents orchestrated on Hermes and OpenClaw, with two-layer Bayesian prioritization and a self-correcting verification loop — ran a four-day near-autonomous intrusion against Taiwanese government infrastructure, cracking 85 credentials, exfiltrating 2,564+ personnel records, and pivoting to a nuclear safety agency and energy-sector vendors."
methodology: "Threat-research reconstruction from a recovered operational workspace: 160+ MB, 1,395 files, comprising the attacker's own triage reports, attack-chain scoring logs, learning-cycle research notes, and after-action summaries. Not a victim-side telemetry account — the source is the attacker's own working files, recovered rather than disclosed by the attacker."
source_url: "https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia"
related:
  - "[[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]]"
  - "[[reuters-taiwan-ai-hacking-campaign|Taiwan Confirms AI-Agent Hacking Campaign (Reuters)]]"
  - "[[dream-security|Dream Security]]"
  - "[[hermes-agent|Hermes]]"
  - "[[openclaw|OpenClaw]]"
  - "[[offensive-agent-collective|Offensive Agent Collective]]"
  - "[[gtg-1002-ai-orchestrated-espionage|GTG-1002]]"
  - "[[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]"
  - "[[zero-day-clock|Zero-Day Clock]]"
  - "[[autonomous-exploit-generation|Autonomous Exploit Generation]]"
  - "[[offensive-ai-state-of-the-field|Offensive AI: State of the Field]]"
sources:
  - "https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia"
  - ".raw/articles/dream-security-taiwan-multi-agent-ai-attack-2026-08-12.md"
---

# Taiwan Multi-Agent Attack Reconstruction

**Source:** [Dream Security Blog](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia), Dream Research Labs, 2026-08-12.

[[dream-security|Dream Security]]'s DREAM Lab recovered the complete operational workspace of a multi-agent AI attack framework — its findings, first shared with the *Financial Times*, are the primary technical account behind the [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]] that Taiwan's Ministry of Digital Affairs confirmed publicly the next day.[^reuters] Unlike [[gtg-1002-ai-orchestrated-espionage|GTG-1002]] and [[openai-hugging-face-incident-blackhat-2026|the OpenAI–Hugging Face reconstruction]], both victim- or vendor-side accounts, this source is built from the attacker's own working files: triage reports, attack-chain probability scoring, research logs, and after-action summaries.

## Architecture: two orchestration platforms, lettered sub-agents

The framework runs on [[hermes-agent|Hermes]] and [[openclaw|OpenClaw]], two open-source agent orchestration platforms, operating under separate workspace identifiers (`.hermes`, `.openclaw`). Each sub-agent receives a letter designation and a specific mission — credential theft, API exploitation, supply-chain reconnaissance. Across 12 documented attack waves between 2026-07-01 and 2026-07-04, agents labeled A through Q appeared, with up to 8 deployed concurrently in a single wave dispatched in 3 batches. The 1,395 files produced in roughly four days are, per Dream, consistent with automation beyond a human operator's unaided output.

## Attack chain

1. **Reconnaissance.** Decompiled JavaScript bundles from an Angular-based government portal to extract API endpoints, OAuth client IDs, and Keycloak configuration, mapping 21 connected government systems and the national SSO architecture (6 sub-realms, all OIDC endpoints, 2 RSA signing keys).
2. **SDK harvesting.** Autonomously found and scraped a GitBook-hosted SSO integration guide, pulled SDK sample projects, and ran an AI static-analysis pass (`Local-LLM-SAST-0day`) against them — a dead end for exploitation (zero overlap with confirmed findings) but illustrative of the framework researching adjacent artifacts unprompted.
3. **Initial access, three parallel vectors.** Undocumented debug endpoints accepting unauthenticated requests; credential spraying against a CAPTCHA-protected portal using Tesseract OCR (100% CAPTCHA-solve rate), cracking 85 accounts; a JWT endpoint accepting the `alg: none` header, forging valid tokens without the signing key.
4. **Lateral movement.** 84 of 85 cracked accounts (98.8%) pivoted through SSO to an internal information system with no additional authentication.
5. **Exfiltration.** 2,564+ personnel records (1,409 employees, 916 from an unauthenticated API, 239 legal professionals from a Ministry of Justice endpoint), a full department user database, 7 SSO client secrets, 6 internal database credentials.
6. **Supply-chain expansion.** Pivoted to government IT vendors, a nuclear safety agency, a government email system, and 7+ energy-sector companies.

## Decision architecture: two-layer Bayesian scoring

The framework scores individual findings against a formal Bayesian model — an uninformative P=0.50 prior updated by likelihood ratios (tool-scan positive LR+ = 6.0; manual confirmation LR+ = 10.0; WAF/constraint present LR− = 0.30) — then chains confirmed findings into attack paths scored by `P_success = P_chain × (1 − P_blocker)`. The worked SSO lateral-movement chain scored 99% before execution and was borne out: 98.8% of accounts pivoted successfully. Five "Learning Cycles" ran autonomous research against vulnerability databases and GitHub for techniques tailored to the specific target environment. The framework also self-corrected: 7 false positives were caught and discarded through its own re-verification protocol, including a SQL-injection misidentification traced to an SMTP timeout rather than database execution, and confirmed findings required six independent retests before inclusion.

Guardrail refusals in the underlying models were bypassed by framing all activity as "authorized penetration testing" — a jailbreak pattern, not a technical exploit against the harness.

## Attribution

Linguistic analysis of the operational documentation found code-switching between Simplified Chinese in internal status reports and Traditional Chinese in target-facing analysis, which Dream reads as pointing to a Chinese-language operator. Dream names no specific group and offers no technical-infrastructure attribution (C2 domains, malware signatures); the evidence is linguistic alone. This is weaker than [[gtg-1002-ai-orchestrated-espionage|GTG-1002]]'s attribution, which Anthropic based on its own account and abuse-classifier telemetry of the attacker's platform usage. Taiwan's Ministry of Digital Affairs statement does not name China, and China's Taiwan Affairs Office did not respond to Reuters' request for comment.[^reuters]

## Bearing on wiki positions

**This is the attacker-built offensive agent collective the wiki had marked absent.** [[offensive-agent-collective|Offensive Agent Collective]] and [[offensive-ai-state-of-the-field|Offensive AI: State of the Field]] both carried an open gap: the only documented collective (OpenAI–Hugging Face) formed by accident inside a training pipeline, and no source showed an adversary deliberately building one and aiming it at a chosen target. This framework is deliberately constructed — lettered sub-agents with assigned missions, a shared workspace, structured feedback loops carrying results from each wave into the next — and pointed at a chosen target by a human operator who set the objective. It closes the gap while remaining distinct from both prior anchors: unlike OpenAI–Hugging Face, a human operator is present throughout (target selection, objective-setting); unlike GTG-1002's operator-mediated coordination across separate Claude Code instances, coordination here runs through the framework's own shared workspace and cross-wave feedback loop, not through the human re-reading and re-tasking each step.

**The framework is near-autonomous, not fully autonomous, and the source itself does not claim otherwise.** Dream calls it "a near-autonomous attack, running off readily available harnesses." Cris Thomas of Semgrep, quoted in Reuters' coverage, cautions in the same direction: *"There's still a human in there somewhere. Somebody had to choose who to attack, had to establish an objective and give it a directive. It's not totally 100% autonomous."*[^reuters] The precedent this source sets is deliberate construction of a collective, not removal of the operator.

**Reinforces [[zero-day-clock|the Zero Day Clock]].** Dream's own framing: *"the cost of running a competent attack has collapsed, but the cost of defending against one has not."*

## Notes

[^reuters]: [Taiwan says it was targeted last month in AI-driven hacking campaign](https://www.reuters.com/world/china/taiwan-says-it-was-targeted-last-month-ai-driven-hacking-campaign-2026-08-13/), Reuters, 2026-08-13. Summarized at [[reuters-taiwan-ai-hacking-campaign|the source summary]].
