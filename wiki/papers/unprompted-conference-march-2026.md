---
type: talk
title: "Unprompted Conference I"
address: c-000167
created: 2026-05-02
updated: 2026-06-02
origin: aggregated
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
tags:
  - papers
  - talks
  - conference
  - agentic-ai
  - prompt-injection
  - red-teaming
  - vulnerability-discovery
  - observability
  - governance
  - mcp
  - browser-agents
status: summarized
year: 2026
authors: ["Gadi Evron (CFP Chair)"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 3–4, 2026 (with March 2 unofficial reception and March 4 evening)"
license: "Public agenda (unpromptedcon.org)"
key_claim: "AI security has moved from theoretical to operational: in early 2026 practitioners are running autonomous bug-finding agents at production scale (FENRIR, AISLE, Promp2Pwn, XBOW), defending real agent platforms (Stripe, Google Workspace, Block, Perplexity, Salesforce, Dropbox), and observing AI-assisted attacks in the wild (Sysdig 8-min AWS escalation, EtherRAT, Shai-Hulud)."
methodology: "Single-track + dual-stage two-day conference; ~52 talks (Stage 1 + Stage 2) split across offense, defense, governance, observability, and tooling. Catalog below was built from the published agenda; abstracts paraphrased — primary source is the agenda copy at unpromptedcon.org and the YouTube archive (cited inline)."
contradicts: []
supports:
  - "[[ai-security-standards-in-q1-2026]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[lethal-trifecta]]"
  - "[[prompt-injection-containment]]"
  - "[[guardian-agent]]"
  - "[[oversight-layer]]"
  - "[[mcp-security]]"
related:
  - "[[indirect-prompt-injection]]"
  - "[[rag-hardening]]"
  - "[[agent-observability]]"
  - "[[supply-chain-security-for-agents]]"
  - "[[agent-sandboxing]]"
  - "[[credential-proxy-pattern]]"
sources:
  - .raw/articles/unprompted-conference-march-2026-agenda-2026-05-02.md
  - .raw/talks/unprompted-conference-talks-mar-2026.md
source_url: https://unpromptedcon.org/#
---

# Unprompted Conference

Source: [unpromptedcon.org agenda](https://unpromptedcon.org/#); talk videos on the `@un_prompted` YouTube channel.

Two-day single-narrative practitioner conference, San Francisco, March 3–4, 2026. CFP Chair: [[knostic|Gadi Evron]] (CEO, Knostic). Described as "intimate, raw, fun": practitioner-only, with sharp talks and live demos.

> [!note] Which Unprompted is this
> The source page headlines "Unprompted II ... Coming Back This September" but its body says "Unprompted is back for the second time in (or around) September." Read literally, the September 2026 event is Unprompted II and this March 3–4 agenda is the prior conference; the page also references material "from the first Unprompted" with YouTube videos already published. The iteration count is ambiguous on the source itself.
## Conference logistics

- **Stage 1 + Stage 2** running in parallel both days (Stage 2 opens later — 09:35 on Day 1, 09:10 on Day 2).
- **Evening events**: Mar 2 unofficial reception at The UNDERDOGS Cantina; Mar 3 official event at The Hibernia; Mar 4 easy-going get-together.
- **Materials from "the first Unprompted"** (per the source's own phrasing): YouTube channel `@un_prompted`; conference NotebookLM at `notebooklm.google.com/notebook/78ee3710-1741-488d-af06-159f518e9510`.
- **Detailed agenda**: `docs.google.com/spreadsheets/d/1J0ZbIvR5H7mAp43Io-xff_00EkX6evawPCbpTNqrRo0`.

## Day 1 — Stage 1 (Tuesday, March 3, 2026)

| Talk | Speaker(s) / Org | Notable data points |
|---|---|---|
| Opening Words — "Research conferences aren't effective" | Gadi Evron, [[knostic\|Knostic]] | Format pitch: structured matchmaking over random encounters; nods to Joe Stewart's ACoD talk |
| Evaluating Threats & Automating Defense: How Google is Advancing Code Security | Heather Adkins, [[google\|Google]] (VP Security Engineering) | **CodeMender** (Google's AI-driven code security tool); Google's full AI security strategy |
| [[measuring-agent-effectiveness-talk\|The Hard Part Isn't Building the Agent: On Measuring Agent Effectiveness]] | Joshua Saxe, [[meta\|Meta]] (AI Security Tech Lead) | From naive precision/recall to multi-dim eval (reasoning quality, evidence gathering, tool-calling); **genetic algorithms + AI coding tools for automated agent improvement**; live demo |
| Security Guidance as a Service: AI-Native Blueprint for Defensive Security | Shruti Datta Gupta + Chandrani Mukherjee, [[adobe\|Adobe]] | Centralized security knowledge powering multiple defensive AI capabilities; "consistent, evaluated, bespoke guidance" |
| [[guardrails-beyond-vibes-talk\|Guardrails Beyond Vibes: Shipping Security Agents in Production]] | [[jeffrey-zhang\|Jeffrey Zhang]] + [[sid-shah\|Siddh Shah]], [[stripe\|Stripe]] | **Threat modeling + security request routing agents** in production; modular orchestrator/child sequential pipeline; golden-standard + [[llm-as-a-judge\|LLM-as-a-Judge]] offline eval; phased rollout to all Stripe devs; AlphaEvolve prompt evolution failed at cost constraints; five concrete learnings |
| Code Is Free: Securing Software in the Agentic Future | Paul McMillan + Ryan Lopopolo, [[openai\|OpenAI]] | "Engineering-first, zero-friction" security via LLM-authored invariants; the **"Code is Free"** thesis |
| AI Agents for Exploiting "Auth-by-One" Errors | Brendan Dolan-Gavitt + Vincent Olesen, **XBOW** | AuthN + AuthZ validators as the unlock for autonomous exploit agents; real-world examples discovered in production |
| [[binaryshield-ai-fingerprints-talk\|Developing & Deploying AI Fingerprints for Advanced Threat Detection]] | Natalie Isak + Waris Gill, [[microsoft\|Microsoft]] | **BinaryShield** — privacy-preserving prompt-injection fingerprints for cross-service threat intel; F1 0.94 vs SimHash 0.77; arXiv:2509.05608 |
| When Passports Execute: Exploiting AI-Driven KYC Pipelines | Sean Park, **TrendAI** (Principal Threat Researcher) | **Document-embedded injects** in KYC extraction agents; cross-record reads/writes via compliance verification chain; LLM-generated high-success payloads validated with a Claude Code extraction agent |
| FENRIR: AI Hunting for AI Zero-Days at Scale | Peter Girnus + Derek Chen, **TrendAI** | **100+ vulns since mid-2025; 21 CVEs patched incl. multiple CVSS 9.8 RCEs**; multi-stage pipeline (static pre-triage → L1 LLM prune → L2 LLM deep-verify → confidence-based human routing); cites IRIS 2.5× over CodeQL, Big Sleep SQLite zero-day |
| AI Notetakers: The Most Important Person in the Room | Joe Sullivan, Ukraine Friends / Joe Sullivan Security | Steering the AI notetaker as governance risk and IR opportunity; consent + discovery exposure |
| AI go Beep Boop! | Adam Laurie ("Major Malfunction"), [[alpitronic\|Alpitronic]] (CISO) | **Claude pwned an LPC chip in 7 minutes that the speaker had failed to glitch in 6 weeks**; in <1 month rewrote his whole glitching platform |
| [[taming-shai-hulud-with-ai-talk\|Zeal of the Convert: Taming Shai-Hulud with AI]] | Rami McCarthy, [[wiz\|Wiz]] | **Shai-Hulud (2025)** post-mortem — internet-scale GitHub data leaks; from vibe-coded scrapers → multi-agent triage engines for victimology + secret impact |
| Anatomy of an Agentic Personal AI Infrastructure | [[daniel-miessler\|Daniel Miessler]], Unsupervised Learning | Personal AI infra deepdive + companion open-source project |
| Black-hat LLMs | Nicholas Carlini, [[anthropic\|Anthropic]] (Research Scientist) | "Recent SOTA models can find 0-day vulns in large software extensively human-tested for decades"; threat-landscape revision |
| Vibe Check: Security Failures in AI-Assisted IDEs | Piotr Ryciak, **Mindgard** (AI Red Teamer) | Catalog of exploitation patterns across **OpenAI Codex, Amazon Kiro, Google Antigravity, Cursor**, others; zero-click / one-click / autorun / time-delayed trigger taxonomy |

## Day 1 — Stage 2 (Tuesday, March 3, 2026)

| Talk | Speaker(s) / Org | Notable data points |
|---|---|---|
| Establishing AI Governance Without Stifling Innovation | Billy Norwood, FFF Enterprises (CISO) | Risk-based AI governance committee in healthcare services; successes + failures |
| Enterprise AI Governance at Snowflake | Ragini Ramalingam, [[snowflake\|Snowflake]] (Director, Enterprise Security) | Cross-functional governance framework, embedding security into emerging tech |
| Three Phases of AI Adoption: GPU Lottery → Enterprise Agreements | Chase Hasbrouck, **U.S. Army Cyber Command** (Chief of Forensics/Malware Analysis) | 2023 fragmented research previews / 2024 token budgets killed experimentation / 2025 enterprise agreements remove cost barriers; cultural change as the constraint |
| [[sift-claude-code-dfir-talk\|SIFT — FIND EVIL!! I Gave Claude Code R00t on the DFIR SIFT Workstation]] | Rob T. Lee, [[sans-institute\|SANS Institute]] (CAIO; Chief of Research) | Protocol SIFT = Claude Code + SIFT DFIR workstation over MCP; cites **Anthropic GTG-1002 — Claude Code at 80–90% autonomous execution**; framed experimental, not court-admissible |
| [[genai-endpoint-observability-talk\|Can You See What Your AI Saw?: GenAI Endpoint Observability for Detection Engineers]] | Mika Ayenson, [[elastic\|Elastic]] | Telemetry gaps + correlation across multi-level ancestry chains; case for extending **OpenTelemetry semantic conventions** to GenAI tool activity |
| [[syara-semantic-detection-talk\|Detecting GenAI Threats at Scale with YARA-Like Semantic Rules]] | [[mohamed-nabeel\|Mohamed Nabeel]], [[palo-alto-networks\|Palo Alto Networks]] (Sr Principal Researcher) | **SYARA** — YARA syntax + cost-ordered semantic layers (strings → embeddings → ML → LLM); open-source (`pip install syara`); cheap-layer pre-filter cuts LLM cost ~95% |
| The Advent of Confidential AI | Raghu Yeluri, [[intel\|Intel]] (Fellow) | TEE-based confidential AI for inferencing data + prompts + context; remote attestation; two real deployments |
| Tenderizing the Target: Soaking Code in Synthetic Vulnerabilities | Aaron Grattafiori + Skyler Bingham, [[nvidia\|NVIDIA]] | **Marinade** — agentic workflow that injects realistic exploitable vulnerabilities into Django/Spring Boot/Java/Rails codebases; preserves functionality; ships per-vuln validation script |
| [[hooking-coding-agents-with-cedar-talk\|Hooking Coding Agents with Cedar]] | [[matt-maisel\|Matt Maisel]], [[sondera\|Sondera]] (CTO/Cofounder) | **Rust hooks + Cedar policies** as a deterministic reference monitor for shell, file, and tool actions; open-source; Cursor + Claude Code + Gemini CLI; four-type **trajectory event model** (action/observation/control/state); IFC taint tracking across turns; policy agent for Cedar generation + formal validation over MCP |
| [[glass-box-security-talk\|Glass-Box Security: Operationalizing Mechanistic Interpretability for Defending AI Agents]] | [[carl-hurd\|Carl Hurd]], [[starseer\|Starseer]] (Co-Founder/CTO) | "Glass-Box Security" paradigm — forward-pass hooks + cosine similarity (intent) + scalar projection (strength); behavior-based detection rules in YARA-style AI modules; canary-model pattern for frontier-model coverage; argues for sovereign infra; semantic traceability vs syntactic traceability for agent trust |
| [[ai-security-larsen-effect-talk\|The AI Security Larsen Effect: How to Stop the Feedback Loop]] | Maxim Kovalsky, **Consortium Networks** (MD, AI Security CoE) | "60+ vendors, 15 one-pagers"; capability-based framework that decides configure/buy/build; live demo with agentic healthcare chatbot + PHI + Azure + CrowdStrike |
| Kinetic Risk: Securing & Governing Physical AI in the Wild | Padma Apparao, [[intel\|Intel]] | **Vision-Language-Action (VLA)** models in robotics/autonomy; sensor spoofing + embodied instruction manipulation; argues NIST AI RMF + EU AI Act fall short for non-deterministic embodied AI |
| [[trajectory-aware-post-training-talk\|Trajectory-Aware Post-Training of Open-Weight Models for Security Agents]] | [[aaron-brown\|Aaron Brown]] + Madhur Prashant, [[aws\|AWS]] | Open-source pipeline (env setup, data collection, **reward function design**, two-stage SFT→GRPO) on NVIDIA DGX Spark; releases configs, eval harness, **fine-tuned GLM-4.7 30B Flash weights on HuggingFace** |
| AI Found 12 Zero-Days in OpenSSL. What Does It Mean For The Industry? | Adam Krivka + Ondrej Vlcek, [[aisle\|AISLE]] | **OpenSSL Jan 2026 update: 12 vulns found and reported by AISLE's AI** (see [[aisle-openssl-12-of-12\|the AISLE 12-of-12 paper page]]); **3 hidden 20+ years** (incl. **CVE-2025-15467 CVSS 9.8** dating to 1998); hundreds more across curl, Linux kernel, wolfSSL |

## Day 2 — Stage 1 (Wednesday, March 4, 2026)

| Talk | Speaker(s) / Org | Notable data points |
|---|---|---|
| Opening Words | Gadi Evron, [[knostic\|Knostic]] | — |
| 200 Bugs/Week/Engineer: How We Rebuilt Trail of Bits Around AI | Dan Guido, **Trail of Bits** (CEO) | **200 bugs/week/engineer claim**; AI-native consulting "operating system" of incentives + defaults + guardrails + verification; internal/external skills repos; opinionated config baselines; sandboxing; pricing/staffing model changes |
| 8 Minutes to Admin. We Caught It in the Wild. Welcome to VibeHacking | [[sergej-epp\|Sergej Epp]], [[sysdig\|Sysdig]] (CISO) | **Two AI-assisted campaigns** — (1) **8-minute AWS escalation**, stolen creds → full admin; (2) **EtherRAT** — fileless Node.js implant using **Ethereum smart contracts for C2**; behavioral attribution methodology; launched the [[zero-day-clock\|Zero Day Clock]] here |
| macOS Vulnerability Research: Augmenting Apple's Source Code + OS Logs with AI Agents | Olivia Gallucci, [[datadog\|Datadog]] | AI for triage of open-source diffs, exploit-potential identification, fuzz-target prioritization on shared macOS/iOS open-source code |
| Promp2Pwn — LLMs Winning at Pwn2Own | Georgi G, **Interrupt Labs** (Director of Research) | Agentic AI bug-hunter for Pwn2Own; **found a vulnerability in Samsung Bixby** |
| [[breaking-the-lethal-trifecta-talk\|Breaking the Lethal Trifecta (Without Ruining Your Agents)]] | [[andrew-bullen\|Andrew Bullen]], [[stripe\|Stripe]] (Head of AI Security) | Stripe's containment architecture for [[lethal-trifecta\|Lethal Trifecta]]: [[smokescreen\|Smokescreen]] egress proxy + agent-tag CI gate; [[toolshed\|Toolshed]] central MCP + `ToolAnnotations`; queued/batched/optimistic confirmations; coins the [[lethal-bifecta\|Lethal Bifecta]] for sensitive writes |
| [[building-secure-agentic-systems-talk\|Building Secure Agentic Systems: Lessons from Daily-Driver Agents]] | [[brooks-mcmillin\|Brooks McMillin]], [[dropbox\|Dropbox]] (Infrastructure Cloud Security Engineer) | Per-agent MCP tool scoping (73-tool fleet); **[[agent-memory-isolation\|memory isolation by class-name namespace]]** (cross-agent leakage failure + fix); **[[context-aware-trimming\|context-aware security-event pinning]]** (N-token attack-hiding pattern); LLM firewall over-tuning lessons; delegation chains named as open gap |
| [[rethinking-security-agent-evaluation-talk\|Rethinking How We Evaluate Security Agents for Real-World Use]] | Mudita Khurana, [[airbnb\|Airbnb]] (Staff Security Engineer) | Capability-centric framework; the **find → confirm exploit → patch → validate** workflow; observability into planning, reasoning, tool-use, context |
| [[securing-workspace-genai-at-google-talk\|Securing Workspace GenAI at Google Speed: Surviving the Perfect Storm]] | [[nicolas-lidzborski\|Nicolas Lidzborski]], [[google\|Google]] (Principal Engineer, Workspace Security) | "**Perfect Storm**" = sensitive data + untrusted content + external command execution; **calendar invitation as agent-hijack vector**; defense-in-depth blueprint for Gemini + Workspace; introduces [[prompt-as-code\|Prompt as Code]], [[agency-gap\|Agency Gap]], [[orchestration-hijacking\|Orchestration Hijacking]], [[recursive-prompt-injection\|Recursive Prompt Injection (and Semantic Gaslighting)]], [[plan-validate-execute\|Plan-Validate-Execute Pattern]], [[sentinel-tokens\|Sentinel Tokens (Prompt Delimitation)]] |
| Operation Pale Fire: How We Red-Teamed Our Own AI Agent | Wes Ring + Josiah Peedikayil, [[block\|Block]] | Red-team of **goose** (Block's open-source AI agent) |
| Training BrowseSafe: Lessons from Detecting Prompt Injection in Production Browser Agents | Kyle Polley, [[perplexity\|Perplexity]] | **BrowseSafe** in production protecting browser agents; **fine-tuned MoE Qwen-30B**; **F1 ~0.91 at sub-100ms**; **BrowseSafe-Bench** w/ high-entropy realistic HTML; data flywheel from production feedback |
| [[ai-automation-boundary-threat-hunting-talk\|Exploring the AI Automation Boundary for Threat Hunting at Datadog]] | Arthi Nagarajan, [[datadog\|Datadog]] | Single agent → orchestrator-subagent system; hypothesis-driven query gen, iterative refinement, evidence narrowing |
| [[detection-deception-engineering-orbie-talk\|Detection & Deception Engineering in the Matrix]] | [[bob-rudis\|Bob Rudis]] + Glenn Thorpe, [[greynoise\|GreyNoise Labs]] | **Orbie** — agent over internet-scale honeypot data; emergent threats, campaigns, detection rules; "domain expert knowledge in tooling > model choice" |

## Day 2 — Stage 2 (Wednesday, March 4, 2026)

| Talk | Speaker(s) / Org | Notable data points |
|---|---|---|
| Total Recon: How We Discovered 1000s of Open Agents in the Wild | Avishai Efrat + Roey Ben Chaim, [[zenity\|Zenity]] | **1000s of exposed agents** (copilots, custom agents, AI middleware) reachable + enumerable + over-permissioned; releases **PowerPwn** recon tool |
| [[your-agent-works-for-me-now-talk\|Your Agent Works for Me Now]] | [[johann-rehberger\|Johann Rehberger]] (Red Team Director) | **[[promptware\|Promptware]]** = engineered prompts that act like malware; **[[delayed-tool-invocation\|delayed tool invocation]]** bypasses Google's Workspace tool deactivation control; **[[agent-commander-prompt-c2\|Agent Commander]]** prompt-level C2 with zero-click Gmail enrollment; previously undisclosed exploits against Gemini, Copilot, Xcode, OpenClaw, KimiCloud |
| [[capability-based-authorization-talk\|Capability-Based Authorization for AI Agents — Warrants That Survive Prompt Injection]] | [[niki-aimable-niyikiza\|Niki Aimable Niyikiza]], Founder @ [[tenuo\|Tenuo]] / SE @ [[snap\|Snap]] | **Cryptographic [[tenuo-warrant\|Tenuo Warrants]]** (Macaroons/UCAN/Biscuits/CaMeL lineage) — six properties: signed, scoped, ephemeral, holder-bound, verifiable offline, delegation-aware; **[[monotonic-attenuation\|monotonic attenuation]]** across hops freezes the blast radius; 4 deployment modes; ~55μs auth / ~200ns deny; 53/53 violations rejected on 5,700 fuzz probes; **baseline 90%→0% multi-agent ASR** on custom harness; live LangGraph demo with 4 enforcement scenarios |
| Injecting Security Context During Vibe Coding | Srajan Gupta, **Dave** (Sr Security Engineer) | **MCP server** that injects threat models + security requirements + OWASP guidance into the AI coding loop pre-generation; verifies output post-generation |
| Source to Sink: How to Improve LLM First-Party Vuln Discovery | Scott Behrens + Justice Cassel, [[netflix\|Netflix]] | "Mass-closed 200 AI-generated findings" therapy; **agentic pipeline that thinks before it screams** |
| The Parseltongue Protocol: A Deep Dive into 100+ Textual Obfuscation Methods | Joey Melo, [[crowdstrike\|CrowdStrike]] (AI Red Teaming Specialist) | **100+ encoding/encryption techniques × 9 leading AI models × 17,000+ malicious prompts**; safety-system gaps |
| Why Most ML Vulnerability Detection Fails (And What Actually Worked for Kernel Bugs) | Jenny Guanni Qu, **Pebblebed** (AI Researcher) | **125K Linux kernel commits**; "hard negatives" hurt; subsystem boundaries are where bugs hide; **average kernel security bug survives 2.1 years undetected** |
| [[1-8m-prompts-30-alerts-talk\|1.8M Prompts, 30 Alerts: Hunting Abuse in a User-Defined Agent Ecosystem]] | [[matt-rittinghouse\|Matt Rittinghouse]] + [[millie-rittinghouse\|Millie Rittinghouse]], [[salesforce\|Salesforce]] | **Agentforce defense at scale** — 12,000+ unique daily agents across 55,000 orgs, ~1.8M daily prompts; **three-level ensemble model** (user/agent/org); **<30 high-fidelity daily alerts**; [[prompt-volume-to-alert-ratio\|ratio ~60,000:1]]; 12–24 hr batch detection; roadmap to hot-path auto-containment |
| AI Security with Guarantees | Ilia Shumailov, AI Sequrity Company (CEO) | Security guarantees for modern AI agents incl. **computer use** |
| [[osint-to-knowledge-graph-talk\|From OSINT Chaos to Knowledge Graph: Production-Scale AI-Powered Threat Intel]] | [[dongdong-sun\|Dongdong Sun]], [[palo-alto-networks\|Palo Alto Networks]] | Production AI pipeline from millions of threat reports → queryable knowledge graph |
| [[beyond-the-chatbot-talk\|Beyond the Chatbot — Delivering an Agentic SOC for Real-World Defense]] | [[peter-smith\|Peter Smith]] + [[ravi-kiran-sharma\|Ravi Kiran Sharma]], [[salesforce\|Salesforce]] | **Polyphonic (Supervisor-Worker) architecture** — moves past monolithic black-box copilots |
| Are Your LLM's Safety Mechanisms Intact? Detecting Backdoors with White-Box Analysis | Akash Mukherje, **Realm Labs** (Co-Founder) | **White-box safety-signal analysis** — backdoors that selectively weaken refusal but pass black-box eval; refusal-correlated internal signals |

## Notable data points (incidents + capability evidence)

Representative capability evidence and incident metrics from the agenda:

- **AISLE** — 12 OpenSSL 0-days fixed Jan 2026, **3 hiding 20+ years**; hundreds more across curl / Linux kernel / wolfSSL.
- **TrendAI FENRIR** — 100+ vulns since mid-2025, **21 CVEs incl. multiple CVSS 9.8 RCEs**.
- **Sysdig** — **8-minute AWS escalation** caught in the wild; **EtherRAT** fileless Node.js with Ethereum smart-contract C2.
- **Adam Laurie / Alpitronic** — Claude glitched an LPC in **7 minutes** vs human 6 weeks.
- **Salesforce Agentforce** — 1.8M prompts/day reduced to **<30 high-fidelity alerts**.
- **CrowdStrike Parseltongue** — **17,000+ malicious prompts × 100+ obfuscation methods × 9 leading models**.
- **Pebblebed** — **avg kernel security bug undetected for 2.1 years**; 125K-commit training set.
- **Perplexity BrowseSafe** — F1 ~0.91 at <100ms latency, fine-tuned Qwen-30B MoE.
- **PAN SYARA** — 98% detection at **<100ms / \$0.001/query**.
- **Mindgard** — exploit catalog across **Codex, Kiro, Antigravity, Cursor**.
- **Zenity** — **1000s of exposed agents** in the public internet; PowerPwn recon tool.
- **SANS / Rob T. Lee** cites **Anthropic GTG-1002** — adversaries running Claude Code at **80–90% autonomous**.
- **Carlini / Anthropic** — recent SOTA models finding 0-days in extensively-tested software.
- **Trail of Bits** — claims **200 bugs/week/engineer** in their AI-native operating model.

## Recurring patterns across the agenda

- **Production agent containment is now the practitioner topic.** Stripe (×2), Google Workspace, Block, Perplexity, Dropbox, Salesforce (×2), Airbnb, and Snap all present production-agent defenses, not lab work.
- **Agentic vuln discovery is a category.** AISLE, TrendAI FENRIR, XBOW, Interrupt Labs, NVIDIA Marinade, Datadog/macOS, Pebblebed, Netflix, Carlini/Anthropic — all describe AI agents as production vuln finders.
- **Reference-monitor-by-policy is a concrete pattern.** Sondera (Cedar), Snap (capability warrants / UCAN), Dave (MCP context injection), Stripe (CI-time tool annotations), Trail of Bits (sandboxing) — converging on policy-evaluated tool calls.
- **Observability is glass-box, not black-box.** Starseer (mechanistic interp), Realm Labs (white-box backdoor analysis), Elastic (OTel for GenAI), Salesforce (behavioral baselines), GreyNoise (Orbie) — model internals + behavioral telemetry > output filters.
- **Browser + IDE agents are the new attack surface.** Perplexity BrowseSafe, Mindgard (Codex/Kiro/Antigravity/Cursor), Stripe (lethal trifecta), Google Workspace (calendar injection), Johann Rehberger (Gemini/Copilot promptware).
- **Governance talks are operational, not aspirational.** Snowflake, FFF Enterprises, US Army, Trail of Bits — describe the failure modes of governance, not the framework checklists.

## Cross-references in this wiki

- Architecture & CMM: [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] · [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] · [[agentic-ai-security-cmm-crosswalk|Agentic AI Security CMM — Standards Crosswalk Matrix]]
- Concepts called out by name: [[lethal-trifecta|Lethal Trifecta]] (Stripe Bullen) · [[indirect-prompt-injection|Indirect Prompt Injection]] (TrendAI Park, Google Workspace) · [[mcp-security|MCP Security]] (SANS, Dave) · [[non-human-identity|Non-Human Identity (NHI)]] (Snap warrants, Stripe) · [[guardian-agent|Guardian Agent]] · [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] · [[agent-observability|Agent Observability]] (Starseer, Realm, Elastic, Salesforce) · [[shadow-automation|Shadow Automation]] (Hasbrouck) · [[supply-chain-security-for-agents|Supply Chain Security for Agentic AI]] (Wiz Shai-Hulud, Mindgard, AISLE)
- Practices that map to talks: [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]] · [[rag-hardening|RAG Hardening]] · [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]] · [[agent-sandboxing|Agent Sandboxing]]

## Related pages in this wiki

- Architecture & CMM: [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] · [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]
- Defender-side synthesis: [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] organizes the defender talks above by SOC function (triage, detection engineering, threat hunting, response action, and evaluation).
