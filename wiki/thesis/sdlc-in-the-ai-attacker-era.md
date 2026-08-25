---
type: thesis
title: "SDLC in the AI-Attacker Era"
address: c-000023
created: 2026-05-13
updated: 2026-08-24
tags:
  - thesis
  - sdlc
  - sec-against-ai
  - attack-surface
  - supply-chain
status: developing
origin: produced
scope_axis:
  - sec-against-ai
question: "How do SDLC, supply chain, identity, and attack-surface assumptions need to evolve when adversaries have frontier AI capability, and which existing controls remain load-bearing vs. which need rework?"
current_position: "Developing — quantified evidence now shows time-to-exploit collapsing to hours and AI-assisted supply-chain attacks scaling, while standards calibrated against human-paced adversaries lag; existing AI-security controls carry into the inverse framing, and coordinated-disclosure and patch-window assumptions need recalibration. The recalibration reaches two variables. Vendor abuse telemetry shows actors shipping competent attack tooling they could not have written, so the assumed adversary population moves alongside the timelines. Four remedies the thesis leans on are now bounded. Disclosure measured net-negative for embedded devices at human pace, before agents entered the picture. A memory-safe rewrite retires the memory-corruption property class and inherits the rest, as 79 CVEs against a shipped Rust coreutils reimplementation show. A verified fix still has to reach the running estate, and Google, which automates the generation of those fixes, states it has no approach to redeployment at scale. Severity ranking presumes a queue short enough to sort, and Adkins argues that agentic discovery will exhaust CVSS as a triage instrument. A primary-source August 2026 disclosure supplies the existence proof for fully automated discovery-to-cluster-admin inside 13 hours against unknown flaws, and extends the supply-chain surface from public registries to the organization's own artifact repository."
last_revised: 2026-08-24
related:
  - "[[supply-chain-security-for-agents]]"
  - "[[gemini-cli-workspace-trust-rce]]"
  - "[[ai-era-supply-chain-hardening]]"
  - "[[slopsquatting]]"
  - "[[nsa-ai-ml-supply-chain-guidance-2026]]"
  - "[[ai-bom]]"
  - "[[anti-patterns-and-failure-modes]]"
  - "[[agent-availability-threats]]"
  - "[[least-agency-principle]]"
  - "[[capability-based-authorization]]"
  - "[[tenuo-warrant]]"
  - "[[ai-coding-agent-governance]]"
  - "[[plan-validate-execute]]"
  - "[[ai-spm]]"
  - "[[dspm]]"
  - "[[nhi-governance-for-agents]]"
  - "[[guardian-agent-metagovernance]]"
  - "[[iso-iec-42001]]"
  - "[[nist-ai-rmf]]"
  - "[[microsoft-zt4ai]]"
  - "[[microsoft-sdl]]"
  - "[[capability-floor-collapse]]"
  - "[[gtg-5004-no-code-ransomware]]"
  - "[[gtg-2002-vibe-hacking-extortion]]"
  - "[[llm-attack-navigator]]"
  - "[[anthropic-threat-intelligence-reports]]"
  - "[[microsoft-sdl-evolving-security-practices]]"
  - "[[nist-ssdf]]"
  - "[[nist-sp-800-218a]]"
  - "[[standards-review-nist-sp-800-218a-2026-Q2]]"
  - "[[apostol-vassilev]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[glasswing]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[crowdstrike]]"
  - "[[palo-alto-networks]]"
  - "[[anthropic-2026-agentic-coding-trends]]"
  - "[[collaboration-paradox]]"
  - "[[pwc-agentic-sdlc-in-practice]]"
  - "[[pwc-stage-coverage-tiers]]"
  - "[[metr-rct-2025]]"
  - "[[prt-scan-supply-chain-campaign]]"
  - "[[zero-day-clock]]"
  - "[[citizen-coders]]"
  - "[[guardian-agent-metagovernance]]"
  - "[[jfrog-ssc-state-of-union-2026]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[securing-agentic-coding]]"
  - "[[microsoft-cli-coding-agent-adoption-study]]"
  - "[[claude-code-github-action-credential-exposure]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[offensive-agent-collective]]"
  - "[[artifactory]]"
  - "[[vulnops]]"
  - "[[agentic-soc-ra-exposure-vulnops]]"
  - "[[vulnerability-research-agentic-age-keynote]]"
  - "[[vulnerability-properties]]"
  - "[[arizona-state-university]]"
  - "[[yan-shoshitaishvili]]"
  - "[[autonomous-code-security-google-talk]]"
  - "[[four-flynn]]"
sources:
  - "[[.raw/articles/anthropic-glasswing-2026-05-13.md]]"
  - "[[.raw/papers/anthropic-2026-agentic-coding-trends-report.pdf]]"
  - "[[.raw/papers/pwc-future-of-solutions-dev-gen-ai-2026.pdf]]"
  - "[[.raw/articles/microsoft-sdl-evolving-security-practices-2026-02-03.md]]"
  - "[[.raw/papers/nist-sp-800-218.pdf]]"
  - "[[.raw/papers/nist-sp-800-218A.pdf]]"
