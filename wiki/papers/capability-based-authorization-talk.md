---
type: talk
title: "Capability-Based Authorization for AI Agents"
created: 2026-05-03
updated: 2026-05-03
tags:
  - papers
  - talks
  - capability-based-authorization
  - warrants
  - delegation
  - multi-agent-security
  - prompt-injection-containment
  - macaroons
  - ucan
  - cedar
  - camel
  - tenuo
  - unprompted-2026
status: summarized
year: 2026
authors: ["Niki Aimable Niyikiza"]
venue: "Unprompted Conference, San Francisco — Stage 2 Lecture 03, Wednesday March 4, 2026, 10:00"
license: "Conference talk; slides via attendee Google Drive share; transcript whisper-extracted from same"
key_claim: "Identity-based authorization gives agents *ambient* authority — full credentials at deploy time, hope they only use what they need at run time. This is structurally wrong for multi-agent flows because it has no language for **delegation**: when Agent A hands work to Agent B, no existing primitive expresses 'B inherited a narrowed slice of A's authority for this specific task.' The fix is **capability-based authorization** — task-scoped, signed, ephemeral, holder-bound, offline-verifiable, delegation-aware **warrants** that **monotonically attenuate** across hops. Warrants are not anti-prompt-injection — they constrain agents *even if prompt-injected*. Tenuo's reference implementation (Rust core + Python bindings, open source, 4 deployment models) drives a multi-agent-delegation attack-success rate from 90% to 0% on a custom benchmark."
methodology: "Practitioner talk by Tenuo's founder. Frames the problem as 'AI confused deputy under ambient authority,' surveys 60 years of capabilities prior art (Dennis & Van Horn 1966 → Google Macaroons 2014 → UCAN/Biscuits late 2010s → DeepMind CaMeL 2025 → DeepMind 'Intentional AI Design' Feb 2026), specifies a 6-property warrant primitive, gives a worked SOC-triage demo with 4 enforcement scenarios, and reports security + performance harness numbers. Closes with a 'map is not the territory' lesson on why constraint *design* (path normalization, URL canonicalization) matters more than the cryptography."
contradicts: []
supports:
  - "[[capability-based-authorization]]"
  - "[[tenuo-warrant]]"
  - "[[monotonic-attenuation]]"
  - "[[ambient-vs-derived-authority]]"
  - "[[least-agency-principle]]"
  - "[[oversight-layer]]"
  - "[[prompt-injection-containment]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
related:
  - "[[niki-aimable-niyikiza]]"
  - "[[tenuo]]"
  - "[[snap]]"
  - "[[lethal-trifecta]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[pdp-pep-for-non-tool-mediated-actions]]"
  - "[[unprompted-conference-march-2026]]"
sources:
  - .raw/talks/2026-03-04_Niki-Aimable-Niyikiza_Capability-Based-Authorization_slides.pdf
  - .raw/talks/2026-03-04_Niki-Aimable-Niyikiza_Capability-Based-Authorization_transcript.md
source_url: https://drive.google.com/file/d/17PGo15fAidNQxdpkhyHH0ZtxehVjduNb/view
aliases:
  - papers/capability-based-authorization-niyikiza-talk
  - capability-based-authorization-niyikiza-talk

---

# Capability-Based Authorization for AI Agents — Warrants That Survive Prompt Injection

> **Speaker:** [[niki-aimable-niyikiza|Niki Aimable Niyikiza]] — Founder @ [[tenuo|Tenuo]] / Security Engineering @ [[snap|Snap]]; previously infrastructure security at Google, Datadog, Snapchat (~10 years).
> **Talk:** Unprompted Conference, San Francisco, **March 4, 2026, 10:00 — Stage 2 Lecture 03**.
> **Materials:** Slides PDF (12 pages) + audio-transcribed transcript.
> **Companion:** [[breaking-the-lethal-trifecta-talk|Bullen — Breaking the Lethal Trifecta]] runs the same day, 11:20, Stage 1 — addresses the same containment problem from the **egress-and-tool-annotation** angle. Niyikiza addresses it from the **delegation-aware-capability** angle. The two answers are complementary.

## The structural problem

Agents are not microservices. Microservices are deterministic — fixed identity, predictable call graph, authority inferable from identity. Agents reason at run time, adapt to inputs, and can spawn sub-agents in ways that were unknowable at deploy time. Untrusted input and non-deterministic reasoning are *core to the architecture*, not edge cases.

