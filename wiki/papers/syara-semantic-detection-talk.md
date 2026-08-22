---
type: talk
title: "SYARA: Semantic Rules for GenAI Threats"
address: c-000152
created: 2026-05-26
updated: 2026-06-02
tags:
  - papers
  - talks
  - detection-engineering
  - prompt-injection
  - yara
  - sec-of-ai
  - ai-in-sec-defense
status: summarized
scope_axis: [sec-of-ai, ai-in-sec-defense]
year: 2026
authors: ["Mohamed Nabeel"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 3, 2026"
key_claim: "SYARA keeps YARA's rule syntax and adds four matcher constructs — string, semantic similarity, ML classifier, and LLM — executed cheapest-first, so detection captures attacker intent across paraphrases while a cheap pre-filter gates the expensive LLM and cuts cost by about 99% at scale."
methodology: "Practitioner talk on the open-source SYARA project (MIT; pip install syara) by a Palo Alto Networks researcher. Transcript and slide deck are now captured to .raw/talks/; the benchmark, latency, and cost figures are the speaker's reported results over the project's own test sets."
source_url: "https://github.com/nabeelxy/syara"
related:
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[detection-deception-engineering-orbie-talk|Detection & Deception Engineering (Orbie)]]"
  - "[[genai-endpoint-observability-talk|GenAI Endpoint Observability]]"
  - "[[osint-to-knowledge-graph-talk|OSINT to Knowledge Graph]]"
  - "[[indirect-prompt-injection|Indirect Prompt Injection]]"
  - "[[prompt-injection|Prompt Injection]]"
  - "[[llm-as-a-judge|LLM-as-a-Judge]]"
  - "[[unprompted-conference-march-2026|Unprompted March 2026]]"
  - "[[mohamed-nabeel|Mohamed Nabeel]]"
  - "[[palo-alto-networks|Palo Alto Networks]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - "https://github.com/nabeelxy/syara"
  - "https://unpromptedcon.org/#"
  - ".raw/talks/2026-03-03_Mohamed-Nabeel_Detecting-GenAI-Threats-at-Scale_transcript.md"
  - ".raw/talks/2026-03-03_Mohamed-Nabeel_Detecting-GenAI-Threats-at-Scale_slides.pdf"
---

# Detecting GenAI Threats at Scale with YARA-Like Semantic Rules (SYARA)

