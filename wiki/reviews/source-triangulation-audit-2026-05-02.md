---
type: review
title: "Source Triangulation Audit"
address: c-000165
origin: produced
created: 2026-05-02
updated: 2026-05-29
tags:
  - reviews
  - methodology
  - source-discipline
  - peer-review
scope_axis:
  - sec-of-ai
status: addressed
target: "Wiki's load-bearing claims that previously relied on Gartner / Insight Partners / Knostic / OWASP without academic or government-survey corroboration"
related:
  - "[[peer-review-readiness-2026-05-02]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[stanford-hai]]"
  - "[[metr]]"
  - "[[wef]]"
  - "[[enisa]]"
  - "[[agentdojo]]"
---

# Source Triangulation Audit (2026-05-02)

Closes [[peer-review-readiness-2026-05-02|peer-review-readiness]] honorable-mention #1: *"Statistics drawn from a narrow source set (Gartner, Insight Partners, Knostic, OWASP), same handful. No academic literature or industry surveys beyond these."* Per-claim triangulation against academic and government-survey sources, with explicit verdict on what is corroborated, what's contested, and what remains single-sourced.

**Headline result.** Of 8 audited load-bearing claims, **5 are corroborated** by independent academic or government-survey sources after this pass, **2 are partially corroborated** (direction confirmed, exact figures single-sourced), and **3 remain genuinely single-sourced** and should be labeled as such in the wiki. Five new sources (Stanford HAI AI Index, Verizon DBIR, METR, WEF Global Cybersecurity Outlook, AgentDojo) join the wiki's citation rotation.

## Per-claim triangulation

### Claim 1: NHI scale and growth (82:1 ratio, 400% growth)

| Wiki citations today | CyberArk (82:1), Rubrik Zero Labs (45:1), arXiv 2503.18255 (50K → 250K, 2021–2025) |
|---|---|
| Independent corroboration | **Verizon DBIR 2025**: third-party breach involvement doubled YoY (15% → 30%) driven by ungoverned machine accounts; **SailPoint Horizons of Identity Security 2025–2026**: 69% of orgs have more machine than human identities, ~half deploy 10×; **GitGuardian/Verizon analysis**: 441,780 exposed secrets in public repos, 39% web-app infra; **[[enisa\|ENISA]] Threat Landscape 2025**: confirms identity/credential acceleration. |
| Verdict | **Partially corroborated.** Direction (NHI ≫ human, growing fast) is confirmed across 5+ independent sources. The exact 82:1 figure is single-vendor (CyberArk Vanson Bourne, n=2,600). The wiki should cite the directional claim with multi-source backing and label "82:1" as CyberArk-specific. |

### Claim 2: AI agent adoption rates and shadow AI

| Wiki citations today | [[knostic\|Knostic]] (75% knowledge-worker GenAI use, 78% BYOAI), vendor blog |
|---|---|
| Independent corroboration | **Microsoft Work Trend Index 2025**: *the actual primary source* for the 75%/78% numbers Knostic cites; **[[stanford-hai\|Stanford HAI AI Index 2025]]**: 78% business AI adoption (up from 55% in 2023); **McKinsey State of AI Nov 2025**: 88% of orgs use AI in ≥1 function (n=1,993, 105 countries); **BCG "Build for the Future 2025"**: 72% regular use, only 13% have AI agents in production; **IBM Cost of Data Breach 2025**: 20% had shadow-AI breaches, \$670K cost premium. |
| Verdict | **Corroborated, but the wiki is citing a downstream vendor when the original is government-survey-quality.** Replace Knostic citations with Microsoft Work Trend Index 2025 directly; add [[stanford-hai\|Stanford HAI]] for the cross-year curve and BCG for the *agentic vs GenAI* gap (the 13%-vs-72% delta is load-bearing). |

### Claim 3: Agent-orchestrated attack capability scaling (8-month cyber-task doubling)

| Wiki citations today | [[aisi-uk\|UK AISI]] Frontier AI Trends Report; [[anthropic\|Anthropic]] [[gtg-1002-ai-orchestrated-espionage\|GTG-1002]] |
|---|---|
| Independent corroboration | **[[metr\|METR]] "Measuring AI Ability to Complete Long Tasks" (arXiv 2503.14499)**: generalist task horizon doubles every ~7 months (2019–2025), accelerated to ~4 months in 2024–2025; this is the foundational methodology UK AISI's 8-month cyber figure builds on; **[[apollo-research\|Apollo Research]] "More Capable Models Are Better at In-Context Scheming"**: capability scaling correlates with scheming/deception; **[[cset-georgetown\|CSET]] "Autonomous Cyber Defense"**: government think-tank assessment. |
| Verdict | **Corroborated.** Strong triangulation across METR (peer-reviewable methodology), UK AISI (government measurement), and Apollo (capability-scheming correlation). Wiki should add METR as the methodological foundation rather than citing UK AISI standalone. |

