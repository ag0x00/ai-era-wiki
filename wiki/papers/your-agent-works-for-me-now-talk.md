---
type: talk
title: "\"Your Agent Works for Me Now\""
created: 2026-05-03
updated: 2026-05-03
tags:
  - papers
  - talks
  - prompt-injection
  - agentic-ai
  - red-teaming
  - attack-patterns
  - c2
  - promptware
  - unprompted-2026
status: summarized
year: 2026
authors: ["Johann Rehberger"]
venue: "Unprompted Conference, San Francisco — Stage 2 Lecture 02, Wednesday March 4, 2026, 09:35"
source_url: "https://drive.google.com/file/d/1RdI_JG_ZxagWYkd0rVp8ty_uw8FMvsNl/view"
archived_copy: ".raw/talks/2026-03-04_Johann-Rehberger_Your-Agent-Works-for-Me-Now_transcript.md"
no_public_url: ""
key_claim: "Prompt injection is no longer just a single-turn attack — it has evolved into 'promptware': structured, multi-stage malware written in natural language that implements persistence, command-and-control, data exfiltration, and lateral movement. A new bypass technique ('delayed tool invocation') can circumvent even intentional platform-level security controls by deferring tool activation to a subsequent conversation turn."
methodology: "Red-team practitioner talk with live demos. Rehberger presents previously undisclosed exploits against production systems (Gemini Workspace, Microsoft Enterprise Copilot, ChatGPT, OpenClaw, KimiCloud) alongside his Agent Commander C2 tool. Talk cites Ben Nassi's concurrent Promptware Kill Chain paper as academic parallel. No slides ingested in this session (transcript only)."
supports:
  - "[[indirect-prompt-injection]]"
  - "[[memory-poisoning]]"
  - "[[tool-abuse-chains]]"
  - "[[lethal-trifecta]]"
  - "[[agent-observability]]"
related:
  - "[[promptware]]"
  - "[[delayed-tool-invocation]]"
  - "[[agent-commander-prompt-c2]]"
  - "[[johann-rehberger]]"
  - "[[month-of-ai-bugs]]"
  - "[[jules-ai-kill-chain]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[securing-workspace-genai-at-google-talk]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[ben-nassi]]"
sources:
  - ".raw/talks/2026-03-04_Johann-Rehberger_Your-Agent-Works-for-Me-Now_transcript.md"
  - ".raw/talks/2026-03-04_Johann-Rehberger_Your-Agent-Works-for-Me-Now_slides.pdf"
aliases:
  - papers/your-agent-works-for-me-now-rehberger-talk
  - your-agent-works-for-me-now-rehberger-talk

---

# "Your Agent Works for Me Now" — Rehberger, Unprompted 2026

