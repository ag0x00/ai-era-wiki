---
type: comparison
title: "OSS AI Vuln-Discovery Harness Landscape"
address: c-000341
created: 2026-08-31
updated: 2026-09-01
tags:
  - comparisons
  - ai-vuln-discovery
  - open-source
  - harnesses
  - vulnops
  - ai-in-sec-defense
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
  - sec-of-ai
subjects:
  - "[[defending-code-harness]]"
  - "[[vvah]]"
  - "[[raptor]]"
  - "[[deepsec]]"
  - "[[ai-deep-sast]]"
  - "[[security-audit-skill]]"
  - "[[trail-of-bits-skills]]"
  - "[[mdash]]"
  - "[[codemender]]"
  - "[[big-sleep]]"
  - "[[mythos]]"
  - "[[claude-code-security]]"
  - "[[codex-security]]"
  - "[[openant]]"
dimensions:
  - "acquisition"
  - "deployment-shape"
  - "definition-of-a-finding"
  - "code-execution"
  - "patch-generation"
  - "isolation-posture"
  - "model-dependency"
  - "licence-and-maintenance"
  - "published-evidence"
verdict: "An open-source licence removes the procurement gate on the harness and leaves the provider gate on the capability; the open field supplies installability, model choice and a readable method, and supplies no published evidence, no maintenance commitment and no composition with the estate."
related:
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[semgrep]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[defending-code-harness]]"
  - "[[vvah]]"
  - "[[deepsec]]"
  - "[[ai-deep-sast]]"
  - "[[security-audit-skill]]"
  - "[[trail-of-bits-skills]]"
  - "[[raptor]]"
  - "[[mdash]]"
  - "[[codemender]]"
  - "[[big-sleep]]"
  - "[[mythos]]"
  - "[[claude-code-security]]"
  - "[[codex-security]]"
  - "[[openant]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[ai-vuln-discovery-benchmark-landscape]]"
  - "[[agent-sandbox-isolation-landscape]]"
  - "[[vulnops]]"
  - "[[harness-config-as-supply-chain-artifact]]"
  - "[[autonomous-exploit-generation]]"
  - "[[adversarial-reflexion]]"
sources:
  - "[[semgrep-oss-ai-security-harness-comparison|Comparing Open Source AI Code Security Harnesses]]"
  - ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
  - "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
---

# OSS AI Vuln-Discovery Harness Landscape

Every commercial AI vulnerability-discovery programme compared below reaches an operator through a vendor gate. [[mdash|MDASH]] runs in limited private preview, [[claude-code-security|Claude Code Security]] and [[codex-security|Codex Security]] as research previews bound to an enterprise subscription, and [[codemender|CodeMender]] as a managed Google Cloud preview since July 2026. [[big-sleep|Big Sleep]] stays inside Google, and [[mythos|Mythos]] has no general availability planned. Semgrep's July 2026 survey sets nine open-source harnesses against that picture, and a team can install any of the nine today under Apache 2.0, MIT or CC-BY-SA.[^semgrep] This page compares the two populations and states what the open path supplies an operator and what it withholds.

## The nine harnesses

Semgrep sorts the field into three categories. **LLM-led exploitgen** drives a target to a crashing end-state and reads the crash as the vulnerability oracle, which Semgrep frames as a new form of fuzzing. **LLM-skill-boosting vulnerability research** installs research method inside a model so it reasons about code the way a human vulnerability researcher would. **SAST+LLM hybrids** feed deterministic program analysis into a model to narrow the search space.[^semgrep] Semgrep sells into this market, so the three categories carry its framing and serve here as a sorting key.

