# Lecture 3: The Five Data Sources: Roles, Costs & Core vs Non-Core
Section: The Five Data Sources

What each of the five sources contributes to the briefing — Twelve Data cross-asset candles, CoinGecko crypto, FRED yields, QuantGist macro calendar, Marketaux news — and why the core/non-core tiering matters.

Strong automation starts with knowing your inputs. This briefing stands on five data sources, and each plays a distinct role. Source one, Twelve Data, gives us daily candles — the last two bars — for seven symbols: the S and P 500 ETF, the Nasdaq ETF, a Europe ETF, an emerging-markets ETF, gold, long-term treasuries, and the euro-dollar pair. From those two bars we compute the daily percent change for each asset. This is the backbone of the price section, and it's a core source. Source two, CoinGecko, gives bitcoin and ethereum prices plus their twenty-four-hour change. Also core. Source three, FRED — the St. Louis Fed's database — gives the ten-year treasury yield, series D G S ten, with the last three observations. That is the interest-rate anchor of the whole briefing. Also core.

**Key points:**
- Twelve Data: SPY QQQ VGK EEM GLD TLT EURUSD (core)
- CoinGecko: BTC & ETH, keyless option (core)
- FRED: US 10Y yield, DGS10 (core)
- 2 daily bars → computed % change

Source four, QuantGist, provides the macro calendar — CPI, non-farm payrolls, FOMC, PCE releases. This one is non-core, and it's the only paid source in the lineup. Source five, Marketaux, gives English financial news filtered by symbol — the raw material for the briefing's headline section. Non-core as well. Now, why do we tier sources into core and non-core? Because the workflow treats them differently. Each source carries a core flag. When a core source fails, it lands in a dedicated list called core source failures, and that list is injected straight into the AI prompt with instructions to surface it prominently in the briefing. A dead core source is news the reader deserves to know. When a non-core source fails, the briefing ships anyway — the macro calendar being absent on a quiet Tuesday is not a crisis.

**Key points:**
- QuantGist: macro calendar (non-core, PAID)
- Marketaux: symbol-filtered EN news (non-core)
- core:true failure → core_source_failures list
- prompt instructs AI to flag core failures
- non-core failure → briefing still ships

On cost: Twelve Data's free tier is eight hundred requests a day and eight per minute — far beyond the once-a-day cadence of a morning brief. Marketaux's free tier is a hundred requests a day. FRED is free with registration. CoinGecko can run completely keyless on the public endpoint. QuantGist is the one paid item, and in the next lecture I'll show you the exact, tested procedure for cutting that branch out of the workflow. Your action item after this lecture is simple: register the three free keys — Twelve Data, FRED, and Marketaux. Each takes under five minutes, and you will need them in section five when we wire the credentials.

**Key points:**
- Twelve Data free: 800 req/day, 8 req/min
- Marketaux free: 100 req/day
- FRED free w/ registration; CoinGecko keyless
- Action: register 3 free keys now (<5 min each)