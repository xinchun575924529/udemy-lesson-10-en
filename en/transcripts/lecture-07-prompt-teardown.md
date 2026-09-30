# Lecture 7: The Disciplined Prompt: A Section-by-Section Teardown
Section: AI Agent Disciplined Prompting

Walk the ~2,500-word system prompt: CORE RULES against fabrication, the hedged-causality vocabulary, MARKET/RATES concept guards, MACRO and NEWS limits, and the fixed 13-field JSON output contract.

The single most valuable asset in this template is not a node — it's a document: the roughly two-and-a-half-thousand-word system prompt inside the AI Agent node. This prompt is engineered the way a contract is engineered, so let's read it the way a lawyer reads a contract, clause by clause. The opening clause is CORE RULES: never invent prices, events, or causal relationships; if data for something is missing, say so explicitly. This sounds obvious, but stating it as the first clause puts a hard ceiling on the model's favorite failure mode — confident confabulation. In our end-to-end validation run, every figure in the resulting briefing matched the input data one hundred percent.

**Key points:**
- The prompt is the #1 asset — a contract
- CORE RULES: never invent prices/events/causation
- Missing data must be stated explicitly
- L2: 100% fidelity to input, zero fabrication

The next clauses turn vague caution into a concrete vocabulary. When causality is uncertain, the model is not told be careful — it's given exact phrases: may be related, is in focus, is a potential driver. That distinction is huge. Telling a model to be careful changes nothing; giving it a whitelist of hedged phrases mechanically lowers the assertiveness of its claims. Then the MARKET and RATES rules defend against the three most common concept errors in market summaries: market regime must be exactly one of five enumerated values; T L T is a bond price, not a yield — remember, bond prices and yields move in opposite directions, and models conflate them constantly; and the ten-year yield may only ever be cited from the FRED data, never inferred from T L T's price move.

**Key points:**
- Hedging whitelist: 'may be related', 'is in focus', 'is a potential driver'
- market_regime: exactly 1 of 5 enum values
- TLT = bond PRICE (inverse of yield)
- US10Y must cite FRED only

The MACRO clause says: events underscore today contains only events scheduled for the report date, and where actual, forecast, or previous values are absent from the data, the fields must be null — no estimated numbers, ever. The NEWS clause allows a maximum of three headlines, requires each to be genuinely market-relevant, and — my favorite line — explicitly permits returning an empty array. Giving the model permission to say there is no news worth reporting is what stops it from padding the section with filler. Finally, the output contract: a fixed JSON schema with all thirteen top-level fields enumerated by name and type. This schema is not just for the model — it is the target the Validate node, in the next section, enforces by execution. Prompt promises, validator enforces. That pairing is the whole architecture.

**Key points:**
- MACRO: today only; absent values → null
- NEWS: max 3, market-relevant, [] allowed
- No filler: 'no news' is a valid answer
- Fixed 13-field JSON schema
- Prompt promises → Validate enforces