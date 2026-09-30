# Lecture 12: Extending the Workflow & Course Wrap-Up
Section: Deploy & Extend

The three standard customization drills — changing tracked symbols, adding a sixth data source in five steps, and localizing the briefing language — plus a recap of the portable engineering patterns you've acquired.

The last lecture is about making this system yours. Customization A: changing the tracked symbols. Say you follow Japanese equities and want E W J instead of E E M. Touch exactly two nodes: the symbol parameter on Fetch Twelve Data, and the symbols parameter on Fetch Marketaux. The Normalize code stays untouched — the envelope absorbs the change. That's the dividend of normalization: swap what data flows in without touching the machinery that processes it. One caveat on Twelve Data's free tier: eight requests per minute. Seven symbols fit comfortably; if you go wild and track thirty, batch them across multiple Fetch nodes.

**Key points:**
- Change symbols: edit 2 params only (Twelve Data + Marketaux)
- Normalize untouched — the pattern's dividend
- Free tier: 8 req/min → batch if many symbols

Customization B, the five-step drill for adding a sixth data source. Step one: drop in an HTTP Request node configured the template way — continue on error, retry on fail. Step two: copy any existing Normalize node and change three things: the source name string, the core boolean, and the extraction section that pulls business fields into data. Step three: bump the Merge node's number inputs by one and wire your branch in. Step four: add the new source's name to the expected array in the Prepare node — that keeps the placeholder-synthesis machinery aware of it. Step five, optional but recommended: add a short paragraph to the prompt describing the new source's fields, so the AI knows how to reason about it. The challenge exercise suggests a great candidate: FRED's VIX series, V I X C L S, as a fear gauge — and then editing the MARKET section of the prompt so the AI factors volatility into its regime call.

**Key points:**
- ① httpRequest (continue-on-error + retry)
- ② Copy a Normalize: change source/core/extraction
- ③ Merge numberInputs +1 and wire
- ④ Prepare expected[] += new source
- ⑤ (Opt.) describe fields in prompt — e.g. VIX (VIXCLS)

Customization C: localization. The briefing language is one line of prompt text — change the instruction that all human-readable fields must be in English to your target language, and translate the small hard-coded template strings in the Telegram and Discord Build nodes. The Validate node needs zero changes — enum values like risk underscore on are language-independent, which is exactly why the contract uses enums instead of prose. Let's close with what you actually own now. You own a production briefing bot. You own the normalization envelope — a pattern you can wrap around any set of unruly A P Is. You own the prompt-as-contract plus validator-as-enforcement architecture for shipping AI output safely. And you own a battery of standard drills: trim a source, add a source, change symbols, localize. Go ship a briefing — and thank you for taking this course.

**Key points:**
- Localize: 1 prompt line + Build-node strings
- Validate unchanged — enums are language-free
- You own: bot + envelope + contract/gate + drills
- Go ship a briefing. Thank you!