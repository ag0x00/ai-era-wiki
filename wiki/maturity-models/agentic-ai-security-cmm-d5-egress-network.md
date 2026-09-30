---
type: maturity-model
title: "CMM D5: Egress and Network"
address: c-000127
created: 2026-05-25
updated: 2026-09-29
tags:
  - maturity-models
  - cmm
  - egress
  - network
  - recalibration
  - sec-of-ai
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[kimi-k3-sandbox-escape|Kimi K3 Sandbox Escape]]"
  - "[[solo-io|Solo.io]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[microsoft-zt4ai]]"
  - "[[lethal-trifecta]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[securing-agentic-coding]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[offensive-agent-collective]]"
  - "[[openai-dsewiki-agent-collusion]]"
  - "[[artifactory]]"
  - "[[owasp-ai-exchange]]"
  - "[[a2a-protocol]]"
  - "[[cyera-agent-guardian-release]]"
  - "[[agent-escape]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[oss-ai-vuln-discovery-harness-landscape]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[defending-code-harness]]"
  - "[[cmm-known-limitations]]"
  - "[[claude-cowork]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[cmm-stress-test-canadian-fi-google-2026-09]]"
sources:
  - https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization
  - https://a2a-protocol.org/latest/specification
  - https://owaspai.org/go/agentsandboxing/
  - https://owaspai.org/go/limitresources/
  - https://owaspai.org/go/modelaccesscontrol/
  - https://owaspai.org/go/oversight/
  - https://owaspai.org/go/supplychainmanage/
  - https://owaspai.org/go/testing/
verified: 2026-09-29
verified_against:
  - ".raw/papers/owasp-ai-exchange-development-time-threats-2026-08-19.md"
  - ".raw/papers/owasp-ai-exchange-runtime-appsec-threats-2026-08-18.md"
  - ".raw/papers/owasp-ai-exchange-testing-2026-08-19.md"
verified_findings: 0
verified_note: "Final source recheck after approved rewrite; MCP and A2A normative text checked live; no open findings."
---

# CMM D5: Egress and Network

## Domain decision and boundary

D5 determines whether the deployment confines each agent's network reach and mediates calls at the point they cross a network boundary. The assessor follows the actual deployment shape: model requests, tool calls, remote MCP, local MCP processes that make outbound requests, inter-agent messages, code execution, and supplier-operated routes. The destination inventory records each permitted endpoint and any service that can relay an agent-controlled request beyond it. The [[agentic-ai-security-cmm-measurement-protocol|Assessor's Handbook]] records an opaque applicable supplier route as unanswerable unless scoped supplier evidence or a test supports the decision.

The network owner enforces destination and route policy outside instructions and code the agent can change. [[agentic-ai-security-cmm-d2-identity|D2]] supplies the calling identity and task binding; [[agentic-ai-security-cmm-d3-control-least-agency|D3]] supplies action scope. D5 checks whether those decisions govern network calls. [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] owns prompt and tool-output handling inside the runtime. A gateway can carry both D4 screening and D5 routing, but the two determinations remain separate. The [[agentic-ai-security-reference-architecture|Reference Architecture]] places the network boundary and evidence flows.

| Path | D5 assessment boundary |
|---|---|
| Hosted model and network tool | Agent workload through gateway to endpoint, including the supplier-held portion when it can relay requests |
| Remote HTTP MCP | Client, broker, server, token audience, and any server-side outbound connection |
| Local stdio MCP | Approved process launch and credentials; outbound traffic from the child process joins the agent's route inventory |
| Inter-agent exchange | Sender, transport or broker, receiver, and any later delegated call |
| Agent-run code | Shell, browser, package process, or other child with its own network capability |

MCP's [authorization specification](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization) defines an optional HTTP authorization framework and directs stdio implementations to obtain credentials from the environment. D5 therefore tests these transports differently. A local stdio server does not fail merely because no OAuth broker sits between two local processes.

## Failure paths

- A model client, child process, or permitted service reaches a destination outside the effective allowlist through direct IP, another resolver, a redirect, or a relay.
- A gateway records traffic but cannot bind a call to the agent and tool, or a permitted TLS endpoint carries a request for another origin.
- A remote MCP server accepts the wrong token audience; a local MCP process launches from an unapproved source or inherits broader credentials than its task requires.
- A peer accepts a replayed message or a changed Agent Card, then calls a tool under authority the first agent did not delegate.

