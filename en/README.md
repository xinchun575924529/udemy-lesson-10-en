# Daily AI Market Briefing: Multi-Source Data + AI Agent + Three-Way Delivery — lesson-10 (English Edition)

> **One-liner: one morning briefing teaches you the complete engineering pattern of "multi-source normalization → disciplined AI output → multi-channel delivery."**
> Five data sources — equities, crypto, treasury yields, macro calendar, financial news — feed one disciplined AI analyst. Every weekday at 7:45 AM it automatically delivers a structured English market briefing to Telegram, Discord, and Postgres. A dead data source never breaks the pipeline; invalid AI output gets blocked at the gate.

- Course: **lesson-10** | Source template: [n8n.io/workflows/19457](https://n8n.io/workflows/19457/)
- Project: 19457 (official n8n template) | Factory repo: `n8n-lesson-factory/projects/19457-send-daily-ai-market-briefings-to-telegram-disco/`
- Version: L3 | Date: 2026-09-25 (Asia/Shanghai)

---

## What you will walk away with

1. **A production-ready daily market briefing bot**: structured English briefings on weekdays, delivered three ways (Telegram / Discord / Postgres).
2. **The "normalization + health" engineering pattern**: each of the 5 sources gets an isomorphic Normalize node (four states: ok/error/empty/invalid + error sanitization). Any source can fail without breaking the main chain — a pattern you can port to any multi-source project.
3. **A disciplined AI Agent prompt template**: no fabrication, hedged causal language, JSON-only output — verified on DeepSeek with 100% fidelity to input data (L2 validation).
4. **A hard validation layer for AI output**: JSON parse failures, enum violations, and missing fields get blocked or backfilled — dirty data never reaches your database or group chat.

## Who this course is for

- You have completed **lesson-05 (Telegram AI Assistant)** or equivalent: you can import an n8n workflow, configure credentials, and read JSON.
- Individuals or studios building **financial/news automation** (daily briefs, price watch, sentiment monitoring); or freelancers delivering "AI briefing" services to clients.

## Prerequisites

- An n8n 2.x instance (local or cloud).
- **Zero paid services required**: DeepSeek (~$0.002 per run), CoinGecko keyless, Twelve Data / FRED / Marketaux free tiers; the paid QuantGist source can be trimmed (the course shows you how).
- Telegram bot / Discord webhook / Postgres all have free paths (checklist in Chapter 05).

## Table of contents

| Ch. | Title | In one line |
|---|---|---|
| 00 | Overview & template import | Get 19457 running and read its skeleton in 10 minutes |
| 01 | The five data sources | What each source provides, how to get free keys, how to trim the paid one |
| 02 | The normalization pattern (soul of this course) | Four-state machine + error sanitization, line by line |
| 03 | AI Agent disciplined prompting | 2,500-word English prompt teardown + swapping to DeepSeek |
| 04 | Validation & three-way delivery | Validate interception logic; TG/Discord/PG formatting |
| 05 | Deploy, troubleshoot & extend | Go live, change timezone/symbols, the standard drill for adding a source |

## Files in this package

- `README.md` (this file) | `docs/00~05` six chapters | `exercises/exercise.md` three-tier exercises | `script/promo-video.md` promo narration script
- `workflow.json` — the original template workflow (import and go; model swap steps in docs/03)
- `site/index.html` — course showcase page (auto-deployed via GitHub Pages)

## Validation record (L2 summary)

- **Unit-level (Part A)**: 7 Code nodes faithfully ported, **53/53 assertions passed**; 4 template behavioral quirks discovered (see docs/02 and factory repo l2-report.md).
- **Full pipeline (Part B)**: 5 mocked sources + real DeepSeek call → validation passed (mild_risk_on) → TG/Discord/SQL artifacts all produced; AI output 100% consistent with input data, zero fabrication.