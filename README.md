# ZION + ChainGuard — Cross-Chain Security Intelligence Agent

> Sovereign crypto intelligence with real-time cross-chain oracle monitoring, built on ElizaOS and powered by Nosana decentralized compute.

Nosana Builders Challenge: ElizaOS Edition submission.

## What It Does

ZION is an autonomous AI agent that combines **real-time market intelligence** with **cross-chain security monitoring**. It reads live data from Chainlink oracles across 6 EVM chains, detects stablecoin depeg events, monitors bridge risk, and generates builder-authority style analysis.

**9 Custom Actions:**

| Action | Description | Data Source |
|--------|-------------|-------------|
| `GET_CRYPTO_PRICE` | Real-time price for 30+ coins | CoinGecko API |
| `GET_FEAR_GREED` | Market sentiment analysis | alternative.me |
| `GET_MARKET_OVERVIEW` | Top 10 assets by market cap | CoinGecko API |
| `GENERATE_MARKET_BRIEF` | Builder-authority content generation | CoinGecko + Fear&Greed |
| `SECURITY_SCAN` | Smart contract security framework | Static knowledge base |
| `CROSS_CHAIN_SCAN` | Oracle health across 6 EVM chains | Chainlink on-chain feeds |
| `STABLECOIN_MONITOR` | USDC/USDT/DAI depeg detection | Chainlink on-chain feeds |
| `BRIDGE_RISK_REPORT` | Bridge TVL and risk analysis | DeFiLlama API |
| `WHALE_ALERT` | Large transfer and volume monitoring | CoinGecko + DeFiLlama |

**2 Context Providers:**
- `market-data-context`: Background BTC/ETH/SOL prices + Fear & Greed
- `chainguard-security-context`: Live ETH oracle price + USDC peg status

## Architecture

```
┌─────────────────────────────────────────────┐
│              ElizaOS Runtime                 │
│                                             │
│  ┌─────────────────┐  ┌──────────────────┐  │
│  │  ZION Plugin    │  │ ChainGuard Plugin│  │
│  │  (5 actions)    │  │  (4 actions)     │  │
│  │  - Prices       │  │  - Oracle Scan   │  │
│  │  - Sentiment    │  │  - Depeg Monitor │  │
│  │  - Market Data  │  │  - Bridge Risk   │  │
│  │  - Content Gen  │  │  - Whale Alert   │  │
│  │  - Security     │  │                  │  │
│  └────────┬────────┘  └────────┬─────────┘  │
│           │                    │             │
│  ┌────────▼────────────────────▼─────────┐  │
│  │         Context Providers              │  │
│  │  market-data + chainguard-security     │  │
│  └────────────────────────────────────────┘  │
└──────────────────┬──────────────────────────┘
                   │
    ┌──────────────┼──────────────────┐
    │              │                  │
    ▼              ▼                  ▼
┌────────┐  ┌──────────┐  ┌──────────────┐
│CoinGecko│  │Chainlink │  │  DeFiLlama   │
│  API    │  │ On-Chain │  │    API       │
│(prices) │  │(6 chains)│  │(bridges/TVL) │
└────────┘  └──────────┘  └──────────────┘
```

## Chains Monitored

| Chain | Oracle Feeds | RPC |
|-------|-------------|-----|
| Ethereum | BTC, ETH, USDC, USDT, DAI, LINK | eth.llamarpc.com |
| Arbitrum | BTC, ETH, USDC, USDT | arb1.arbitrum.io |
| Polygon | BTC, ETH, USDC, USDT | polygon-rpc.com |
| Base | ETH, USDC | mainnet.base.org |
| Avalanche | (supported) | api.avax.network |
| Optimism | (supported) | mainnet.optimism.io |

## Quick Start

```bash
# Clone and install
git clone https://github.com/ElromEvedElElyon/agent-challenge.git
cd agent-challenge
git checkout elizaos-challenge
pnpm install

# Configure
cp .env.example .env
# Edit .env with your settings (Nosana endpoint pre-configured)

# Run locally
pnpm dev
```

## Environment Variables

## Claim Your Nosana Builders Credits

All challenge participants get **free compute credits** to deploy and run their agents on Nosana.

**How to claim:**

