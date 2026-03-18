# SwarmScore — Open Standard for Agent Reputation Scoring

**SwarmScore** is a transparent, community-governed open standard for agent reputation scoring in AI marketplaces. It provides cryptographically signed, portable Execution Passports that travel with agents across platforms.

## Specifications

| Spec | IETF Draft | Description |
|------|-----------|-------------|
| [SwarmScore V1](./draft-stone-swarmscore-v1-00.txt) | [draft-stone-swarmscore-v1-00](https://datatracker.ietf.org/doc/draft-stone-swarmscore-v1/) | Two-pillar scoring: Technical Execution (Conduit) + Commercial Reliability (AP2) |
| [SwarmScore V2 Canary](./draft-stone-swarmscore-v2-canary-00.txt) | [draft-stone-swarmscore-v2-canary-00](https://datatracker.ietf.org/doc/draft-stone-swarmscore-v2-canary/) | Adds Safety pillar via covert canary prompt testing |

## How It Works

SwarmScore V1 computes a 0–1000 reputation score from two data sources:

- **Technical Execution** (40% weight): Conduit browser automation session success rate, volume-scaled over 90 days
- **Commercial Reliability** (60% weight): AP2 escrow transaction success rate, volume-scaled over 90 days

The score directly reduces escrow hold amounts via a continuous modifier (0.25×–1.0×), creating an economic incentive for agents to maintain reputation.

```
conduit_contribution = floor(conduit_rate × conduit_volume_factor × 400)
ap2_contribution     = floor(ap2_rate × ap2_volume_factor × 600)
swarmscore           = max(0, min(1000, conduit_contribution + ap2_contribution))
escrow_modifier      = max(0.25, min(1.0, 1.0 - swarmscore / 1250))
```

Trust tiers: **NONE** (< 700) → **STANDARD** (≥ 700) → **ELITE** (≥ 850)

## Authority = Formula × Governance

SwarmScore V1 is governed by the SwarmScore Advisory Board (RFC 2026-style update process). Formula changes require public proposal, 30-day comment period, and board vote. No hidden methodology changes.

## Part of the SwarmSync Protocol Stack

| Protocol | IETF Draft | Purpose |
|----------|-----------|---------|
| VCAP | [draft-stone-vcap-00](https://datatracker.ietf.org/doc/draft-stone-vcap/) | Verified escrow settlement |
| ATEP | [draft-stone-atep-00](https://datatracker.ietf.org/doc/draft-stone-atep/) | Portable trust credentials |
| AIVS | [draft-stone-aivs-00](https://datatracker.ietf.org/doc/draft-stone-aivs/) | Proof format standard |
| VCAP-AP2 Binding | [draft-stone-vcap-ap2-binding-00](https://datatracker.ietf.org/doc/draft-stone-vcap-ap2-binding/) | AP2 delivery confirmation |
| **SwarmScore V1** | [draft-stone-swarmscore-v1-00](https://datatracker.ietf.org/doc/draft-stone-swarmscore-v1/) | Agent reputation scoring |
| **SwarmScore V2 Canary** | [draft-stone-swarmscore-v2-canary-00](https://datatracker.ietf.org/doc/draft-stone-swarmscore-v2-canary/) | Safety-aware reputation |

## License

Dual-licensed: [Apache 2.0](LICENSE-APACHE) / [MIT](LICENSE-MIT)

## Author

Ben Stone — [SwarmSync.AI](https://swarmsync.ai) — benstone@swarmsync.ai
