---
type: comparison
title: "MCP Exposure Measurements"
address: c-000319
created: 2026-08-22
updated: 2026-08-22
tags:
  - comparisons
  - mcp
  - measurement
  - vulnerability-taxonomy
  - exposure-management
status: developing
scope_axis:
  - sec-of-ai
origin: produced
related:
  - "[[mcp-security]]"
  - "[[mcp-cves-q1-2026]]"
  - "[[tool-poisoning]]"
  - "[[tool-abuse-chains]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[non-human-identity]]"
  - "[[agentshield]]"
  - "[[ai-security-standards-in-q1-2026]]"
sources:
  - "[[.raw/articles/trend-micro-exposed-mcp-servers-2025-07-16.md]]"
  - "[[.raw/articles/trend-micro-exposed-mcp-servers-update-2026-04-28.md]]"
  - "[[.raw/articles/trend-micro-mcp-vulnerability-sweep-2026-05-27.md]]"
  - "[[.raw/articles/endor-labs-mcp-classic-vulnerabilities-2026-01-23.md]]"
---

# MCP Exposure Measurements

Four published measurements of Model Context Protocol risk are in circulation, and they are routinely quoted as if they answered one question. They do not. One counts reachable hosts, one counts source files that call a risky API, one counts defects a reviewer confirmed, and one counts advisories. A control decision drawn from the wrong one buys the wrong control.

**The headline figures differ by a factor of twenty because they measure different populations by different methods.** An 82% API-usage rate[^endor] and a 4% confirmed-exploitable rate[^tm-sweep] are both accurate statements about MCP, and each answers a question the other leaves open.

## The four measurements

| Measurement | Population | Method | Headline figure | As of |
|---|---|---|---|---|
| Deployment posture | Internet-reachable MCP servers | Network scan for unauthenticated endpoints | 1,467 servers with no client authentication or traffic encryption | 2026-04-28 |
| API-usage surface | 2,614 MCP implementations | Static dependency analysis of sensitive API calls | 82% call file-system operations prone to path traversal (CWE-22) | 2026-01-23 |
| Confirmed defect rate | 19,077 open-source MCP repositories | Three-stage AI triage, then manual confirmation of a random sample | ≈4% of repositories hold an exploitable vulnerability | 2026-05-27 |
| Disclosed advisories | Named MCP server projects | CVE and ZDI disclosure records | 30+ MCP CVEs filed January–February 2026[^cve-count] | 2026-02-28 |

## Decision supported by each measurement

**Deployment posture answers whether an attacker can reach an MCP server without credentials.** It is the only one of the four that describes running systems rather than code. It supports exposure-management and network-segmentation decisions, and it is the figure to quote when arguing that MCP servers belong in a private subnet behind an authenticating proxy.

**API-usage surface answers how much risky code the ecosystem contains.** A file-system call marks a place where a vulnerability can arise; it arises when the path escapes canonicalization against a configured boundary. The measure supports triage — which servers deserve review first — and it overstates the ecosystem's defect count if read as a vulnerability rate.

**Confirmed defect rate answers how many repositories a reviewer would actually flag.** It is the closest of the four to the question most readers think they are asking, and it is the hardest to obtain, because it requires manual confirmation.

**Disclosed advisories answer what is already public and patchable.** They undercount by construction: an advisory exists only where someone looked, reported, and got a record assigned.

## Deployment posture

Trend Micro scanned for internet-reachable MCP servers twice. The July 2025 scan found 492 servers running with no client authentication and no traffic encryption, collectively exposing 1,402 tools; more than 90% of those tools offered direct read access to the backing data source, and roughly 74% of the servers were hosted on AWS, Azure, GCP or Oracle.[^tm-2025] At least eight of the 492 servers directly managed cloud-provider resources, exposing tools that list, create, modify and delete them.[^tm-2025]

The April 2026 rescan found 1,467 such servers, close to triple the earlier count.[^tm-2026] Three details from that scan carry more weight than the headline:

- 1,227 of the exposed servers still used the deprecated Server-Sent Events transport, so the population is not tracking protocol updates.[^tm-2026]
- The `execute_sql` tool appeared on 70 hosts and the Graphiti agent-memory implementation on 39, both reachable without authentication.[^tm-2026]
- At least three exposed servers offered a `progress_note` tool addressing patient medical records.[^tm-2026]

The growth figure needs care. It records what a scan found on two dates, and MCP adoption itself grew over the same period, so it evidences insecure adoption outpacing remediation. It does not measure a rate of new misconfiguration.

## Implementation defect rate

Endor Labs analyzed 2,614 MCP implementations and reported that 82% use file-system operations prone to path traversal (CWE-22), 67% use APIs related to code injection (CWE-94), and 34% use APIs related to command injection (CWE-78).[^endor] The published wording is *use ... prone to*. It records that a sensitive API call is present; whether that call is reachable and exploitable is a separate finding. This wiki quoted the figure as "82% vulnerable to path traversal" until 2026-08-22; that phrasing overstated the source and has been corrected on [[mcp-cves-q1-2026|MCP CVEs Q1 2026]] and the pages that repeated it.

Trend Micro's repository sweep measured the other end of the same question. An agent pipeline flagged 17,558 candidate vulnerabilities across 19,077 open-source MCP repositories; a second pass cut that to roughly 15,000; a random sample of 2,287 candidates went to a third agent, which flagged 438, of which manual review confirmed 93.[^tm-sweep] That yields an agent precision of 21.2% and an end-to-end true-positive rate near 4.1%, extrapolating to a point estimate of about 770 repositories with an exploitable vulnerability, in a range of 600 to 1,650.[^tm-sweep] Among the 93 confirmed defects, SQL injection accounted for 26%, remote code execution 22.5%, path traversal 19%, and authentication bypass 7.5%.[^tm-sweep]