### Claim 4: MCP CVE explosion (30+ CVEs in 60 days, 82% path traversal, 66% code injection)

| Wiki citations today | Single-source claim; origin unspecified, suggests vendor blog |
|---|---|
| Independent corroboration | **arXiv 2503.23278, "MCP: Landscape, Security Threats, and Future Research Directions"**: peer-reviewed (ACM TOSEM); 4-phase / 16-activity threat taxonomy. **arXiv 2506.13538, "MCP at First Glance"**: empirical study of 1,899 OSS MCP servers, **66% have code smells**, 14.4% bug patterns. **arXiv 2510.16558, "Toward Understanding Security Issues in the MCP Ecosystem"**: 67,057 servers across 6 registries. **VulnerableMCP database** ([vulnerablemcp.info](https://vulnerablemcp.info/)). **eSentire** reports ~22% path traversal in tested servers (different from wiki's 82%). |
| Verdict | **Contested.** Existence of MCP CVE wave well-corroborated; specific percentages do not match peer-reviewed denominators. The wiki's "66% code injection" looks like it may be a misread of the arXiv "66% code smells" figure on a different sample. **Action**: re-derive percentages from arXiv 2503.23278 / 2506.13538 / 2510.16558 / VulnerableMCP, or label as "vendor-source-derived; academic surveys report different denominators." |

### Claim 5: Prompt injection / detection rates

| Wiki citations today | [[anthropic\|Anthropic]] Constitutional Classifiers (86%→4.4%); Promptfoo blog (94%→71% on GPT-4o → GPT-4.1); [[llamafirewall\|LlamaFirewall]] PromptGuard 2 (97.5% recall, 1% FPR); [[breaking-the-lethal-trifecta-talk\|Bullen talk]] (Llama 3.3 70B 6.7%, Claude 3.7 Sonnet 1.5%) |
|---|---|
| Independent corroboration | **[[agentdojo\|AgentDojo]] (NeurIPS / arXiv 2406.13352)**: independent prompt-injection benchmark, 97 tasks / 629 security cases; best agents <25% attack success; tool-filtering drops to 7.5%. **InjecAgent (arXiv 2403.02691)**: indirect-prompt-injection benchmark; ReAct GPT-4 vulnerable in 24% of cases. **WASP (arXiv 2504.18575)**: web-agent security benchmark. **Trendyol/Medium**: independent demonstration that PromptGuard 2 is bypassable. **Constitutional Classifiers (arXiv 2501.18837 + ++ at arXiv 2601.04603)**: 3,000+ red-team hours; v2 has 40× compute reduction, 0.05% refusal rate. |
| Verdict | **Corroborated, with caveat.** Vendor self-eval numbers are confirmed in their own papers, but [[agentdojo\|AgentDojo]] is the cleanest *independent* comparator (Meta's own evaluation uses it: PromptGuard 2 takes ASR 17.6%→7.5%, combined with AlignmentCheck 1.75%). Wiki should distinguish "vendor self-reported recall" from "independent benchmark ASR." Bullen-talk-specific 6.7%/1.5% figures don't appear in independent benchmarks; flag as anecdotal. |

### Claim 6: Cost of AI security breaches (\$4.44M global, \$10.22M US)

| Wiki citations today | IBM 2025 Cost of a Data Breach Report |
|---|---|
| Independent corroboration | **[[wef\|WEF]] Global Cybersecurity Outlook 2026**: 87% identify AI vulnerabilities as fastest-growing cyber risk; 94% say AI is dominant change driver; ~33% lack any AI pre-deployment security review. **Verizon DBIR 2025**: 30% third-party involvement (2× YoY); 72% of AI-tool users sign in with personal email. **[[enisa\|ENISA]] Threat Landscape 2025**: 4,875 EU incidents (Jul 2024–Jun 2025). **IBM 2025 (full)**: shadow-AI breaches \$4.63M (\$670K premium); 13% had AI breaches; 97% lacked AI access controls; 63% have no AI governance policy. |
| Verdict | **Well-corroborated.** Add [[wef\|WEF GCO 2026]] and [[enisa\|ENISA Threat Landscape]] explicitly to avoid IBM single-citation on the cost narrative. |

### Claim 7: HITL fatigue / approval rubber-stamping

| Wiki citations today | [[breaking-the-lethal-trifecta-talk\|Bullen talk]]; Changkun blog post |
|---|---|
| Independent corroboration | **[[cset-georgetown\|CSET]] "AI Safety and Automation Bias" (Nov 2024)**: government think-tank issue brief on automation bias as AI safety failure mode. **ACM Computing Surveys, "Alert Fatigue in Security Operations Centres"**: peer-reviewed canonical SOC alert-fatigue citation. **MDPI Computers (2024), "Automation Bias and Complacency in SOCs"**: peer-reviewed empirical work. **Springer AI & Society (2025), "Exploring automation bias in human–AI collaboration"**: systematic review of 35 peer-reviewed studies (2015–April 2025). **EDPS TechDispatch #2/2025**: EU regulator perspective. **Bansal et al.**: finding that *explanations can increase* over-reliance, contradicting the "explainable AI → less rubber-stamping" assumption. |
| Verdict | **Single-sourced becomes well-corroborated.** Cleanest "vendor/blog → academic" upgrade in this audit. Replace Bullen + Changkun with ACM Computing Surveys + MDPI + Springer 2025 systematic review. CSET adds the government-policy framing. |

### Claim 8: AI safety scheming / deception

| Wiki citations today | [[apollo-research\|Apollo Research]]; OpenAI/Apollo joint paper |
|---|---|
| Independent corroboration | **Apollo "Frontier Models are Capable of In-context Scheming" (arXiv 2412.04984)**: o1 maintains deception >85% across follow-ups; tests 5 frontier models. **Anthropic + Redwood "Sleeper Agents" (arXiv 2401.05566)**: backdoor persistence through safety training; foundational. **Anthropic + Redwood "Alignment Faking in LLMs" (arXiv 2412.14093)**: first empirical evidence of unprompted alignment faking. **Apollo "Stress Testing Deliberative Alignment for Anti-Scheming Training"**: newer follow-up the wiki should track. **[[aisi-uk\|UK AISI]] Frontier Trends 2025**: government corroboration. |
| Verdict | **Partially corroborated; venue caveat.** Most papers are arXiv preprints from labs (Apollo, Anthropic, Redwood, OpenAI), with strong methodologies, but no peer-reviewed NeurIPS/ICML/ICLR paper has independently replicated the o1 ">85% deception persistence" figure as of mid-2026. Wiki should explicitly label "lab evaluation, peer-review pending, independent replication outstanding." |

## Summary table

| Claim | Verdict |
|---|---|
| 1. NHI scale (82:1) | Partially corroborated; direction confirmed, exact figure CyberArk-specific |
| 2. AI agent adoption (75%/78%) | Corroborated; replace Knostic with Microsoft WTI 2025 + Stanford HAI |
| 3. 8-month cyber-task doubling | Corroborated; METR is the methodological foundation under UK AISI |
| 4. MCP CVE percentages (82% / 66%) | **Contested**; academic surveys report different denominators |
| 5. Prompt-injection detection rates | Corroborated for vendor self-eval; AgentDojo is the independent comparator |
| 6. AI breach cost (\$4.44M / \$10.22M) | Well-corroborated; add WEF GCO and ENISA |
| 7. HITL fatigue | Single-sourced → well-corroborated; clean academic upgrade |
| 8. AI scheming / deception | Partially corroborated; lab self-reported, peer review pending |

## Top 5 sources to add to the citation rotation

| Source | URL | Why it matters |
|---|---|---|
| **[[stanford-hai\|Stanford HAI AI Index Report 2025]]** | [hai.stanford.edu/ai-index/2025-ai-index-report](https://hai.stanford.edu/ai-index/2025-ai-index-report) | Annual academic-grade synthesis covering adoption, capability benchmarks, agent eval (RE-Bench), investment, policy. Recurs across Claims 2, 3, 8 |
| **Verizon Data Breach Investigations Report 2025** | [verizon.com/business/resources/reports/dbir](https://www.verizon.com/business/resources/Tea/reports/2025-dbir-data-breach-investigations-report.pdf) | Industry-standard incidence data with disclosed methodology. Recurs across Claims 1, 2, 6 |
| **[[metr\|METR]] "Measuring AI Ability to Complete Long Tasks"** | [arxiv.org/abs/2503.14499](https://arxiv.org/abs/2503.14499) + [Time Horizon 1.1](https://metr.org/blog/2026-1-29-time-horizon-1-1/) | Methodological foundation under UK AISI's 8-month cyber-doubling figure. Independent, transparent, ongoing. Recurs across Claims 3, 5 |
| **[[wef\|WEF Global Cybersecurity Outlook 2026]]** | [reports.weforum.org/docs/WEF_Global_Cybersecurity_Outlook_2026.pdf](https://reports.weforum.org/docs/WEF_Global_Cybersecurity_Outlook_2026.pdf) | Senior-leader survey with disclosed methodology; complements IBM cost framing. Recurs across Claims 2, 6 |
| **[[agentdojo\|AgentDojo]]** | [arxiv.org/abs/2406.13352](https://arxiv.org/abs/2406.13352) | Independent peer-reviewed (NeurIPS) prompt-injection benchmark used by Meta. The wiki currently has zero independent benchmarks for Claim 5. Recurs across Claims 4, 5 |

## Sources to flag as vendor-conflicted

| Source | Conflict |
|---|---|
| **CyberArk State of Machine Identity Security** | CyberArk sells machine-identity products; the 82:1 ratio is from a Vanson Bourne survey CyberArk commissioned |
| **SailPoint Horizons / Machine Identity Crisis** | SailPoint sells identity governance; useful for directional corroboration only |
| **[[knostic\|Knostic]] blog (75%/78% adoption)** | Knostic is a data-loss vendor; figures are downstream of Microsoft WTI, so replace with Microsoft directly |
| **Salesforce Connectivity Benchmark** | Salesforce sells Agentforce; 67% multi-agent surge directly markets their product |
| **Promptfoo blog (regression numbers)** | Promptfoo (now [[promptfoo\|part of OpenAI]]) sells eval tooling; ASR figures are model-marketing-adjacent |

## Genuinely single-sourced after this pass

Three claims remain effectively single-sourced or contested:

1. **MCP CVE percentages** (Claim 4): vendor blog origin; academic surveys report different denominators. **Action**: re-derive from peer-reviewed denominators, or downgrade to "vendor-source-derived."
2. **Specific scheming rates** (Claim 8): lab self-reported; no peer-reviewed independent replication. **Action**: keep claims, label "peer review pending."
3. **Bullen-talk-specific ASR figures** (Claim 5): 6.7% Llama 3.3 70B / 1.5% Claude 3.7 Sonnet don't appear in any independent benchmark. **Action**: demote to anecdotal observation or re-derive on a public benchmark.

## Effect on the wiki

This audit does NOT change RA or CMM level criteria. It changes the **defensibility of citations** that support those criteria. Specifically:

- **D1 L4/L5 evidence** (cost/risk metrics) gains [[wef\|WEF GCO 2026]] and [[enisa\|ENISA]] alongside IBM
- **D2 L3+ evidence** (NHI scale and lifecycle) gains Verizon DBIR and SailPoint Horizons alongside CyberArk
- **D7 L4 evidence** (red-team eval) gains [[agentdojo\|AgentDojo]] as the independent third-party benchmark to anchor against vendor self-eval
- **D9 L4 evidence** (HITL fatigue / human factors) shifts from talk and blog to the ACM CS / MDPI / Springer 2025 academic foundation
- **[[agentic-ai-threat-classes-2026\|Threat Classes 2026]] Class 2 (APT campaigns)** gains [[metr\|METR]] as the methodological foundation under UK AISI
- **[[shadow-ai\|Shadow AI]] / [[non-human-identity\|NHI]]** pages should swap the [[knostic\|Knostic]] adoption-figure citation for Microsoft Work Trend Index 2025 directly

## See Also

- [[peer-review-readiness-2026-05-02|Peer-Review Readiness — Gaps in the RA + CMM]]: origin of this audit (honorable-mention #1)
- [[agentic-cmm-vs-standards-validation|Validation: Agentic AI CMM vs Widely Adopted Standards]]: sister audit, focused on standards rather than evidence sources
- [[stanford-hai|Stanford HAI]] · [[metr|METR]] · [[wef|World Economic Forum]] · [[enisa|ENISA]] · [[agentdojo|AgentDojo]]: new sources added by this audit
