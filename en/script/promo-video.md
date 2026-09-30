# lesson-10 (EN) Promo Video Narration (~90s)

## Hook (0–4s)
"Every weekday morning, an AI writes my global market briefing for free — and when a data source dies, it tells me the truth about it."

## Pain (4–15s)
"If you trade or run a finance channel, you know the routine: the moment you wake up you're hopping across five or six sites — US equities, crypto, treasury yields, today's macro releases, overnight headlines. Half an hour, gone."

## Demo (15–55s)
"So I built a pipeline in n8n: at 7:45 AM, five data sources get pulled in parallel — equities, bitcoin, treasury yields, the macro calendar, and financial news. Each source passes through a normalization gate first; if one dies or gets rate-limited, it's flagged automatically and the chain never breaks.
Then a disciplined AI analyst takes over — the prompt hard-codes the rules: never invent numbers, and uncertain causality must be phrased as 'may be related'.
The AI's output goes through a hard validation gate; malformed JSON gets blocked outright. Better no briefing than a wrong briefing.
Finally the brief is delivered to Telegram and Discord, and archived into Postgres."

## Receipts (55–70s)
"What does it cost? A DeepSeek call runs about a fifth of a cent. Of the five data sources, three are free registrations and one needs no key at all. The whole chain runs itself every weekday."

## CTA (70–90s)
"The full course is up on Udemy — six sections, twelve lectures, from importing the template to cloud deployment, with the complete workflow JSON included. Enroll now, and tomorrow morning your briefing writes itself."

## Visual notes
- 0–15s: fast cuts of chaotic site-hopping → 15s+: n8n canvas capture (highlight fetch group) → AI JSON output → TG message arrival close-up → course landing screenshot
- BGM: light electronic, steady pace
- Hard-burned captions; key lines: "Better no briefing than a wrong briefing" / "A fifth of a cent per brief"