These paths set the negative tests below. Tests start from the agent workload or the reachable service that would make the request, under a policy and code revision equivalent to production.

## L1–L5 progression

| Level | Observable D5 outcome |
|---|---|
| L1 | Agent network reach has no effective deployment-specific boundary. |
| L2 | The applicable paths use an externally enforced destination allowlist, and the operator records their transitive reach. |
| L3 | Call identity, route mediation, DNS closure, resource ceilings, MCP transport controls, and inter-agent transport checks cover the paths in use. |
| L4 | Brokered peer traffic, scoped tool credentials, contract checks, segmentation, and tested origin handling reduce reachable effects. |
| L5 | Direct bypass and agent-controllable relays are constrained; applicable task, vulnerability-response, and multi-proxy decisions are enforced and checked. |

## Criterion catalogue

A criterion is met when every applicable route passes its stated test. A missing route is not applicable only when the assessment inventory and configuration show that it cannot occur. An existing route that the assessor cannot inspect remains unanswerable. Each record identifies the policy revision, sampled paths, refused request, and supplier evidence where needed. The bold criterion name is the canonical definition; the core page gives only a short restatement.

### L2 detail

- **D5-ALLOW.** Every agent-originated network connection, including outbound calls from agent-launched children, passes a destination allowlist that the agent cannot bypass by changing its own instructions or code. DNS lookups use the controlled resolver graded below. Test an unlisted hostname and address from each distinct route and autonomy mode; either reaching its destination fails. *Evidence:* enforced policy and route inventory, plus denied connections.
- **D5-REACH.** The operator maintains a dated record of each allowed destination and its reachable downstream hosts or accounts, including configured relays and supplier-declared reach. Compare the record with the deployed allowlist and each controlled destination's forwarding configuration; an unrecorded permitted destination fails. *Evidence:* reach record, policy revision, and destination configuration or scoped supplier statement.

### L3 detail

