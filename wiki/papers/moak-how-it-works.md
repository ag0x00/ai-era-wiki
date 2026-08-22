---
type: paper
title: "MOAK: Autonomous AI Exploit Pipeline"
address: c-000085
created: 2026-05-19
updated: 2026-05-29
tags:
  - papers
  - offensive-ai
  - exploit-generation
  - autonomous-exploitation
  - kev
  - agentic-ai
  - benchmark
status: summarized
year: 2026
authors: ["Yuval Zadok", "Karen Katz", "Itay Chechik"]
venue: "MOAK blog"
source_url: "https://moak.ai/"
archived_copy: ".raw/articles/moak-how-it-works-2026-04-09.md"
key_claim: "A five-agent agentic pipeline (Collector → Researcher → Builder → Exploiter → Judge) autonomously exploits 174 of 178 CISA KEVs (97.8%) using only a CVE number as input, including KEVs disclosed after the models' knowledge cutoffs. Claude Opus 4.6 achieves 98% autonomous exploitation rate on the post-cutoff KEV benchmark."
methodology: "Agentic pipeline tested against containerizable KEVs. Benchmark: KEVs published after all tested models' knowledge cutoffs (Sep '25 baseline). Models evaluated: DeepSeek V3.2, GPT 5, Claude Sonnet 4.5, Gemini 3.1 Pro, GPT 5.4, Claude Opus 4.6. Validation via Docker CTF: vulnerable + patched environment pair, secret flag capture, Judge agent verifies no workaround used."
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
related:
  - "[[moak|MOAK]]"
  - "[[autonomous-exploit-generation|Autonomous Exploit Generation]]"
  - "[[zero-day-clock|Zero Day Clock]]"
  - "[[vulnops|VulnOps]]"
  - "[[xbow-mythos-evaluation|XBOW × Mythos Evaluation]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[offensive-ai-state-of-the-field|Offensive AI: State of the Field]]"
aliases:
  - papers/moak-how-it-works-2026-04-09
  - moak-how-it-works-2026-04-09

---

# How Does MOAK Work? — Autonomous AI Exploit Pipeline

