# Lecture 2: Import the Template & Read the 17-Node Map
Section: Getting Started: Overview & Import

Import template 19457 into your n8n instance, run it once safely, and learn to read the workflow by its seven functional groups — trigger/config, fetch, normalize, aggregate, AI, validate, deliver.

Let's get the template onto your canvas. Open n8n dot io slash workflows slash 19457, click Use Workflow to copy the JSON — or simply use the workflow dot json file shipped with this course. In your n8n canvas, click the three dots at the top right, choose Import from File or URL, and load it. One rule: do not activate the schedule yet. Instead, hit Execute once manually. You will see a wall of red errors, and that is exactly what we want — none of the credentials are configured yet. Today's goal is not a green run; it's learning to read the skeleton.

**Key points:**
- Import via n8n.io/workflows/19457 or bundled workflow.json
- Do NOT activate the schedule yet
- First manual run = all red is expected
- Goal: read structure, not get green

Seventeen nodes sounds like a lot until you group them the way the template author did, with sticky notes. Group one: trigger and configuration — a Schedule Trigger named When Weekdays at seven forty-five A M, running the cron expression forty-five seven star star one dash five, plus a Setup node that holds global config like timezone and model names. Group two: five Fetch nodes, all HTTP Request nodes, and every one of them is configured with on error continue regular output plus retry on fail. That single setting is the first line of defense — a dead source becomes data, not a crash. Group three: five Normalize nodes, one Code node per source, the soul of this course, covered in depth in section three. Group four: aggregation — a Merge node with five inputs, then Prepare Data for AI Model, which builds the health report and the core-failure list. Group five: the AI itself — an AI Agent node with a chat model attached. Group six: Validate AI Output, the customs gate. And group seven: three delivery branches, each with a Build Code node followed by a send node.

**Key points:**
- ① Trigger + Setup config (cron 45 7 * * 1-5)
- ② Fetch ×5 — onError=continue + retry
- ③ Normalize ×5 — the soul (Section 3)
- ④ Merge + Prepare — health report
- ⑤ AI Agent ⑥ Validate ⑦ Build+Send ×3

Three pitfalls to flag right now, before they cost you an hour. First, the chat model on the canvas is named g p t dash five point six luna — that model does not exist. It is a placeholder from the template's author, and the AI node will fail until you replace it in section four. Second, the Postgres branch expects a table called public dot briefings to exist already; the create-table statement is in section five. Third, the template ships with its timezone set to Europe Berlin — change that in the Setup node to whatever timezone your audience actually wakes up in. With the map in your head, let's move to the data sources themselves.

**Key points:**
- ⚠ gpt-5.6-luna is a fake placeholder model
- ⚠ Postgres needs public.briefings pre-created
- ⚠ Default timezone is Europe/Berlin — change it