- **D5-A2A-AUTH.** Both endpoints authenticate an inter-agent network peer with a cryptographic identity before accepting traffic. Shared static keys do not identify separate agents. Test an unknown sender. *Evidence:* endpoint identity policy and refusal. Not applicable without inter-agent traffic.
- **D5-A2A-CHAIN.** A delegated message carries verifiable references to the delegating agent and the human accountable for the run, and the receiver preserves them in downstream calls. Test a message with a missing or substituted reference. *Evidence:* delegated message and downstream record. Not applicable without inter-agent delegation.
- **D5-A2A-REPLAY.** The receiver rejects a duplicate inter-agent message under a documented nonce, sequence, or equivalent replay rule. Test a previously accepted message again. *Evidence:* receiver rule and refusal. Not applicable without inter-agent traffic.
- **D5-A2A-TLS.** Receiving network endpoints for inter-agent traffic accept TLS 1.3 and reject earlier TLS versions. Test an older-version handshake. *Evidence:* endpoint configuration and handshake result. Not applicable without inter-agent network endpoints.
- **D5-ALLOW-LOCK.** The agent, its task input, and the principal for whom it acts cannot expand the allowlist used for that same run. Test a user, repository, or workspace setting that the run can alter. *Evidence:* policy source, write permissions, and refused self-change.
- **D5-CEILING.** A component outside agent-controlled code enforces a per-agent windowed ceiling on all applicable model, tool, and MCP calls, including local MCP invocations. Test a call beyond the ceiling; an uncovered call class fails. *Evidence:* keyed policy, route coverage, and refusal or hold.[^aix-limitresources]
- **D5-CEILING-TOKENS.** Model usage is charged to a per-agent token budget over a defined window; the mediation point rejects or stops usage that would exceed the budget. A per-call output maximum alone does not meet this outcome. *Evidence:* token accounting, budget configuration, and a stopped or refused call.
- **D5-CEILING-TOOLS.** A component outside agent-controlled code enforces a per-task or per-session ceiling on tool and MCP invocations. Test an invocation beyond it. *Evidence:* keyed counter and refusal. Not applicable without tool calls.[^aix-limitresources]
- **D5-DNS.** The resolver serving the agent answers only names admitted by the agent's destination policy. Test an unlisted name. *Evidence:* resolver policy and refused lookup.[^aix-sandbox]
- **D5-DNS-ONLY.** Network policy routes agent lookups only to that resolver and refuses alternate DNS, DNS over TLS, and unapproved DNS over HTTPS. Test from the agent workload, including a child process. *Evidence:* resolver route policy and refused alternate lookup.
- **D5-GATEWAY.** Network calls to models, tools, and remote MCP servers pass an agent-aware gateway outside agent-controlled code under every permitted autonomy mode. Internal endpoints count. Test a direct connection for each call class; a bypass fails. A local stdio call is not a network call, but its child's outbound connections remain in scope. *Evidence:* route policy, gateway records, and bypass tests.
- **D5-GATEWAY-AUTHZ.** The gateway permits a network tool or remote MCP call only under a rule that binds the calling agent or defined agent group to the named tool. Test another agent and an unlisted tool. *Evidence:* active policy and denied calls. Not applicable without those calls.
- **D5-GATEWAY-SCREEN.** Inline controls on mediated calls apply the organization's restricted-data rule to outbound content and its prompt-injection rule to inbound results before delivery. Test both directions; a retrospective alert alone fails. *Evidence:* rules, route coverage, and blocked or redacted samples.
- **D5-MCP-BROKER.** For remote HTTP MCP, an outside-agent broker mediates each call and validates the credential's issuer, intended MCP resource, and calling principal. A direct or wrong-audience call is refused. For local stdio MCP, the controlled launcher admits only approved server executables, limits inherited credentials, and prevents task input from substituting the command. No HTTP bearer token is required for the local pipe.[^mcp-auth] Test the applicable route with an unapproved server or invalid credential. *Evidence:* remote broker policy and refusal, or local launch policy, environment scope, and denied substitution. Not applicable without MCP.
- **D5-MCP-PIN.** The approved MCP server version or local executable and each tool definition have recorded fingerprints. The client or broker compares the served definitions at connection or launch and alerts on a mismatch. Test a changed definition; a version label without comparison fails. *Evidence:* registry, fingerprint comparison, and alert. Not applicable without MCP.

### L4 detail

- **D5-A2A-BROKER.** Inter-agent messages pass an enforcement point outside both peers that authenticates the sender and validates the protocol envelope before delivery. Direct peer routes are refused. *Evidence:* network policy, broker rules, and a denied direct or malformed message. Not applicable without inter-agent traffic.[^aix-sandbox]
- **D5-A2A-SCREEN.** A control screens inter-agent messages for prompt-injection instructions and restricted data before they enter the receiving agent's context. Test a matched message. *Evidence:* inline rule and refusal or hold. Not applicable without inter-agent traffic.
- **D5-CONTRACT.** Responses from supplied model or AI services are checked against an approved interface contract, including fields the deployment relies on for security decisions; incompatible changes are held or refused. *Evidence:* contract revision, validator, and rejected response. Not applicable without a supplied service.[^aix-supplychainmanage]
- **D5-GATEWAY-EXCHANGE.** The gateway supplies an authenticated network tool with a credential scoped to that tool; it does not forward a broader agent-held token as the tool's authority. Test the token at another tool. *Evidence:* exchange or equivalent issuance record and rejection. Not applicable without authenticated network tools.
- **D5-INSPECT.** For agent-run code that can construct outbound requests, the egress design binds the effective upstream origin to the approved destination and refuses domain fronting or a hidden change of origin inside an allowed connection. TLS termination with inner-host inspection is one implementation; a proxy that constructs and verifies the upstream request or an equivalent endpoint-bound design also qualifies. Test a disallowed origin through an allowed TLS endpoint. *Evidence:* enforcement design, configuration, and refused fronted request. Not applicable when no agent-run code can construct network requests.[^fronting]
- **D5-MCP-CVE.** The operator maps each MCP server and relevant local package version to applicable advisories, supplier notices, and security findings. It records coverage gaps where disclosure is immature. An applicable finding reaches the response owner. Test a seeded finding against the inventory. Absence from one feed is not evidence of safety. *Evidence:* version inventory, intelligence sources and coverage record, matching result, and response ticket. Not applicable without MCP.[^aix-supplychainmanage]
- **D5-MCP-POISON.** The approver screens new and changed MCP tool definitions for model-directed instructions before an agent loads them. Test a poisoned definition, including one present in the first approved version. *Evidence:* review rule, approval record, and held definition. Not applicable without MCP.
- **D5-MCP-RUGPULL.** A changed MCP tool definition cannot reach an agent until an authorized person approves the new fingerprint. Test an unapproved server-side change. *Evidence:* in-path comparison rule and refused definition. Not applicable without MCP.
- **D5-ORCHESTRATE.** An orchestrator that delegates work cannot directly reach a sensitive write, protected data service, or unapproved external destination through a route that bypasses the scope and approval enforced on its delegated path. Authorized direct calls that receive the same policy decision remain allowed. Test the direct path with a call the delegated route would deny. *Evidence:* orchestrator route and policy, plus refusal. Not applicable without delegation.[^aix-oversight]
- **D5-SEGMENT.** An agent that consumes untrusted content can reach sensitive internal services only through the named tool endpoints and network paths approved for its task. Test another internal service. *Evidence:* sensitive-service inventory, segmentation policy, and refusal. Not applicable without untrusted content.[^aix-sandbox]