The gap between the 82% figure[^endor] and the 4% figure[^tm-sweep] needs no reconciling. The two studies cover different populations — implementations indexed for dependency analysis against open-source repositories on GitHub — and they stop at different points on the same path. Endor Labs stops where a risky call appears; Trend Micro stops where a reviewer confirmed a reachable defect. Most risky calls are correctly bounded, which is why the second number is far smaller than the first.

Trend Micro's sweep also excluded missing server-level authentication from its defect count on the grounds that most MCP servers do not implement it, which routes that condition to the deployment-posture measurement instead of the defect rate.[^tm-sweep] The two measurements partition the problem between them.

## Disclosed cloud vulnerabilities

The April 2026 rescan carried three disclosures filed through the Zero Day Initiative against unofficial AWS and Azure MCP servers, each scored CVSS 9.8: ZDI-CAN-28042 against a Microsoft-affiliated server with no CVE assigned at publication, and CVE-2026-5058 and CVE-2026-5059 against `aws-mcp-server`.[^tm-2026] All three are command injection in a wrapper around a cloud CLI, exploited to run commands on a host that usually carries a broad IAM role.

The wiki already carries the categories these instantiate. Command injection in a CLI wrapper is the mechanism behind the confirmed-defect category [[tool-abuse-chains|tool abuse chains]] describes, and the escalation path — steal instance credentials, move laterally, reach the cloud account — is the egress-plane threat the [[agentic-ai-security-reference-architecture|AAI-S reference architecture]] already treats. The blast radius is what changes. A command-injection defect in an ordinary MCP server reaches that server's data source; the same defect in a cloud-management MCP server reaches the account hosting it, because the process usually runs under a broad IAM role.

A related figure sits behind these disclosures. Trend Micro reports that 48% of more than 19,000 MCP server source trees recommend storing secrets in `.env` files or plaintext JSON, which is what converts a command-injection foothold into credential theft.[^tm-2026]

## Selecting a figure

| Decision | Figure to cite |
|---|---|
| Justifying network segmentation or an authenticating proxy | 1,467 reachable unauthenticated servers, April 2026 |
| Prioritizing which servers to review | 82% call path-traversal-prone file APIs |
| Estimating the ecosystem's real defect burden | ≈4% of repositories, 600–1,650 |
| Arguing a specific server is patchable today | The CVE record for that server |

Citing the API-usage figure to describe defect burden inflates the estimate by roughly twenty times; citing the confirmed-defect rate to describe deployment risk understates it, because an unauthenticated server needs no defect at all to leak its data source.

> [!gap] Unmeasured population
> All four measurements cover public artifacts — reachable hosts, indexed implementations, open-source repositories, filed advisories. None covers MCP servers written inside an organization and never published, which is where most enterprise MCP traffic is expected to run. No published measurement of that population was located as of 2026-08-22.

## Notes

[^cve-count]: The 30+ count is the wiki's own Q1-2026 tally, recorded on [[mcp-cves-q1-2026|MCP CVEs Q1 2026]] and sourced to an internal working paper rather than to a disclosure database. That paper misstated the adjacent Endor Labs statistic (see above), so treat the count as an order-of-magnitude figure pending a check against the CVE record.

[^tm-2025]: Alfredo Oliveira and David Fiser, [*MCP Security: Network-Exposed Servers Are Backdoors to Your Private Data*](https://www.trendmicro.com/vinfo/us/security/news/cybercrime-and-digital-threats/mcp-security-network-exposed-servers-are-backdoors-to-your-private-data), Trend Micro, 2025-07-16. Local copy: `.raw/articles/trend-micro-exposed-mcp-servers-2025-07-16.md`.

[^tm-2026]: Alfredo Oliveira and David Fiser, [*Update on Exposed MCP Servers: The Threat Widens to the Cloud*](https://www.trendmicro.com/vinfo/us/security/news/vulnerabilities-and-exploits/update-on-exposed-mcp-servers-the-threat-widens-to-the-cloud), Trend Micro, 2026-04-28. Local copy: `.raw/articles/trend-micro-exposed-mcp-servers-update-2026-04-28.md`.

[^tm-sweep]: Alfredo Oliveira and David Fiser, [*Hunt Them All: An AI-Powered Vulnerability Sweep of 19,000 MCP Servers*](https://www.trendmicro.com/vinfo/us/security/news/vulnerabilities-and-exploits/hunt-them-all-an-ai-powered-vulnerability-sweep-of-19-000-mcp-servers), Trend Micro, 2026-05-27. Local copy: `.raw/articles/trend-micro-mcp-vulnerability-sweep-2026-05-27.md`.

[^endor]: Peyton Kennedy, [*Classic Vulnerabilities Meet AI Infrastructure: Why MCP Needs AppSec*](https://www.endorlabs.com/learn/classic-vulnerabilities-meet-ai-infrastructure-why-mcp-needs-appsec), Endor Labs, 2026-01-23, restating figures from the Endor Labs [2025 Dependency Management Report](https://www.endorlabs.com/lp/state-of-dependency-management-2025). Local copy: `.raw/articles/endor-labs-mcp-classic-vulnerabilities-2026-01-23.md`.