**Source:** [SYARA on GitHub](https://github.com/nabeelxy/syara) · [syara.org](https://syara.org) · [Conference agenda](https://unpromptedcon.org/#)

A practitioner talk by [[mohamed-nabeel|Mohamed Nabeel]] ([[palo-alto-networks|Palo Alto Networks]]) at the [[unprompted-conference-march-2026|Unprompted Conference (March 2026)]] on **SYARA**, an open-source detection engine for GenAI threats. SYARA borrows YARA's rule syntax to keep the learning curve low, then adds matcher constructs that score meaning rather than literal bytes.

## The Argument

YARA is the gold standard for detecting malware binaries and scripts. Palo Alto Networks detects millions of samples a day with YARA rules. That model does not transfer to GenAI threats, because **the new binary is natural language**: the payload is no longer a PE or a script but a sentence, and the same intent appears in many surface forms. YARA cannot match on intent, so it cannot detect prompt injection, jailbreaks, phishing, or brand impersonation at the level where they actually vary.

SYARA targets that gap. It keeps YARA's familiar rule structure — named patterns plus a boolean `condition` — and replaces the single string matcher with a choice of four constructs, listed here in increasing order of cost and detection efficacy.

| Construct | What it matches | Relative cost |
|---|---|---|
| `string` | Literal substrings, as in classic YARA | Lowest |
| `similarity` | Semantic embedding distance to a natural-language phrase | Low |
| `classifier` | A binary or multi-class ML model's verdict | Medium |
| `llm` | A model prompted to judge intent | Highest |

The `similarity` construct is the load-bearing addition. A rule states what to catch in natural language — for example, *don't execute previous rules* — and the engine matches paraphrases by embedding distance, so an analyst no longer has to enumerate every variant of an injection string. The `classifier` construct accepts any binary or multi-class model: a Hugging Face phishing detector dropped in as-is, or a fine-tuned [DeBERTa](https://huggingface.co/microsoft/deberta-v3-base) model. The `llm` construct runs an analyst-written prompt against any hosted Gemini or OpenAI model, or a locally hosted model served through [Ollama](https://ollama.com/), provided the model is wrapped to meet a fixed interface contract.

## Threat Surface

The number-one GenAI threat is [[prompt-injection|prompt injection]]. In the web space it arrives through query parameters, through URL hashes and fragments, or dynamically detonated in the browser, where the browser itself calls the LLM and runs obfuscated JavaScript. The browser-detonated path is invisible to many network solutions, because the malicious instruction never crosses the wire as a recognizable string — it is assembled and executed client-side. This is the [[indirect-prompt-injection|indirect prompt injection]] case, scaled to the open web.

Two emerging threats round out the surface. Brand impersonation has never been easier to produce with generative tooling. As agents begin communicating with each other, agent-to-agent impersonation — one agent posing as another — becomes the next variant of the same problem.

## Execution Order and Defense in Depth

The engine is optimized to **execute the cheapest rule first**. For a disjunctive (`OR`) condition, a match on the cheap layer short-circuits the rest; a conjunctive (`AND`) condition runs every clause. The recommended practice mirrors security's defense-in-depth posture: combine constructs so the cheap layers resolve most inputs and the expensive ones handle only the residual, balancing efficacy against cost and latency.

**ClickFix layered rule.** A three-condition rule chains `string` → `similarity` → fine-tuned `classifier` (DeBERTa). On the project's test set, the simple string layer catches about 50% of the threats; the remaining roughly 45–48% are caught by the more expensive similarity and classifier layers. A pure string rule would miss half the threats; a classifier-only rule would catch most but cost far more per input. Layering recovers nearly the full detection rate while cutting cost by about half against the classifier-only baseline. ClickFix is not GenAI-specific, but it has acquired social-engineering variants, including campaigns that use it to deliver agent "skills" to a victim's agent.

## The Pre-Filtering Pattern

The headline result is the **pre-filter pattern**: pair a cheap construct (string or classifier) with an expensive LLM, so the LLM only sees inputs the pre-filter could not clear. A classifier alone is high-recall but false-positive-prone, which makes it unacceptable as the sole verdict but well-suited to gating — a few false positives are tolerable when the LLM makes the final call.

A brand-impersonation rule demonstrates the pattern with two conditions: a classifier pre-filter built from an off-the-shelf Hugging Face phishing detector, followed by an LLM verdict. Measured over 10,000 URLs with [Gemini 2.5 Pro](https://deepmind.google/technologies/gemini/):

| Metric | LLM on every input | With pre-filter |
|---|---|---|
| Cost (10K URLs) | `\$750` | `\$13.5` |
| Wall-clock runtime | Hours | Minutes |
| Per-call latency | ~4.5 s (LLM) | ~ms (classifier) |

The pre-filter cut cost by **about 99%**. At Palo Alto Networks' production scale of millions of URLs a day, an unconditioned LLM call is a bottleneck; the pre-filter makes LLM-grade detection affordable on that volume.

## Architecture

SYARA follows a pluggable factory-pattern design. Two component types sit in front of the matchers and are swappable without touching the rule:

- **Cleaners** strip HTML decoration and boilerplate down to the substantive text, so a classifier or embedding model is not fed page fluff.
- **Chunkers** split input into sentences, paragraphs, or overlapping windows. Matching a whole document against one phrase scores low, so chunking is needed to surface a local match; the rule references only the chunk strategy used, and the engine handles the rest. As in YARA, the matcher walks all chunks and returns the best match, or every match when several hit.

The LLM, classifier, and similarity backends are interchangeable behind their interface contracts, so a deployment can start with the most capable model to confirm a rule works, then step down to the cheapest model that still passes — lowering cost at scale.

## Operational Practice

SYARA is open source under MIT, installable with `pip install syara`, with documentation and demos at [syara.org](https://syara.org).[^repo] Rule rollout follows the same discipline as a YARA rule: run a new rule in shadow production for about one week before enabling it in production, because the live feed always carries inputs the test set did not. For classifier constructs, the decision threshold is tuned in CI/CD against known-benign and known-malicious ground truth, so a rule is validated before it spends LLM budget in production. The cost, latency, and coverage figures in this summary are the speaker's, drawn from the slide deck and transcript.[^deck]

## Placement

This is evidence for the detection-engineering capability in the [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] thesis. It sits next to GreyNoise's [[detection-deception-engineering-orbie-talk|Orbie]] as the detection-engineering cluster of the Unprompted agenda, and downstream of [[genai-endpoint-observability-talk|GenAI Endpoint Observability]], which supplies the LLM-call telemetry that detection rules read. It shares a Palo Alto Networks lineage with [[osint-to-knowledge-graph-talk|OSINT to Knowledge Graph]]. The `llm` construct is a contained, cost-gated form of [[llm-as-a-judge|LLM-as-a-Judge]]: the same frontier-model judgment, reserved for the inputs a cheaper layer cannot resolve. The structural claim is the talk's core point — a pure-LLM gate is the wrong shape, because it spends a frontier-model call on inputs a regex would catch.

> [!gap] Open questions
> The talk reports figures over the project's own test sets (the ClickFix and brand-impersonation examples) rather than an independent benchmark. External reproduction of the ~99% cost reduction and the 50% string-layer coverage on unrelated corpora is not yet available. The cleaner and chunker libraries shipped out of the box were not enumerated in the talk.

## Notes

[^repo]: SYARA project repository (README, matcher documentation, examples): [github.com/nabeelxy/syara](https://github.com/nabeelxy/syara). MIT-licensed; installable via `pip install syara`.
[^deck]: Conference slide deck and transcript captured to `.raw/talks/` (2026-03-03). The deck carries the layer-by-layer benchmark tables and the pre-filter cost comparison; the transcript states the `\$750` → `\$13.5`, ~4.5 s LLM vs. millisecond classifier, and ~99% cost-reduction figures directly.
