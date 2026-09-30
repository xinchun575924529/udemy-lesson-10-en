# Chapter 00 — Course Overview & Template Import

## Learning goals
- Explain, in one sentence, what this workflow does for you every weekday morning
- Import the template into your own n8n instance and understand the 17 nodes by group

## What this workflow does

```
Weekday 07:45 schedule trigger
  → Pull 5 data sources in parallel (equity candles / crypto / treasury yields / macro calendar / financial news)
  → One Normalize node per source → unified four-state envelope {ok, error, empty, invalid}
  → Merge 5 branches → Prepare: aggregation + source health
  → AI Agent (disciplined prompt) produces structured English briefing JSON
  → Validate: hard gate (invalid JSON / enum violation → throw and block)
  → Three-way delivery: Telegram message / Discord embed / Postgres insert
```

## Hands-on: import the template

1. Open https://n8n.io/workflows/19457/ → click "Use workflow" to copy the JSON (or use the `workflow.json` in this course package).
2. In the n8n canvas → top-right "…" → Import from File/URL.
3. **Do not activate** after import. Run it manually once and watch the errors — everything failing is expected (no credentials configured yet). The point of this chapter is to read the structure.

## Node map (grouped by sticky notes)

| Group | Nodes | Role |
|---|---|---|
| Trigger + config | "When Weekdays at 7:45 AM", "Setup Workflow Configuration" | cron `45 7 * * 1-5`; global config (timezone / model name) |
| Fetch | Fetch ×5 (httpRequest) | All configured with `onError: continueRegularOutput` + `retryOnFail` — **a dead source never breaks the chain** |
| Normalize | Normalize ×5 (code) | Unified four-state output — the soul of this course (Chapter 02) |
| Aggregate | "Merge Source Data" (5 inputs), "Prepare Data for AI Model" | Merge + health report + core-source failure list |
| AI | "AI Market Briefing Agent" + "OpenAI Chat Model" | Chapter 03 shows how to swap in DeepSeek |
| Validate | "Validate AI Output" | Triple gate: JSON / enum / required fields |
| Deliver | "Build Telegram Message" → "Send to Telegram"; "Build Discord Embed" → "Send to Discord"; "Create Database Insert Query" → "Insert into Postgres" | All three Build nodes are pure Code nodes — unit-testable first |

## Common pitfalls
- ⚠️ The model name on the template, `gpt-5.6-luna`, **is a fictional placeholder** — the AI node will fail until you swap it per Chapter 03.
- ⚠️ The Postgres node requires a pre-created table `public.briefings`; the DDL is in Chapter 04.
- ⚠️ The template's default timezone is `Europe/Berlin` — change it in the Setup node to match your target audience.

## Chapter check
You can draw (or narrate) the data-flow diagram above, and point out which stage guarantees "a dead source doesn't break things" and which stage guarantees "the AI can't talk nonsense into production".