Every major agent framework today ships **identity-based authorization**: at deploy time the agent gets credentials sized to the broadest task it might do, and at run time those credentials are ambient — used for whatever the agent decides to do, whether the action was *intended, hallucinated, or injected*. The slide phrases the failure crisply:

> Identity-based auth treats all three the same.

This is the **AI confused deputy**: the agent has more authority than the current task needs, the task isn't expressed anywhere the system can check, so the system has no basis to refuse. Niyikiza's frame: the architectural fundamentals of secure multi-agent flows are **derived authority** + **delegation**, but the infrastructure we're deploying onto was built around ambient authority. The two don't compose.

### The valet-key analogy (slide 3)

| Option A — The Master Key | Option B — The Valet Key |
|---|---|
| Check the badge. Hand over your full key. | Hand over a key that starts one car, caps speed, disables the trunk and glove box, and dies in 2 hours. |
| It starts the engine, opens the trunk + glove box, works until the shift ends. Hope the valet only uses what they need. | You don't need to trust the valet. The key is the policy. |
| **Ambient authority. Identity determines access. The key doesn't know what the task is.** | **Derived authority. Task determines access. Each handoff can only narrow what was given.** |

The talk's load-bearing claim: every major agent framework ships Option A. Agents deserve a valet key.

## Omissions in "the stack everyone builds"

| Layer | What it answers | When |
|---|---|---|
| Identity | "Who is this agent?" | At connection |
| Policy engines | "Is this role allowed to do this?" | At decision point |
| Model guardrails | "Does this output look safe?" | At content layer |
| Sandbox | "Is this runtime isolated?" | At execution |

The hole: **delegation-aware authorization at execution time**. When Agent A delegates to Agent B, every layer above can verify B's *identity*, but **none can verify that B's authority came from A, that the scope narrowed at the hop, or that the task justified the access**. There is no primitive in the stack that encodes the delegation chain.

## The Warrant primitive (slide 6)

A **Tenuo Warrant** is a capability artifact with six declared properties:

| Property | Meaning |
|---|---|
| **Signed** | Issuer's cryptographic signature. Cannot be forged or modified. |
| **Scoped** | Specific tool, specific action, explicit constraints (paths, hosts, args). |
| **Ephemeral** | Short TTL. Expires with the task. |
| **Holder-bound** | Tied to the holder's key — proof-of-possession. A stolen warrant without the private key is useless. |
| **Verifiable offline** | Local verification by any verifier; no central authorization service round-trip. |
| **Delegation-aware** | Embeds the full delegation chain from the original minter to every agent in between. |

Slide-7 mental model: "**corporate expense card** with spend limits, approved vendors, auto-expiring." Transcript variant: corporate-Amex-vs-task-specific-debit-card for the intern's business trip.

**Warrants are not authentication.** The warrant artifact is **not a secret**. It can travel in HTTP headers; theft is by design not a compromise. The verifier checks (a) the constraints against the requested action and (b) that the request is signed by the holder's private key. The cryptographic envelope does the auth; the warrant body does the authz.

See [[tenuo-warrant|Tenuo Warrant]] for the full primitive.

## Subtractive delegation + monotonic attenuation (slide 7)

The mathematical invariant of the protocol is

```
W2 ⊆ W1 ⊆ W0
```

— each child agent's warrant is a *subset* of its parent's warrant. Capabilities, constraints, and TTL **can only shrink** at every delegation hop. The slide's framing:

> Even if a sub-agent is fully compromised, it cannot exceed what it was granted. **The blast radius is frozen.**

Two side-effects fall out of the design:

1. **Cryptographic provenance.** The chain is embedded in the artifact. Verification is local — no central auth-z server in the request path.
2. **Tamper-evident audit logs.** Every action produces a cryptographic receipt; receipts can be sent to a control plane and replayed offline. *"You don't have to trust anybody in between, because you have the cryptographic receipts of the action that took place and why it was authorized."*

See [[monotonic-attenuation|Monotonic Attenuation]].

## The demo: SOC incident-response pipeline (slide 8)

```
SOC Lead ──W0──▶ Triage Agent ──W1≤W0──▶ Investigation Agent ──W2≤W1──▶ Tool/API
                                                                          (read_logs)
```

`W2` is scoped specifically to `read_logs()` on the auth service. Four enforcement scenarios:

