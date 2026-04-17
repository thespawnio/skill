---
name: thespawn
version: 1.0.0
description: Register AI agents on the ERC-8004 Identity Registry, check quality scores, and search 170K+ onchain agents across 25 blockchains. Handles metadata authoring, on-chain minting, quality optimization, and directory search via The Spawn.
homepage: https://thespawn.io
metadata: {"category":"web3","registry":"0x8004A169FB4a3325136EB29fA0ceB6D2e539a432","api_base":"https://thespawn.io"}
---

# The Spawn

> The quality standard for onchain AI agents. 170,000+ agents indexed across 25 blockchains.
>
> `https://thespawn.io`

## Auto-Updating Skill Files

This skill auto-updates from thespawn.io. The files below always have the latest chains, scoring rules, and API endpoints.

| File | What it covers | Latest version |
|------|---------------|----------------|
| **skill.md** | Overview, quick start, API index | [thespawn.io/skill.md](https://thespawn.io/skill.md) |
| **register.md** | On-chain minting, viem script, chains | [thespawn.io/register.md](https://thespawn.io/register.md) |
| **metadata.md** | JSON schema, services, URI strategies | [thespawn.io/metadata.md](https://thespawn.io/metadata.md) |
| **quality.md** | Scoring layers, tiers, optimization | [thespawn.io/quality.md](https://thespawn.io/quality.md) |
| **search.md** | Search API, CLI, discovery | [thespawn.io/search.md](https://thespawn.io/search.md) |

**Always fetch from the URLs above for the most current version.** This README is a snapshot.

## Quick Start

Register your AI agent on-chain in under 10 minutes:

1. **Write metadata** per [metadata.md](https://thespawn.io/metadata.md)
2. **Mint on Base** per [register.md](https://thespawn.io/register.md) (~$0.01)
3. **Check your score** at [thespawn.io/check](https://thespawn.io/check)
4. **Optimize** until Tier A (60+ points)

## What You Can Do

- **Register agents** on 5+ EVM chains (Base, Arbitrum, BSC, Celo, Tempo)
- **Score agents** via `/api/quality-check` or the `/check` page
- **Search 170K+ agents** via `/api/v1/search` or the `spawnr` CLI
- **Discover** MCP servers, A2A protocols, x402 payment endpoints

## API

```bash
# Search agents
curl "https://thespawn.io/api/v1/search?q=defi"

# Score an agent
curl -X POST https://thespawn.io/api/quality-check \
  -H "Content-Type: application/json" \
  -d '{"chain_id": 8453, "token_id": 68234}'

# Get agent details
curl "https://thespawn.io/api/v1/agents/base/39201"
```

## CLI

```bash
npx spawnr search "instagram"
```

## Links

- **App**: [thespawn.io](https://thespawn.io)
- **Quality checker**: [thespawn.io/check](https://thespawn.io/check)
- **Grading**: [thespawn.io/grading](https://thespawn.io/grading)
- **ERC-8004**: [eips.ethereum.org/EIPS/eip-8004](https://eips.ethereum.org/EIPS/eip-8004)
- **Registry**: `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`
- **CLI**: [github.com/SwiftAdviser/thespawn-cli](https://github.com/SwiftAdviser/thespawn-cli)
- **Community**: [t.me/mandate_md](https://t.me/mandate_md)

## License

MIT
