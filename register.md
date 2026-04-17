# Register an Agent

> Part of [The Spawn skill](https://thespawn.io/skill/skill.md). Re-fetch: `curl -s https://thespawn.io/skill/register.md`

Register your AI agent on the ERC-8004 Identity Registry. One on-chain transaction, any supported chain, under $0.05.

## What You Will Do

1. Prepare a funded EVM wallet (any chain in the table below).
2. Write metadata JSON that scores Tier A on The Spawn rubric. See [METADATA.md](https://thespawn.io/skill/metadata.md).
3. Send ONE on-chain transaction: `register(string agentURI)` on the ERC-8004 Identity Registry.
4. Verify on-chain, then verify the agent appears at `https://thespawn.io/agents/{chain}/{agent_id}` within about 5 minutes.

No hosted backend, no API key, no custodial mint.

## Supported Chains

| Chain | chainId | Registry | Mint cost | RPC (public) |
|-------|--------:|----------|----------:|--------------|
| Base | 8453 | `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432` | ~$0.01 | `https://mainnet.base.org` |
| Arbitrum | 42161 | `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432` | ~$0.02 | `https://arb1.arbitrum.io/rpc` |
| BSC | 56 | `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432` | ~$0.05 | `https://bsc-dataseed1.binance.org` |
| Celo | 42220 | `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432` | ~$0.01 | `https://forno.celo.org` |
| Tempo | 4217 | `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432` | ~$0.01 | `https://tempo-mainnet.drpc.org` |

**Default: Base.** Cheapest gas, widest wallet support, fastest indexer.

**Do not mint on testnets.** The Spawn does not index them. A testnet mint succeeds on-chain but never appears on thespawn.io.

## Prerequisites

1. **A funded EVM wallet** on one of the chains above. You need the raw private key. For Base, about $0.05 worth of ETH is plenty.
2. **Bun or Node.js 20+** with `viem`:

   ```bash
   mkdir my-agent && cd my-agent
   bun init -y && bun add viem dotenv
   ```

3. **A `.env` file** next to your script:

   ```
   MINT_PRIVATE_KEY=0xYOUR_PRIVATE_KEY_HERE
   ```

No SDK required. The raw viem path below works on every indexed chain.

## Mint Script

The ERC-8004 registry uses a two-transaction pattern: `register()` returns your new agent ID, then `setAgentURI(id, uri)` stores the metadata.

Create `mint.ts`:

```typescript
import 'dotenv/config';
import {
  createPublicClient,
  createWalletClient,
  http,
  parseAbi,
  parseEventLogs,
} from 'viem';
import { privateKeyToAccount } from 'viem/accounts';
import { base } from 'viem/chains';
import { readFileSync, writeFileSync } from 'fs';

const REGISTRY = '0x8004A169FB4a3325136EB29fA0ceB6D2e539a432' as const;

const REGISTRY_ABI = parseAbi([
  'function register() returns (uint256)',
  'function setAgentURI(uint256 agentId, string newURI)',
  'function tokenURI(uint256 tokenId) view returns (string)',
  'function ownerOf(uint256 tokenId) view returns (address)',
  'event Registered(uint256 indexed agentId, string agentURI, address indexed owner)',
]);

async function main() {
  const pk = process.env.MINT_PRIVATE_KEY as `0x${string}`;
  if (!pk) throw new Error('Set MINT_PRIVATE_KEY in .env');

  const account = privateKeyToAccount(pk);
  const chain = base; // swap for arbitrum, bsc, celo, or tempo as needed

  const walletClient = createWalletClient({ account, chain, transport: http() });
  const publicClient = createPublicClient({ chain, transport: http() });

  // Load metadata and build a data: URI (immutable, on-chain)
  const metadata = JSON.parse(readFileSync('metadata.json', 'utf8'));
  const metadataJson = JSON.stringify(metadata);
  const agentURI = `data:application/json;base64,${Buffer.from(metadataJson).toString('base64')}`;

  console.log('Chain:        ', chain.name, `(${chain.id})`);
  console.log('From:         ', account.address);
  console.log('Metadata size:', metadataJson.length, 'bytes');
  console.log('Registry:     ', REGISTRY);

  // Tx 1: register()
  console.log('\nTx 1: register() ...');
  const tx1 = await walletClient.writeContract({
    address: REGISTRY, abi: REGISTRY_ABI, functionName: 'register', args: [],
  });
  console.log('  tx:', tx1);
  const rc1 = await publicClient.waitForTransactionReceipt({ hash: tx1 });
  const events = parseEventLogs({ abi: REGISTRY_ABI, logs: rc1.logs, eventName: 'Registered' });
  const registered = events[0];
  if (!registered) throw new Error('No Registered event.');
  const agentId = registered.args.agentId;
  console.log('  agent id:', agentId.toString());

  // Tx 2: setAgentURI(agentId, dataUri)
  console.log('\nTx 2: setAgentURI(' + agentId.toString() + ', <dataUri>) ...');
  const tx2 = await walletClient.writeContract({
    address: REGISTRY, abi: REGISTRY_ABI, functionName: 'setAgentURI',
    args: [agentId, agentURI],
  });
  console.log('  tx:', tx2);
  const rc2 = await publicClient.waitForTransactionReceipt({ hash: tx2 });
  console.log('  block:', rc2.blockNumber.toString(), 'gas:', rc2.gasUsed.toString());

  // Verify on-chain
  const onchainURI = await publicClient.readContract({
    address: REGISTRY, abi: REGISTRY_ABI, functionName: 'tokenURI', args: [agentId],
  });
  const owner = await publicClient.readContract({
    address: REGISTRY, abi: REGISTRY_ABI, functionName: 'ownerOf', args: [agentId],
  });
  if (owner.toLowerCase() !== account.address.toLowerCase()) throw new Error('Owner mismatch');
  if (onchainURI !== agentURI) throw new Error('tokenURI mismatch');
  console.log('\nVerified on-chain: ownerOf + tokenURI OK.');

  const slug = chain.id === 8453 ? 'base'
    : chain.id === 42161 ? 'arbitrum'
    : chain.id === 56 ? 'bsc'
    : chain.id === 42220 ? 'celo'
    : chain.id === 4217 ? 'tempo'
    : chain.id.toString();

  const out = {
    chainId: chain.id, chainSlug: slug,
    agentId: agentId.toString(), owner,
    registry: REGISTRY,
    registerTxHash: tx1, setUriTxHash: tx2,
    blockNumber: rc2.blockNumber.toString(),
    totalGas: (rc1.gasUsed + rc2.gasUsed).toString(),
    agentSpawnUrl: `https://thespawn.io/agents/${slug}/${agentId.toString()}`,
  };
  writeFileSync('mint-receipt.json', JSON.stringify(out, null, 2));
  console.log('\nView on The Spawn in ~5 min: ' + out.agentSpawnUrl);
}

