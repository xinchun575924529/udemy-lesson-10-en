# Udemy Landing Page Copy — lesson-10 (EN)
> 机器起草 → 老板亲办粘贴。字段按 Udemy Course Landing Page 表单组织。

## Course Title (≤60 chars, 实测 56)
**AI Market Briefing Automation with n8n: Multi-Source to Delivery**

## Course Subtitle (≤120 chars, 实测 113)
**Build a self-healing n8n pipeline: 5 data sources, a disciplined AI analyst, hard validation, and Telegram/Discord/Postgres delivery.**

## Course Description

Every weekday morning, traders and finance creators lose 30 minutes hopping between sites: equities, crypto, treasury yields, macro calendars, overnight headlines. In this course you'll build — and deeply understand — a production n8n workflow that does all of it for you: at 7:45 AM it pulls five data sources in parallel, normalizes them into one clean envelope, hands them to an AI analyst bound by a contract-grade prompt, validates the AI's output with a deterministic gate, and delivers a structured English market briefing to Telegram, Discord, and Postgres. If a source dies, the pipeline degrades gracefully and tells the truth about it. If the AI misbehaves, nothing ships.

This course treats the official n8n template #19457 like professional engineers, not tourists. All seven Code nodes were ported into a test harness and verified with 53 assertions; the full pipeline ran end-to-end against a real DeepSeek model with 100% data fidelity. You'll learn the four quirks we found in the template's code — because they're exactly the kind of thing that bites you in production.

**What makes this course different:**
- The normalization pattern — wrap any unruly API into a four-state envelope (ok / error / empty / invalid) with sanitized errors safe to feed into prompts
- Prompt-as-contract engineering — a 2,500-word disciplined prompt teardown plus the validator node that enforces its output schema by execution
- Real failure drills — trim a paid data source, add a sixth source, swap models, localize the briefing language
- Near-zero running cost: DeepSeek ≈ $0.002/run, three free-tier keys, one keyless source

**Course structure:** 6 sections, 12 lectures, ~33 minutes of dense video plus written chapters, exercises at three difficulty tiers, and the complete importable workflow JSON.

## Learning Outcomes (【课程目标】bullets)
1. Build and deploy a production daily market-briefing bot in n8n 2.x with Telegram, Discord, and Postgres delivery
2. Apply the normalization pattern (four-state envelope + error sanitization) to any multi-source API project
3. Engineer contract-grade AI prompts and enforce their output with a deterministic validation gate
4. Wire API credentials correctly (Query vs Header auth) and trim or extend data sources with standard drills
5. Deploy on a schedule with correct timezone handling, monitoring, and error alerting

## Intended Audience
- n8n users who can import workflows, create credentials, and read JSON (e.g. completed a basic Telegram-bot workflow)
- Traders, finance-content creators, and analysts who want an automated daily briefing
- Freelancers and studios delivering "AI daily brief" automation to clients

## Requirements (【先修要求】)
- An n8n 2.x instance (local or cloud)
- Basic comfort with JSON and importing n8n workflows
- No paid APIs required: DeepSeek (≈$0.002/run), free tiers for Twelve Data / FRED / Marketaux, CoinGecko keyless; the one paid source is trimmed in the course

## Instructor Profile (机器起草 → 老板定稿)
- **Headline (60 chars):** `n8n automation engineer · AI pipeline craftsman · Course builder`（59 chars）
- **Bio (~100 words):**
  "I'm an automation engineer focused on n8n and applied AI workflows. I run a course factory that takes official n8n templates through a full engineering pipeline — unit-test porting of every Code node, end-to-end validation against real models, then structured teaching materials. My courses teach patterns, not button-clicking: how to make multi-source data pipelines that fail gracefully, how to make AI outputs contract-safe, and how to deploy them so they run unattended for months. My workflow packages are open on GitHub, and every claim in my lectures is backed by a test record."

## 定价建议
首发 $19.99（含 Deals Program 勾选）。12 lecture / 33min 属"小而精"区间，靠促销走量。

## Promo / 封面 / 预告片
- 封面：`assets/course-cover-2048x1152.png`（2048x1152）
- 预告片：`video/promo/promo-en.mp4`（≤2min）