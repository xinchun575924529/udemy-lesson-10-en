# Chapter 03 — AI Agent: Disciplined Prompting + Swapping to DeepSeek

## Learning goals
- Teardown why this ~2,500-word English prompt is "hallucination-proof", section by section
- Replace the placeholder fake model with DeepSeek and run green on the first try

## ⚠️ Mandatory: replace the fake model

The Chat Model node in the template is named `gpt-5.6-luna` — **a fictional future placeholder by the template author; it does not exist**. Do not run without replacing it. Steps:

1. Delete the "OpenAI Chat Model GPT-5.6 Luna" node;
2. Drop in a **DeepSeek Chat Model** node (built into n8n 2.x as `@n8n/n8n-nodes-langchain.lmChatDeepSeek`);
3. Create a DeepSeek credential (register at api.deepseek.com and top up ~¥1 — enough for hundreds of runs);
4. Select model `deepseek-chat`, temperature 0.3 recommended;
5. Wire it into the AI Agent node's Chat Model input;
6. In "Setup Workflow Configuration", change `model_provider` to `deepseek` and `model_name` to `deepseek-chat` (both fields are written into the Postgres record for traceability).

> Verified in L2: DeepSeek-chat + the original prompt + `response_format=json_object` → valid JSON, 100% fidelity to input data, zero fabrication.

## Prompt teardown (the most valuable asset in the template)

| Section | Intent | Why it works |
|---|---|---|
| CORE RULES | Never invent prices/events/causation; missing data must be stated | Draws hard red lines — hallucination rate collapses |
| "may be related / is in focus / is a potential driver" | Mandatory hedged wording when causality is uncertain | Turns "be careful" into a concrete vocabulary, not a slogan |
| MARKET/RATES RULES | market_regime is one of five values; TLT is a bond price, not a yield; US10Y may only cite FRED | Prevents the most common conceptual confusions (bond price ↑ = yield ↓) |
| MACRO | events_today only for the same day; missing actual/forecast/previous → null | No confabulated numbers |
| NEWS | Max 3 headlines, must be truly market-relevant, else return `[]` | "No news" is allowed — no filler |
| Output contract | Fixed JSON schema (all 13 top-level fields enumerated) | Gives the downstream Validate node a concrete target |

### Data injection points (three expressions)
Inside the prompt, data is injected via `{{ JSON.stringify($('Prepare Data for AI Model').item.json.xxx) }}`:
- `normalized_sources` — all normalized data from the five sources
- `source_health` — per-source health (the AI writes `missing_sources` from this)
- `core_source_failures` — core failures (the AI must flag these prominently)

### Key teaching point
**Prompt engineering in this template is not mysticism — it's a contract**: the prompt promises an output schema → the Validate node enforces it → anything invalid throws and the run stops. Next chapter covers the enforcement side.

## Hands-on tasks
1. Deliberately misconfigure a core source's key (e.g. FRED), run once, and check whether the AI output's `missing_sources` reports it honestly;
2. Raise temperature to 1.5, run three times, compare JSON stability (and feel why production recommends 0.3).

## Chapter check
- After swapping to DeepSeek, a manual run passes the Validate node without errors;
- You can explain what "Never claim that a headline or macro event caused a market move unless the supplied data directly supports it" protects against.