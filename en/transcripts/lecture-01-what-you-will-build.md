# Lecture 1: What You Will Build: The Daily AI Market Briefing
Section: Getting Started: Overview & Import

Tour the finished product: a weekday 7:45 AM pipeline that pulls five market data sources, feeds a disciplined AI analyst, validates the output, and delivers a structured briefing to Telegram, Discord, and Postgres.

Welcome to the course. Before we touch a single node, let's look at the finish line. By the end of this course you will have a fully automated pipeline that, every weekday at seven forty-five in the morning, wakes up on its own, pulls five different market data sources in parallel, hands everything to an AI analyst bound by strict rules, quality-checks whatever the AI writes, and then delivers a clean, structured market briefing to three places at once: a Telegram chat, a Discord channel, and a Postgres database. No human in the loop. And here is the important part: it is engineered to fail gracefully. If a data source dies, the pipeline does not crash — it marks the source, warns the AI, and still ships the briefing. If the AI writes garbage, the output gets blocked at the gate. Better no briefing than a wrong briefing.

**Key points:**
- Weekday 07:45 trigger, fully unattended
- 5 data sources in parallel
- Disciplined AI analyst + hard validation gate
- Delivers to Telegram, Discord & Postgres
- Fails gracefully — never lies, never crashes

This course is built around an official n8n template, number 19457, and we treat it like professional engineers, not tourists. We don't just import it and hope. Over the L2 validation phase we ported all seven of its Code nodes into a test harness and ran fifty-three assertions against them — all passing. We also ran the whole pipeline end to end with a real DeepSeek model call, and verified that every number in the AI's briefing matched the input data exactly. Zero fabrication. Along the way we found four genuine behavioral quirks in the template's code, and I'll show you every one of them, because those quirks are exactly the kind of thing that bites you in production at seven in the morning.

**Key points:**
- Built on official n8n template #19457
- 7 Code nodes ported, 53/53 assertions pass
- End-to-end run with real DeepSeek call
- AI output 100% faithful to input data
- 4 behavioral quirks documented & explained

What do you need starting out? An n8n version 2 instance, local or in the cloud. You should be comfortable importing a workflow, creating credentials, and reading JSON — nothing beyond that. On the money side, the whole course is designed to cost you essentially nothing: the AI model call is DeepSeek at roughly a fifth of a cent per run, CoinGecko needs no key at all, and Twelve Data, FRED, and Marketaux all have free tiers. The single paid source, QuantGist, can be trimmed from the workflow, and we have a whole section showing you the standard drill for cutting it cleanly. Let's get started.

**Key points:**
- Prerequisite: n8n 2.x + basic JSON literacy
- DeepSeek ≈ $0.002 per run
- CoinGecko keyless; 0.1‑3 free-tier keys
- Paid source (QuantGist) fully trimmable