### L5 detail

- **D5-A2A-SIGN.** For Agent2Agent traffic, the publisher signs each Agent Card the deployment publishes under a documented trust profile, and consumers verify the issuer signature on each card they use before calling its endpoints or capabilities. An unsigned, altered, or untrusted card is refused. General message signatures are not required by this criterion. *Evidence:* signing profile, trusted keys, published and consumed cards, and refusal test. Not applicable without Agent2Agent traffic.[^a2a-card]
- **D5-FEDERATE.** Where agent-aware egress proxies for this deployment operate in two or more clouds, they activate one approved policy revision before the deployment relies on a change. Test a proxy with a stale revision. *Evidence:* approval record, distribution state, active revisions, and cross-proxy decision test. Not applicable to other topologies.
- **D5-FEDERATE-RECONCILE.** For that multi-cloud topology, reconciliation joins each proxy's decisions by agent and policy revision and identifies calls an approved revision would deny. Test an injected mismatch. *Evidence:* decision logs, comparison report, and mismatch result. Not applicable when D5-FEDERATE is not applicable.
- **D5-GATEWAY-ONLY.** Network policy leaves every agent workload and agent-launched child only the approved resolver and mediated routes through the gateway or another policy-enforcing proxy. A central gateway and an agent-adjacent proxy are both valid. Test direct access to an allowed internal service and an unlisted external address; either bypass fails. *Evidence:* workload routes, enforcing policy, and refused direct connections.
- **D5-MCP-QUARANTINE.** A policy classifies an applicable MCP vulnerability or security finding as urgent when its severity, exploitability, and this deployment's reach warrant immediate containment. The control automatically removes that server or local executable from the affected agents' reach before further use; an authorized owner restores it after remedy or accepted mitigation. Test a seeded urgent finding. *Evidence:* classification and admission rules, denied launch or call, and authorized restoration. Not applicable without MCP.
- **D5-RELAY.** An internal service that can make an outbound request from agent-controlled input is confined to approved downstream destinations. Test a supplied URL, redirect, or equivalent relay input that names an unapproved host. A reachable service with no such request path is outside this criterion; a service with an agent-controllable open relay fails. *Evidence:* relay inventory, service egress policy, and refusal.
- **D5-SSRF.** An agent's code or tools cannot reach outside its allowlist by using a direct internal, link-local, or metadata address, a redirect, DNS rebinding, or a forged local name mapping. Test each path the deployment exposes from the agent position. *Evidence:* destination binding rules and refused SSRF attempts.
- **D5-TASK-EGRESS.** Each applicable outbound decision binds the authenticated agent, current task, and approved upstream destination, and rejects another actor, another destination, or a call after the task closes. A token, session, or gateway state can carry the authority; D2-TASKBIND and D3-TASKSCOPE provide its prerequisite inputs. *Evidence:* bound decision inputs and three refusal tests. A supplier-held path requires scoped evidence or an unanswerable verdict.

