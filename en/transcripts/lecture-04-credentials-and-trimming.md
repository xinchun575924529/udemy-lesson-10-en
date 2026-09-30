# Lecture 4: Wiring Credentials & Trimming the Paid Source
Section: The Five Data Sources

Map each template credential placeholder to the correct n8n credential type (Query vs Header auth), then perform the standard four-step drill to safely remove the paid QuantGist branch.

Credentials are where template imports most often go wrong, because the failure mode is subtle: pick the wrong credential type and every single source returns an error, with no hint why. So let's be precise. Twelve Data authenticates with a query parameter named apikey — in n8n that is an HTTP Query Auth credential, name apikey, value your key. FRED is the same pattern, except the parameter is named api underscore key. Marketaux is also query auth, named api underscore token. CoinGecko's optional demo key travels in a header — so that's an HTTP Header Auth credential named x dash c g dash demo dash api dash key. And note the asymmetry: three query auth, one header auth. Mix them up and all five sources light up red. If you see the all-sources-error symptom later, this table is the first place to check.

**Key points:**
- Query Auth: Twelve Data (apikey), FRED (api_key), Marketaux (api_token)
- Header Auth: CoinGecko (x-cg-demo-api-key)
- Keyless CoinGecko: set auth to none
- Symptom 'all sources error' → check this table first

Now the trimming drill — removing the paid QuantGist branch cleanly. Four steps. Step one: delete two nodes — the Fetch QuantGist Macro Calendar HTTP node, and the Normalize QuantGist Calendar Data Code node. Step two: open the Merge Source Data node and change number inputs from five down to four. Step three — and this is the delightful part — do nothing to the Prepare node. The template authors designed it to expect all five sources by name, and when one is missing it synthesizes an error-state placeholder automatically. We verified this in validation testing: trim the branch and the briefing cycle continues, with quantgist simply reported as absent. Step four, optional: if you want a macro calendar without paying, add a couple of macro series from FRED — you already have that key — or subscribe to a public I C S release calendar.

**Key points:**
- ① Delete Fetch + Normalize QuantGist (2 nodes)
- ② Merge numberInputs: 5 → 4
- ③ Prepare untouched — auto-synthesizes placeholder (L2-verified)
- ④ Optional free macro: FRED series or ICS calendar

One reverse lesson worth burning into memory: if you delete only the Fetch node and forget its Normalize node, nothing breaks — the orphaned Normalize receives empty input and politely outputs the empty state. Safe, but you now have a dead node sitting on your canvas, and dead nodes are how you confuse the next engineer, who is probably future you. Delete branches in pairs. With sources understood and credentials mapped, we're ready for the heart of this course: the normalization pattern itself.

**Key points:**
- ⚠ Never orphan a Normalize node
- Delete branches in pairs (Fetch + Normalize)
- Dead nodes confuse future-you