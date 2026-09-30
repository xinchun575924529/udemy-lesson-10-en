# Exercises (three tiers)

## Exercise 1 (open-book · concepts)
1. Which three of the five sources are core? What does the core/non-core tiering affect? (Hint: `core_source_failures`)
2. What are the four states of a Normalize node? On a weekend FRED has no new data — which state is that, and why is it not a fault?
3. Why must error messages be sanitized before entering the AI prompt?

## Exercise 2 (hands-on · modification)
1. Swap the tracked symbol `EEM` for `EWJ` (Japan ETF), run green, and screenshot the Telegram message;
2. Trim the QuantGist branch (standard drill in docs/01), confirm Prepare auto-synthesizes the error placeholder and the briefing still cycles;
3. Add a second language to the briefing: edit the prompt + the template copy in the two Build nodes.

## Exercise 3 (real-world · full chain)
1. Register free keys for Twelve Data / FRED / Marketaux + create a TG bot with BotFather, then run the full chain for real;
2. Replace Postgres with a free Supabase instance and complete the insert;
3. **Challenge**: add a 6th data source (suggestion: FRED's VIX series `VIXCLS` as a fear gauge) following the five steps in docs/05-B, and have the AI factor it into regime detection (edit the MARKET section of the prompt).

## Self-assessment
- Exercise 2 done: you've internalized the portability of the normalization pattern;
- Exercise 3 done: you can independently deliver "AI daily-brief" projects to clients.