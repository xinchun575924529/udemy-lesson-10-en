# Chapter 02 — The Normalization Pattern (Soul of This Course)

## Learning goals
- Read the Normalize node's "four-state machine + error sanitization" line by line
- Port this pattern to any future multi-source project of your own

## Why normalization matters

The five sources return completely different shapes: Twelve Data keys by symbol, CoinGecko keys by coin, FRED gives an `observations` array, QuantGist gives grouped events, Marketaux gives an article list. Feed all that raw into an AI prompt and the model swims across five schemas — hallucination skyrockets. The template's answer: **one Normalize Code node after every source, all emitting the same envelope**:

```json
{
  "source": "twelve_data",
  "core": true,
  "status": "ok | error | empty | invalid",
  "data": { "...only when ok..." },
  "error": "only when error, and sanitized"
}
```

## The four-state machine (the check order is the code order)

```js
const failed  = Boolean(raw?.error || raw?.errors || raw?.status === 'error' || (raw?.code && raw?.message));
const empty   = raw == null || (typeof raw === 'object' && !Array.isArray(raw) && Object.keys(raw).length === 0);
// per-source structural validation → invalid
const status  = failed ? 'error' : empty ? 'empty' : invalid ? 'invalid' : 'ok';
```

- **error**: the API explicitly failed (httpRequest uses `continueRegularOutput`, so the error object flows into the Code node)
- **empty**: empty response (FRED has nothing new on weekends/holidays — **a normal business state, not a fault**)
- **invalid**: structure mismatch (early warning that an API changed shape)
- **ok**: business fields extracted (Twelve Data's Normalize even computes `change_percent` for you)

## Error sanitization — sanitizeError (why it's valuable)

Raw errors are long and filthy (AxiosError stacks, internal paths). The sanitizer does three things:
1. Strips stacks: `AxiosError:...`, `at ...`, n8n internal paths all removed;
2. Normalizes high-frequency errors: `credentials not found` / `401` / `403` / `429` / invalid apikey → fixed short messages;
3. Truncates at 300 characters.

**Sanitized errors go into the AI prompt** — clean, consistent error descriptions let the AI honestly state "source X is missing due to rate limiting" instead of being confused by stack traces.

## Four behavioral quirks found in testing (fixed by L2 assertions — interview-grade details)

1. A bare `{message:'...'}` (no error/status/code fields) is **not** judged as error — it falls into invalid;
2. `sanitizeError` never reads `r.code`; HTTP prefixes only come from status/statusCode/response.status;
3. `{statusCode:500}` alone **does not satisfy the failed check** (statusCode isn't in the check list) and leaks through as ok — in real runs the httpRequest layer emits an error field first that catches it, but you should know the gap exists;
4. The Prepare node's `all_sources_completed` is **always true** (missing branches are synthesized before the check) — for data completeness, look at `core_source_failures`.

## Prepare Data for AI Model: aggregation + health

```js
// Missing branches auto-synthesize an error placeholder; core failures listed separately
core_source_failures: [{source:'fred', status:'error', error:'...'}, ...]
source_health: { twelve_data: {status, error, core}, ... }  // goes into the prompt
```

## Hands-on task
Feed these four inputs into the Normalize Twelve Data node, predict the status, then run and verify:
`{}`, `{status:'error', code:401, message:'apikey is incorrect'}`, `{foo:1}`, `{SPY:{values:[{close:'600'},{close:'594'}]}}`
(Answers: empty / error + "API key missing or invalid" / invalid / ok with change_percent ≈ 1.01%)

## Chapter check
Without looking at the code: state the four-state decision order, and explain why error text must be sanitized before entering the AI prompt.