**Source:** [MOAK blog, April 9, 2026](https://moak.ai/). Authors: Yuval Zadok, Karen Katz, Itay Chechik. Local copy: `.raw/articles/moak-how-it-works-2026-04-09.md`.

## Key Claim

A five-agent autonomous pipeline converts a bare CVE number into a working, validated exploit — simulating the full manual exploitation process — with a **97.8% success rate across 178 CISA KEVs**, including CVEs disclosed after the models' training cutoffs. [[moak|MOAK]] frames itself as a defensive research instrument and first-of-its-kind real-time exploitability predictor.

**98% exploitation rate with Claude Opus 4.6.** On the post-knowledge-cutoff KEV benchmark (KEVs published after Sep '25), Claude Opus 4.6 achieves **98% autonomous exploitation rate** — a 4.5× jump over Claude Sonnet 4.5 (22%) within the same model family. This is the most concrete published benchmark for autonomous CVE exploitation.

## The Five-Agent Pipeline

### 1. Collector Agent
Accepts a single CVE number as input. Gathers CVE description, vulnerable and patched source code. **Architectural guardrail**: for new-CVE simulation, restricted to sources that exclude POCs or exploitation details — claimed to be the first workflow with this constraint for true new-CVE simulation.

### 2. Researcher Agent
Distills exploitation primitives from CVE description and patch diffs. Builds a **primitive-chain graph** (directed mind map of exploitation paths). Spawns a **multi-model sub-agent swarm** (Claude + GPT + Gemini) with four rotating roles:

| Role | Function |
|---|---|
| **Prioritizer** | Scores active leads by chain relevance |
| **Lead Researchers** | Each takes a top-scored lead; scoped to their lead + predecessors only |
| **Contrarian** | Assumes all existing leads are wrong; finds alternative path from scratch |
| **Verifier** | Statically verifies whether any lead has completed a full exploitation chain |

Agents **rotate roles between runs** — role diversification designed to reduce groupthink and prevent single-model bias from anchoring the research. This is a domain application of [[adversarial-reflexion|adversarial-reflexion]]-style independent checking.

### 3. Environment Builder Agent
Provisions a Docker environment matching vulnerability requirements (including auth if needed). Inserts a **secret flag** — location varies by vulnerability type (file flag for read-file vulns, DB record for SQL injection, etc.). Produces two identical environments: one vulnerable, one patched. Both passed to the Exploiter.

### 4. Exploiter Agent
Builds an exploit program from the Researcher's recipe. Runs it against the vulnerable environment in a feedback loop — error logs returned on failure — until the flag is captured and the exploit verifiably fails against the patched environment.

### 5. Judge Agent
Verifies three properties: exploit is genuine (uses the actual vulnerability, not a workaround), environment is realistic, no external exploitation information leaked in. **Specific failure mode caught**: Builder sometimes created high-privilege access paths (e.g., admin web credentials) that the Exploiter used instead of the real vulnerability. Judge supervises both Builder and Exploiter to prevent this class of shortcut. Produces a final **exploitability score** and structured judgment report.

## Model Benchmark

Benchmark: CISA KEVs published after all models' knowledge cutoffs (Sep '25 baseline). Metric: **autonomous exploitation rate**.

| Model | Cutoff | Exploitation Rate |
|---|---|---|
| DeepSeek V3.2 | Dec '25 | 15% |
| GPT 5 | Aug '25 | 18% |
| Claude Sonnet 4.5 | Sep '25 | 22% |
| Gemini 3.1 Pro | Feb '26 | 76% |
| GPT 5.4 | Mar '26 | 90% |
| **Claude Opus 4.6** | Feb '26 | **98%** |

The inflection between the Sep '25 cohort (15–22%) and the Feb '26+ cohort (76–98%) is dramatic: a **3–6× jump** attributable to frontier-model capability gains over roughly five months.

MoA (Mixture of Agents — role switching in the research process) provides additional improvement beyond the base model rate.

## Methodology and Results

- **178 KEVs tested; 174 exploited** (97.8% overall success rate).
- **Same success rate on post-knowledge-cutoff KEVs** — the guardrails holding new-CVE simulation to no-POC sources validated.
- Primary test languages: Python, JavaScript, Java, PHP. Native C application CVEs tested with similar results.
- Acknowledged scope limits: complex closed-source frameworks (e.g., Firefox), closed-source application CVEs — expected to require more HITL or specialized tooling.
- **Live production use**: workflow currently runs on newly disclosed CVEs, providing real-time exploitability assessment within hours. Live dashboard available; practitioners can request assessments for specific CVEs.

## Relationship to Existing Wiki Concepts

- **[[zero-day-clock|Zero Day Clock]]** — MOAK provides the mechanistic explanation for *how* TTE collapses to hours: autonomous pipeline, hours of compute, no human bottleneck. The clock documents *that* the median TTE has collapsed to zero-day by 2025; MOAK explains *why*.
- **[[autonomous-exploit-generation|Autonomous Exploit Generation]]** — MOAK is the most fully documented production instance of this pattern. See that concept page for the broader landscape.
- **[[vulnops|VulnOps]]** — MOAK's real-time exploitability predictor is the attacker-side forcing function that makes continuous VulnOps (as opposed to periodic patch management) structurally necessary.
- **[[adversarial-reflexion|Adversarial Reflexion]]** — the Contrarian role and role-rotation pattern in the Researcher swarm share the same epistemic motivation: independent falsification to eliminate sycophancy/groupthink.

## Safety Position

MOAK explicitly frames the system as defensive: controlled isolated environments, no published artifacts, no open-source release. Academic/research access available on request.

> [!gap] Three additional blog posts noted but not yet ingested
> "Mother of All KEVs" (how the workflow was built), "Test Case: React2Shell" (21-minute exploit of React2Shell), and "Wake Up Call" (machine-speed defenders framing) are referenced on the site but content not available via fetch. Direct ingestion from the user's browser would add significant detail.
