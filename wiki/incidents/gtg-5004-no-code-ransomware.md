---
type: incident
title: "No-Code Ransomware Operation"
aliases:
  - "GTG-5004"
address: c-000287
created: 2026-08-16
updated: 2026-08-16
tags:
  - incidents
  - offensive-ai
  - ransomware
  - malware-development
  - capability-transfer
status: confirmed
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
incident_class: "ransomware-as-a-service development and sale"
attack_with_or_on_ai: "with AI"
date_observed: 2025-01
date_disclosed: 2025-08
target: "No named victims; the operation sold ransomware to other criminals rather than deploying it"
threat_actor: "Tracked by Anthropic as GTG-5004; UK-based, unnamed"
impact: "Working ransomware with ChaCha20 encryption, EDR evasion, and anti-recovery capability sold on dark-web forums at \\$400–\\$1,200 per tier; Anthropic assesses the actor as unable to implement the components without model assistance"
related:
  - "[[anthropic-threat-intelligence-reports]]"
  - "[[gtg-2002-vibe-hacking-extortion]]"
  - "[[capability-floor-collapse]]"
  - "[[llm-attack-navigator]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
  - "[[zero-day-clock]]"
  - "[[verizon-dbir-2026]]"
  - "[[anthropic]]"
sources:
  - "[[.raw/reports/anthropic-threat-intelligence-report-2025-08.pdf]]"
  - "https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf"
---

# GTG-5004 — No-Code Ransomware Operation

## Summary

A UK-based actor built and sold ransomware-as-a-service on dark-web forums while, in Anthropic's assessment, being unable to implement its technical components without model assistance. Anthropic disclosed the operation in its August 2025 threat intelligence report under the designation GTG-5004.[^tir-5004] The product was not a proof of concept: it shipped ChaCha20 file encryption with per-infection RSA key management, two distinct direct-syscall techniques for bypassing endpoint detection hooks, reflective DLL injection, shadow-copy deletion, and a PHP victim-management console, priced in three commercial tiers.[^tir-tech]

The claim that carries the case is Anthropic's own: the actor "does not appear capable of implementing encryption algorithms, anti-analysis techniques, or Windows internals manipulation without Claude's assistance."[^tir-dependency] That is an assessment made from the inside of the tooling — from the prompt record — and it is not recoverable from malware analysis alone.

## Attack vector

The operation ran as a commercial product line rather than a campaign. Anthropic observed it active on Dread, CryptBB, and Nulled since at least January 2025, operating a `.onion` site with a ProtonMail contact address and marketing through posts that ranged from plain sale listings to product announcements with video demonstrations. The actor claimed the products were for educational and research use while advertising private crypting services on criminal forums.[^tir-commerce]

| Tier | Price | Contents |
|---|---|---|
| Ransomware binary | \$400 | Ransomware DLL and executable |
| Full RaaS kit | \$800 | Adds a PHP console and command-and-control tooling |
| FUD crypter | \$1,200 | Windows 10/11 fully-undetectable crypter for native binaries |

The technical capability behind those tiers, as described in the report:

- **Encryption.** ChaCha20 stream cipher applied to the first 256KB of each file, with AES-256 available as an alternative; RSA keypair generation per infection through the Windows CNG API; encrypted files marked with an `.enc` extension; enumeration across all fixed drives and mapped network shares with user directories prioritized.
- **Endpoint-detection evasion.** Two direct-syscall techniques that bypass user-mode API hooks: FreshyCalls, which parses syscall numbers from the `ntdll.dll` export table, and RecycledGate, which locates existing `syscall; ret` sequences inside `ntdll.dll`. String obfuscation and anti-debugging sit alongside them.
- **Delivery and persistence.** Reflective DLL injection to load without disk artifacts, and code-cave infection that writes the payload into unused space in existing PE executables.
- **Anti-recovery.** Volume Shadow Copy deletion, targeted extension enumeration across accessible drives, and extension of the encryption pass to mapped network resources.
- **Operations.** A separate decryption utility for payment verification, a PHP-based victim-management console, and Tor command and control.

Anthropic describes a visible development arc across the prompt record: basic encryption and evasion first, then anti-analysis and recovery prevention, then delivery mechanisms and command-and-control infrastructure, advancing as the actor supplied continued directional prompting rather than new expertise.[^tir-timeline]

## Timeline

- 2025-01 — first observed sales activity on dark-web forums; the report reproduces a 17 January 2025 sales offering
- 2025-01 to 2025-08 — capability develops across three phases from basic encryption to full command-and-control infrastructure
- 2025-08 — Anthropic bans the account and deploys detection for malware upload, modification, and generation on the platform[^tir-mitigation]

## Significance

**The collapse is in the capability floor, not in development speed.** The wiki's existing argument on attacker economics — carried by [[zero-day-clock|Zero Day Clock]] and [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] — is that AI compresses the time from vulnerability to working exploit. That argument concerns actors who could already do the work, doing it faster. GTG-5004 is a different measurement: the actor could not do the work at all, and the artifact shipped anyway. Anthropic frames the outcome as advanced malware capabilities being "generated rather than developed".[^tir-implications] The generalized claim is on [[capability-floor-collapse|Capability Floor Collapse]].

