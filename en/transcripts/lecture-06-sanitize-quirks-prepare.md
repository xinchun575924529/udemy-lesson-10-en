# Lecture 6: Error Sanitization, Four Quirks & the Prepare Node
Section: The Normalization Pattern

Why errors must be sanitized before entering the AI prompt, the four tested behavioral quirks of the template's code, and how Prepare Data for AI Model builds source_health and core_source_failures.

Every error in this pipeline is destined for the AI prompt. That's the key insight behind sanitize error, the little function in each Normalize node. Raw API errors are filthy: Axios stack traces, internal file paths, multi-line dumps. Feed that to a language model and you either blow up the prompt budget or confuse the model into weird prose. The sanitizer does three things. One: it strips stacks — anything matching AxiosError patterns, at-lines, n8n internal paths, gone. Two: it maps high-frequency failures to fixed short phrases — credentials not found, four-oh-one, four-oh-three, four-two-nine, invalid apikey all become one-line canonical messages. Three: it truncates whatever remains at three hundred characters. The result is an error vocabulary so clean and consistent that the AI can honestly write in the briefing, quote, the rates section is missing due to rate limiting, unquote — and actually be right.

**Key points:**
- All errors flow into the AI prompt
- sanitizeError: strip stacks → canonical phrases → 300-char cap
- 401/403/429/credentials → fixed short messages
- Clean errors let AI report outages honestly

Now the four quirks we found during validation testing — these are interview-grade details about this exact codebase. Quirk one: a bare object containing only a message field — no error, no status, no code — does not trigger the failed check. It falls through to invalid. Quirk two: the sanitizer never reads a response's code property; HTTP status prefixes come only from status, statusCode, or response dot status. Quirk three, the subtle one: an object with only statusCode five hundred does not satisfy the failed check either, because statusCode is not in the failed condition — it leaks through as ok. In production this gap is saved by the HTTP Request layer, which emits its own error field on failure, but you should know the gap exists before you reuse the pattern elsewhere. Quirk four: the Prepare node's all sources completed flag is always true, because Prepare synthesizes placeholder envelopes for missing branches before it checks completeness. If you want to know whether the data is really whole, read core source failures, not that flag.

**Key points:**
- Quirk 1: bare {message} → invalid, not error
- Quirk 2: sanitizer ignores r.code
- Quirk 3: {statusCode:500} alone leaks as ok
- Quirk 4: all_sources_completed is ALWAYS true
- → Judge completeness by core_source_failures

Which brings us to the Prepare Data for AI Model node, the last stop before the AI. It does three jobs. First, tolerance: any expected source that's missing gets a synthesized error placeholder — that's the mechanism that lets you trim QuantGist safely. Second, triage: it walks all five envelopes, pulls the failed core sources into core source failures — each entry carrying the source name, its status, and its sanitized error — and builds source health, a per-source map of status, error, and core flag. Third, packaging: all of these are exposed as single items the AI prompt can inject with simple expressions. Keep this mental model: Normalize makes every source speak the same language; Prepare writes the morning report's cover memo about who showed up for work and who called in sick.

**Key points:**
- Prepare = tolerance + triage + packaging
- Missing source → synthesized error placeholder
- core_source_failures: name/status/sanitized error
- source_health: per-source status map
- Normalize = same language; Prepare = cover memo