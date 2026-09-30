# Chapter 04 — The Validation Layer & Three-Way Delivery

## Learning goals
- Understand the Validate node's "triple enforcement"
- Wire up Telegram / Discord / Postgres delivery (pick any subset)

## Validate AI Output: the customs gate for AI output

```
AI output → ① locate the text (fallback across output/text/response/message/content)
          → ② JSON.parse (failure → throw; the whole execution fails, nothing is delivered)
          → ③ market_regime enum check (five values only; out of range → throw)
          → ④ field normalization backfill (assets/drivers/watch_next coerced to standard structure; non-array → [])
```

Design philosophy: **hard errors (unparseable / enum violation) break the chain — better no briefing today than a wrong briefing; soft problems (missing fields) get backfilled and pass**.

A practical detail: if the AI returns `market_regime` as an object like `{label:'Risk-Off'}`, Validate normalizes it to `risk_off` — tolerant of small model whims, but never of arbitrary values.

## Three delivery channels

### ① Telegram
- "Build Telegram Message" (Code): formats the JSON into a TG message (handles the 4096-char limit with truncation), outputting `telegram_text`;
- "Send to Telegram": needs a Bot token (free from @BotFather) + chat_id (message your bot, then query getUpdates).
- ⚠️ Servers in mainland China need a proxy/overseas node to reach Telegram; an overseas cloud host connects directly.

### ② Discord
- "Build Discord Embed" (Code): assembles the embed JSON (title / color / field blocks);
- "Send to Discord": the laziest path is channel → Settings → Integrations → **Webhook** — create one and switch the node to an HTTP Request POSTing to the webhook URL. Free, zero approval.

### ③ Postgres insert
- "Create Database Insert Query" (Code): builds a parameterized INSERT (JSON fields cast with `::jsonb`);
- "Insert into Postgres": needs a table — the DDL:

```sql
CREATE TABLE public.briefings (
  id BIGSERIAL PRIMARY KEY,
  generated_at timestamptz DEFAULT now(),
  market_regime text, headline text, summary text,
  assets jsonb, events_today jsonb, top_news jsonb,
  drivers jsonb, opportunities jsonb, watch_next jsonb,
  raw_sources jsonb, model_provider text, model_name text
);
```

- Free Postgres: Supabase / Neon give you one on signup (the 500MB free tier holds decades of daily briefs).
- Don't want Postgres? Delete that whole branch (the sticky notes say it: unused delivery branches can simply be disabled).

## Hands-on tasks
1. Wire only Telegram and run green end-to-end (the most common delivery shape);
2. Insert a tampering Code node before Validate that sets `market_regime` to `euphoric`, confirm the execution throws at Validate and Telegram receives nothing — feel the value of the customs gate.

## Chapter check
At least one channel actually delivers a message/record, and you can state which class of problems Validate throws on and which it backfills.