# Lecture 5: The Normalization Envelope & the Four-State Machine
Section: The Normalization Pattern

Why five wildly different API shapes must collapse into one envelope {source, core, status, data, error}, and how the four-state decision order — error, empty, invalid, ok — is implemented in the Normalize Code nodes.

This section is the soul of the course. Look at what the AI would face without normalization: Twelve Data keys its response by symbol, CoinGecko keys by coin, FRED returns an observations array, QuantGist returns grouped events, Marketaux returns an article list. Five incompatible schemas, all landing in one prompt. An AI forced to swim across five schemas hallucinates — it grabs a field from the wrong structure and invents the rest. The template's answer is the normalization pattern: after every Fetch node sits a Normalize Code node, and every one of them emits the exact same envelope. Source: the source's name. Core: a boolean. Status: one of four states — ok, error, empty, invalid. Data: the extracted business fields, present only when ok. And error: a sanitized message, present only when error. One envelope for everything means the prompt, the validator, and your own debugging brain each learn one schema, not five.

**Key points:**
- 5 sources = 5 incompatible raw schemas
- One Normalize Code node per source
- Unified envelope: source / core / status / data / error
- Prompt, validator & you learn ONE schema

Inside each Normalize node, the status is decided by a four-state machine, and the order of checks is the order of the code. First check: failed. Did the API explicitly blow up? We look for an error field, an errors field, a status of error, or a code-and-message pair. Remember, the Fetch node was configured to continue on error, so a dead source's error object flows into the Code node as input — error becomes data. Second check: empty. Is the raw input null, or an object with zero keys? This matters because some emptiness is normal business — FRED publishes no new yield observation on weekends or holidays, so empty on a Saturday is correct behavior, not a fault. Third check: invalid, which is source-specific structural validation. If the shape doesn't match what we expect, something changed upstream — think of invalid as early warning that an API changed its response format. Only if all three checks pass is the status ok, and only then do we extract business fields. Twelve Data's normalizer even computes the percent change between the two daily bars for you.

**Key points:**
- Check order = code order: error → empty → invalid → ok
- error: API exploded; error object flows in as data
- empty: zero-key response — normal on weekends (FRED)
- invalid: structure mismatch = API-drift early warning
- ok: extract fields (+ computed change_percent)

Let's cement this with the hands-on prediction drill from the docs. Feed the Twelve Data normalizer four inputs. First: an empty object. What state? Empty. Second: an object with status error, code four-oh-one, and a message saying the apikey is incorrect. Error — and the sanitizer will rewrite it to a short, clean message: API key missing or invalid. Third: an object with a single arbitrary field foo. None of the failed checks match, it's not empty, and it doesn't have the expected symbol-keyed structure — so, invalid. Fourth: a proper S P Y object with two daily bars at six hundred and five ninety-four. Ok — with a computed change of about plus one point zero one percent. Predict before you run; if your predictions match, you've internalized the machine.

**Key points:**
- {} → empty
- {status:error, code:401} → error + sanitized msg
- {foo:1} → invalid
- SPY 2 bars → ok, change ≈ +1.01%