main().catch(e => { console.error(e); process.exit(1); });
```

Run:

```bash
bun run mint.ts
```

Total cost on Base: about $0.01 (~1.1M gas across both txs).

## Verify

**On-chain** (any public RPC):

```bash
ID=68234
curl -s -X POST https://mainnet.base.org \
  -H 'Content-Type: application/json' \
  -d "{\"jsonrpc\":\"2.0\",\"method\":\"eth_call\",\"params\":[{\"to\":\"0x8004A169FB4a3325136EB29fA0ceB6D2e539a432\",\"data\":\"0xc87b56dd$(printf '%064x' $ID)\"},\"latest\"],\"id\":1}"
```

**On The Spawn:**

```bash
curl -sI https://thespawn.io/agents/base/68234
# Expect: HTTP/2 200 within 5 minutes of the mint
```

## Alternative: agent0-sdk

If you prefer a higher-level API:

```typescript
import { SDK } from 'agent0-sdk';
import { privateKeyToAccount } from 'viem/accounts';

const sdk = new SDK({
  chainId: 8453,
  rpcUrl: 'https://mainnet.base.org',
  signer: privateKeyToAccount(process.env.MINT_PRIVATE_KEY as `0x${string}`),
});

const agent = sdk.createAgent('Name', 'Description...', 'https://image.url');
agent.setActive(true);
agent.setTrust(true);
const result = await agent.registerHTTP('https://example.com/metadata.json');
console.log(`Agent ID: ${result.agentId}`);
```

Install: `bun add agent0-sdk viem`

The raw viem path is recommended because it works on every chain and has no hidden surprises.

## What NOT to Do

- **Do not mint on testnets.** The Spawn does not index them.
- **Do not POST to `/api/agents/register`** expecting an on-chain mint. That endpoint writes a database row for preview, not a transaction.
- **Do not skip the description.** Empty or one-sentence descriptions knock you out of Tier A.
- **Do not set `x402Support: true` unless the endpoint returns HTTP 402.** The scorer checks.

## Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| Tx succeeds with 21000 gas, no event | Sent to an EOA, wrong address | Check the chain table above |
| `register` function not found | ABI mismatch or wrong chain | Use the ABI above verbatim |
| 404 after 10 minutes | Indexer catching up | Check the agent page manually |
| Score is Tier B or worse | Liveness layer is 0 | Add a working MCP `tools/list` endpoint |
