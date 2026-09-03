---
type: concept
title: "VulnOps: Vulnerability Operations"
address: c-000069
created: 2026-05-15
updated: 2026-09-01
tags:
  - concepts
  - vulnops
  - vuln-management
  - operating-model
  - mythos-era
status: developing
scope_axis: [ai-in-sec-defense, sec-against-ai]
no_public_url: "Emerging operating-model term from two independent framings; no single canonical external source."
related:
  - "[[agentic-vulnerability-discovery]]"
  - "[[mythos-ready-security-program|Mythos-ready Security Program]]"
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - "[[vulnops-l1-soc-extinction|CYBR.SEC.Media VulnOps article]]"
  - "[[vulnerability-operations-center|Vulnerability Operations Center (VOC)]]"
  - "[[vulnops-implementation-roadmap|VulnOps Implementation Roadmap]]"
  - "[[verizon-dbir-2026|Verizon DBIR 2026]]"
  - "[[first-vulnerability-forecast-2026|FIRST Vulnerability Forecast 2026]]"
  - "[[jfrog-ssc-state-of-union-2026|JFrog 2026 SSC State of the Union]]"
  - "[[gadi-evron|Gadi Evron]]"
  - "[[heather-adkins|Heather Adkins]]"
  - "[[jonathan-cran|Jonathan Cran]]"
  - "[[mallory|Mallory]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[anthropic-glasswing-initial-update|Glasswing initial update]]"
  - "[[agentic-soc-state-of-the-field|Agentic SOC — State of the Field]]"
  - "[[agentic-soc-ra-exposure-vulnops|Agentic SOC Exposure and VulnOps Surface]]"
  - "[[openant|OpenAnt]]"
  - "[[codex-security|Codex Security]]"
  - "[[claude-code-security|Claude Code Security]]"
  - "[[mdash|MDASH]]"
  - "[[big-sleep|Big Sleep]]"
  - "[[codemender|CodeMender]]"
  - "[[moak|MOAK]]"
  - "[[autonomous-exploit-generation|Autonomous Exploit Generation]]"
  - "[[google-cloud-codemender-preview]]"
  - "[[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]"
  - "[[vulnerability-research-agentic-age-keynote]]"
  - "[[vulnerability-properties]]"
  - "[[arizona-state-university]]"
  - "[[analyzer-ordering-confound]]"
  - "[[autonomous-code-security-google-talk|Autonomous Code Security at Google]]"
  - "[[four-flynn|Four Flynn]]"
  - "[[oss-ai-vuln-discovery-harness-landscape|OSS AI Vuln-Discovery Harness Landscape]]"
  - "[[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]]"
  - "[[semgrep|Semgrep]]"
  - "[[vvah|VVAH]]"
  - "[[defending-code-harness|defending-code-harness]]"
sources:
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - "[[vulnops-l1-soc-extinction|CYBR.SEC.Media VulnOps article]]"
  - ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
---

# VulnOps — Vulnerability Operations

**VulnOps** is an emerging operational model that fuses previously-separate security functions into a single agent-augmented discipline. The term arrives from two independent directions in the same six-month window.

### Framing 1 — Discovery and remediation as a DevOps-shaped function

VulnOps is a permanent function staffed and automated like DevOps. It owns continuous discovery of zero-day vulnerabilities across the entire software estate and the automated remediation pipelines that act on them. [[heather-adkins|Heather Adkins]] (VP of Security Engineering, Google), [[gadi-evron|Gadi Evron]] (CEO, Knostic), and Bruce Schneier (Inrupt; Harvard Kennedy School) introduced the concept jointly in October 2025. It rests on a September 2025 warning by Adkins and Evron that autonomous vulnerability discovery and exploitation were roughly six months away. The [[mythos-ready-briefing|Mythos-ready briefing]] files it as long-term Priority Action #11.

### Framing 2 — Threat-intelligence and vulnerability-management fusion

VulnOps fuses cyber threat intelligence with vulnerability management. Customers of [[mallory|Mallory]] (founder [[jonathan-cran|Jonathan Cran]]) describe the goal as un-siloing the information so an agent can operationalize it: *"un-silo the information so that it can be brought into the context window for the agent to be able to operationalize it."* The function maps global intelligence automatically onto organization-specific assets, cloud environments, code, and infrastructure-as-code. The [[vulnops-l1-soc-extinction|CYBR.SEC.Media May 2026 article]] records this framing.