| Project | Owner | Semgrep's category | Deployment shape | Licence | Stars |
|---|---|---|---|---|---|
| [[defending-code-harness\|defending-code-harness]] | [[anthropic\|Anthropic]] (original), [[semgrep\|Semgrep]] (fork) | Exploitgen | Standalone pipeline | Apache | 6K |
| [[security-audit-skill\|security-audit-skill]] | [[cloudflare\|Cloudflare]] | Skill-boosting | Agent-native skill | MIT | 2K |
| [[trail-of-bits-skills\|trailofbits/skills]] | [[trail-of-bits\|Trail of Bits]] | Skill-boosting | Agent-native skills | CC-BY-SA | 6K |
| `capitalone/vulnhunter` | Capital One | Skill-boosting | Agent-native skills | Apache | 500 |
| `google/mantis` | [[google\|Google]] | Skill-boosting | Agent-native skills | Apache | 400 |
| [[raptor\|RAPTOR]] | Community (gadievron) | SAST+LLM, overlapping exploitgen | Standalone pipeline | MIT | 3K |
| [[deepsec\|deepsec]] | [[vercel\|Vercel Labs]] | SAST+LLM | Standalone pipeline | Apache in the category list; the capability matrix says confirm in-repo | 5K |
| [[ai-deep-sast\|ai-deep-sast]] | [[cisco\|Cisco]] | SAST+LLM | Standalone pipeline | Apache | 50 |
| [[vvah\|VVAH]] | [[visa\|Visa]] | SAST+LLM | Standalone pipeline | Apache | 600 |

Star counts are as Semgrep stated them at publication and move afterwards. Eight of the nine carry a company owner rather than an individual maintainer, and only RAPTOR comes from a community project. Corporate engineering released under an open-source licence now fills a category that previously held maintainer-side tooling such as [[openant|OpenAnt]], which Semgrep does not survey.

Two artifacts inside the survey carry different weight, and the distinction governs most of the cells below. Semgrep wrote the categorisation, the star counts, the finding-definition and execution tables and the market-structure argument. A separate capability matrix, covering seven of the nine and leaving `vulnhunter` and `mantis` without a row, is labelled by Semgrep as an LLM-generated reading of the repositories, as are the per-tool detail sections.[^semgrep] Isolation postures, stage counts, model defaults and per-tool behaviour all come from that machine reading.

### Deployment shape

Semgrep separates standalone pipelines from agent-native skills, and the split governs most of what an operator has to arrange. A pipeline installs as a program or a CI job, issues its own model calls, runs unattended in CI, supplies its own sandbox, and costs per-run infrastructure plus tokens. A skill installs as a prompt pack inside Claude Code or Codex, runs on the host agent's model, needs the agent to drive it, and inherits the host agent's isolation and its cost.[^semgrep] Four of the nine ship as skills.

No commercial programme below ships in the skill shape; each arrives as a product with its own surface, whether a dashboard, a CI integration, or a managed cloud service. A skill adds an audit methodology to an agent an engineering team already runs, so acquisition is an installation rather than a procurement, and every security property of the run belongs to the host harness. [[harness-config-as-supply-chain-artifact|Harness configuration as a supply-chain artifact]] carries what that inheritance costs.

## Dimensions against the commercial set

| Dimension | Semgrep's nine | Commercial and vendor-internal programmes |
|---|---|---|
| Acquisition | Public repository under Apache 2.0, MIT or CC-BY-SA | Preview application, enterprise subscription, or no offer at all |
| Deployment shape | Five standalone pipelines, four agent-native skills | Hosted product, managed cloud service, or vendor-internal programme |
| Definition of a finding | Varies by harness: a triaged static match, a verified pipeline candidate, a reproducible AddressSanitizer crash, a re-validated static match | Each product names a validation stage and reports findings that clear it |
| Code execution in validation | Two of the five pipelines compile and run binaries | Disclosed for MDASH, Big Sleep, Codex Security and CodeMender; undisclosed for Claude Code Security |
| Patch generation | Three of the five pipelines | CodeMender, Codex Security and Claude Code Security generate patches under human approval |
| Isolation posture | Assembled per harness: gVisor plus egress allowlist; Landlock, seccomp and namespaces; bubblewrap, Seatbelt or microVM; a tool sandbox without Bash; none for the static scanner | Vendor-operated, or customer-managed for CodeMender; the primitive is undisclosed for Codex Security |
| Model dependency | Swappable on four of the five pipelines; one runs a security-tuned 8B model locally | Bound to the seller's model, with CodeMender offering a choice among Google models |
| Maintenance | No support commitment; several closed to contributions or unmaintained | Vendor support inside the preview's terms |
| Published evidence | No benchmark score, recall figure or finding count for any of the nine | Recall or false-positive figures for MDASH, Codex Security and Big Sleep; finding and patch volumes for CodeMender and Claude Code Security |

