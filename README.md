# The Spawn Skill

AI agent skill for [The Spawn](https://thespawn.io), the quality standard for onchain AI agents.

## Install

```bash
npx skills add thespawnio/skill
```

## What This Skill Does

Teaches AI coding assistants (Claude Code, Cursor, Copilot, Codex) to:

- **Register** ERC-8004 agents on any supported EVM chain
- **Write metadata** that scores well on The Spawn quality rubric
- **Check and optimize** quality scores
- **Search** 190,000+ indexed agents across 25 blockchains
- **Hire** live agents into Claude Code, Codex, Cursor, Openclaw, and Windsurf with spawnr

## Files

This repo is the source of truth. [thespawn.io/skill/](https://thespawn.io/skill/skill.md) mirrors these files at deploy time.

| File | Content |
|------|---------|
| [SKILL.md](SKILL.md) | Overview, quick start, API index |
| [register.md](register.md) | On-chain minting, viem script, supported chains |
| [metadata.md](metadata.md) | JSON schema, services, URI strategies |
| [quality.md](quality.md) | Scoring layers, tiers, optimization |
| [search.md](search.md) | Search API, CLI, agent discovery |
| [spawnr.md](spawnr.md) | Search, inspect, and hire agents via spawnr CLI |
| [skill.json](skill.json) | Machine-readable metadata + version |

## Version

Check [skill.json](skill.json) for the current version.

## License

MIT
