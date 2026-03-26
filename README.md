# ZION Personal Agent

**Sovereign crypto intelligence agent built on ElizaOS, deployed on Nosana.**

Nosana Builders Challenge: ElizaOS Edition submission.

---

## What is ZION?

ZION is an autonomous crypto intelligence agent that combines real-time market data, security analysis, and builder-authority content generation into a single personal AI system. It runs on decentralized infrastructure via Nosana, powered by Qwen3.5-27B inference.

ZION is not a chatbot. It is an intelligence system designed for crypto operators who need:

- **Real-time market data** from CoinGecko (10,000+ assets, no API key required)
- **Market sentiment tracking** via the Crypto Fear & Greed Index
- **Security analysis frameworks** for smart contract evaluation
- **Builder-authority content generation** with live data backing every claim
- **Sovereign deployment** on Nosana's decentralized GPU network

---

## Architecture

```
User Request
    |
    v
ElizaOS Runtime (Qwen3.5-27B via Nosana)
    |
    +-- ZION Character (builder-authority persona)
    |
    +-- ZION Plugin (src/index.ts)
    |     |
    |     +-- GET_CRYPTO_PRICE     -> CoinGecko /simple/price
    |     +-- GET_FEAR_GREED       -> alternative.me /fng
    |     +-- GET_MARKET_OVERVIEW  -> CoinGecko /coins/markets
    |     +-- GENERATE_MARKET_BRIEF -> Combined data + content gen
    |     +-- SECURITY_SCAN        -> Vulnerability framework
    |     +-- Market Data Provider -> Background BTC/ETH/SOL context
    |
    +-- @elizaos/plugin-bootstrap (core capabilities)
    +-- @elizaos/plugin-openai (Nosana endpoint)
```

All external API calls use free, public endpoints. No API keys required for core functionality.

---

## Custom Plugin: ZION Crypto Intelligence

The custom plugin (`src/index.ts`) provides five actions and one context provider:

### Actions

| Action | Trigger | Data Source | Description |
|--------|---------|-------------|-------------|
| `GET_CRYPTO_PRICE` | "price of BTC", "how much is ETH" | CoinGecko | Fetches live price, market cap, 24h volume, and 24h change for any of 10,000+ cryptocurrencies |
| `GET_FEAR_GREED` | "market sentiment", "fear and greed" | alternative.me | Returns the current Fear & Greed Index value with builder-authority analysis of what it means |
| `GET_MARKET_OVERVIEW` | "market overview", "top coins" | CoinGecko | Top 10 cryptocurrencies by market cap with prices, changes, and market assessment |
| `GENERATE_MARKET_BRIEF` | "write a tweet", "market brief" | CoinGecko + alternative.me | Generates 3 builder-authority style tweet options using live BTC/ETH/SOL data and Fear & Greed |
| `SECURITY_SCAN` | "is this contract safe", "security check" | Static knowledge | Smart contract security framework with top 15 vulnerabilities, tools, and verification links |

### Provider

| Provider | Description |
|----------|-------------|
| `market-data-context` | Automatically injects current BTC/ETH/SOL prices and Fear & Greed into every conversation for contextual awareness |

### Supported Coins (with aliases)

The price action resolves common aliases automatically: BTC, ETH, SOL, BNB, XRP, ADA, DOGE, DOT, AVAX, MATIC, LINK, UNI, ATOM, NEAR, APT, SUI, ARB, OP, NOS, FIL, AAVE, MKR, LDO, RUNE, and any CoinGecko ID.

---

## Character: ZION

The character definition (`characters/zion-agent.character.json`) establishes ZION's identity:

- **Voice**: Builder-authority style. Short declarative sentences. Data first, interpretation second. No emojis. No hashtags.
- **Domain**: Crypto markets, DeFi, smart contract security, decentralized AI, MCP servers
- **Philosophy**: Sovereign AI for sovereign individuals. Your data, your compute, your control.
- **Knowledge**: 15 embedded knowledge entries covering crypto markets, security vulnerabilities, DeFi protocols, and builder culture
- **Post examples**: 7 builder-authority style sample posts demonstrating the voice

---

## Quick Start

### Prerequisites

- Node.js 23+
- bun or pnpm
- Docker (for deployment)

### Local Development

```bash
# Clone your fork
git clone https://github.com/YOUR-USERNAME/agent-challenge
cd agent-challenge

# Configure environment
cp .env.example .env
# Edit .env with the Nosana endpoint URL

# Install ElizaOS CLI
bun i -g @elizaos/cli

# Start in development mode
elizaos dev
```

Open http://localhost:3000 to interact with ZION.

### Example Interactions

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
You: What is Bitcoin trading at?
ZION: [Fetches live BTC price, market cap, volume, and 24h change from CoinGecko]

You: What is the market sentiment?
ZION: [Returns Fear & Greed Index with builder-authority analysis]

You: Give me a market overview
ZION: [Top 10 coins by market cap with prices and market assessment]

You: Write a tweet about Solana
ZION: [Generates 3 builder-authority style tweet options with live SOL data]

You: Is this contract safe? 0x1234...
ZION: [Provides security framework with Etherscan links and vulnerability checklist]
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

## Deploy to Nosana

### Step 1: Build Docker Image

```bash
docker build -t standardbitcoin10/zion-personal-agent:latest .
docker run -p 3000:3000 --env-file .env standardbitcoin10/zion-personal-agent:latest
# Verify at http://localhost:3000
docker push standardbitcoin10/zion-personal-agent:latest
```

### Step 2: Deploy via Nosana Dashboard

1. Visit [dashboard.nosana.com/deploy](https://dashboard.nosana.com/deploy)
2. Paste the contents of `nos_job_def/nosana_eliza_job_definition.json`
3. Select `nvidia-3090` or `nvidia-rtx-4090` market
4. Deploy and wait for node assignment

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

```bash
npm install -g @nosana/cli

nosana job post \
  --file ./nos_job_def/nosana_eliza_job_definition.json \
  --market nvidia-3090 \
  --timeout 300 \
  --api YOUR_API_KEY
```

---

## Project Structure

```
agent-challenge/
  characters/
    zion-agent.character.json   # ZION character definition
    agent.character.json        # Original template (kept for reference)
  src/
    index.ts                    # ZION Crypto Intelligence Plugin (5 actions + 1 provider)
  nos_job_def/
    nosana_eliza_job_definition.json  # Nosana deployment config
  Dockerfile                    # Container configuration
  .env.example                  # Environment variable template
  package.json                  # Dependencies and scripts
  tsconfig.json                 # TypeScript configuration
```

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