**The product is competent and unoriginal, which is the expected shape.** Every technique in the list is established prior art: FreshyCalls and RecycledGate are published syscall-resolution methods, ChaCha20 is a standard cipher, reflective injection dates to the 2000s. That is consistent with the [[verizon-dbir-2026|Verizon DBIR 2026]] finding that AI-assisted malware carries a median of 55 known prior examples performing the same function and that under 2.5% of observations involve uncommon techniques.[^dbir-novelty] The two sources describe the same phenomenon from opposite ends. The DBIR measures that the output is not novel; GTG-5004 shows that novelty was never the point, because the change is assembly of known parts by someone who could not assemble them.

**Attribution signal degrades.** Anthropic notes that attribution becomes harder when code style reflects model patterns rather than an author's habits.[^tir-implications] Malware-authorship clustering assumes a human author with consistent habits: shared idioms, reused helper routines, compiler fingerprints. A model-generated codebase carries the model's habits, which are shared with every other actor using the same model.

## Defensive lessons

- **Skill-based actor tiering mis-ranks this actor.** A triage model that sorts unattributed ransomware by implementation sophistication would place GTG-5004 with capable malware developers and would be wrong about who is behind it, what else they can do, and how they will respond to disruption. Capability and actor sophistication have come apart; see [[llm-attack-navigator|LLM ATT&CK Navigator]] for the population-scale version of the same finding.
- **The evasion techniques are known and detectable.** FreshyCalls and RecycledGate both leave observable artifacts, namely direct syscall invocation from outside `ntdll.dll` and abnormal export-table parsing, and detection for both predates this actor. The control question is coverage of published techniques, not preparation for novel ones.
- **Shadow-copy deletion and network-share enumeration remain the highest-value detection points.** Neither changed because the malware was model-generated, and both sit late enough in the chain to be actionable and early enough to precede irreversible encryption.
- **Vendor-side detection targeted the artifact class, not the actor.** Anthropic's stated response was detection for malware upload, modification, and generation on the platform rather than a signature for this operation, which is the control shape that generalizes across the population the [[llm-attack-navigator|Navigator]] later measured.

## Mapping

- MITRE ATT&CK, per the technique set Anthropic describes: T1486 (Data Encrypted for Impact), T1490 (Inhibit System Recovery), T1620 (Reflective Code Loading), T1027 (Obfuscated Files or Information), T1562.001 (Impair Defenses: Disable or Modify Tools), T1106 (Native API), T1587.001 (Develop Capabilities: Malware)
- Threat class: outside the [[agentic-ai-threat-classes-2026|five-class expansion]], which models adversaries against an enterprise's own agentic systems; this actor used a model as a development tool against no AI target

## Source

[Anthropic — *Threat Intelligence Report: August 2025*](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf), "No-code malware: selling AI-generated ransomware-as-a-service", pp. 15–17. Local copy: `.raw/reports/anthropic-threat-intelligence-report-2025-08.pdf`. Series page: [[anthropic-threat-intelligence-reports|Anthropic Threat Intelligence Reports]].

## See also

- [[gtg-2002-vibe-hacking-extortion|Vibe-Hacking Extortion Campaign]] — the same report's operations-side capability-transfer case
- [[capability-floor-collapse|Capability Floor Collapse]] — the axis this case anchors on the development side
- [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] — the attacker-economics thesis this case extends from speed to floor
- [[verizon-dbir-2026|Verizon DBIR 2026]] — independent breach telemetry on the novelty ceiling of AI-assisted malware

## Notes

[^tir-5004]: Anthropic, [*Threat Intelligence Report: August 2025*](https://www-cdn.anthropic.com/b2a76c6f6992465c09a6f2fce282f6c0cea8c200.pdf), p. 15: "a UK-based threat actor (tracked as GTG-5004) has leveraged Claude to develop, market, and distribute ransomware with advanced evasion capabilities."
[^tir-tech]: Ibid., pp. 15–17, "Malware analysis".
[^tir-dependency]: Ibid., p. 15.
[^tir-commerce]: Ibid., p. 15, "RaaS commercial operation". Forums named: Dread, CryptBB, Nulled.
[^tir-timeline]: Ibid., p. 17: "Evolution timeline: Our analysis identified clear development phases showing increasing sophistication as the actor provided continued directional prompting."
[^tir-mitigation]: Ibid., p. 17: account banned; "we implemented new methods for detecting malware upload, modification, and generation on our platform."
[^tir-implications]: Ibid., p. 17, "Implications" and "Operational transformation": "Technical competence is outsourced rather than acquired"; "Attribution becomes more challenging as code style reflects AI patterns"; "advanced malware capabilities are generated rather than developed".

[^dbir-novelty]: Verizon Business, [*2026 Data Breach Investigations Report*](https://www.verizon.com/business/resources/reports/2026-dbir-data-breach-investigations-report.pdf), GenAI section. See [[verizon-dbir-2026|Verizon DBIR 2026]].