Both framings center the same structural observation: previously-separate functions need to be operationalized as one because modern attacks do not respect the boundaries. The two are compatible and complementary. The Mythos-ready briefing's DevOps-shaped discovery-and-remediation function requires the CTI-to-environment-context fusion Mallory's customers describe; the Mallory framing's threat-intel-meets-vulnerability-management discipline requires the discovery and remediation pipelines the Mythos-ready briefing names. The wiki treats both as load-bearing sourced framings rather than competing claims.

## Rationale

Quarterly pen tests and reactive patching cycles cannot keep pace with continuous AI-driven discovery. Existing CVE/NVD infrastructure and patch-prioritization workflows were built for a caseload of dozens of critical CVEs per month. The [[zero-day-clock|Zero Day Clock]] documents the structural problem: the median time-to-exploit collapsed from 771 days in 2018 to zero-day by 2025, where the exploit now arrives on or before disclosure for the median exploited vulnerability.[^zdc] Vulnerability management as a periodic activity is structurally outmatched.

Two independent, non-vendor sources bound the problem from outside the term's own coinage. The [[verizon-dbir-2026|Verizon DBIR 2026]] reports that vulnerability exploitation (31%) overtook stolen credentials (13%) as the leading initial-access vector across more than 22,000 confirmed breaches, even as remediation slowed: the share of CISA KEV criticals fully patched fell from 38% to 26%.[^dbir] Those figures come from breach telemetry, not benchmark capability. The [[first-vulnerability-forecast-2026|FIRST Vulnerability Forecast]] projects CVE publication crossing 50,000 for the first time in 2026, with a median near 59,000 and a 90% confidence-interval ceiling past 117,000.[^first] Volume exceeds human triage capacity and arrives faster than human-paced patching. VulnOps is organized to absorb both pressures.

The volume is also mostly noise. JFrog's 2026 analysis found that **66% of analyzed CVEs had a low applicability rate (0–20%)**, meaning the conditions to exploit them are rarely met in practice, and only **12% were highly exploitable** in real environments.[^jfrog-ssc] The scarce resource is therefore not patching capacity but the exploitability triage that separates the reachable flaws from the rest. VulnOps treats that triage as a first-class discipline.

Triage scope is the second half of that discipline. [[autonomous-exploit-generation|Autonomous Exploit Generation]] records agents reaching code execution through a code path adjacent to the defect they were handed, so a remediation scoped to the reported line leaves the path the agent used. The queue inherits that difference, and a closed ticket bounds the fix rather than the finding.