## Prerequisites and blockers

D5-GATEWAY-AUTHZ needs the calling identity graded in D2. D5-TASK-EGRESS also needs task binding in D2 and task scope in D3. The assessor tests the input at its owner, then tests the refusal at the D5 boundary. A failed prerequisite is recorded as a failed criterion or a blocker to the proposed D5 target, not as a numeric score cap. [[agentic-ai-security-cmm-d8-supply-chain|D8]] tests the released broker, launcher, and Agent Card verification code; [[agentic-ai-security-cmm-d7-observability|D7]] grades the records needed to reconstruct a decision.

## Deployment-shape differences

| Shape | Route and evidence consequence |
|---|---|
| Read-only assistant | Model and retrieval routes remain in scope; MCP and peer criteria follow the actual topology. |
| Local coding agent | Shell children and local stdio MCP processes join the route inventory. Local launch control replaces an HTTP token test on the local pipe. |
| Vendor-managed suite | Tenant connector settings define customer-controlled reach; supplier-held routes need scoped statements or tests. |
| Multi-agent deployment | Authentication, replay, brokering, and screening follow peer messages even when a platform connector carries them. |
| Multi-cloud proxy topology | Policy distribution and reconciliation apply only to proxies that actually enforce this deployment's traffic. |

[[securing-agentic-coding|Securing Agentic Coding]] gives a dated coding-harness example of the route inventory and the proxy's hostname-versus-origin limit; its product settings are evidence for a particular deployment, not D5 criteria.

## Implementation and effort drivers

Start with the route inventory and transitive reach. An agent-aware gateway is useful only if model, tool, child-process, and internal paths cannot bypass it. The operator then tests from each workload position, including a local MCP server process. Per-agent ceilings and tool-scoped credentials add counter and issuance operations. MCP definition approval and vulnerability response add continuing inventory and review work. L5 adds direct-route refusal, task expiry, relay controls, and reconciliation only for applicable topologies. An agent-adjacent proxy can implement these outcomes, but deployment of a sidecar beside every agent is not a maturity condition. Strong network containment still permits a harmful action at an approved destination. D3 grades its authorization and D6 grades the data it carries.

## Sources and material limits

[OWASP AI Exchange](https://owaspai.org/go/agentsandboxing/) supports default-deny network segmentation, monitored proxy or service-mesh routing, controlled DNS, and platform-enforced limits. Its [supply-chain guidance](https://owaspai.org/go/supplychainmanage/) calls for vulnerability monitoring while acknowledging immature disclosure for MCP servers. The [MCP authorization specification](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization) and [A2A specification](https://a2a-protocol.org/latest/specification) define protocol mechanisms; the level outcomes above are this model's assessment choices. These route tests do not substitute for security testing of the gateway, MCP server, or agent runtime.

[^mcp-auth]: [Model Context Protocol, Authorization, 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization), Protocol Requirements and Token Handling. HTTP authorization is optional in the protocol; when used, the protected resource validates token audience. Stdio retrieves credentials from the environment.
[^a2a-card]: [A2A Protocol, Agent Card Signing §8.4](https://a2a-protocol.org/latest/specification). The specification permits JWS Agent Card signatures and sets their verification format; it does not make general per-message signatures a condition of A2A use.
[^aix-limitresources]: [OWASP AI Exchange, Limit Resources](https://owaspai.org/go/limitresources/), platform-enforced limits for model and tool use.
[^aix-oversight]: [OWASP AI Exchange, Oversight](https://owaspai.org/go/oversight/), secure orchestration pattern.
[^aix-sandbox]: [OWASP AI Exchange, Agent Sandboxing and Isolation](https://owaspai.org/go/agentsandboxing/), network segmentation and controlled DNS.
[^aix-supplychainmanage]: [OWASP AI Exchange, Supply Chain Manage](https://owaspai.org/go/supplychainmanage/), supplied-service contract changes and component vulnerability monitoring.
[^fronting]: [Anthropic, Claude Code Sandboxing](https://code.claude.com/docs/en/sandboxing), Security Limitations. A proxy that trusts a client-supplied hostname without inspecting the TLS destination can be bypassed by domain fronting; TLS termination is one possible stronger design.