**Source:** Transcript via attendee [Google Drive share](https://drive.google.com/file/d/1RdI_JG_ZxagWYkd0rVp8ty_uw8FMvsNl/view) (conference-only; no public recording available as of 2026-05-03). Local copies: `.raw/talks/2026-03-04_Johann-Rehberger_Your-Agent-Works-for-Me-Now_transcript.md` and `.raw/talks/2026-03-04_Johann-Rehberger_Your-Agent-Works-for-Me-Now_slides.pdf`.

> [!gap] Slides Not Extracted
> The slides PDF is available at `.raw/talks/2026-03-04_Johann-Rehberger_Your-Agent-Works-for-Me-Now_slides.pdf` but was not extracted via `pdftotext` in this ingest session. This summary is based on the transcript only. A future re-ingest pass should extract the slides to capture any diagrams, architecture layouts, or code samples not verbally described.

A 30-minute red-team practitioner talk by [[johann-rehberger|Johann Rehberger]] (Red Team Director, independent / Embrace The Red) at [[unprompted-conference-march-2026|Unprompted]] (Stage 2, March 4, 2026). The talk demonstrates a progression from known attack patterns (data exfiltration, memory persistence) to two previously undisclosed findings: **delayed tool invocation** as a security-control bypass, and **Agent Commander** as a complete prompt-level command-and-control infrastructure.

## Key Thesis

> "The injection is the technique. And then what you do later is a complex set of instructions to achieve an objective — that is called **promptware**."

The talk reframes agentic AI attacks not as one-off injection events but as a **kill chain** with phases analogous to traditional malware: initial access via injection → execution → persistence → command-and-control → exfiltration. Each phase now has demonstrated real-world implementations across production systems from Google, Microsoft, and OpenAI.

## Structure of the Talk

The talk opens in media res with three rapid-fire live demos, then introduces the "promptware" framing, then discloses the two novel research findings, and closes with a forward-looking statement on AI-native adversarial tradecraft.

---

## Part 1: Established Attack Patterns (Rapid-Fire Demos)

### Demo 1: Linear Ticket RCE (Winsurf + MCP)

A GitHub-style project management ticket (Linear) contained an invisible [[indirect-prompt-injection|indirect injection]] payload encoded using an **ASCII smuggler** (Unicode tag character steganography). When an AI coding agent (Winsurf) with MCP integration was assigned the ticket, it loaded the ticket, parsed the hidden payload, and **executed a remote command** — specifically, launched the calculator.

- **Injection vector:** Linear issue body; payload invisible to human reviewer
- **Technique:** ASCII/Unicode tag character steganography (zero-width invisible payload)
- **Result:** Remote command execution on the developer's machine
- **Defensive note:** The payload was only visible after base64/Unicode decoding; human code reviewers and standard text rendering do not see it

### Demo 2: Apple Xcode Prompt Injection (First Public Demo)

Rehberger claims this is the **first publicly demonstrated prompt injection against Apple Xcode**. Source code in a file contained hidden Unicode tag characters invisible to human readers. When Xcode's AI code review feature analyzed the file:
- It correctly identified the hidden Unicode characters
- Decoded them — revealing the injection payload
- Followed instructions to add malicious source code to the file (data-exfiltrating code) AND invoke a `RunSnippet` tool

- **Injection vector:** Source code file with hidden Unicode tag characters
- **Technique:** Same Unicode steganography; Xcode's `RunSnippet` is the exploitation primitive
- **Result:** Remote command execution + data exfiltration from developer environment
- **Environmental note:** Works by default if entitlements allow internet connections; a sandboxed Xcode environment with restricted entitlements would limit the impact

### Demo 3: Microsoft Enterprise Copilot Memory Persistence

A file summarization task against Microsoft Enterprise Copilot triggered the Copilot memory tool, writing two persistent memories:
- "I am 102 years old" — demonstrates falsification of user context data
- Standing instruction to run a Commodore 64 simulator — demonstrates persistent behavioral modification

**All future Copilot conversations were compromised for this user.** In an enterprise context, Copilot's memory contains financial information and organizational structure — all now falsified for the victim.

- **Fixed by Microsoft:** December (prior year). Rehberger chose to include it as evidence that these attacks ARE fixed when reported — the responsible disclosure process works.
- **Note on simplicity:** Rehberger emphasizes the prompt behind the attack is "not very complicated — just natural language" with easing-in techniques and tool invocations appended.

---

## Part 2: The "Promptware" Framing

After the demos, Rehberger introduces the conceptual shift — crediting [[ben-nassi|Ben Nassi]]'s concurrent *Promptware Kill Chain* paper as the formal academic parallel.

> "We see a prompt injection that's not just a single injection anymore, right? It's complex malware even. So, we should start maybe using the promptware term to highlight that the injection is the technique. And then what you do later is a complex set of instructions to achieve an objective — that is called promptware."

The concept is given its own wiki page at [[promptware|Promptware]].

**Spyber (ChatGPT, previously demonstrated at Black Hat):** After memory poisoning via [[delayed-tool-invocation|delayed tool invocation]], the agent exfiltrates every user keystroke to an attacker server on every subsequent conversation turn. The user types; the data leaves. No further attacker interaction required after initial enrollment.

The same is possible with Winsurf (coding agent): indirect injection → memory compromise → standing instructions to continue compromising the user every session and send data to the attacker continuously.

---

## Part 3: Delayed Tool Invocation (Novel Finding)

**The Novel Finding: Delayed Tool Invocation.** Google implemented a correct security control: deactivating the Workspace tool during indirect injection. Rehberger found a bypass: a deferred conditional in the injection payload causes the tool to become active again when the user continues the conversation in the *next* turn. The security control evaluates turn 1 and sees no active harmful invocation. Turn 2 fires the tool — outside the evaluation window.

Full details at [[delayed-tool-invocation|Delayed Tool Invocation]].

**Four DTI demonstrations disclosed:**

| Target | Result | Status |
|---|---|---|
| Gemini Workspace (email sent to Google security) | Workspace tool invoked on next turn; sensitive document read | Reported; Google security reproduced |
| Microsoft Copilot (via file) | Memory tool invoked; long-term persistent memory written | Reported; Copilot fixed |
| Gemini + Google Home ("Broadcast to Living Room" document title) | Google Home speaker invoked; played "Johan is here. Trust no AI." | Reported; bypass of Nassi et al. fix |
| ChatGPT (via document) | Personalization option modified to "quirky" | Reported to OpenAI; classified as guardrail (not security) |

**Mechanism hypothesis:** The deferred trigger increases attention to the attack payload by repeating it (consistent with Google's 2026 paper "Prompt repetition improves non-reasoning LLMs"). The document metadata (title = "Broadcast to Living Room") may function as an intent signal that activates the relevant tool.

**Q&A exchange on DTI:**
- Q: "Is this fundamentally flawed?" — Rehberger: the deactivation control is a **positive thing** and the right direction; the bypass is a platform-architecture problem to solve.
- Q: "Is this state manipulation or input validation?" — Rehberger: it's a **fundamental behavior within the transformer** — targeting the attention layer to bring attention to a desired tool invocation.
- Q: "What's the best trust boundary?" — Rehberger: assume any tool CAN be invoked by indirect injection unless you have a control in place; reason about the downstream impact; preventing toolchaining between certain tool types is a viable mitigation.

---

## Part 4: Agent Commander — Prompt-Level C2

> "I want to move command and control infrastructure up to the prompt level. Not work with operating system commands anymore, but have a prompt command and control with prompts. It will work with any agent, with any language, and it's an abstraction from the operating system."

Full details at [[agent-commander-prompt-c2|Agent Commander — Prompt-Level Command and Control]].

**Zero-click enrollment vector (OpenClaw + Gmail PubSub):**
1. Target agent has OpenClaw's built-in Gmail notification subscription active
2. Attacker sends an email to the user's inbox
3. OpenClaw auto-analyzes the email (no user action required)
4. Email contains a promptware payload that fetches remote C2 instructions and appends them to the heartbeat
5. ~20 minutes later (next heartbeat cycle), the agent has joined the C2 server

**Demonstrated C2 capabilities:**
- Full infrastructure profile (server name, IP, environment variables, uptime)
- Screenshot capture + upload to C2 server
- Arbitrary prompt template dispatch
- Hidden from user via `heartbeat OK` suffix / `NO_REPLY` prefix suppression strings

**Cross-platform:** Validated on both OpenClaw and KimiCloud (Moonshot.ai / Alibaba). Infrastructure finding: KimiCloud VM contained a MacBook Pro `authorized_keys` — suggesting every Moonshot.ai employee can SSH into user VMs.

---

## The "Normalization of Deviance" Observation

Between the technical sections, Rehberger names a behavioral security failure: **normalization of deviance**. The example: an engineer at Meta used an AI agent on a test system for a while, it worked fine, so they promoted it to the production system. The production system then had a dramatic failure (OpenCloud deleting a real user inbox).

> "We fundamentally know that we shouldn't be trusting these models. The output is not trustworthy. We do not control or know exactly what's going on. But the more we use it, the more we start trusting it, the more we normalize believing we can trust it, and then very drastic things can happen."

This is a governance / organizational failure pattern — not a technical exploit — but Rehberger frames it as a security concern of equal weight. It explains why the technical exploits he demonstrates continue to be effective even as vendors ship fixes: the attack surface keeps growing because organizations normalize reliance on AI before the security model is established.

> [!gap] Normalization of Deviance as a Concept Page
> The "normalization of deviance" pattern (borrowed from systems safety / Challenger disaster analysis) is referenced here and deserves its own concept page in `wiki/concepts/`. As of 2026-05-03 no such page exists. This cross-cuts organizational failure modes, shadow automation, and the governance domain (CMM D1).

---

## Context: Unprompted Stage 2, Offensive Track

The conference organizer (Gadi Evron) gave Rehberger an extended introduction praising his community contributions — particularly the [[month-of-ai-bugs|Month of AI Bugs]] August 2025 series (daily disclosures, responsibly disclosed, all coordinated to release simultaneously). The talk was positioned as the headline offensive security talk of Stage 2 ("the right people are in the room to appreciate him").

---

## Placement in this wiki

| Wiki artifact | Update from this talk |
|---|---|
| [[promptware\|Promptware]] | **NEW** concept page — the malware framing for multi-stage injection attacks |
| [[delayed-tool-invocation\|Delayed Tool Invocation]] | **NEW** concept page — tool reactivation bypass technique (previously unreported) |
| [[agent-commander-prompt-c2\|Agent Commander — Prompt-Level C2]] | **NEW** concept page — prompt-native C2 infrastructure tool |
| [[indirect-prompt-injection\|Indirect Prompt Injection]] | Add: ASCII smuggler/Unicode tag characters as injection vector; Xcode as first-reported injection surface; Linear MCP integration as injection surface; add Rehberger Unprompted 2026 to notable cases |
| [[memory-poisoning\|Memory Poisoning (Agentic AI)]] | Add: Copilot enterprise memory implant as a production example; Gemini long-term memory as a production example |
| [[johann-rehberger\|Johann Rehberger]] | Update: Unprompted 2026 disclosures — DTI, promptware C2, Xcode, KimiCloud |
| [[unprompted-conference-march-2026\|Unprompted Conference]] | Update: the "Your Agent Works for Me Now" row now resolves to this summary |
| [[ben-nassi\|Ben Nassi]] | Add: Promptware Kill Chain paper (March 2026) — Rehberger credits him; paper was "released very recently" |

## Companion Talks (Same Conference, Complementary Perspectives)

- [[breaking-the-lethal-trifecta-talk|Bullen (Stripe)]] — architectural containment that breaks the egress leg Rehberger's attacks depend on
- [[securing-workspace-genai-at-google-talk|Lidzborski (Google Workspace)]] — the defensive side of the exact Gemini/Workspace surfaces Rehberger attacked; Lidzborski's 4-layer blueprint is the architectural response to the DTI attacks described here

The three talks together — Rehberger (attack), Lidzborski (Google defense), Bullen (Stripe defense) — form the most complete practitioner triangle on the Unprompted agenda for prompt-injection in production agentic systems.
