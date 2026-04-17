---
name: thespawn
version: 1.0.0
description: Register AI agents on the ERC-8004 Identity Registry, check quality scores, and search 170K+ onchain agents across 25 blockchains. Handles metadata authoring, on-chain minting, quality optimization, and directory search via The Spawn.
homepage: https://thespawn.io
metadata: {"category":"web3","registry":"0x8004A169FB4a3325136EB29fA0ceB6D2e539a432","api_base":"https://thespawn.io"}
---

# The Spawn

> **Site:** `https://thespawn.io` | **Site map:** [llms.txt](https://thespawn.io/llms.txt)
>
> The quality standard for onchain AI agents. 170,000+ agents indexed across 25 blockchains.

## Skill Files

This repo is the source of truth. thespawn.io mirrors these files at deploy time.

| File | Content | Mirror |
|------|---------|--------|
| **SKILL.md** (this file) | Overview, quick start, API index | [thespawn.io/skill/skill.md](https://thespawn.io/skill/skill.md) |
| **register.md** | On-chain minting, viem script, chains | [thespawn.io/skill/register.md](https://thespawn.io/skill/register.md) |
| **metadata.md** | JSON schema, services, URI strategies | [thespawn.io/skill/metadata.md](https://thespawn.io/skill/metadata.md) |
| **quality.md** | Scoring layers, tiers, optimization | [thespawn.io/skill/quality.md](https://thespawn.io/skill/quality.md) |
| **search.md** | Search API, CLI, discovery | [thespawn.io/skill/search.md](https://thespawn.io/skill/search.md) |
| **skill.json** | Machine-readable metadata + version | [thespawn.io/skill/skill.json](https://thespawn.io/skill/skill.json) |

**Install:**
```bash
npx skills add thespawnio/skill
```

**Check for updates:** compare `version` in [skill.json](skill.json) against your local copy.

## What The Spawn Does

The Spawn is the quality standard for ERC-8004 onchain AI agents. We index every agent registered on 25 blockchains, grade them on a 0-100 quality scale, and surface the best ones.

**For agent builders:** Register your agent on-chain and get graded. High-quality agents get featured, searchable, and integrated into developer tools.

**For agent consumers:** Search and discover working agents by capability: MCP servers, A2A protocols, x402 payment endpoints, and more.

## What You Can Do

| Action | Guide | When to use |
|--------|-------|-------------|
| **Register an agent** | [register.md](register.md) | Mint a new ERC-8004 agent on any supported chain |
| **Write metadata** | [metadata.md](metadata.md) | Author the JSON that defines your agent's identity and services |
| **Optimize quality** | [quality.md](quality.md) | Understand and improve your agent's quality score |
| **Search agents** | [search.md](search.md) | Find agents by name, capability, chain, or protocol |

## Quick Start: Register an Agent

The fastest path from zero to a live, indexed agent:

1. **Write metadata** following [metadata.md](metadata.md)
2. **Mint on-chain** following [register.md](register.md)
3. **Check your score** at `https://thespawn.io/check` or via [quality.md](quality.md)
4. **Optimize** until you hit Tier A (60+ points)

Total time: under 10 minutes. Total cost on Base: about $0.01.

## Supported Chains

| Chain | chainId | Typical mint cost |
|-------|--------:|------------------:|
| Base | 8453 | ~$0.01 |
| Arbitrum | 42161 | ~$0.02 |
| BSC | 56 | ~$0.05 |
| Celo | 42220 | ~$0.01 |
| Tempo | 4217 | ~$0.01 |

**Default: Base.** Cheapest gas, widest wallet support, fastest indexer.

Full chain table with RPCs and registry addresses in [register.md](register.md).

## API Endpoints

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/api/v1/search?q=...` | GET | None | Search agents by keyword |
| `/api/v1/agents/{chain}/{id}` | GET | None | Get agent details |
| `/api/v1/agents` | POST | API key | Register agent programmatically |
| `/api/quality-check` | POST | None | Score an agent |

Full API reference in [search.md](search.md).

## CLI

```bash
npx spawnr search "instagram"
npx spawnr search "mcp defi"
```

## Links

- App: `https://thespawn.io`
- Quality checker: `https://thespawn.io/check`
- Grading methodology: `https://thespawn.io/grading`
- Manifesto: `https://thespawn.io/manifesto`
- ERC-8004 spec: `https://eips.ethereum.org/EIPS/eip-8004`
- Registry contract: `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`
- Community: `https://t.me/mandate_md`
- CLI: `https://github.com/SwiftAdviser/thespawn-cli`

## License

MIT