| # | Scenario | Outcome |
|---|---|---|
| 1 | **Happy path** — investigation agent reads logs within scope | Chain verified; allowed. |
| 2 | **Poisoned logs** — log content includes a prompt injection telling the investigation agent to read `vault/credentials/<path>` | Execution blocked. The vault path is not in `W2`. Constraint violation deterministically denied at the verifier. |
| 3 | **Exfiltration** — injected instruction tries to POST data to `evil-attacker.com` | Execution blocked. `send_http` capability was *stripped from W2 at delegation*. Capability denied. |
| 4 | **Impersonation** — attacker steals the warrant and replays it from another agent | Holder-binding rejected. Proof-of-possession check fails because the attacker doesn't have the holder's private key. |

The demo runs on **two real LangGraph agents**; the integration is one line of code (`tenuo_tool_node` replaces the framework's `tool_node`). Tenuo Core is in Rust with Python bindings. Slide-12 GitHub: `https://github.com/tenuo-ai/tenuo/`.

## Deployment models (slide 9)

The same warrant protocol can be enforced at four points in an architecture:

| Mode | Where it sits | Best for |
|---|---|---|
| **In-process** | Interceptors / middleware / drop-in nodes inside the framework | LangGraph, CrewAI, Temporal — fast iteration |
| **Sidecar** | Separate process, same host | Polyglot stacks; framework-agnostic |
| **Gateway** | Proxy between agents and tools | Fleet-wide enforcement |
| **MCP proxy** | Inside the MCP tool server, client-side, server-side, or both | Any MCP-compatible agent (this is the easiest because MCP already has structured tool + arg definitions) |

Transcript names additional in-process integrations: A2A protocol, Temporal, "9 frameworks supported." Slide names three: LangGraph, CrewAI, Temporal.

## Validation results (slide 10)

| Harness | Number | Notes |
|---|---|---|
| End-to-end authorization (constraints + PoP) | **~55 μs** | Rust core, CBOR encoding, Mac M3 Max |
| Denials | **~200 ns** | Fast-path for the common case |
| Constraint + cryptographic-integrity violations rejected | **53/53** | 100% on the in-house violation set |
| Adversarial fuzzing probes | **5,700** | "0 bypasses" |
| Multi-agent-delegation baseline ASR | **90% → 0%** | Under task-scoped delegation, with **zero impact on task completion** |

Caveat the speaker raises explicitly: there is **no public benchmark** for delegation-aware authorization at multi-agent scale. The 90%→0% number is on Tenuo's own custom harness — credible signal, not yet third-party-replicated. Open call to the research community to build a shared benchmark.

## "The map is not the territory" — the constraint-design lesson (slide 11)

The hardest part of building a real warrant system is not the cryptography. It's **making the constraint language match the runtime environment**. A regex that matches `/data/foo` on paper does not stop `"/data/../etc/passwd"` from resolving to `/etc/passwd` on the kernel. Three layers:

| Layer | Name | What lives here | Example |
|---|---|---|---|
| 1 | **Map** | Logical constraint (regex, string glob) | `path matches /data/.*` |
| 1.5 | **Annotated map** | Constraint + normalization (Subpath, UrlSafe) | Apply path normalization *before* the check |
| 2 | **Territory** | Execution guard (path_jail, url_jail, OS sandbox) | Kernel-level enforcement at the actual syscall |

The slide reports a **constraint design fix improving rejection from 87.5% to 100%** on a real harness — the fix wasn't cryptography, it was design. Slide cites four CVEs as evidence the failure mode is real and current:

- **CVE-2024-3571** — LangChain
- **CVE-2025-3046** — LlamaIndex
- **CVE-2025-61784** — LlamaFactory
- **CVE-2025-66032** — Claude Code allowlist bypasses

Concrete bypasses: `"/data/../etc/passwd"` → `/etc/passwd` (path traversal); `"http://2852039166/"` → `169.254.169.254` (decimal-encoded IP for AWS instance metadata); `"http://127.0.0.1\@evil.com"` → `evil.com` (URL userinfo trick). Reference blog post on Niyikiza's site: `niyikiza.com/posts/map-territory`.

## Boundaries of capability-based authorization

Niyikiza was explicit:

> "Tenuo is not trying to solve prompt injection. We're trying to constrain the agent at the execution time, **even if it is prompt-injected**. We are not trying to be, like, there's not gonna be prompt injection, we're not trying to say there's not gonna be hallucination, but we are making sure that **the authorization is frozen, even if your agent is prompt-injected**."

This frames warrants as a **runtime containment** primitive, not a model-layer defense. The same posture as [[breaking-the-lethal-trifecta-talk|Bullen's containment thesis]] — assume prompt injection will happen; constrain the blast radius architecturally.

## Prior art surveyed (transcript-only)

| Era | Work | Contribution to Tenuo's design |
|---|---|---|
| 1966 | Dennis & Van Horn | Original capability formulation; addresses confused deputy |
| 2014 | **Google Macaroons** | Tokens with **caveats** — attenuation primitive |
| Late 2010s | **UCAN** (User-Controlled Authorization Networks), **Biscuits** | Cryptographic capability tokens; decentralized auth |
| March 2025 | **Google DeepMind, "Defeating Prompt Injection by Design" (CaMeL)** | First time capability-based authorization was named as the model that addresses prompt injection. P-Q (Privileged + Quarantined) split. |
| Feb 2026 | **DeepMind, "Intentional AI Design"** *(transcript title-best-effort; needs verification)* | Reiterates capability systems as the proper auth framework for multi-agent flows. |

The Tenuo Warrant is positioned as **the productization** of this 60-year lineage, applied to the reality of agent frameworks running in real enterprise infrastructure.

## Q&A highlights (transcript-only)

1. **How is the scope of the child warrant determined at runtime?**
   *"The policy is gonna be deterministic, that's the goal."* Two run-time options:
   - **Orchestrator-driven** (typical): the orchestrator agent decides which task to delegate; the warrant for the child is minted to that task scope. Top-warrant scope is the absolute ceiling — sub-agents can never exceed it.
   - **Approval-gated**: certain actions require an external key-holder (real human) to approve at runtime. Maps to enterprise SSO (Okta / Google ID) without losing cryptographic purity.

2. **Warrants aren't secrets — how do you authenticate the agent then?**
   The verifier does *two* checks: (a) authorization — constraints versus the requested action; (b) cryptographic integrity — signature chain back to the root key + proof-of-possession by the holder. The warrant body is public; the envelope is what auth-n's the request.

3. **How do warrants map to OAuth / AWS IAM / domain-specific perms?**
   Tenuo sits **on top of** existing IAM. The constraint language supports basic logic, regex, and CEL (Common Expression Language) for arbitrary expressivity. *"Before you actually hit your IAM on GCP or your IAM on AWS, you check that the API call is in the warrant."* The warrant either deny-fails before workload-identity is consulted, or passes through and lets the cloud IAM do its existing job.

4. **(Audience)** — *"This feels like a finite-state automaton wrapping the AI's actions."* Speaker affirmed; that's exactly the framing. *"There's too many horror stories out there… this seems like one of the first steps towards actually creating that boundary."*

## Slide-only vs transcript-only — what each input contributed

| Slide-only (canonical) | Transcript-only |
|---|---|
| Speaker affiliation: **Founder @ Tenuo / Security Engineering @ Snap** (transcript said "Snapchat"; slides are canonical) | Prior-art lineage names + dates (Dennis & Van Horn, Macaroons, UCAN, Biscuits, CaMeL, "Intentional AI Design") |
| Performance numbers: 55 μs auth / 200 ns deny / 5,700 fuzz probes / 53/53 violations | "9 frameworks supported" claim (slide names 3) |
| Multi-agent-delegation ASR **90% → 0%** | "Tenuo not trying to solve prompt injection" framing |
| 4 enforcement scenarios with named labels (Happy Path / Poisoned Logs / Exfiltration / Impersonation) | Q&A: orchestrator-driven vs approval-gated scope, OAuth/IAM stacking |
| 4 CVEs cited as constraint-design evidence (`CVE-2024-3571`, `CVE-2025-3046`, `CVE-2025-61784`, `CVE-2025-66032`) | "Map vs Territory" 3-layer naming (slide is canonical: Map / Annotated Map / Territory) |
| Map-vs-Territory: 87.5% → 100% improvement on real harness | "Tenuo not trying to solve prompt injection" framing |
| Repo: `https://github.com/tenuo-ai/tenuo/`; email: `niki@tenuo.ai` | Bypass example strings (`/data/../etc/passwd`, decimal-encoded IP, URL userinfo) |
| Subtractive Delegation diagram (`W0 ▶ W1 ≤ W0 ▶ W2 ≤ W1`) | The valet-key analogy in full prose form |
| | Corporate-Amex-vs-debit-card analogy |
| | Open call: "looking for researchers who want to collaborate on building benchmarks for multi-agent authorization" |

Without the slides we'd have the lineage and analogies but no numbers, no CVEs, no canonical company name (transcription noise made "Tenuo" come through as "Tenure" / "Teno" / "10-year-old"), no performance claims. Without the transcript we'd have the architecture but not the rationale, the run-time scope-determination story, the OAuth/IAM stacking, or the open research questions. Both are required.

## Consequences for this corpus

- **The PDP/PEP gap is partly closed.** [[pdp-pep-for-non-tool-mediated-actions|PDP/PEP for non-tool-mediated agent actions]] flagged this gap explicitly. Niyikiza is one of the *two* publicly disclosed answers (the other is Sondera Cedar, also at Unprompted). Capability warrants address the gap from the **delegation-aware authorization** angle; Cedar addresses it from the **policy-language reference monitor** angle. Both pages now cross-reference this talk.
- **CMM D3 L5 has an exemplar.** [[agentic-ai-security-cmm-2026|CMM 2026]] D3 L5 already names "Capability tokens / Warrants per task with cryptographic binding" as a target evidence pattern but with no exemplar shipped. Tenuo is the first shipped vendor-neutral exemplar. CMM page now cites this talk.
- **Bifecta-Trifecta containment story now has a second leg.** Bullen's containment is *egress-side* (Smokescreen + agent-tag CI) + *tool-call-side* (Toolshed + ToolAnnotations). Niyikiza's containment is *delegation-side* (warrant attenuation + holder-binding). The two stack: a Stripe-style architecture *with* warrants would have all three legs of containment.
- **AI confused deputy is now a tracked concept.** The talk's framing — ambient authority, derived authority, delegation as the missing primitive — is one of the cleanest structural arguments in the wiki. See [[ambient-vs-derived-authority|Ambient vs Derived Authority]].
- **Constraint-design CVE list** cited on the slide gives the wiki a starting set for a future incident-class page on **prompt-injection-tooling-bypass CVEs** (LangChain / LlamaIndex / LlamaFactory / Claude Code allowlist). All four are real CVEs and all four target the constraint-as-string-match anti-pattern Niyikiza warned about.

## Open questions surfaced by this talk

1. **Single-broker vs mesh for warrant verification.** Niyikiza shows offline verification — by design, no central server. But a fleet-scale Tenuo deployment still needs a place to *issue* the root warrant and to *collect* receipts. How does that operationally compose with existing PEPs at Stripe / Block / Salesforce scale?
2. **What benchmark does the community converge on?** Niyikiza's open call is real — there is no shared benchmark for multi-agent delegation-aware authorization. Without one, the 90%→0% number stays vendor-internal. A benchmark plus Block's goose red-team (Operation Pale Fire) and Mindgard's Vibe Check might form a coalition.
3. **CaMeL × Warrants — composability?** CaMeL's privileged/quarantined LLM split is a *model-layer* containment; warrants are an *execution-layer* containment. Are they redundant, or do they compose into a stronger guarantee? The talk hints at composition but doesn't show it.
4. **Does the Map-vs-Territory lesson generalize beyond paths/URLs?** The 87.5%→100% jump is shown for path/URL constraints. What about *data-flow* constraints (PII tags), *semantic* constraints (intent classification), *behavioral* constraints (rate / cumulative-amount caps)?

## See also

- [[capability-based-authorization|Capability-Based Authorization]] — concept page (history, primitives family)
- [[tenuo-warrant|Tenuo Warrant]] — the 6-property primitive in detail
- [[monotonic-attenuation|Monotonic Attenuation]] — the W2 ⊆ W1 ⊆ W0 invariant
- [[ambient-vs-derived-authority|Ambient vs Derived Authority]] — the structural distinction
- [[pdp-pep-for-non-tool-mediated-actions|PDP/PEP for non-tool-mediated agent actions]] — gap page; this talk is one of the two answers
- [[breaking-the-lethal-trifecta-talk|Bullen — Breaking the Lethal Trifecta]] — companion talk same day
- [[lethal-trifecta|Lethal Trifecta]] · [[lethal-bifecta|Lethal Bifecta]]
- [[tenuo|Tenuo]] · [[niki-aimable-niyikiza|Niki Aimable Niyikiza]]
