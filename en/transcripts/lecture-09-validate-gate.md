# Lecture 9: Validate AI Output: The Customs Gate
Section: Validation & Delivery

The four-stage enforcement pipeline: multi-field text extraction, JSON.parse-or-throw, market_regime enum check, and tolerant field backfill — hard errors kill the run, soft problems get repaired.

The prompt makes promises; the Validate AI Output node enforces them. Think of it as a customs gate sitting between the AI and the outside world — Telegram, Discord, and your database. Nothing crosses without inspection. Stage one: find the text. Different model integrations return the answer under different property names, so the node looks across output, text, response, message, and content until it finds the payload. Stage two: parse. J S O N dot parse runs on that text, and if parsing fails, the node throws. Not warns, not logs — throws. The entire n8n execution fails, and because the delivery branches are downstream, nothing — no Telegram message, no Discord post, no database row — gets sent. Stage three: the enum check. The market regime field must be one of the five enumerated values from the prompt contract. Anything outside the list throws as well. Stage four: tolerant repair. Soft problems — assets, drivers, or watch next arriving as something other than an array — are coerced to standard structure, or backfilled with an empty array, and the run continues.

**Key points:**
- Stage 1: find text across output/text/response/message/content
- Stage 2: JSON.parse fail → THROW (run dies, nothing ships)
- Stage 3: market_regime enum of 5 → out-of-range throws
- Stage 4: soft fields coerced/backfilled, run continues

The design philosophy deserves a beat: hard errors break the chain, soft problems get backfilled and pass. Why so brutal on the hard side? Because the failure_modes are asymmetric. A malformed or hallucinated briefing that reaches a thousand subscribers damages trust permanently; a briefing that simply doesn't go out today is a shrug. When in doubt, don't send — then alert. There's also a delightful bit of tolerance engineering in stage three: if the model returns market regime as an object with a label property — say, capital-R Risk dash Off — the validator normalizes it to the lowercase snake-case enum risk underscore off. It forgives small stylistic whims, but never arbitrary values. Gatekeepers should be picky about content, not capitalization.

**Key points:**
- Philosophy: better NO briefing than a WRONG briefing
- Wrong briefing = permanent trust damage
- Tolerance: {label:'Risk-Off'} → risk_off
- Picky about content, not capitalization

Prove the gate works with the tamper experiment: insert a Code node just before Validate that forcibly sets market regime to the word euphoric. Execute. You should see the run die at Validate, and your phone should stay silent — no Telegram message arrives. That silence is the sound of the architecture doing its job. If you take only one habit from this course into your own AI projects, make it this one: never let model output reach a customer without a deterministic gate in between.

**Key points:**
- Tamper test: force market_regime='euphoric'
- Run dies at Validate; Telegram stays silent
- Rule for all AI projects: deterministic gate before customers