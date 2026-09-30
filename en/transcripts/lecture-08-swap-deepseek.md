# Lecture 8: Swapping to DeepSeek & Wiring the Data Injection
Section: AI Agent Disciplined Prompting

Replace the fictional gpt-5.6-luna model with a DeepSeek Chat Model node (temperature 0.3, JSON mode), sync the Setup config fields, and understand the three expression injection points feeding the prompt.

Time to perform mandatory surgery on the AI node. The model wired into the template, g p t dash five point six luna, does not exist — it is a placeholder name the author left for a future model. If you execute as-is, the AI node fails every single time. Here is the six-step swap. One: delete the OpenAI Chat Model node. Two: drag in a DeepSeek Chat Model node — it ships built-in with n8n version two under the LangChain node family. Three: create a DeepSeek credential: register at api dot deepseek dot com, top up a trivial amount — one yuan runs the workflow hundreds of times — and paste in the key. Four: pick the model deepseek dash chat and set temperature to zero point three. Five: connect it to the AI Agent's chat model input. Six: open the Setup Workflow Configuration node and update two fields — model provider to deepseek and model name to deepseek dash chat. Those two fields get written into the Postgres record, so months from now you can trace exactly which brain wrote which briefing.

**Key points:**
- gpt-5.6-luna = fictional placeholder, must replace
- Built-in DeepSeek Chat Model node (n8n 2.x)
- deepseek-chat + temperature 0.3
- Sync Setup node model_provider & model_name
- Config fields written to Postgres for traceability

Why DeepSeek, and why zero point three? Because this workflow's entire contract with downstream machinery is emit valid JSON, every time. During validation we ran the untouched prompt against deepseek dash chat with response format set to json underscore object — and got parseable JSON, one hundred percent data fidelity, and no fabrication, on the first run. Temperature is the other half of that stability story. Here's the hands-on experiment from the docs I insist you actually perform: run the pipeline once at zero point three, then set temperature to one point five and run three more times. Watch the JSON wobble — fields drifting, enum values wandering. Low temperature is not a style choice here; it's a reliability requirement.

**Key points:**
- L2: deepseek-chat + json_object = valid on first run
- temperature ≤ 0.3 is a reliability spec
- Exercise: temp 1.5 ×3 runs → watch JSON wobble

Last, the data injection. The prompt is static text with three live expressions, each calling J S O N dot stringify on a field from the Prepare node: normalized sources injects the five envelopes; source health injects the per-source status map the AI uses to write its missing sources list; and core source failures injects the critical-condition list the AI must surface prominently. And one more honesty experiment: deliberately break one core key — mistype your FRED key — run the pipeline, and read the output. The briefing should openly report that rates data is missing. When the AI tells on its own infrastructure, the system is working as designed.

**Key points:**
- 3 injections: normalized_sources, source_health, core_source_failures
- via JSON.stringify expressions into prompt text
- Honesty test: break FRED key → AI must report it