Every isolation value, model default and stage count in the open column comes from the LLM-generated matrix and detail sections.[^semgrep] The remaining open-column cells come from the survey's human-written body. The commercial column restates what each product's own page records from its vendor's announcement. [[agentic-ai-security-cmm-d3-control-least-agency|D3 Control & Least-Agency]] grades the tool-allowlist narrowing behind two of those rows: VVAH's no-Bash tool sandbox and deepsec's read-only agent tools.

## Operator consequences

### Access without a vendor relationship

An open-source licence settles who may install the harness, and the model's provider still decides who may drive it. Semgrep reports the exploitgen category as the hardest of the three to use in practice, because model guardrails stop an LLM generating exploits and an operator needs trusted-access or cyber-verification standing with the provider.[^semgrep] [[defending-code-harness|defending-code-harness]] accepts a find only when the crash reproduces under AddressSanitizer three times out of three, and verifies its patch by re-attack. That harness runs under the same provider decision that gates [[mythos|Mythos]] and the commercial previews, so its Apache licence removes the procurement gate and leaves the capability gate standing. [[autonomous-exploit-generation|Autonomous exploit generation]] carries the constraint as a scope limit on the class.

The other two categories cross no such gate. A hybrid or a skill pack asks a model to read code and reason about it, and Semgrep records no guardrail refusing that request. Installing one therefore obtains discovery and triage, and leaves execution-verified proof behind the provider's decision.

### Model choice and data residency

Four of the five pipelines accept a model of the operator's choosing: [[vvah|VVAH]] runs vendor-neutral backends behind a Sonnet and Opus default, [[deepsec|deepsec]] defaults to Codex GPT-5.5 with Claude and Pi alternatives, [[raptor|RAPTOR]] runs multi-model consensus across Claude and GPT, and [[ai-deep-sast|ai-deep-sast]] runs either a frontier model or the security-tuned Foundation-Sec-8B on the operator's own machine.[^semgrep] The commercial programmes bind the reasoner to the seller. [[claude-code-security|Claude Code Security]] runs Claude Opus 4.6, [[codex-security|Codex Security]] runs inside Codex, and [[codemender|CodeMender]] lets a customer select among Google models, with third-party frontier models planned for later in 2026.

No commercial programme compared here is documented as running its reasoner on the operator's hardware. CodeMender's enterprise terms reach VPC traffic routing, zero retention of source code and customer-operated sandboxes, and the model still runs at Google. Microsoft has not stated whether MDASH deploys on an operator's own infrastructure. ai-deep-sast's fully local mode answers a data-residency requirement that the commercial column leaves open, and Semgrep records the trade it makes as depth and proof for breadth and speed.

### Method visible end to end

A pipeline published under an open licence discloses its stages as code. VVAH's eleven stages, RAPTOR's staged exploitability filter and defending-code-harness's T0–T3 patch ladder are all readable in the repository.[^semgrep] The commercial products name their stages and ship none of them: [[codex-security|Codex Security]] states that validation attempts the exploit in an isolated sandbox and leaves the sandbox primitive undisclosed, and [[mdash|MDASH]] names five stages without saying which models occupy which role. An assessor scoring a control against evidence reads the open harness, and takes the commercial one on its vendor's description.

The same visibility reaches the false-positive control. Semgrep names four open-source implementations of adversarial validation, in which a second independent agent tries to falsify each finding, and reports the technique working best when a different model attempts the disproof.[^semgrep] [[adversarial-reflexion|Adversarial Reflexion]] holds the mechanism and its vendor-side instances.

### Evidence the open field does not publish

Measurement runs the other way. Semgrep compares seven of the nine on vulnerability scope, language coverage, output format, isolation, default model and licence, and publishes no recall figure, no false-positive rate and no finding count for any of the nine.[^semgrep] The commercial programmes publish numbers: [[mdash|MDASH]] leads the [[cybergym|CyberGym]] public leaderboard and reports Microsoft-internal historical-recall figures, [[codex-security|Codex Security]] reports a recall figure against private golden repositories, and [[big-sleep|Big Sleep]] reports a false-positive rate of zero scoped to deep memory-safety bugs. Only the leaderboard entry is independently checkable, so the asymmetry runs between vendor-reported numbers and no numbers at all.