1. Visit [nosana.com/builders-credits](https://nosana.com/builders-credits)
2. Sign up or log in with your wallet
3. Your credits will be added to your account automatically
4. Use these credits to deploy your ElizaOS agent to the Nosana network

These credits cover the compute costs for running your agent during the challenge period.

> **Note:** Credits are airdropped twice a day. Please be patient if you don't see them immediately after signing up.

---

## Configure Your LLM

Nosana provides a hosted **Qwen3.5-27B-AWQ-4bit** endpoint for challenge participants. Update your `.env`:

```env
OPENAI_API_KEY=nosana
OPENAI_API_URL=https://6vq2bcqphcansrs9b88ztxfs88oqy7etah2ugudytv2x.node.k8s.prd.nos.ci/v1
MODEL_NAME=Qwen3.5-27B-AWQ-4bit
```
OPENAI_API_KEY=           # Nosana Qwen endpoint (see .env.example)
OPENAI_API_URL=https://3gsrmj6gchzyws9bnc835apd4fh6t5tyeppmbxmzrzhn.node.k8s.prd.nos.ci/v1
```

```env
OPENAI_API_KEY=ollama
OPENAI_API_URL=http://127.0.0.1:11434/v1
MODEL_NAME=qwen3.5:27b
```

---

## Configure Your Embedding Model

Nosana provides a hosted **Qwen3-Embedding-0.6B** endpoint for embeddings (used for RAG, semantic search, and memory). Update your `.env`:

```env
OPENAI_EMBEDDING_URL=https://4yiccatpyxx773jtewo5ccwhw1s2hezq5pehndb6fcfq.node.k8s.prd.nos.ci/v1
OPENAI_EMBEDDING_API_KEY=nosana
OPENAI_EMBEDDING_MODEL=Qwen3-Embedding-0.6B
OPENAI_EMBEDDING_DIMENSIONS=1024
```

**Model Details:**
- **Model ID:** `Qwen3-Embedding-0.6B`
- **Dimensions:** 1024
- **Provider:** Nosana decentralized inference

---

## Customize Your Agent

### 1. Define your agent's character

Edit `characters/agent.character.json` to define your agent's personality, knowledge, and behavior:

```json
{
  "name": "MyAgent",
  "bio": ["Your agent's backstory and capabilities"],
  "system": "Your agent's core instructions and behavior",
  "plugins": ["@elizaos/plugin-bootstrap", "@elizaos/plugin-openai"],
  "clients": ["direct"]
}
```

### 2. Add plugins

Extend your agent by adding plugins to `package.json` and your character file:

| Plugin | Use Case |
|--------|----------|
| `@elizaos/plugin-bootstrap` | Required base plugin |
| `@elizaos/plugin-openai` | OpenAI-compatible LLM (required for Nosana endpoint) |
| `@elizaos/plugin-web-search` | Web search capability |
| `@elizaos/plugin-telegram` | Telegram bot client |
| `@elizaos/plugin-discord` | Discord bot client |
| `@elizaos/plugin-twitter` | Twitter/X integration |
| `@elizaos/plugin-browser` | Browser/web automation |
| `@elizaos/plugin-sql` | Database access |

Install a plugin:
```bash
pnpm add @elizaos/plugin-web-search
```

Add it to your character file:
```json
{
  "plugins": ["@elizaos/plugin-bootstrap", "@elizaos/plugin-openai", "@elizaos/plugin-web-search"]
}
```

### 3. Build custom actions (optional)

Add your own custom logic in `src/index.ts`. See the example plugin already included.

### 4. Persistent storage

SQLite is configured by default — sufficient for development and small-scale agents. For a production-grade personal agent, consider:

- A mounted volume on Nosana
- External database (PostgreSQL, PlanetScale, etc.)
- Decentralized storage (Arweave, IPFS)

---

## Deploy on Nosana

```bash
docker build -t your-image:latest .
docker push your-image:latest
# Use nos_job_def/job.json for Nosana deployment
```

## How ChainGuard Works

The ChainGuard plugin makes **direct eth_call** requests to Chainlink AggregatorV3Interface contracts on each chain. It:

Edit `nos_job_def/nosana_eliza_job_definition.json` and update the Docker image reference:

```json
{
  "version": "0.1",
  "type": "container",
  "meta": {
    "trigger": "cli"
  },
  "ops": [
    {
      "type": "container/run",
      "id": "eliza-agent",
      "args": {
        "image": "yourusername/nosana-eliza-agent:latest",  // <- Change this
        "ports": ["3000:3000"],
        "env": {
          "OPENAI_API_KEY": "nosana",
          "OPENAI_API_URL": "https://6vq2bcqphcansrs9b88ztxfs88oqy7etah2ugudytv2x.node.k8s.prd.nos.ci/v1",
          "MODEL_NAME": "Qwen3.5-27B-AWQ-4bit"
        }
      }
    }
  ]
}
```

> **Security Note:** For production deployments, avoid hardcoding sensitive environment variables. Consider using Nosana secrets management or external secret stores.

### Step 3: Deploy via Nosana Dashboard (Easiest)

This is the recommended method for beginners:

1. Visit the [Nosana Dashboard](https://dashboard.nosana.com/deploy)
2. Connect your Solana wallet (you need this for authentication and using credits)
3. Click **Expand** to open the job definition editor
4. Copy and paste the contents of your `nos_job_def/nosana_eliza_job_definition.json` file
5. Select your preferred compute market:
   - `nvidia-3090` — High performance (recommended for production)
   - `nvidia-rtx-4090` — Premium performance
   - `cpu-only` — Budget option (slower inference)
6. Click **Deploy**
7. Wait for a node to pick up your job (usually 30-60 seconds)
8. Once running, you'll receive a public URL to access your agent

### Step 4: Deploy via Nosana CLI (Advanced)

For developers who prefer the command line or want to automate deployments:

1. First get your API key at [https://deploy.nosana.com/account/](https://deploy.nosana.com/account/)
2. Edit the [Nosana ElizaOS Job Definition File](./nos_job_def/nosana_eliza_job_definition.json)
3. Learn more about [Nosana Job Definition Here](https://learn.nosana.com/deployments/jobs/job-definition/intro.html)

This is **real on-chain data reading**, not API wrappers. The agent directly interacts with smart contracts via public RPC endpoints.

## Tech Stack

- **Runtime**: ElizaOS v2
- **Language**: TypeScript
- **Model**: Qwen3.5-27B-AWQ-4bit (via Nosana)
- **On-chain**: ethers-free raw RPC calls (eth_call)
- **APIs**: CoinGecko, DeFiLlama, alternative.me
- **Deploy**: Docker + Nosana decentralized compute

## Built By

**Padrao Bitcoin Labs** — Building sovereign crypto infrastructure.

---

## Technical Details

- **Runtime**: ElizaOS v2 with Qwen3.5-27B-AWQ-4bit via Nosana inference
- **Plugin API**: Custom TypeScript plugin using `@elizaos/core` Plugin interface
- **External APIs**: CoinGecko (public, free tier), alternative.me (public, free)
- **Coin resolution**: Automatic alias mapping for 30+ common symbols to CoinGecko IDs
- **Content generation**: Three distinct styles per request (data-driven, contrarian, market brief)
- **Security framework**: Top 15 vulnerability checklist with tool recommendations
- **Context provider**: Background market data injected into every conversation turn

---

## Nosana Integration

ZION is built specifically for Nosana's decentralized compute:

- **Inference**: Qwen3.5-27B model served via Nosana's hosted endpoint
- **Deployment**: Docker container deployed to Nosana GPU marketplace
- **Job Definition**: Custom Nosana job definition with environment configuration
- **Compute Market**: Compatible with nvidia-3090 and nvidia-rtx-4090 markets
- **Sovereign by design**: No AWS, no GCP, no Azure. Decentralized compute only.

---

## Built With

- [ElizaOS](https://elizaos.com) -- AI agent framework
- [Nosana](https://nosana.com) -- Decentralized GPU compute
- [Qwen3.5-27B](https://huggingface.co/Qwen/Qwen3.5-27B) -- Language model
- [CoinGecko API](https://www.coingecko.com/en/api) -- Crypto market data
- [Alternative.me](https://alternative.me/crypto/fear-and-greed-index/) -- Fear & Greed Index

---

## Star History

<a href="https://www.star-history.com/?repos=nosana-ci%2Fagent-challenge&type=date&legend=top-left">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/image?repos=nosana-ci/agent-challenge&type=date&theme=dark&legend=top-left" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/image?repos=nosana-ci/agent-challenge&type=date&legend=top-left" />
   <img alt="Star History Chart" src="https://api.star-history.com/image?repos=nosana-ci/agent-challenge&type=date&legend=top-left" />
 </picture>
</a>

## License

MIT

---

**ZION -- Sovereign crypto intelligence. Decentralized compute. Builder-authority signal.**
