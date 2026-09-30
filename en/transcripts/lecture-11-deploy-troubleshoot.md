# Lecture 11: Deployment & Troubleshooting Playbook
Section: Deploy & Extend

The go-live checklist — green manual run before activation, timezone traps (instance TZ vs report_timezone), overseas hosting, error-trigger alerting — plus the five-row troubleshooting table for every failure we've seen.

Everything works in manual runs. Now let's make it run at seven forty-five every weekday without you. The deployment checklist has four items, and the order matters. Item one: never activate a schedule you haven't seen green. A full manual run — five sources ok, Validate passing, at least one delivery landed — is the entry ticket to activation. Item two: timezones, the number-one silent killer of scheduled briefings. There are two separate knobs. The Schedule Trigger's cron follows the n8n instance timezone, which defaults to U T C — set the T Z and GENERIC underscore TIMEZONE environment variables. Separately, the report underscore timezone field in the Setup node controls the news lookback window and how event times are displayed inside the briefing. Mixing these two up gets you a briefing that arrives at the wrong hour talking about the wrong day. Item three: hosting. Every fetch hits the open internet, and Telegram needs a non-mainland route — an overseas cloud host, say Singapore, is the path of least resistance. Item four: monitoring. Turn on execution-log retention, and add an Error Trigger workflow that pings you — because remember the design: a Validate throw means no briefing shipped today, and you want to hear that silence from an alarm, not from your readers.

**Key points:**
- ① Green manual run BEFORE activating schedule
- ② Two TZ knobs: instance TZ (TZ/GENERIC_TIMEZONE) vs report_timezone
- ③ Overseas host = least-resistance path
- ④ Execution logs + Error Trigger alert

Now the troubleshooting playbook — five symptoms, five causes, five fixes. Symptom: all sources error. Cause: the credential-type mix-up from lecture four, query versus header auth swapped. Fix: recheck the wiring table. Symptom: the AI node says model not found. Cause: you didn't replace the placeholder g p t five point six luna. Fix: section four's six-step swap. Symptom: Validate throws not valid JSON. Cause: temperature too high or JSON mode off. Fix: temperature at or below zero point three, response format json underscore object on DeepSeek. Symptom: your alert channel floods with empty-state warnings every weekend. Cause: you're monitoring empty as if it were error. Fix: empty is a normal business state — alert only on error, and ideally only on error in a core source. Symptom: Telegram sends time out with E TIMEDOUT. Cause: the server is in mainland China. Fix: overseas node or proxy, as we said.

**Key points:**
- All error → credential type mix-up (L4 table)
- model not found → fake model not swapped
- not valid JSON → temp/JSON-mode
- Weekend empty spam → monitor errors only (core)
- TG ETIMEDOUT → mainland server → go overseas

Notice the pattern across all five: every failure maps to exactly one lecture of this course. That's not an accident — the course structure mirrors the failure surface of the system, which is what makes it a reference you'll keep coming back to after deployment, not just a tutorial you watch once.

**Key points:**
- Every failure maps to one lecture
- Course structure = system failure surface