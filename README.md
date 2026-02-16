# NOFX Skills for OpenClaw

> Agent skills for [NOFX](https://github.com/NoFxAiOS/nofx) — the open-source AI-powered crypto trading operating system.

These skills enable [OpenClaw](https://openclaw.ai) agents to interact with the NOFX platform for automated crypto market analysis, AI-driven trading signals, strategy management, and periodic reporting.

---

## 📦 Skills Included

### [`nofx`](./nofx/)

The core integration skill. Gives your agent full access to the NOFX platform:

- **Market Data** — AI500/AI300 signals, OI rankings, institutional fund flows, funding rates, long-short ratios
- **Strategy Management** — Create, edit, and manage AI trading strategies via browser automation
- **Trader Control** — Start/stop AI traders, monitor P&L, review decision logs
- **AI Debate Arena** — Pit multiple AI models against each other for bull/bear analysis
- **Backtesting** — Run historical strategy backtests with AI models
- **Dashboard** — Portfolio overview, equity curves, position management

### [`nofx-ai500-report`](./nofx-ai500-report/)

Automated market intelligence reporting from the NOFX AI500 scoring system:

- **30-Minute Reports** — Comprehensive market snapshots delivered to Telegram/Discord/Slack
- **15-Minute Monitoring** — New coin entry/exit alerts with detailed analysis
- **Full Data Coverage** — OI changes (7 timeframes), institutional flows, K-line technicals, delta, funding rates
- **Actionable Signals** — Each report includes trading suggestions based on multi-factor analysis

---

## 🚀 Getting Started

### Prerequisites

1. **OpenClaw** — Install from [openclaw.ai](https://openclaw.ai)
2. **NOFX** — Either use the hosted version at [nofxos.ai](https://nofxos.ai) or self-host from [GitHub](https://github.com/NoFxAiOS/nofx)
3. **API Key** — Get your NOFX API key from the platform settings

### Installation

**Option A: Via ClawHub (recommended)**

```bash
clawhub install nofx
clawhub install nofx-ai500-report
```

**Option B: Manual**

```bash
# Clone this repo
git clone https://github.com/NoFxAiOS/nofx-skills.git

# Copy skills to your OpenClaw workspace
cp -r nofx-skills/nofx ~/.openclaw/workspace/skills/
cp -r nofx-skills/nofx-ai500-report ~/.openclaw/workspace/skills/
```

### Configuration

Create `skills/nofx/config.json` in your OpenClaw workspace:

```json
{
  "api_key": "cm_your_api_key_here",
  "base_url": "https://nofxos.ai"
}
```

---

## 📊 NOFX Skill — Usage Guide

### Market Data API

All market data is accessible via the NOFX REST API. Your agent uses these automatically when you ask about crypto markets.

**AI Signals:**
```bash
# AI500 — Top coins scored by AI (score > 70)
curl "https://nofxos.ai/api/ai500/list?auth=$KEY"

# AI300 — Quantitative flow signals (S/A/B levels)
curl "https://nofxos.ai/api/ai300/list?auth=$KEY&limit=10"

# Single coin analysis
curl "https://nofxos.ai/api/ai500/{SYMBOL}?auth=$KEY"
```

**Open Interest:**
```bash
# OI increase ranking
curl "https://nofxos.ai/api/oi/top-ranking?auth=$KEY&limit=10&duration=1h"

# OI decrease ranking
curl "https://nofxos.ai/api/oi/low-ranking?auth=$KEY&limit=10&duration=1h"
```

**Institutional Fund Flow:**
```bash
# Inflow ranking
curl "https://nofxos.ai/api/netflow/top-ranking?auth=$KEY&limit=10&duration=1h&type=institution"

# Outflow ranking
curl "https://nofxos.ai/api/netflow/low-ranking?auth=$KEY&limit=10&duration=1h&type=institution"
```

**Funding Rates & Long-Short Ratio:**
```bash
# Funding rate extremes
curl "https://nofxos.ai/api/funding-rate/top?auth=$KEY&limit=10"
curl "https://nofxos.ai/api/funding-rate/low?auth=$KEY&limit=10"

# Long-short ratio anomalies
curl "https://nofxos.ai/api/long-short/list?auth=$KEY&limit=10"
```

**Duration options:** `1m`, `5m`, `15m`, `30m`, `1h`, `4h`, `8h`, `12h`, `24h`, `2d`, `3d`, `5d`, `7d`

### Strategy Management

Your agent can create and manage trading strategies through the NOFX web interface:

1. **Natural Language** — Describe your strategy in plain English, the agent translates it into a structured config
2. **AI-Powered** — Strategies use AI models to make entry/exit decisions based on market data
3. **Multi-Indicator** — Combine EMA, RSI, ATR, Bollinger Bands, OI, funding rates, and quantitative signals

### Trader Control

- Create AI traders with your preferred model (DeepSeek, Claude, GPT, Gemini, Grok, Qwen, Kimi)
- Connect to any supported exchange (Binance, Bybit, OKX, Bitget, KuCoin, Gate, Hyperliquid, Aster, Lighter)
- Monitor positions, P&L, and trade history in real-time

### AI Debate Arena

Pit multiple AI models against each other:
- Assign roles: Bull, Bear, or Analyst
- Models debate the market outlook for any symbol
- Get consensus recommendations backed by multi-perspective analysis

---

## 📈 AI500 Report Skill — Usage Guide

### Setting Up Automated Reports

Ask your OpenClaw agent to set up AI500 monitoring. It will create two cron jobs:

**1. Market Report (every 30 minutes)**

Generates a comprehensive report including:
- Current AI500 selections with scores and cumulative returns
- Per-coin OI changes across 7 timeframes (5m → 24h)
- Institutional fund flow analysis
- K-line technical analysis (15m/1h/4h) with trend, volume, support/resistance
- OI and fund flow ranking tables (TOP8 increase + TOP8 decrease)
- Actionable trading suggestions

**2. Coin Monitor (every 15 minutes)**

Watches for changes in the AI500 pool:
- 🚨 **New Entry Alert** — Detailed analysis of newly selected coins
- ⚠️ **Removal Notice** — Notification when coins exit the pool
- Silent when no changes occur

### Report Format

Reports use Unicode box-drawing characters in code blocks, optimized for Telegram readability:

```
┌──────────────────────────────────────┐
│ 🪙 BTCUSDT   Score: 85.2  Peak: 88.1
│ Price: $97,500  Entry: $92,100
│ Gain: +5.9%  Rate: 0.01%
├─ OI Changes ─────────────────────────┤
│ 5m:+0.1% │ 15m:+0.3% │ 1h:+1.2%
│ 4h:+3.5% │ 8h:+5.1% │ 24h:+8.7%
├─ Institutional Flow ─────────────────┤
│ 1h:+$57M │ 4h:+$271M │ 24h:+$655M
└──────────────────────────────────────┘
```

### K-line Analysis Method

For each timeframe (15m/1h/4h), the skill fetches 10 candles and computes:
- **Trend Direction** — 3 consecutive candle direction → Uptrend / Downtrend / Sideways
- **Bull/Bear Ratio** — Green vs red candle count out of 10
- **MA Alignment** — MA3 vs MA7 → Bullish / Bearish alignment
- **Volume Change** — Last 3 candles avg vs previous 3 → percentage change
- **Support/Resistance** — Lowest low and highest high of 10 candles

---

## 🏗️ Supported Infrastructure

### Exchanges

| Type | Exchanges |
|------|-----------|
| **CEX** | Binance, Bybit, OKX, Bitget, KuCoin, Gate.io |
| **DEX** | Hyperliquid, Aster DEX, Lighter |

### AI Models

DeepSeek, Qwen, OpenAI (GPT), Anthropic (Claude), Google (Gemini), xAI (Grok), Moonshot (Kimi)

### Delivery Channels

Telegram, Discord, Slack — via OpenClaw's messaging system

---

## 📁 Repository Structure

```
nofx-skills/
├── README.md
├── nofx/
│   ├── SKILL.md              # Core skill definition
│   ├── config.example.json   # Configuration template
│   ├── references/
│   │   ├── api-examples.md   # API response examples
│   │   ├── browser-automation.md
│   │   ├── deployment.md     # NOFX deployment guide
│   │   ├── exchanges.md      # Supported exchanges
│   │   ├── faq.md
│   │   ├── grid-trading.md   # Grid trading guide
│   │   ├── market-charts.md  # Chart analysis
│   │   ├── multi-account.md  # Multi-account management
│   │   ├── strategy-schema.md
│   │   └── webhooks.md       # Notification setup
│   └── scripts/
│       ├── generate-report.sh
│       └── nofx-api.sh
└── nofx-ai500-report/
    ├── SKILL.md              # Report skill definition
    ├── references/
    │   ├── monitor-job.md    # Monitor cron setup
    │   ├── report-job.md     # Report cron setup
    │   └── video-pipeline.md # Video report generation
    └── scripts/
        └── monitor.sh        # Coin monitor script
```

---

## 🔗 Links

| Resource | URL |
|----------|-----|
| NOFX Platform | https://nofxos.ai |
| NOFX Source Code | https://github.com/NoFxAiOS/nofx |
| OpenClaw | https://openclaw.ai |
| ClawHub | https://clawhub.com |
| API Documentation | https://nofxos.ai/api-docs |

---

## 📄 License

MIT — Same as NOFX.