Publication is a practice, and the licence does not determine it. [[openant|OpenAnt]] publishes per-project filter ratios and per-stage token cost across five open-source codebases under the same open terms. Semgrep also supplies the structural reason a count would not settle the comparison: the definition of a finding varies across the harnesses, so two totals describe different objects. [[ai-vuln-discovery-benchmark-landscape|The benchmark landscape]] carries the shared-oracle problem.

### Maintenance and the fork disposition

Semgrep predicts that no reference open-source harness will emerge today, and that many companies will instead build their own "shop jigs" for vulnerability finding, an analogy Semgrep credits to tptacek on Hacker News. Semgrep leaves an open-source leader available later and rules it out only for now, because the field moves faster than a shared artifact can settle.[^semgrep] Semgrep names the field's pace as the reason several of the nine ship marked "no external contributions accepted" or unmaintained. Anthropic's original reference harness is reported unmaintained, and Semgrep maintains a fork under its own name. An operator adopting from this field acquires a design pattern and a maintenance obligation; a preview customer acquires a support relationship bounded by the preview's terms.

### Estate composition

[[codemender|CodeMender]]'s AI Threat Defense path has [[wiz|Wiz]] orchestrate the run: Wiz calls CodeMender to scan, enriches the findings with deployment context from its Security Graph, triggers an offensive agent, and then directs CodeMender to generate and test context-enriched patches. Nothing in the surveyed field composes with an asset graph or a runtime signal. The nine take a repository and return findings, so an organization whose triage question is which finding is reachable in production supplies the estate context itself. [[vulnops|VulnOps]] holds that function.

## Verdict

An open-source licence removes the procurement gate on the harness and leaves the provider gate on the capability. For discovery and triage the open field now supplies what the commercial previews supply, with model choice, a readable method and one fully local option on top. For execution-verified proof it does not, because that category depends on a model whose provider decides who may drive it. What the open path gives up in exchange is published evidence, a maintenance commitment, and composition with the rest of the estate.

## Caveats

- **The comparison sets are not matched.** The open column comes from one survey by one vendor. The commercial column is assembled from six programmes — MDASH, CodeMender, Big Sleep, Mythos, Claude Code Security and Codex Security — each written up from a vendor announcement, a conference talk or product documentation, and each carrying that source's disclosure incentive. No figure in either column was produced by a common measurement.
- **Semgrep is a competitor in this market** and states an announcement of its own planned for Black Hat in August 2026, so the category boundaries and the use-case recommendations carry that interest.
- **The sharpest cells rest on the survey's LLM-generated sections.** Isolation postures, stage counts, model defaults and per-tool behaviour all restate a machine reading of the repositories rather than the projects' own documentation. RAPTOR's stage count is disputed between that reading and the project's own README; [[raptor|the RAPTOR page]] records both figures.
- **deepsec's licence is unsettled in the source itself.** The category list says Apache; the capability matrix carries a footnote instructing a reader to confirm the licence in-repo before relying on it. Both readings stand until the repository is checked.
- **Star counts measure attention at one point in time.** They rank neither capability nor adoption.

## See also

- [[semgrep-oss-ai-security-harness-comparison|Comparing Open Source AI Code Security Harnesses]] — the source summary.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — the thesis this field belongs to.
- [[ai-vuln-discovery-benchmark-landscape|AI Vuln-Discovery Benchmark Landscape]] — the benchmark stack none of the nine appears on.
- [[agent-sandbox-isolation-landscape|Agent Sandbox Isolation Landscape]] — where the isolation postures above meet the sandbox market.
- [[vulnops|VulnOps]] — the function that consumes these findings.

## Notes

[^semgrep]: [Semgrep — Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses), July 2026. No day-level publication date is exposed; July is inferred from an embedded screenshot dated 2026-07-20 and the article's forward reference to a Black Hat announcement in August 2026, and no author is named. The human-written body carries the categorisation, the star counts, the six cross-cutting findings, the finding-definition and execution tables, and the market-structure argument; the per-tool detail sections and two of the four tables are labelled by Semgrep as LLM-generated summaries. Summarized at [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].
