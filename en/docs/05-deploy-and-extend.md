# Chapter 05 — Deploy, Troubleshoot & Extend

## Learning goals
- Deploy the briefing workflow for 7×24 unattended operation
- Master the three most common customization moves

## Deployment checklist

1. **Get a fully green manual run first** (5 sources ok, Validate passes, at least one delivery succeeds) before activating the Schedule;
2. **Timezone**: the Schedule Trigger's cron `45 7 * * 1-5` follows the n8n instance timezone (UTC by default!). Set `TZ` and `GENERIC_TIMEZONE` in the n8n environment; the `report_timezone` field in the Setup node controls the news lookback window and event display times — don't confuse the two;
3. **Cloud hosting advice**: all data sources plus TG/Discord need outbound internet; a mainland-China host needs a workaround for Telegram — an overseas node (e.g. a Singapore ECS) is the path of least resistance;
4. **Monitoring**: enable execution-log retention in n8n; a Validate throw = no delivery that day, so add an Error Trigger workflow that alerts the admin via Feishu/TG.

## Three high-frequency customizations

### A. Change the tracked symbols
Two edits: ① the `symbol` parameter of "Fetch Twelve Data" (comma-separated; mind the free tier's 8 req/min — if you add many symbols, batch the calls); ② the `symbols` parameter of "Fetch Marketaux". The Normalize code **stays untouched** — that's the dividend of the normalization pattern.

### B. Add a sixth data source (the standard drill)
1. Drop an httpRequest node (`onError: continueRegularOutput` + retry);
2. Copy any Normalize node, change three things: the `source` name, the `core` boolean, the extraction section;
3. Bump the Merge node's `numberInputs` by 1 and wire it in;
4. Add the new source name to Prepare's `expected` array;
5. (Optional) Add a field-description paragraph for the new source to the prompt.

### C. Localization (e.g. to Chinese)
Change "All human-readable fields must be in English" in the prompt to your target language; Validate needs no change (enum values are language-independent). Note the small amounts of English template copy inside the Telegram/Discord Build nodes — translate those too.

## Troubleshooting quick reference

| Symptom | Likely cause | Fix |
|---|---|---|
| All sources error | Wrong credential type (Query vs Header Auth mixed up) | Check the wiring table in docs/01 |
| AI node: model not found | Fake model gpt-5.6-luna not replaced | Swap to DeepSeek per docs/03 |
| Validate: not valid JSON | Temperature too high / JSON mode off | temperature ≤ 0.3; enable response_format on DeepSeek |
| Weekend `empty` spamming your alerts | Monitoring treats empty as error | empty is a normal business state; alert only on error + core sources |
| TG send fails with ETIMEDOUT | Server is in mainland China | Route TG via an overseas node or proxy |

## Chapter check
The workflow activates and auto-produces briefings for 3 consecutive weekdays (or equivalent manual-trigger records), and you can complete one "add a data source" drill.