---

# SDLC in the AI-Attacker Era

## On this page

- [Question](#question)
- [Current position](#current-position)
- [Time-to-exploit collapse](#time-to-exploit-collapse)
- [Supply-chain attack surface](#supply-chain-attack-surface)
- [Vendor and standards response](#vendor-and-standards-response)
- [Remediation assumptions under agentic scale](#remediation-assumptions-under-agentic-scale)
- [Counter-evidence](#counter-evidence)
- [Open sub-questions](#open-sub-questions)
- [Notes](#notes)

## Question

How do SDLC, supply chain, identity, and attack-surface assumptions need to evolve when adversaries have frontier AI capability, and which existing controls remain load-bearing versus which need rework? Specifically: which assumptions in SLSA, [[nist-ssdf|SSDF]], CSAF, and ISO 27001 were calibrated against a human-paced attacker and now require explicit recalibration? Where does the existing [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] (which addresses securing *AI systems*) need extension to address securing *non-AI systems against AI-augmented attackers*?

## Current position

The wiki's coverage of supply chain, governance, and SDLC controls ([[supply-chain-security-for-agents|supply chain security for agents]], [[ai-bom|AI-BOM]], [[ai-coding-agent-governance|coding-agent governance]], [[least-agency-principle|least-agency]], [[plan-validate-execute|plan-validate-execute]]) was framed for securing AI systems. The inverse problem is securing classical SDLC against AI-augmented attackers. The tooling overlaps; the threat model does not. Four claims now carry quantified or government-anchored evidence rather than projection.

First, the time-to-exploit window has collapsed. The median time from CVE disclosure to first observed exploitation fell from 771 days in 2018 to zero-day by 2025, when the exploit now arrives on or before the advisory.[^zdc] AI systems generate working proof-of-concept exploit code from a CVE disclosure in 10 to 15 minutes at roughly \$1 per attempt.[^csa-poc] In the first quarter of 2025, 28.3% of newly-exploited vulnerabilities had exploitation evidence within a day of disclosure.[^vulncheck] Coordinated-disclosure timelines built on a 90-day human-paced assumption do not hold against this curve; a vulnerability disclosed Monday can be weaponized the same day. One academic source questions the direction rather than the pace: measured on embedded devices, with no agents involved, disclosure endangered roughly three times as many devices as it secured.[^asu-keynote] Under that reading, a faster pipeline treats the wrong variable.

Second, the supply chain leads the attack surfaces available to AI-augmented adversaries, and public reporting now documents the campaigns rather than projecting them. Over roughly three weeks the [[prt-scan-supply-chain-campaign|prt-scan campaign]] opened well over 500 malicious pull requests against public GitHub repositories with AI-generated, language-aware payloads, and took AWS keys, Cloudflare API tokens, and Netlify auth tokens with them. It used no zero-day. Default CI/CD permission configurations were enough.[^prt-scan] A new attack class, [[slopsquatting|slopsquatting]], exploits the developer's use of AI tooling rather than any flaw in the registry: adversaries register package names that coding assistants reliably hallucinate.[^slop] Malicious package uploads are rising sharply alongside these named campaigns.[^jfrog]

Third, the control categories the wiki already documents carry into this framing, but the standards do not. Existing primitives apply with little change. AI-BOM, skill-registry scanning, and pre-install vetting from [[supply-chain-security-for-agents|supply chain security for agents]] hold when the attacker's tooling is the agentic stack. The [[plan-validate-execute|Plan-Validate-Execute]] human-in-the-loop pattern translates to mandatory human review for AI-assisted merges. [[iso-iec-42001|ISO/IEC 42001]], [[nist-ai-rmf|NIST AI RMF]], and [[microsoft-zt4ai|Microsoft ZT4AI]] anchor the management-system and zero-trust framing. What those instruments look like when applied to non-AI systems facing AI-augmented attackers remains open. No SLSA, SSDF, or CSAF revision yet addresses that recalibration directly.

Fourth, the recalibration reaches the assumed adversary as well as the assumed pace. Every figure above measures an interval — days to exploit, minutes to a working proof of concept, dollars per attempt — and each assumes a capable adversary and measures the pace. Anthropic's August 2025 threat intelligence report measures something the interval figures cannot see: [[gtg-5004-no-code-ransomware|GTG-5004]] sold working ransomware with ChaCha20 encryption, two direct-syscall EDR bypasses, and shadow-copy deletion, while Anthropic reads that same operator's prompt record as showing someone who could not implement encryption, anti-analysis, or Windows internals manipulation without model assistance.[^tir-5004] [[gtg-2002-vibe-hacking-extortion|GTG-2002]] reached at least 17 organizations in roughly a month as a single operator.[^tir-vh] Across 832 banned accounts, an operator's assessed technical sophistication barely tracked the risk their activity posed, at r = 0.28.[^nav] Recalibrating the timelines and leaving the assumed adversary population unchanged covers one of the two variables that moved. The concept page is [[capability-floor-collapse|Capability Floor Collapse]].

That distinction decides where defensive money goes. If only the pace changed, defenders should shorten their own cycle: tighter patch windows, automated advisory ingestion, pre-staged remediation. If the population also changed, they should first cover published techniques completely, because the newly-capable operator reuses documented tradecraft — detection for FreshyCalls and RecycledGate existed before GTG-5004 sold them, and the [[verizon-dbir-2026|Verizon DBIR 2026]] finds a median of 55 prior public examples behind AI-assisted malware. Both readings hold. They buy different things.

Coding-agent governance reads in both directions at once. As a defensive control class it covers rules-file integrity, IDE-extension provenance, dependency-name and typosquat defense, and destructive-action classification, the surface that secures an organization's own AI-augmented developers. Viewed inversely, the same surface describes the attacker's productivity stack. The category is anchored by the wiki's [[guardian-agent-metagovernance|guardian-agent vendor set]]; [[ai-coding-agent-governance|Knostic]] is one example among that set. The guidance stands on the control class, not on any single vendor.

## Time-to-exploit collapse

The window between disclosure and exploitation is the thesis's load-bearing quantitative claim. Multiple independent sources now anchor it. The [[zero-day-clock|Zero Day Clock]] places median time-to-exploit at 771 days in 2018 and zero-day by 2025 across a dataset of CVE-exploit pairs.[^zdc] The Cloud Security Alliance "Collapsing Exploit Window" whitepaper traces the same curve through median figures: 756 days in 2018, roughly 32 days in 2022, roughly 5 days in 2023, and 32.1% of analyzed first-half-2025 CVEs exploited on or before public disclosure.[^csa-curve] The same whitepaper reports that AI systems generate functional proof-of-concept exploit code from a CVE disclosure in 10 to 15 minutes at roughly \$1 per attempt.[^csa-poc] VulnCheck's Q1 2025 analysis found that, of 159 vulnerabilities first reported exploited in the wild, 28.3% had exploitation evidence within a day of disclosure.[^vulncheck] Rapid7's 2026 Cyber Threat Landscape Report adds an independent dataset: confirmed exploitation of newly disclosed high/critical vulnerabilities rose 105% year over year, to 146 in 2025 from 71 in 2024, while mean time-to-exploit fell from 61.0 to 28.5 days.[^rapid7] The Rapid7 mean and the Zero Day Clock median diverge because the distribution is right-skewed: most exploited vulnerabilities are weaponized at or before disclosure, while a slow-exploited tail pulls the mean out to weeks.

Vendor and standards language tracks the same shift. CrowdStrike CTO Elia Zaitsev: "The window between a vulnerability being discovered and being exploited by an adversary has collapsed — what once took months now happens in minutes with AI," adding that "adversaries will inevitably look to exploit the same capabilities."[^glasswing] Palo Alto Networks CPTO Lee Klarich: "There will be more attacks, faster attacks, and more sophisticated attacks. Now is the time to modernize cybersecurity stacks everywhere."[^glasswing] Microsoft's SDL-for-AI post (Yonatan Zunger) frames a parallel speed gap: "AI accelerates development cycles beyond SDL norms. Model updates, new tools, and evolving agent behaviors outpace traditional review processes, leaving less time for testing and observing long-term effects. Usage norms lag tool evolution, amplifying misuse risks."[^msft-sdl] Where CrowdStrike frames the asymmetry as discovery-versus-exploitation time, Microsoft frames it as tool-evolution-versus-usage-norm time; both segments belong to the same collapse, and both invalidate a 90-day coordinated-disclosure assumption. Veracode's Chris Wysopal states the practitioner view plainly: "The patch window has effectively collapsed. That is not a gradual trend; it's a structural break." Every shipped patch is a roadmap that attackers can diff and weaponize faster than enterprises can test and deploy.[^wysopal]

The figures above measure the interval from disclosure to exploitation of a known vulnerability. The [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] measures the other interval, from first access to full control against previously unknown flaws, and it is shorter. Working from a foothold on a third-party benchmark application outside both organizations' perimeters, autonomous agents chained two Hugging Face zero-days — an HDF5 dataset-parsing flaw giving arbitrary file read, then a Jinja template-injection remote code execution that the file read exposed — and moved from a single dataset-worker pod to cluster admin across multiple Hugging Face clusters in under 13 hours.[^bh] No CVE existed to disclose, so the disclosure-to-exploit clock never started. This is the existence proof the collapse argument otherwise lacks. Discovery, exploitation, and privilege escalation ran inside one interval, with no human in the loop and no prior public knowledge of either flaw, which removes the advisory from the timeline the patch window is measured against. The speakers state the resulting asymmetry directly: fully automated offense now has an existence proof and fully automated defense does not.[^bh]

## Supply-chain attack surface

The supply chain is where AI-augmented adversaries are now observed operating. The [[prt-scan-supply-chain-campaign|prt-scan campaign]] (March–April 2026, documented by Wiz) used AI-generated, language-aware payloads to open well over 500 malicious pull requests against public GitHub repositories over roughly three weeks, with an observed success rate under 10% across more than 450 analyzed attempts.[^prt-scan] It verified theft of AWS keys, Cloudflare API tokens, and Netlify auth tokens and compromised at least two npm packages across 106 versions.[^prt-exfil] The attack required no zero-day, only default `pull_request_target` workflow permissions that most organizations have not hardened.[^prt-trigger] Among public 2026 cases, it records AI-assisted CI/CD pipeline exploitation at machine speed in the most detail.

[[slopsquatting|Slopsquatting]] names a structurally new attack class: adversaries register package names that AI coding assistants reliably hallucinate. The arXiv study "We Have a Package for You!" tested 16 LLMs across 576,000 Python and JavaScript code samples, finding a roughly 20% package-name hallucination rate; 43% of hallucinated names recurred across repeated requests.[^slop] A proof-of-concept package seeded under a commonly hallucinated name accumulated over 30,000 downloads in three months.[^slop-poc] Typosquatting detectors match misspellings of names that exist, so an invented name passes them. The [[citizen-coders|Citizen Coders]] dynamic widens the exposure: non-developers generating code through AI are less likely to verify package recommendations against registry histories.

The trend is not confined to single campaigns. Malicious package uploads to public registries rose sharply year over year, with JFrog reporting a 451% surge in malicious npm packages to 171,592 unique instances in 2025, driven by three hijack campaigns that produced more than two million compromised downloads.[^jfrog] Four discrete AI supply-chain attacks, against Trivy, LiteLLM, Telnyx, and Axios, were disclosed within a 50-day window in early 2026.[^four-attacks]

The attack surface also widens from the defender's own AI-authored code. JFrog attributes part of a 20% year-over-year rise in disclosed CVEs, more than 48,000 in 2025, to AI-generated code that omits secure-coding practices, reviving decades-old injection classes such as cross-site scripting and SQL injection at volume.[^jfrog-cve] The same tooling that accelerates development reintroduces the weakness classes secure-SDLC programs spent two decades suppressing.

Every campaign above runs through a public registry, and the control set the wiki documents follows from that shape: scan what arrives, verify publishers, pin and hash dependencies. The internal registry is the same surface with the arrows reversed. In the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]], the organization's own [[artifactory|JFrog Artifactory]] instance was simultaneously the covert channel between isolated agent runs, the indirect egress path out of sandboxes with the internet disabled, and the remote-code-execution surface that yielded the administrative signing key — with no malicious package published to it by an outside party.[^bh] Three properties of an ordinary internal deployment carried it: write access shared fleet-wide rather than scoped per workload, broad outbound internet access on a service every sandboxed workload was permitted to reach, and the service's own unpatched vulnerabilities. A private registry anchors trust for artifacts and sits on the attack surface as an application; the supply-chain control set covers the first role and omits the second. [[supply-chain-security-for-agents|Supply chain security for agents]] carries the control gap: write access to internal artifact repositories held by non-human workload identities.

The coding agent now sits on the attack surface alongside the code it writes. The [[guardfall-shell-injection-audit|GuardFall audit]] (Adversa AI, 2026-06-30) drove ten of eleven surveyed open-source coding agents into arbitrary shell execution using injected READMEs, compromised Makefiles, and malicious MCP servers — delivery channels that arrive with the repository the agent was pointed at. [[claude-code-github-action-credential-exposure|Microsoft Defender research]] (2026-06-05) took a model API key out of a CI workflow through an HTML-comment injection in a pull request. The [[gemini-cli-workspace-trust-rce|Gemini CLI advisory]] (GHSA-wpqr-6v78-jr5g, 2026-04-24) is the third and the most severe at CVSS 10.0, and it extends the class in two directions: the attacker-controlled input was a `.gemini/` configuration directory rather than prose, and the execution it produced ran before the harness sandbox initialized.[^gemini-sdlc] All three reach the same structural finding from different directions: repository content is attacker-controlled input, and an agent holding credentials and egress satisfies the [[agents-rule-of-two|Rule of Two]] in full the moment it reads that content. Where that agent runs determines whether a human is positioned to notice — see [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] for the five variants and [[securing-agentic-coding|Securing Agentic Coding]] for the control catalog.

## Vendor and standards response

Vendors and government bodies are recalibrating against the same capability shift. The [[anthropic-2026-agentic-coding-trends|Anthropic 2026 Agentic Coding Trends Report]] makes the dual-use case at strategic level. Trend 8, "Agentic coding improves security defenses — but also offensive uses," predicts that security knowledge becomes democratized ("any engineer can become a security engineer capable of delivering in-depth security reviews, hardening, and monitoring"), that threat actors scale attacks ("While agents will benefit defensive uses, they will also benefit offensive uses too"), and that agentic cyber-defense systems rise ("Automated agentic systems enable security responses at machine speed").[^anthropic-trends] The report's closing position states the asymmetry directly: "The balance favors prepared organizations. Teams that use agentic tools to bake security in from the start will be better positioned to defend against adversaries using the same technology."[^anthropic-trends] Its named Priority 4 — "Embedding security architecture as a part of agentic system design from the earliest stages" — positions secure-by-design as a strategic recommendation rather than a capability claim.[^anthropic-trends]

[[microsoft-sdl-evolving-security-practices|Microsoft's SDL-for-AI announcement]] is the first major-vendor classical secure-SDLC framework to publish an explicit AI extension scope; its prescribed mitigation pattern is "iterative security controls, faster feedback loops, telemetry-driven detection, and continuous learning."[^msft-sdl] Microsoft SDL carries vendor authority, and NIST and SLSA have published no counterpart, so the standards-side gap remains open.

On the government side, the [[nsa-ai-ml-supply-chain-guidance-2026|NSA 8-nation joint guidance]] (March 2026), co-signed by NSA, CISA, FBI, and allied agencies, provides a government-endorsed supply-chain threat taxonomy across six AI/ML components: Training Data, Model Weights, Software Dependencies, Infrastructure, Third-Party APIs, and Deployment. It names slopsquatting-class software-dependency risks alongside training-data poisoning and model-weight backdooring. CISA's operational response is measurable: the average KEV patch deadline tightened from 19.7 days in 2025 to 14.4 days in 2026, and CISA is reportedly considering a 3-day deadline for KEV-listed flaws.[^kev]

OpenAI's own recommendation, published with the disclosure, is a scope claim about what has to be automated. Automating discovery alone relocates the bottleneck rather than removing it: agents that are demonstrably good at finding zero-days in production infrastructure produce findings faster than engineers can act on them, and the organization ends up with a longer queue rather than a shorter exposure window. The loop the speakers name runs identify, propose a patch, roll it out, and roll it back on an availability regression, with the rollback leg treated as part of the loop rather than as a manual escape hatch — an automated patch pipeline without automated rollback converts a bad fix into an outage.[^bh] Continuous agentic red teaming is the paired recommendation. Model intelligence will examine the estate either way; the organization decides only whether it turns enough of that intelligence on its own infrastructure before a threat actor does. This is the same closing argument [[vulnops|VulnOps]] makes as an operating model, sharpened by a case where the finding rate was set by an autonomous fleet rather than by a scanning schedule; [[agentic-soc-ra-exposure-vulnops|the exposure and VulnOps function]] carries the autonomy gating for the remediation side.

## Remediation assumptions under agentic scale

The sections above measure how fast an adversary reaches a vulnerability. Two of this thesis's implicit remedies — disclose it, or rewrite the code in a memory-safe language — carry assumptions that one academic source tests directly.[^asu-keynote] Two further assumptions — that a generated fix reaches the estate, and that findings stay scarce enough to rank — are bounded by Google, which runs the autonomous-patching programme this wiki has sourced in most detail.[^google-talk]

**Disclosure is measured as net-negative for one device class.** A May 2026 study from [[arizona-state-university|Arizona State University]]'s lab reproduced disclosed embedded-device vulnerabilities against other devices held in the lab. Each disclosure endangered roughly three times as many devices as it secured, with the ratio depending on what is counted as endangerment. The measurement involved no agents, so it isolates a property of the disclosure mechanism at human pace: agentic discovery multiplies that effect rather than causing it. The same lab reports finding at roughly ten times its reportable rate and states plainly that it has no better mechanism to offer. This does not resolve into a recommendation to stop disclosing; it means the page's disclosure argument cannot rest on speed alone, and [[vulnops|VulnOps]] carries the same qualification.

**A memory-safe rewrite retires one property class and inherits the rest.** [[autonomous-code-security-google-talk|Google's March 2026 talk]] leaves the allocation question open, whether to patch C++ at all or rewrite it in Rust. [[vulnerability-properties|Vulnerability properties]] survive reimplementation: 79 CVEs dropped against a Rust reimplementation of coreutils after it shipped in the current Ubuntu release, carrying time-of-check/time-of-use flaws rather than memory corruption. In the same lab's agentic rewrites of libssl, libpng, and libxml, the crypto library reproduced the classic non-memory-safety cryptographic attacks of the original even under explicit instruction naming and forbidding them. Agentic tooling makes large rewrites affordable, which moves the rewrite from a proposal to a decision an organization will actually face — and the new code arrives with no analysis history and a threat model inherited from the original's non-memory-safety flaws. [[zero-day-clock|The Zero Day Clock's]] fourth demand is bounded by the same evidence.

Both findings share a mechanism this thesis otherwise lacks a name for. Vulnerabilities carry transferable properties, which is why disclosure arms an adversary against devices that were never in the advisory's scope, and why a rewrite in a different language reproduces the flaw classes the original was predisposed to.

**A generated fix still has to reach the running estate.** Google names deployment as the step it cannot automate. Flynn listed redeploying auto-mended code at scale as one of three open problems in [[autonomous-code-security-google-talk|Autonomous Code Security at Google]], and located the hardest part of patching in the estates that cannot apply a patch promptly: "I don't know how to solve that with AI."[^google-talk] Where a pipeline generates verified fixes faster than estates absorb them, the exposure window relocates to deployment, which is the leg [[vulnops|VulnOps]] closes with rollout and rollback.

**Severity ranking assumes findings are scarce.** Adkins drew a prioritization consequence from agentic discovery reaching every vulnerability in every system: "We'll have to change the CVSS scoring system because it won't be meaningful anymore."[^google-talk] A severity score sorts a queue, and sorting presumes a queue short enough to work through. The volume evidence she cited is a 30,000-item unanalyzed backlog at the National Vulnerability Database and a 35% rise between 2024 and 2025 in logged vulnerabilities receiving a CVE, against a population in which not every discovered bug receives one at all.[^google-talk] Timelines and adversary population are recalibrated above; severity-based prioritization is the third assumption, and Adkins's claim is that the instrument stops discriminating once discovery stops being the constraint.

## Counter-evidence

> [!gap] Calibrated incident data
> Public incident reports do not yet systematically attribute attacker capability to frontier-AI assistance. Whether an exploit was AI-assisted is rarely a published field. This makes "the threat model is changing" hard to source rigorously.

> [!gap] Real-world productivity gains may be smaller than capability gains
> The [[metr-rct-2025|METR 2025 RCT]] found 16 experienced developers were 19% slower using AI tooling on familiar codebases, despite expecting to be faster.[^metr] This bounds the threat-velocity claim symmetrically: if real-world productivity gains lag capability gains for defenders, the same gap applies to attackers. The exploit-velocity figures above measure capability at the point of generation, not sustained operational throughput.

> [!gap] SLSA / SSDF / CSAF updates for AI-augmented attackers
> [[nist-ssdf|NIST SSDF v1.1 (Feb 2022)]] addresses the secure-development side but not the AI-augmented-adversary side; its threat assumptions remain human-paced. [[nist-sp-800-218a|SP 800-218A (July 2024)]] extends SSDF for AI model development but does not address deployment, operation, or the inverse problem of defending non-AI systems against AI-augmented attackers — [[standards-review-nist-sp-800-218a-2026-Q2|the 2026-Q2 standards review]] confirms it contributes development-time process tasks only, with no runtime guardrail, egress, or agent-identity control. No SLSA, CSAF, or comparable revision yet addresses AI-augmented adversaries directly. Whether the frameworks should be updated, or whether the rules carry unchanged with tighter tolerances, is unresolved. The [[anthropic-glasswing-announcement|Glasswing announcement]] commits to "collaborate with leading security organizations" on this gap (named areas include vulnerability-disclosure processes, SDLC and secure-by-design, supply-chain security, and standards for regulated industries), but no concrete deliverable has landed yet.

## Position history

- **2026-08-24.** [[autonomous-code-security-google-talk|Google's March 2026 conference talk]] bounded two further remediation assumptions this thesis leans on. Flynn named redeploying auto-mended code at scale as one of the hardest problems in patching and stated he has no approach to it, so a generated fix still depends on the estate absorbing it — a gap [[vulnops|VulnOps]]'s rollout-and-rollback leg has to close. Adkins argued that agentic discovery reaching every vulnerability in every system will exhaust CVSS as a triage instrument, citing a 30,000-item NVD backlog and a 35% year-over-year rise in logged CVEs.[^google-talk] Both bound the thesis's remediation-side assumptions rather than its discovery-side timeline claims.

## Open sub-questions

- Does the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] need an extension (new domain D10 "AI-Threat-Calibrated SDLC") or a parallel companion CMM ("Enterprise SDLC vs AI-Augmented Adversaries")? Current judgment: too early; defer the artifact decision until evidence accrues.
- How does the [[agent-availability-threats|agent availability threats]] surface translate to *defending* against availability attacks by AI-augmented adversaries (e.g., autonomous DDoS with adaptive evasion)?
- See [[wiki/gaps/_index|Gaps Index]] for related open questions.

## Notes

[^tir-5004]: Anthropic, [*Threat Intelligence Report: August 2025*](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf), pp. 15–17: the actor "does not appear capable of implementing encryption algorithms, anti-analysis techniques, or Windows internals manipulation without Claude's assistance." See [[gtg-5004-no-code-ransomware|No-Code Ransomware Operation]].

[^tir-vh]: Ibid., pp. 4–10. At least 17 organizations across government, healthcare, emergency services, and religious institutions in roughly one month. See [[gtg-2002-vibe-hacking-extortion|Vibe-Hacking Extortion Campaign]].

[^nav]: Kyla Guru, Alex Moix, and Jacob Klein, [*Mapping AI-enabled cyber threats: Insights from the LLM ATT&CK Navigator*](https://red.anthropic.com/2026/attack-navigator/), Anthropic Frontier Red Team, 2026-06-03: technical sophistication r = 0.28 once decoupled from the composite risk score, across 832 accounts. See [[llm-attack-navigator|LLM ATT&CK Navigator]].

[^zdc]: [The Collapse — Zero Day Clock](https://zerodayclock.com/collapse), Sysdig and collaborators, 2026. Median time-to-exploit across CVE-exploit pairs: 771 days (2018), 84 days (2021), 6.36 days (2023), 4 hours (2024), zero-day (2025–2026).
[^csa-curve]: [Cloud Security Alliance — The Collapsing Exploit Window: AI-Speed Vulnerability Weaponization](https://labs.cloudsecurityalliance.org/research/csa-whitepaper-collapsing-exploit-window-ai-speed-vulnerabil/), AI Safety Initiative, 2026. Median disclosure-to-exploit time 756 days (2018), ~32 days (2022), ~5 days (2023); 32.1% of 432 confirmed-exploitation CVEs in first-half 2025 exploited on or before public disclosure (up from 23.6% in 2024).
[^csa-poc]: [Cloud Security Alliance — The Collapsing Exploit Window: AI-Speed Vulnerability Weaponization](https://labs.cloudsecurityalliance.org/research/csa-whitepaper-collapsing-exploit-window-ai-speed-vulnerabil/), 2026. AI systems generate functional exploit code in 10 to 15 minutes at approximately \$1 per attempt.
[^vulncheck]: [VulnCheck — 2025 Q1 Trends in Vulnerability Exploitation](https://www.vulncheck.com/blog/exploitation-trends-q1-2025), Patrick Garrity, April 2025. Of 159 vulnerabilities first reported exploited in the wild in Q1 2025, 28.3% had exploitation evidence within one day of CVE publication. Summary: [[vulncheck-exploitation-trends-q1-2025|VulnCheck Q1 2025 exploitation trends]].
[^glasswing]: [Anthropic — Project Glasswing](https://www.anthropic.com/glasswing), May 12, 2026. CrowdStrike CTO Elia Zaitsev and Palo Alto Networks CPTO Lee Klarich launch-partner citations.
[^msft-sdl]: [Microsoft Security Blog — Microsoft SDL: Evolving security practices for an AI-powered world](https://www.microsoft.com/en-us/security/blog/2026/02/03/microsoft-sdl-evolving-security-practices-for-an-ai-powered-world/), Yonatan Zunger, February 3, 2026.
[^prt-scan]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. "Across all six waves, the attacker opened well over 500 malicious PRs"; campaign ran from March 11 to April 3, 2026 (roughly three weeks); "<10% success rate" across over 450 analyzed exploit attempts. See [[prt-scan-supply-chain-campaign|prt-scan CI/CD Supply-Chain Campaign]].
[^prt-exfil]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. "Verified credential theft was observed impacting AWS keys, Cloudflare API tokens, and Netlify auth tokens"; "at least two npm packages with a shared maintainer, across 106 versions."
[^prt-trigger]: [Wiz — Six Accounts, One Actor: Inside the prt-scan Supply Chain Campaign](https://www.wiz.io/blog/six-accounts-one-actor-inside-the-prt-scan-supply-chain-campaign), 2026. The attack exploited default `pull_request_target` workflow permissions and used no zero-day.
[^slop]: [arXiv 2406.10279 — We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs](https://arxiv.org/abs/2406.10279), 2024 (Spracklen et al.). 16 LLMs across 576,000 Python and JavaScript code samples; roughly 20% package-name hallucination rate; 43% of hallucinated names recurred across repeated requests.
[^slop-poc]: [SD Times — Hallucinated code, real threat: How slopsquatting targets AI-assisted development](https://sdtimes.com/coding-assistants/hallucinated-code-real-threat-how-slopsquatting-targets-ai-assisted-development/), 2025. Bar Lanyado (Lasso Security) registered a commonly hallucinated package name as an empty PyPI package; it received over 30,000 downloads in three months.
[^jfrog]: [JFrog — 2026 Software Supply Chain Security State of the Union (announcement)](https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs), 2026, report p.5. Malicious npm packages rose 451% to 171,592 unique instances, driven by three hijack campaigns producing more than two million compromised downloads. See [[jfrog-ssc-state-of-union-2026|JFrog 2026 SSC State of the Union]].
[^jfrog-cve]: [JFrog — 2026 Software Supply Chain Security State of the Union (announcement)](https://www.businesswire.com/news/home/20260520126325/en/New-JFrog-Report-Warns-AI-Governance-Fails-as-Software-Supply-Chain-Attacks-Hit-Record-Highs), 2026, report p.5. Over 48,000 new CVEs disclosed in 2025, a 20% increase over 2024; JFrog attributes part of the growth to AI-generated code that omits secure-coding practices, reviving XSS, SQL injection, and other injection classes. See [[jfrog-ssc-state-of-union-2026|JFrog 2026 SSC State of the Union]].
[^four-attacks]: [VentureBeat — Four AI supply-chain attacks in 50 days exposed the release pipeline red teams aren't covering](https://venturebeat.com/security/supply-chain-incidents-openai-anthropic-meta-release-surface-vendor-questionnaire-matrix), 2026. Four disclosed AI supply-chain attacks (Trivy, LiteLLM, Telnyx, Axios) within a 50-day window in early 2026.
[^anthropic-trends]: [Anthropic — 2026 Agentic Coding Trends Report (PDF)](https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf), 2026. Trend 8 predictions, closing position, and Priority 4. Summary: [[anthropic-2026-agentic-coding-trends|2026 Agentic Coding Trends Report]].
[^kev]: [Federal News Network — AI drives new debate around CISA software patching deadlines](https://federalnewsnetwork.com/cybersecurity/2026/05/ai-drives-new-debate-around-cisa-software-patching-deadlines/), May 2026: average KEV deadline 14.4 days in 2026, down from 19.7 days in 2025. [SC Media — CISA reportedly considers 3-day patch deadline for KEV flaws](https://www.scworld.com/news/cisa-reportedly-considers-3-day-patch-deadline-for-kev-flaws), 2026.
[^metr]: [METR — Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/), July 2025 ([arXiv 2507.09089](https://arxiv.org/abs/2507.09089)). 16 experienced developers measured 19% slower on familiar codebases when AI tooling was enabled. Summary: [[metr-rct-2025|METR 2025 RCT]].
[^rapid7]: [Rapid7 — 2026 Cyber Threat Landscape Report](https://www.rapid7.com/research/report/global-threat-landscape-report-2026/), 2026, as reported by [CSO Online](https://www.csoonline.com/article/4156005/patch-windows-collapse-as-time-to-exploit-accelerates.html). Confirmed exploitation of newly disclosed high/critical (CVSS 7–10) vulnerabilities rose to 146 in 2025 from 71 in 2024 (+105%); mean time-to-exploit fell from 61.0 to 28.5 days; median publication-to-KEV-inclusion time fell from 8.5 to 5.0 days.
[^bh]: Michael Dalton and Eric Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026, 2026-08-06. Hugging Face chain: HDF5 dataset-parsing arbitrary file read chained to Jinja template-injection RCE; one dataset-worker pod to cluster admin across multiple clusters in under 13 hours. Artifactory as covert channel, egress path, and RCE surface; recommended fix loop of identify, propose patch, roll out, roll back. Summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]; timeline at [[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]].
[^gemini-sdlc]: [GitHub Advisory Database — GHSA-wpqr-6v78-jr5g](https://github.com/advisories/GHSA-wpqr-6v78-jr5g), 2026-04-24. CVSS 10.0; headless Gemini CLI automatically trusted the workspace folder for configuration and environment loading, and `--yolo` bypassed the fine-grained tool allowlist. The pre-sandbox execution ordering is stated by the reporting researcher at [Novee Security](https://novee.security/blog/google-gemini-cli-rce-vulnerability-cvss-10-critical-security-advisory/), 2026-04-30. See [[gemini-cli-workspace-trust-rce|the incident record]].
[^wysopal]: [CSO Online — Patch windows collapse as time-to-exploit accelerates](https://www.csoonline.com/article/4156005/patch-windows-collapse-as-time-to-exploit-accelerates.html), April 2026. Chris Wysopal (co-founder, Veracode): "The patch window has effectively collapsed. That is not a gradual trend; it's a structural break."
[^asu-keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06): a May 2026 embedded-device study finding disclosure endangers ~3x as many devices as it secures; 79 CVEs against a Rust coreutils reimplementation shipped in Ubuntu, none memory corruption. See [[vulnerability-research-agentic-age-keynote|the talk summary]].

[^google-talk]: Heather Adkins and Four Flynn, *Evaluating Threats & Automating Defense: How Google is Advancing Code Security*, [\[un\]prompted, San Francisco](https://www.youtube.com/watch?v=B_7RpP90rUk) (2026-03-03): redeploying auto-mended code at scale named as one of three open problems; CVSS stated to stop being meaningful once agentic discovery reaches every vulnerability; a 30,000-item NVD unanalyzed backlog and a 35% rise in CVE-carrying vulnerabilities between 2024 and 2025. See [[autonomous-code-security-google-talk|the talk summary]].
