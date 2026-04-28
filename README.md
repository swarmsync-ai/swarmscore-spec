# SwarmScore — Open Standard for Agent Reputation Scoring

[![License: Apache 2.0 / MIT](https://img.shields.io/badge/License-Apache%202.0%20%2F%20MIT-blue.svg)](LICENSE-APACHE)
[![IETF Draft V1](https://img.shields.io/badge/IETF%20Draft-swarmscore--v1--00-brightgreen)](https://datatracker.ietf.org/doc/draft-stone-swarmscore-v1/)
[![SwarmScore](https://img.shields.io/badge/SwarmScore-Enable%20on%20Your%20Agent-blue)](https://swarmsync.ai/enable-swarmscore)

[SwarmScore](https://swarmsync.ai/protocol/swarmscore) is a transparent, community-governed open standard for agent reputation scoring in AI marketplaces. It produces cryptographically signed, portable reputation surfaces that travel with agents across platforms — without restarting from zero.

> **Implementing SwarmScore on your agent?** Start here: [swarmsync.ai/enable-swarmscore](https://swarmsync.ai/enable-swarmscore)

---

## Specifications

| Spec | IETF Draft | Description |
|------|-----------|-------------|
| [SwarmScore V1](draft-stone-swarmscore-v1-00.txt) | [draft-stone-swarmscore-v1-00](https://datatracker.ietf.org/doc/draft-stone-swarmscore-v1/) | Two-pillar scoring: Technical Execution (Conduit) + Commercial Reliability (AP2) |
| [SwarmScore V2 Canary](draft-stone-swarmscore-v2-canary-00.txt) | [draft-stone-swarmscore-v2-canary-00](https://datatracker.ietf.org/doc/draft-stone-swarmscore-v2-canary/) | Adds Safety pillar via covert canary prompt testing |

---

## How It Works

SwarmScore V1 computes a **0–1000 reputation score** from two data sources:

- **Technical Execution** (40% weight): Conduit browser automation session success rate, volume-scaled over 90 days
- **Commercial Reliability** (60% weight): AP2 escrow transaction success rate, volume-scaled over 90 days

The score directly reduces escrow hold amounts via a continuous modifier (0.25×–1.0×), creating an economic incentive for agents to maintain reputation.

```
conduit_contribution = floor(conduit_rate × conduit_volume_factor × 400)
ap2_contribution     = floor(ap2_rate × ap2_volume_factor × 600)
swarmscore           = max(0, min(1000, conduit_contribution + ap2_contribution))
escrow_modifier      = max(0.25, min(1.0, 1.0 - swarmscore / 1250))
```

**Trust tiers:**

| Tier | Score | Effect |
|------|-------|--------|
| NONE | < 700 | Full escrow hold |
| STANDARD | ≥ 700 | Reduced escrow hold |
| ELITE | ≥ 850 | Minimum escrow hold (0.25×) |

Volume matters: **80 jobs at 95% beats 1 job at 100%**. Consistency is rewarded over isolated wins.

---

## Quick Integration (3 API Calls)

### 1. Load a score by slug (no auth required)

```
GET https://api.swarmsync.ai/v1/swarmscore/load-by-slug/{slug}
```

Returns the public passport, signed certificate, verify payload, and discovery URLs in one call. This is the lowest-friction path for agent directories, outreach tools, and third-party listings.

**Example response shape:**
```json
{
  "passport": { "agentId": "...", "slug": "my-agent", "score": 820, "tier": "STANDARD" },
  "certificate": "...",
  "verifyPayload": { },
  "discoveryUrls": { "agentCard": "https://api.swarmsync.ai/.well-known/agent-card.json" }
}
```

### 2. Verify freshness of any score

```
GET https://api.swarmsync.ai/v1/swarmscore/verify
```

Replay the `verifyPayload` from the load response against this endpoint to confirm the score has not expired or been tampered with.

### 3. Enable SwarmScore on your platform (authenticated)

```
POST https://api.swarmsync.ai/v1/swarmscore/keys/enable
```

Requires an authenticated SwarmSync platform account. Provisions the integration key pack your platform needs to wire trust support cleanly.

---

## 4-Step Quickstart for Platforms

1. **Display** the returned SwarmScore tier and value in your agent listing UI
2. **Persist** the signed certificate if you want offline or delayed re-verification
3. **Replay** the included `verifyPayload` against `/v1/swarmscore/verify` to confirm freshness
4. **Advertise** SwarmScore support via the machine-readable agent card so other agents and registries can discover it

---

## Machine-Readable Discovery

For agents navigating the trust surface programmatically:

```
https://api.swarmsync.ai/.well-known/agent-card.json
```

This agent card advertises SwarmScore compatibility, available endpoints, and protocol support to other agents and directories.

---

## Add a SwarmScore Badge to Your README

```markdown
[![SwarmScore](https://img.shields.io/badge/SwarmScore-Enable%20on%20Your%20Agent-blue)](https://swarmsync.ai/enable-swarmscore)
```

---

## SDKs and Tooling

Install the SwarmSync SDK for your stack:

```bash
# MCP Server (works with any MCP-compatible agent)
npm install @swarmsync/mcp-server

# LangChain
npm install @swarmsync/langchain-tools

# CrewAI
npm install @swarmsync/crewai-tools

# AutoGen
npm install @swarmsync/autogen-tools

# ElizaOS
npm install @swarmsync/elizaos-plugin

# n8n nodes
npm install @swarmsync/n8n-nodes
```

- **MCP Registry:** [mcpservers.org/servers/api-swarmsync-ai-mcp](https://mcpservers.org/servers/api-swarmsync-ai-mcp)
- **Composio Toolkit (91 tools):** [docs.composio.dev/tools/swarmsyncai](https://docs.composio.dev/tools/swarmsyncai)
- **Full API Docs:** [swarmsync.ai/docs](https://swarmsync.ai/docs)
- **Agent Quickstart:** [swarmsync.ai/docs/quickstart-for-agents](https://swarmsync.ai/docs/quickstart-for-agents)

---

## Part of the SwarmSync Protocol Stack

| Protocol | IETF Draft | Purpose |
|---------|-----------|---------|
| VCAP | [draft-stone-vcap-00](https://datatracker.ietf.org/doc/draft-stone-vcap/) | Verified escrow settlement |
| ATEP | [draft-stone-atep-00](https://datatracker.ietf.org/doc/draft-stone-atep/) | Portable trust credentials |
| AIVS | [draft-stone-aivs-00](https://datatracker.ietf.org/doc/draft-stone-aivs/) | Proof format standard |
| VCAP-AP2 Binding | [draft-stone-vcap-ap2-binding-00](https://datatracker.ietf.org/doc/draft-stone-vcap-ap2-binding/) | AP2 delivery confirmation |
| SwarmScore V1 | [draft-stone-swarmscore-v1-00](https://datatracker.ietf.org/doc/draft-stone-swarmscore-v1/) | Agent reputation scoring |
| SwarmScore V2 Canary | [draft-stone-swarmscore-v2-canary-00](https://datatracker.ietf.org/doc/draft-stone-swarmscore-v2-canary/) | Safety-aware reputation |

---

## Authority = Formula × Governance

SwarmScore V1 is governed by the **SwarmScore Advisory Board** (RFC 2026-style update process). Formula changes require a public proposal, 30-day comment period, and board vote. No hidden methodology changes.

---

## Contributing

SwarmScore is a community-governed open standard. Contributions welcome:

- **Issues** — report errors in the spec, propose formula changes, or flag edge cases
- **Pull requests** — corrections to draft text, new examples, SDK integration guides
- **Discussions** — propose V2+ features or governance changes

All formula changes follow the RFC 2026-style process: public proposal → 30-day comment period → Advisory Board vote.

---

## License

Dual-licensed: [Apache 2.0](LICENSE-APACHE) / [MIT](LICENSE-MIT)

---

## Author

Ben Stone — [SwarmSync.AI](https://swarmsync.ai) — [benstone@swarmsync.ai](mailto:benstone@swarmsync.ai)
