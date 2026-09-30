# Chapter 01 — The Five Data Sources

## Learning goals
- State what data each source provides and its role in the briefing
- Obtain 4 free API keys, and know how to trim the single paid source

## Source panorama

| Source | Data | Role | Cost | Auth |
|---|---|---|---|---|
| **Twelve Data** | Daily candles (last 2 bars) for SPY/QQQ/VGK/EEM/GLD/TLT/EUR/USD | Cross-asset prices & % change (core) | Free tier: 800 req/day, 8 req/min | query `apikey` |
| **CoinGecko** | BTC/ETH USD price + 24h change | Crypto block (core) | Free (demo key or keyless public endpoint) | header |
| **FRED** | US 10Y treasury yield DGS10, last 3 observations | Rates anchor (core) | Free registration | query `api_key` |
| **QuantGist** | Macro event calendar: CPI/NFP/FOMC/PCE | Today's events (non-core) | ⚠️ **Paid** | header |
| **Marketaux** | English financial news (US/DE, symbol-filtered) | Headlines (non-core) | Free tier: 100 req/day | query `api_token` |

> "Core" = `core: true`: a failed core source lands in the `core_source_failures` list and the AI prompt receives an explicit warning; a failed non-core source doesn't stop the briefing.

## Getting the free keys (under 5 minutes each)

1. **Twelve Data**: twelvedata.com → sign up with email → the API key is shown on the Dashboard.
2. **FRED**: fred.stlouisfed.org → register → My Account → API Keys → Request.
3. **Marketaux**: marketaux.com → register → grab the token on the Dashboard (100 free calls/day is plenty for a daily brief).
4. **CoinGecko**: this course uses the public endpoint; for more stability register a free Demo key and pass it in header `x-cg-demo-api-key`.

## Credential wiring (n8n side)

| Template credential placeholder | n8n credential type | What to fill |
|---|---|---|
| Twelve Data API | HTTP Query Auth | name=`apikey`, value=your key |
| Coingecko Demo API | HTTP Header Auth | name=`x-cg-demo-api-key` (keyless run: set the node's authentication to none) |
| FRED API | HTTP Query Auth | name=`api_key` |
| QuantGist API | HTTP Header Auth | (delete the branch when trimming, see below) |
| Marketaux API | HTTP Query Auth | name=`api_token` |

## Trimming the paid source: the standard drill for removing QuantGist

1. Delete the nodes "Fetch QuantGist Macro Calendar" and "Normalize QuantGist Calendar Data".
2. Change the Merge node's `numberInputs` from 5 to 4.
3. **Leave the Prepare node untouched** — it automatically synthesizes an `error` placeholder for the missing quantgist branch (designed-in fault tolerance, verified in L2).
4. Free macro-calendar alternatives (advanced): add a few macro series from FRED (CPI/unemployment), or use a public ICS release calendar.

> ⚠️ Reverse lesson: if you delete the Fetch node but leave Normalize behind, Normalize receives empty input and outputs `empty` — still safe, but dead nodes on the canvas are sloppy delivery. Don't ship that.

## Chapter check
- You have the 4 free keys (or confirmed the CoinGecko keyless path);
- Without looking, you can name the three core sources and explain why core vs non-core is tiered.