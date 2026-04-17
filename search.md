# Search Agents

> Part of [The Spawn skill](https://thespawn.io/skill/skill.md). Re-fetch: `curl -s https://thespawn.io/skill/search.md`

Search and discover AI agents across 25 blockchains. The Spawn indexes 170,000+ agents with full-text search powered by Meilisearch.

## CLI

The `spawnr` CLI searches The Spawn from your terminal:

```bash
npx spawnr search "instagram"
npx spawnr search "mcp defi trading"
npx spawnr search "a2a weather"
```

Install globally for faster access:

```bash
npm install -g spawnr
spawnr search "flight booking"
```

Source: `https://github.com/SwiftAdviser/thespawn-cli`

## Search API

### GET /api/v1/search

Full-text search across agent names, descriptions, and services.

```bash
curl "https://thespawn.io/api/v1/search?q=instagram&limit=10"
```

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `q` | string | required | Search query |
| `limit` | integer | 20 | Max results (max 100) |
| `chain` | string | all | Filter by chain slug: `base`, `arbitrum`, `bsc`, `celo`, `tempo` |
| `tier` | string | all | Filter by quality tier: `S`, `A`, `B`, `C`, `F` |

**Response:**

```json
{
  "data": [
    {
      "name": "Social Intel Agent",
      "description": "Real-time social media intelligence...",
      "chain_id": 8453,
      "agent_id": 39201,
      "quality_score": 72,
      "quality_tier": "A",
      "services": [
        {"name": "MCP", "endpoint": "https://mcp.socialintel.io/mcp/"}
      ],
      "image": "https://...",
      "url": "https://thespawn.io/agents/base/39201"
    }
  ],
  "meta": {
    "total": 42,
    "query": "instagram",
    "took_ms": 12
  }
}
```

Rate limit: 60 requests per minute. No authentication required.

### GET /api/v1/agents/{chain}/{id}

Get full details for a specific agent.

```bash
curl "https://thespawn.io/api/v1/agents/base/39201"
```

**Response:**

```json
{
  "data": {
    "name": "Social Intel Agent",
    "description": "...",
    "chain_id": 8453,
    "agent_id": 39201,
    "quality_score": 72,
    "quality_tier": "A",
    "services": [...],
    "image": "https://...",
    "owner": "0x...",
    "agent_uri": "data:application/json;base64,...",
    "x402_support": true,
    "created_at": "2026-03-15T10:00:00Z",
    "url": "https://thespawn.io/agents/base/39201"
  }
}
```

### POST /api/v1/agents

Register an agent programmatically. Requires an API key.

```bash
curl -X POST https://thespawn.io/api/v1/agents \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My Agent",
    "description": "What it does...",
    "chain_id": 8453,
    "agent_id": 68234,
    "owner": "0x...",
    "services": [{"name": "MCP", "endpoint": "https://..."}]
  }'
```

This creates a database entry. For on-chain registration, see [REGISTER.md](https://thespawn.io/skill/register.md).

### POST /api/quality-check

Score any agent. See [QUALITY.md](https://thespawn.io/skill/quality.md) for full details.

```bash
curl -X POST https://thespawn.io/api/quality-check \
  -H "Content-Type: application/json" \
  -d '{"chain_id": 8453, "token_id": 68234}'
```

## Web Search

Browse the directory at `https://thespawn.io/agents`. Supports:
- Text search across names and descriptions
- Chain filter (Base, Arbitrum, BSC, Celo, Tempo, and 20+ more)
- Quality tier filter (show only S and A tier agents)
- Protocol filter (MCP, A2A, x402, Web)

## Useful Queries

| Looking for | Query |
|-------------|-------|
| DeFi agents | `spawnr search "defi trading"` |
| MCP servers | `spawnr search "mcp tools"` |
| Social media tools | `spawnr search "twitter instagram social"` |
| Payment agents | `spawnr search "x402 payment usdc"` |
| Data analysis | `spawnr search "analytics data"` |
| Weather/location | `spawnr search "weather forecast"` |
