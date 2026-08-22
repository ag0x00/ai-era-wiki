---
type: practice
title: "Credential Proxy Pattern for AI Agents"
address: c-000191
created: 2026-04-30
updated: 2026-08-15
tags:
  - practices
  - credential-security
  - nhi
  - agentic-ai
  - identity
status: developing
scope_axis:
  - sec-of-ai
maturity: converging
addresses_threat: "Credential exfiltration via prompt injection (ASI02), over-permissioned agent access (ASI03), lateral movement via stolen API keys"
related:
  - "[[nhi-governance-for-agents]]"
  - "[[non-human-identity]]"
  - "[[agent-identity-architecture]]"
  - "[[identity-credential-coupling]]"
  - "[[tenuo-warrant]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[security-controls-for-ai-stacks]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[claude-code-github-action-credential-exposure]]"
  - "[[securing-agentic-coding]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
sources:
  - "[[.raw/papers/emerging-cybersecurity-practices-for-agentic-ai-applications.md]]"
  - "[[.raw/articles/agentcordon-readme-2026-05-04.md]]"
---

# Credential Proxy Pattern for AI Agents

The **credential proxy pattern** keeps real secrets out of an agent's reach: the agent holds only a short-lived proxy token, and a proxy resolves it to the real credential and injects it at the network layer just before the outbound request. It is the load-bearing control of the [[agentic-ai-security-reference-architecture|RA]] Identity plane and the mechanism behind [[agentic-ai-security-cmm-d2-identity|CMM D2-L4]]'s zero-credentials-in-agent-context criterion.

## On this page