**Primary-source confirmation of the bottleneck inversion (2026-05-22).** [[anthropic-glasswing-initial-update|Anthropic's one-month Glasswing update]] states the VulnOps premise as a direct finding from roughly 50 coalition partners: *"Progress on software security used to be limited by how quickly we could find new vulnerabilities. Now it's limited by how quickly we can verify, disclose, and patch."*[^glasswing] The open-source funnel makes it concrete: 6,202 estimated high/critical found, only 1,752 assessed and 75 patched, at a roughly two-week mean patch time, and maintainers asked Anthropic to slow down disclosures.[^glasswing] The update provides the strongest primary-source evidence to date that the scarce resource has shifted from discovery to triage-and-remediation, the premise VulnOps exists to address.

**The bottleneck moves rather than clears.** The August 2026 [[openai-hugging-face-agent-incident|OpenAI–Hugging Face disclosure]] states the same finding as a design constraint on how much of the loop is automated: automating discovery alone relocates the bottleneck to patching and drowns human engineers in findings, so the loop has to close through identification, proposed patch, rollout, and rollback on an availability regression.[^bh] The rollback leg is the part the current framings omit. Applying a fix without a tested automated reversal converts a bad patch into an outage, and the operator's response is to re-insert the human approval the automation was built to remove — which restores the original bottleneck one stage later. The same disclosure motivates continuous agentic red teaming as the input side of the function: the estate will be examined by model intelligence regardless, and the operational question is whether the organization spends enough of it on its own infrastructure before a threat actor does.[^bh] JFrog's applicability figures and the Glasswing funnel bound the triage problem; this source bounds the remediation problem the triage feeds.

The vendor whose autonomous-patching output this wiki has sourced in most detail names the leg before rollback as unsolved. At [un]prompted in March 2026, [[four-flynn|Four Flynn]] listed redeploying automatically mended code at scale as one of three open conundrums and stated plainly that he has no approach: "one of the hardest problems with patching is actually places in the world that struggle to actually apply patches in a timely manner… I don't know how to solve that with AI."[^google-talk] The same talk measures where autonomous fixing currently lands. Of 178 fixes landed in open source, 130 are proactive hardening and 48 are patches, so most of the delivered volume is class elimination rather than repair of a reported bug. Both facts bound property 5 rather than satisfying it: a pipeline can generate a verified fix and still have no mechanism to land it on the estates that need it.

[[moak|MOAK]] provides the mechanistic explanation: a five-agent autonomous pipeline converts a bare CVE number into a validated working exploit at a 97.8% success rate across 178 CISA KEVs, including CVEs disclosed after the models' training cutoffs, with Claude Opus 4.6 reaching a 98% autonomous exploitation rate on the post-cutoff benchmark.[^moak] The human analyst bottleneck, the traditional reason exploits took days to develop post-disclosure, is removed. MOAK runs live against newly-disclosed CVEs and offers a dashboard for practitioners to assess specific CVEs within hours of disclosure.

## Five operating properties

1. **Staffed and automated like DevOps.** A permanent function, not a campaign or a project, designed around continuous flow rather than periodic snapshots. Treat it as an operating capability with on-call structure, runbooks, and dashboards rather than as a quarterly audit cycle. (Mythos-ready framing.)
2. **Owns the full software estate.** Coverage spans own code, AI-generated code, third-party libraries, container images, MCP servers, IDE extensions, agent skills, and rules-files. The function does not stop at *"app the security team owns"*; it follows the dependency chain. (Mythos-ready framing.)
3. **Designed around triage discipline from the start.** With AI-discovery rates exceeding human-paced response, triage is the load-bearing operational discipline. *"Existing CVE/NVD infrastructure and patch-prioritization workflows were built for dozens of critical CVEs per month, not hundreds."* VulnOps treats triage as the primary scarce resource and designs for it explicitly: severity scoring, confidence scoring, deduplication, and prioritization queues are first-class. (Mythos-ready framing.)
4. **Un-silos threat-intel against organization-specific context.** Continuous ingestion of external intelligence (CTI feeds, ISAC data, vendor advisories, GitHub disclosures, government feeds) is **automatically mapped** to the organization's own assets, cloud environments, code repositories, and infrastructure-as-code configurations. Action-on-finding is policy-driven via configurable skill files. *"Threads"* replace conventional case-management *"cases"*; every investigation is a collaborative analyst-agent thread. (Mallory framing per [[vulnops-l1-soc-extinction|CYBR.SEC.Media May 2026]].)
5. **Closes the loop through rollout and rollback.** The automated path runs identify, propose patch, roll out, and **roll back** on an availability regression, and the reversal is engineered as part of the pipeline rather than left as a manual escape hatch. Rollback capability lets remediation autonomy rise: it bounds the cost of a wrong fix, the term [[agentic-soc-ra-exposure-vulnops|the exposure and VulnOps function]] gates remediation autonomy on. (OpenAI–Hugging Face framing.[^bh]) The open-source field reaches this property unevenly: of the five open-source pipelines Semgrep tabulates, three generate a patch — one verified by execution, one by an LLM check — and Semgrep records patch generation as still less common than discovery; none of the five is described as rolling a fix out or back.[^semgrep] VVAH names the interval the function exists to compress, optimizing "Mean Time to Adapt" from discovery to validated fix, per Semgrep's LLM-generated summary.

## Relationship to Existing Wiki Concepts

- **Complementary to [[agentic-ai-security-cmm-2026|the CMM]] but cross-cutting.** VulnOps is not measured by the CMM's nine domains. It is a *function* organizations stand up, akin to a SOC or a Red Team. The most-relevant CMM domains for VulnOps maturity are **D8 (Supply Chain & AI-BOM)** for the third-party / OSS / AI-BOM scope and **D9 (Operations & Human Factors)** for the burnout / triage-fatigue / decommission-lifecycle properties. A formal CMM cross-walk for VulnOps is a candidate revision-pass addition.
- **Operationalized via existing tools.** [[openant|OpenAnt]] (OSS), [[codex-security|Codex Security]] (OpenAI), [[claude-code-security|Claude Code Security]] (Anthropic) are the canonical commercial / open-source instruments for the *discovery* side. The open-source population is larger than those two entries and reaches past discovery. Semgrep catalogues nine open-source harnesses — eight with a company owner and one, RAPTOR, a community project — and several cover legs the properties below name: VVAH proposes a minimal fix and validates it without executing code, raptor and defending-code-harness generate patches, and Capital One's vulnhunter composes find, fix and verify into a remediation loop.[^semgrep] [[oss-ai-vuln-discovery-harness-landscape|The open-source harness landscape]] carries the per-project comparison. [[mdash|MDASH]] and [[big-sleep|Big Sleep]] are the vendor-internal counterparts; [[codemender|CodeMender]] left that category in July 2026, when Google Cloud placed it in [[google-cloud-codemender-preview|managed preview]] as a scan-verify-remediate agent that enterprises can point at their own repositories: the find-to-fix loop sold as one procurement rather than assembled from parts. The Mythos-ready Priority Action 1 (*Point Agents at Your Code and Pipelines*) is the *Monday-morning* entry to a VulnOps capability; PA 11 (*Stand Up VulnOps*) is the 6-to-12-month durable answer. [[vulnops-implementation-roadmap|The VulnOps implementation roadmap]] sequences the enterprise build-out (crawl/walk/run) on Gartner's [[continuous-threat-exposure-management|CTEM]] spine and the [[vulnerability-operations-center|Vulnerability Operations Center]] operating model. The [[claude-partners-opus-cybersecurity|May 2026 Opus partner roundup]] is VulnOps productized through technology and services partners. Its three jobs (offensive testing at scale, closing the find-to-fix gap, governed production) restate the function, with the find-to-fix gap as the explicit target across [[wiz|Wiz]], [[palo-alto-networks|Palo Alto]], [[accenture|Accenture]], [[trend-micro|Trend Micro]], [[deloitte|Deloitte]], and [[pwc|PwC]].
- **Adjacent to [[secure-sdlc-framework-stack-2026|the 2026 Secure-SDLC Framework Stack thesis]].** VulnOps is the candidate **Layer 8** function complementing the parked Layer 4½ harness-config audit and Layer 7 AI-driven vuln-discovery layers. The Mythos-ready briefing argues VulnOps is *the* long-term answer; the framework-stack thesis treats it as a candidate layer parked pending broader peer adoption.
- **Operates against the [[citizen-coders|Citizen Coders]] sprawl.** Proliferation of coding agents to non-developers fragments central IT visibility; VulnOps is the organizational structure that owns the full code-and-dependency landscape regardless of who shipped which artifact.

**Disclosure may itself be the harm.** Every framing above treats disclosure as a stage to be accelerated: find faster, triage faster, patch faster. A May 2026 study from [[arizona-state-university|Arizona State University]]'s lab, on embedded devices and with no agents in the loop, reproduced disclosed vulnerabilities against other devices held in the lab and found that disclosing one vulnerability endangers roughly three times as many devices as it secures.[^asu-keynote] The keynote gives no mechanism for that ratio. One reading, consistent with the rest of the source: a disclosed flaw carries [[vulnerability-properties|properties]] that transfer to other targets, which is the same transfer VulnOps relies on, and the population able to apply them to unpatched devices is larger and faster than the population that ships fixes.

The same lab reports finding at roughly ten times its reportable rate, which is the funnel the Glasswing figures describe, reproduced without a vendor's interest in the result.[^asu-keynote] Its stopgap is a VulnOps-shaped function with no vendor behind it: a public site carrying over a thousand local privilege escalations, publishing CVE details for the disclosed ones and hashes for the remainder, with candidate agent-generated patches under discussion as a proactive measure ahead of vendor fixes. Shoshitaishvili states the lab does not have a better solution.

This bounds what VulnOps can claim, and [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] carries the same qualification against its own disclosure-timeline argument. As a function VulnOps is well-founded on the throughput argument — the volume is real and no human team absorbs it. Whether the pipeline it feeds should run faster is a separate question the throughput argument does not answer, and the one primary source that measures disclosure's net effect answers it negatively for the one device class it studied.

## Adjacent / Open Questions

- **Reporting structure.** Does VulnOps report into Security Engineering, into the CISO directly, or into a new function alongside DevOps? The briefing does not commit. SANS / CSA practitioner experience over the next year will likely produce a default.
- **Tooling consolidation vs best-of-breed.** Multiple vendor-tier instruments exist ([[codex-security|Codex Security]], [[claude-code-security|Claude Code Security]]) plus OSS ([[openant|OpenAnt]], [[raptor|RAPTOR]]); how organizations compose them into a coherent VulnOps stack is still open. A third answer now has a source: Semgrep's July 2026 survey of nine open-source harnesses concludes that no reference open-source harness will emerge today and predicts that many companies will build their own "shop jigs" for vulnerability finding, an analogy Semgrep credits to tptacek on Hacker News; the field's pace is named as the reason several of the nine ship marked "no external contributions accepted" or unmaintained.[^semgrep] Under that reading the composition question resolves to in-house assembly from a shared design pattern rather than to a procurement choice between stacks. What stays open is which parts an organization builds and which it buys, and how a self-assembled harness carries evidence to the rest of the function. The [[adversarial-reflexion|Adversarial Reflexion]] discipline is sourced across all of them, which suggests interoperability is more about *evidence-handoff* than *single-vendor lock-in*.
- **Regulatory implications.** The **[[eu-ai-act|EU AI Act]] (August 2026)** introduces automated audit, incident reporting, and cybersecurity requirements around AI. The standard of care for *"used available AI defensive tools"* will shift; VulnOps capability becomes a candidate due-diligence artifact at the board level.
- **Burnout and team resilience.** Mythos-ready briefing names this directly: VulnOps is built to absorb a volume of work no human team alone can absorb, but the function itself can burn out, so request additional headcount and budget reserve capacity as a design parameter rather than an after-the-fact correction.

## See Also

- [[agentic-vulnerability-discovery|Agentic Vulnerability Discovery]] — the discovery stage whose validated output this function receives and triages.
- [[mythos-ready-briefing|Mythos-ready paper]] — source and the canonical PA 11 (*Stand Up VulnOps*) description.
- [[mythos-ready-security-program|Mythos-ready Security Program (Playbook)]] — operational instrument.
- [[zero-day-clock|Zero Day Clock]] — quantitative anchor for the speed-of-exploitation problem VulnOps responds to.
- [[gadi-evron|Gadi Evron]] · [[heather-adkins|Heather Adkins]] — co-introducers (Sep–Oct 2025).

## Notes

[^zdc]: [The Collapse — Zero Day Clock](https://zerodayclock.com/collapse), Sysdig and collaborators, 2026. Median time-to-exploit across CVE-exploit pairs (CISA KEV, VulnCheck KEV, XDB): 771 days (2018), 84 days (2021), 6.36 days (2023), 4 hours (2024), zero-day (2025–2026).
[^dbir]: [Verizon Business — 2026 Data Breach Investigations Report](https://www.verizon.com/business/resources/reports/2026-dbir-data-breach-investigations-report.pdf), 2026. Vulnerability exploitation 31% vs stolen credentials 13% as initial-access vector (Figure 10, n=20,023) across more than 22,000 confirmed breaches; share of CISA KEV criticals fully remediated fell from 38% (2024) to 26% (2025).
[^first]: [FIRST — Vulnerability Forecast for 2026](https://www.first.org/blog/20260211-vulnerability-forecast-2026), 2026. Published-CVE forecast: crosses 50,000 for the first time; median 59,427; 90% confidence interval 30,012–117,673.
[^glasswing]: [Anthropic — Project Glasswing: An initial update](https://www.anthropic.com/research/glasswing-initial-update), 2026. Open-source scanning funnel: 6,202 estimated high/critical found, 1,752 assessed, 75 patched, ~2-week mean patch time; bottleneck-inversion quote and the maintainer slow-down request.
[^moak]: [MOAK — How Does MOAK Work?](https://moak.ai/#moak), 2026. Five-agent autonomous pipeline; 174 of 178 CISA KEVs exploited (97.8%); Claude Opus 4.6 reaches 98% autonomous exploitation on the post-knowledge-cutoff KEV benchmark.
[^bh]: Michael Dalton and Eric Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026, 2026-08-06. Defender recommendations: fully automated identify → propose patch → roll out → roll back loop; continuous agentic red teaming as a standing function. Summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]].
[^jfrog-ssc]: [JFrog — 2026 Software Supply Chain Security State of the Union (announcement)](https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs), 2026, report p.6. 66% of analyzed CVEs had a low applicability rate (0–20%); only 12% were highly exploitable in real enterprise environments. See [[jfrog-ssc-state-of-union-2026|JFrog 2026 SSC State of the Union]].
[^semgrep]: [Semgrep — Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses), July 2026 (no day-level date exposed; author not named). The nine-project inventory, the market-structure argument and the execution/PoC/patch table are human-written; "Mean Time to Adapt" is from Semgrep's LLM-generated repository summary. Summarized at [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].

[^asu-keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06). See [[vulnerability-research-agentic-age-keynote|the talk summary]].
[^google-talk]: Heather Adkins and Four Flynn, *Evaluating Threats & Automating Defense: How Google is Advancing Code Security*, [\[un\]prompted, San Francisco](https://www.youtube.com/watch?v=B_7RpP90rUk) (2026-03-03): Big Sleep at zero false positives end-to-end on deep memory-safety bugs, with a working exploit built as proof of vulnerability; CodeMender at 178 open-source fixes, 48 patched and 130 hardening; verification presented as the gate, and full autonomy stated as the design intent. See [[autonomous-code-security-google-talk|the talk summary]].
