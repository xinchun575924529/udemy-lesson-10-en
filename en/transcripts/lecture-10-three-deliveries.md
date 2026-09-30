# Lecture 10: Three-Way Delivery: Telegram, Discord & Postgres
Section: Validation & Delivery

Wire each delivery branch: Build Telegram Message + Bot API (4096-char handling), Build Discord Embed + webhook option, and the parameterized Postgres insert with the briefings table DDL and free Supabase/Neon hosting.

Three delivery branches, all following the same shape: a pure Code node that builds a payload, then a send node that ships it. Telegram first. Build Telegram Message converts the briefing JSON into Telegram's message format — bold headline, sections for assets, macro events, and news — and, crucially, it handles Telegram's four-thousand-ninety-six-character limit with intelligent truncation. To send, you need two things: a bot token, free from at-BotFather in about a minute, and a chat I D — message your new bot once, then call the get updates endpoint to read the I D. One infrastructure warning: Telegram's API is unreachable from mainland-China servers; host on an overseas node or route through a proxy.

**Key points:**
- Pattern: Build (Code) → Send, ×3
- TG: Build handles 4096-char limit
- Bot token from @BotFather; chat_id via getUpdates
- ⚠ TG API unreachable from mainland CN servers

Discord is even easier. Build Discord Embed assembles a rich embed — title, color-coded by market regime, field blocks for each section. For sending, the laziest robust option is a channel webhook: channel settings, integrations, webhooks, create — then point an HTTP Request node at the webhook U R L with a P O S T. Free, no app review, no bot permissions to negotiate. Then Postgres: Create Database Insert Query builds a parameterized insert — no string-concatenated S Q L, and the JSON fields are cast with the double-colon j s o n b operator so they land as queryable JSON B columns, not opaque text. The table itself is one create-table statement, in the docs: an i d, a timestamp defaulting to now, the regime, headline, and summary as text, then all the structured sections — assets, events today, top news, drivers, opportunities, watch next, raw sources — as JSON B, plus model provider and model name for traceability.

**Key points:**
- Discord: embed color-coded by regime
- Webhook = free, zero approval (HTTP POST)
- PG: parameterized INSERT, ::jsonb casts
- briefings table DDL in docs/chapter 04
- model_provider/model_name stored for traceability

Do you need all three? No — that's by design. The sticky notes on the canvas say it plainly: unused delivery branches can simply be disabled or deleted. The most common delivery shape is Telegram-only; the Discord community version runs TG plus Discord; client-facing installs usually add the database for an audit trail. For free hosting, Supabase and Neon both hand you a Postgres instance on signup — the five-hundred-megabyte free tier stores literal decades of once-daily briefings. Your exercise for this lecture: get exactly one channel truly green end to end, in the real world, with a message arriving on your phone or a row appearing in a table. Screenshots or it didn't happen.

**Key points:**
- Unused branches can be disabled/deleted by design
- Common shape: Telegram-only
- Free PG: Supabase / Neon (500MB = decades of briefs)
- Exercise: one channel green end-to-end, for real