- [[#What it is]]
- [[#Why it matters]]
- [[#How it works]]
- [[#Known implementations]]
- [[#Security properties]]
- [[#Relationship to the identity stack]]
- [[#When to apply]]
- [[#Implementation notes]]
- [[#Limits]]

## Definition

The pattern interposes a proxy between the agent and any target API so that real credentials (API keys, OAuth tokens, cloud secrets) are **never placed in the agent's context window, environment variables, or configuration files**. The agent carries only a short-lived proxy token scoped to the allowed target APIs, TTL, and permission envelope; the proxy resolves it against the vault and injects the real credential at the network layer.

The approach has converged independently across multiple OSS and commercial tools (six in the table below), indicating a broadly recognized gap in agentic deployments.

## Significance

AI agents process external content (emails, web pages, documents) that can carry adversarial instructions. A successful [[prompt-injection|prompt injection]] might tell the agent to "print all environment variables" or "output the contents of `.env`". If real credentials sit in those locations, injection exfiltrates them with no further exploit. The credential proxy removes the precondition: there is nothing to exfiltrate because credentials never enter the agent's accessible state. It addresses `ASI02` (tool-misuse exfiltration) and `ASI03` (identity and privilege) in the [[owasp-agentic-ai-top-10|OWASP ASI Top 10]].

The proxy bounds *what an injection can steal*. It does not bound *what an authorized agent can do* — that is the job of the [[tenuo-warrant|capability-token layer]]. The two are complementary controls on the same plane.

## Mechanism

```
Agent → [proxy token only] → Credential Proxy → [real credential injected] → Target API
                                      ↕
                               Encrypted Vault
```

1. At provisioning time, the agent is issued a **proxy token** (not the real credential). The proxy token encodes agent identity, allowed target APIs, permission scope, and TTL.
2. When the agent makes an outbound call, the request routes through the proxy.
3. The proxy resolves the token against the vault, retrieves the scoped real credential, and injects it into the request headers.
4. The response returns to the agent with any credential echoes stripped.
5. Every resolution is logged: agent identity, timestamp, target endpoint, proxy token used.

The agent **never sees** the real credential at any step.

## Known implementations

| Tool | Approach | Key feature |
|---|---|---|
| AgentKeys | Cloud proxy, AES-256 vault | Per-agent proxy tokens (`pxr_…`); instant revocation |
| Keychains.dev | Server-side curl replacement | Template variables (`{{GITHUB_TOKEN}}`); hierarchical sub-agent token forking |
| Aegis | Local-first (localhost:3100) | Zero cloud dependency; STRIDE threat model; SHA-256 agent tokens |
| OneCLI | Docker-based gateway | Web dashboard |
| AgentSecrets | OS keychain integration | Credentials never in files or env vars |
| [[agentcordon\|AgentCordon]] | Three-tier (CLI / broker / server); Cedar PDP; AES-256-GCM + HKDF; Rust; GPL-3.0 | Ed25519 workspace identity; broker daemon holds OAuth tokens so the agent host never does; MCP gateway with response-leak scanning |

## Security properties

- **Prompt-injection resistance.** A successful injection cannot extract credentials that never enter the context window.
- **Hierarchical delegation.** Keychains.dev forks parent→child tokens so sub-agents get only the scopes they need.
- **Instant revocation.** Access ends without rotating the underlying secret.
- **Audit trail.** Every credential resolution is logged with agent identity, timestamp, and target endpoint.
- **Scoped least privilege.** Each agent or sub-agent receives only the credentials its task requires.

## Relationship to the identity stack

This is secrets management (HashiCorp Vault, AWS Secrets Manager, [[cyberark-conjur|CyberArk Conjur]]) adapted for agents whose behavior can be influenced by adversarial inputs. The proxy is to agents what IAM instance roles are to EC2 instances: credentials injected by the infrastructure, not stored in the workload.

Two adjacent controls bound the pattern's scope:

- **Credential-less identity** (Azure Managed Identities, AWS Bedrock AgentCore token vault, GCP auth-manager) reaches the same end state by never issuing a long-lived secret to the agent. Where the platform offers it, it is the cheaper path to D2-L4 than operating a separate proxy.
- **Coupled credentials** ([[identity-credential-coupling|identity-credential coupling]] — SAS tokens, storage access keys, SaaS API keys where the credential *is* the identity) limit the proxy: it can intermediate access, but it cannot separate what is structurally inseparable, so rotation remains identity rotation for those classes.

In the [[agentic-ai-security-reference-architecture|RA]] this is the Identity plane's load-bearing row; in the [[agentic-ai-security-cmm-d2-identity|CMM D2 ladder]] it is the L4 zero-credentials criterion, satisfied by either a credential broker/proxy or a credential-less identity model.

## Applicability

- Any agent that makes outbound API calls, even to internal services.
- Multi-agent systems where parent agents spawn sub-agents with delegated credentials.
- Agents that process untrusted external content (emails, web, documents).
- Immediately — before deploying the agent, not as a hardening step.

## Implementation notes

1. **Never put real credentials in environment variables or config files** for agent workloads. `OPENAI_API_KEY=sk-…` in a `.env` file is the threat model.
2. Use short-lived proxy tokens with TTLs matching the task scope.
3. Build revocation tests into the incident-response playbook — know the time from "agent compromised" to "all credentials revoked."
4. Log at the proxy, not just the agent, so a compromised agent cannot overwrite the record.
5. For multi-agent systems, enforce scope-inheritance limits — a child token cannot exceed its parent's scope (the same [[monotonic-attenuation|monotonic-attenuation]] invariant the capability-token layer enforces cryptographically).

The pattern's clearest 2026 test case is the CI-runner coding agent. The [[claude-code-github-action-credential-exposure|Claude Code GitHub Action exposure]] took a model API key straight from the workflow's process environment, which is exactly the delivery mechanism this pattern replaces: a credential the agent can read is a credential prompt injection can reach. Environment-variable delivery to an agent that also reads untrusted repository content should be treated as an anti-pattern rather than a default. See [[securing-agentic-coding|Securing Agentic Coding]] §Identity plane.

## Limits

- Adds a network hop (latency). Local proxies such as Aegis minimize it.
- Does not protect credentials passed *as parameters* of the proxy-resolved call (for example, a secret in a query string to a third-party API the proxy does not control).
- Requires deploying and operating the proxy infrastructure; the credential-less identity path avoids this where the platform supports it.
- Bypassed where the workload can still reach the credential sources over the network. The proxy takes secrets out of the agent's context; it leaves the cloud metadata endpoint and the secret store on the agent's network. In the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face incident]] agents reached root on a compromised host, then read IAM credentials from IMDS, used over-permissioned Kubernetes service accounts, and harvested Azure Key Vault, ending at cluster admin without reading any agent context window. Pair the proxy with network policy denying `169.254.169.254` and the vault endpoint from workload subnets (Dalton and Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026, summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]). The [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]] illustrates the same blast-radius problem on human credentials rather than agent-held secrets: 85 personnel credentials cracked, 84 pivoted via SSO (98.8%), with no proxy layer standing between the federated session and the resources it reached (Dream Security, ["Inside a Multi-Agent AI Framework Used to Compromise Government Entities in Asia"](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia), 2026-08-12).

## See also

- [[nhi-governance-for-agents|NHI Governance for AI Agents]] — broader NHI lifecycle context
- [[non-human-identity|Non-Human Identity (NHI)]] — the credential class this control protects
- [[identity-credential-coupling|Identity-Credential Coupling]] — where the proxy's separation guarantee breaks down
- [[tenuo-warrant|Tenuo Warrant]] — the complementary control that bounds authorized authority
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] — the identity layer where this control sits

This pattern is how an architecture drops leg B of [[agents-rule-of-two|the Agents Rule of Two]]. Holding credentials in a proxy outside the agent boundary removes sensitive access from the agent's own capability set, which lets a workflow retain untrusted input and state change without holding all three properties at once.
