# Grok 4.5 vs GLM

Grok 4.5 is xAI's new frontier model for coding, STEM, knowledge work, and agentic tasks. It is available through xAI and gateways like OpenRouter. Pricing is simple: \$2/M input, \$6/M output, with a 500K context window on OpenRouter. xAI's docs also list function calling, web search, X search, and code execution in the stack. ([openrouter.ai](https://openrouter.ai/x-ai/grok-4.5/uptime))

GLM 5.2 takes another path. [Z.ai](http://z.ai/) pitches it for long-horizon work. The weights are on Hugging Face under MIT, and the model card lists 753B parameters with a 1M-token context target. Hosted pricing is lower in the lanes I would actually use: [Z.ai](http://z.ai/) lists \$1.40/M input and \$4.40/M output, while OpenRouter currently shows \$0.84/M input and \$2.64/M output for `z-ai/glm-5.2`. ([huggingface.co](https://huggingface.co/zai-org/GLM-5.2))

The comparison gets interesting once agent usage enters the picture. Grok has the frontier badge and first-party agent tooling. GLM’s appeal is practical: open weights, a longer context window, and a cheaper meter for token-heavy loops that burn tokens on retries, tool schemas, intermediate reasoning, and dumb little SaaS confirmations.

So I ran both behind the same agent on the same real tool-use benchmark. The SaaS accounts, task set, tool surface, and evaluator stayed fixed. I only swapped the model.

I wanted the answer I need for production SaaS workflows: which model actually gets the account into the right final state, and whether the higher token price buys fewer headaches.

## The two models

| Attribute | Grok 4.5 | GLM 5.2 |
| --- | --- | --- |
| Pricing (hosted, per 1M tokens) | \$2.00 input / \$6.00 output (OpenRouter) | \$0.84 input / \$2.64 output (OpenRouter promo); \$1.40 input / \$4.40 output ([Z.ai](http://Z.ai) list) |
| Prompt caching | xAI recommends setting `prompt_cache_key` or `x-grok-conv-id` to avoid cache-cold runs paying full input price | Not specified in cited sources |
| Context length | 500K context window | 1M-token context target / marketed as 1M context |
| Model size | Not specified in cited sources | 753B parameters (model card) |

### Grok 4.5

Grok 4.5 is the xAI frontier pick here. xAI positions it as the flagship for code, agentic tool calling, knowledge work, and STEM, with configurable reasoning plus built-in function calling, web search, X search, and code execution. The model card lists a 500K context window, \$2.00 per million input tokens, and \$6.00 per million output tokens. ([docs.x.ai](https://docs.x.ai/developers/grok-4-5))

Going in, it looked like the safer expensive bet. The pitch is direct: give it engineering work and tools, then let it grind. The detail that made me slow down was caching. xAI explicitly recommends setting `prompt_cache_key` or `x-grok-conv-id`, because otherwise a long agent loop can land on a cache-cold server and pay full input price. That is exactly the kind of small integration footgun that can quietly turn a good model into a dumb bill. ([docs.x.ai](https://docs.x.ai/developers/grok-4-5))

### GLM 5.2

GLM 5.2 is [Z.ai](http://z.ai/)’s open-weight heavyweight. OpenRouter describes it as a large-scale reasoning model for long-horizon agent workflows and project-level software engineering, with `high` and `xhigh` reasoning modes for complex multi-step automation. It has a 1M-token context window, and the OpenRouter card for `z-ai/glm-5.2` is currently marked 40% off at \$0.84 per million input tokens and \$2.64 per million output tokens. [Z.ai](http://z.ai/)’s own pricing page lists \$1.40 input and \$4.40 output per million tokens. ([openrouter.ai](https://openrouter.ai/z-ai/glm-5.2))

GLM 5.2 is what made the comparison interesting. The weights are on Hugging Face under an MIT license, with local serving paths shown for vLLM and SGLang, so the escape hatch is real if you want to experiment outside a hosted API. The 1M context window also changes how I would run it before any test starts. You can dump a whole messy workspace into it, which is useful when the workspace matters and expensive when the agent starts looking productive instead of making progress. ([huggingface.co](https://huggingface.co/zai-org/GLM-5.2))

Using OpenRouter’s current cards, Grok 4.5 is roughly 2.4x GLM 5.2’s input price and 2.3x its output price: \$2.00 vs \$0.84 in, \$6.00 vs \$2.64 out. If you price GLM 5.2 at [Z.ai](http://z.ai/)’s direct list rate instead of the temporary OpenRouter discount, the gap shrinks to about 1.4x on input and 1.36x on output. ([openrouter.ai](https://openrouter.ai/x-ai/grok-4.5/uptime))

## Composio GoldenEval: Real-world tool-use benchmark

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/grok-vs-glm/image_1.png)

Composio Golden Eval is a real-account tool-use benchmark. Claude Code runs as the agent, talks to tools through the hosted Composio MCP router, and has to finish hard, multi-step SaaS tasks against live accounts.

The run spans 38 scenarios and 76 trials across normal work apps, including Salesforce, HubSpot, Jira, Linear, GitHub, Zendesk, Slack, Gmail, Google Sheets, Notion, Airtable, plus ops and product tools like PagerDuty, PostHog, LaunchDarkly, and Datadog.

Pass/fail comes from discovery plus API readback against the real account state. The evaluator makes extra API reads to find the run’s tagged writes, checks the expected state, and fails anything extra or missing. No LLM transcript judge gets to skim the conversation and decide whether the intent looked good enough. Each write is uniquely tagged and cleaned up after the run. That is how you test noisy live accounts without trashing them.

The tasks hit production failure modes. Exact-set reconciliation can fail because of one extra Gmail message with a label. Dedup requires identity normalization before duplicate detection. Scoped mutation stops the agent from fixing rows that were supposed to stay unchanged. Audit work forces full collection, including paginated results. The verifier treats almost-right as a fail.

### What the tasks actually look like

In Salesforce, one task asks the agent to find Account records whose website domain normalizes to a target and whose name carries the run tag. Then it has to treat the normalized domain as the identity, report the duplicate groups, and pick the newest-created record as canonical. That leaves a lot of room for dumb mistakes: comparing raw websites, missing the tag scope, or choosing the wrong canonical record.

The Gmail task is deceptively small. Find every message whose subject contains a specific token, create the label if it does not exist, and apply it to exactly those messages and no others. If the agent grabs the first matching page, or labels a nearby message because the subject looks similar, the account readback catches it.

Linear tests the same thing from the mutation side. In a team backlog, the agent has to set an estimate of 1 point on every tagged issue with no estimate, while leaving already-estimated issues untouched. Bulk edits are where agents love to overreach, so the scoped part matters.

GitHub makes the agent audit repos for hardcoded credentials by searching Python files for a `token = ...` pattern. The catch is coverage: it has to walk the full paginated result set instead of stopping at whatever comes back first.

With Google Sheets, the precision is tiny. Using data filters and grid ranges, the agent has to change one target row’s value and leave the rest alone. A wrong row or broad range update gives it away in the final sheet state, and a loose filter does too.

## How I ran it

I ran both models as the agent behind the same Claude Code + Composio MCP setup, one attempt per scenario, across the same 38 scenarios. I pointed Claude Code at OpenRouter for Grok 4.5 and used [Z.ai](http://z.ai/)'s Claude Code runtime for GLM 5.2. I kept the same tool surface and scenario set, with identical grading. Only the model changed.

Grading was pass/fail per trial using real-account API readback verifiers, with tag-scoped cleanup.

| Metric | Grok 4.5 | GLM 5.2 |
| --- | --- | --- |
| Scenarios passed | 36/38 (95%) | 37/38 (97%) |
| Quality | 95% | 97% |
| Avg time per scenario | 164.2s | 101.4s |
| Avg turns | 13.7 | 7.1 |
| Avg tool calls | 12.7 | 6.1 |
| Avg tokens per scenario | 508,421 | 320,480 |
| Price (input / output per 1M) | \$2.00 / \$6.00 | \$0.84 / \$2.64 |

GLM 5.2 came out ahead, but barely on pass rate: 37/38 versus 36/38, or 97% versus 95%. The bigger gap was how much work it took to get there. GLM 5.2 averaged 101.4 seconds per scenario, 7.1 turns, 6.1 tool calls, and 320,480 tokens. Grok 4.5 averaged 164.2 seconds, 13.7 turns, 12.7 tool calls, and 508,421 tokens for the same 38-scenario run.

## Findings

Pass rate was close enough to make the ranking fragile. GLM 5.2 finished 37/38, or 97%. Grok 4.5 finished 36/38, or 95%. Both cleared almost the whole suite. The misses were real-account failures I wanted to catch, mainly exact cell targeting and exact-set completion, plus one paginated-recall miss.

On Google Sheets, Grok missed `sheets datafilter gridrange`, the scenario that changes exactly one target row’s Value cell per tab and touches nothing else. The verifier expected only row 21 on one tab and row 18 on another to change. Grok changed the wrong cells. It is the nasty version of a spreadsheet task, where the model can understand the goal and still lose because the edit lands one row off or touches the wrong range. GLM 5.2 passed that one.

In Linear, Grok missed `linear backlog estimates`, the scenario for setting estimate 1 on every unestimated tagged backlog issue in a Linear team while leaving already estimated issues alone. The readback found that not every required backlog issue ended up at estimate 1, so the completeness check failed. The Linear integration worked at a basic level; the miss was exact-set completion. GLM 5.2 passed it.

GLM 5.2 missed the GitHub code-search scenario, `gh code search secrets`. The task was to use GitHub code search to find every Python file with a hardcoded `token = ...` pattern across repos, covering the full paginated result set. GLM returned only 18 files when the task needed at least 38; the evaluator’s baseline was 48. It stopped at the first page instead of paging through all matches. Grok 4.5 passed that one.

The efficiency split was wider than the pass-rate split. Across all 38 scenarios, Grok 4.5 spent about 19.3M tokens, 104 minutes of agent time, and 483 tool calls. GLM 5.2 spent about 12.2M tokens, 64 minutes, and 232 tool calls. That means Grok used roughly 60% more tokens and roughly 60% more wall-clock time, with more than double the tool calls to cover the same suite.

The Gmail label batch task showed that gap. Both models passed. GLM 5.2 applied the label in 81 seconds with 6 tool calls and 306k tokens. Grok 4.5 took 379 seconds with 25 tool calls and about 1.18M tokens for the same passing result. In tool calls and tokens, Grok did roughly four times the work.

The pattern was broader than one ugly Gmail run. Grok used more tokens than GLM on 34 of the 38 scenarios. On `salesforce external id dedup`, which deduplicates tagged Salesforce Accounts by normalized website domain, GLM finished in 30 seconds with 2 tool calls, while Grok took 415 seconds with 9 tool calls.

Cost compounds both effects because GLM 5.2 did less work and had the lower token price.

## What it costs

For cost, I used runtime token counts and priced every runtime token at each model’s listed input rate as a ceiling. Prompt caching can reduce repeated input charges, so I treat the dollar figures as upper-bound estimates. The ratio is the part I’d trust more.

| Model | Runtime tokens | Est. suite cost | Est. per scenario |
| --- | --- | --- | --- |
| Grok 4.5 | 19,320,020 | \$39 | \$1.02 |
| GLM 5.2 | 12,178,259 | \$10 | \$0.27 |

Grok 4.5 comes out about 3.8x more expensive for the 38-scenario suite. That gap has two pieces: Grok 4.5 costs about 2.4x more per token, then it also processed about 60% more tokens.

The extra spend did not buy the better result here. Grok 4.5 was the more expensive, heavier-running model, and it still finished one scenario behind GLM 5.2.

At volume, I’d care a lot more about the ratio than the exact dollars. A few thousand runs turns \$1.02 vs \$0.27 per scenario into a budget line pretty quickly.

## Verdict

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/grok-vs-glm/image_2.png)

GLM 5.2 won the run on pass rate, 37/38 to 36/38, and it was much leaner while doing it: 101.4 seconds vs 164.2 seconds on average, 320,480 vs 508,421 runtime tokens, and 6.1 vs 12.7 tool calls. If your cost tracks token burn, the price pressure points the same way as the speed result.

Use Grok 4.5 when recall is the scary part, especially if the workflow looks like paginated search. The one GLM miss was `gh code search secrets`, where it returned only 18 files when the task needed at least 38 and the evaluator’s baseline was 48. It stopped at the first page. Grok passed that. If I’m sweeping repos for hardcoded `token = ...` patterns and missing page two is the failure mode that can bite me later, Grok’s slower, heavier run is easier to justify.

For most normal agent workflows in this run, I’d use GLM 5.2. It passed more scenarios, used about 12.2M total tokens instead of Grok’s 19.3M, finished in 64 minutes instead of 104, and made 232 tool calls instead of 483. The gap also showed up on boring real work such as the Gmail label batch task. Both passed there, but GLM finished in 81 seconds with 6 tool calls and 306k tokens. Grok took 379 seconds, 25 tool calls, and about 1.18M tokens for the same passing result.

I would not overread one run over 38 scenarios. Both models landed in the mid-90s here, and the misses were small-but-real verifier failures while the models still completed nearly everything else. Grok got dinged on an exact Google Sheets cell edit and a Linear estimate completeness check. GLM got dinged on paginated GitHub recall. Grok passed GLM’s miss, and GLM passed both of Grok’s misses.

For my own workflow, I’d start with GLM 5.2 as the default because it cost less on this usage and still landed one more pass, with shorter runs. I’d keep Grok 4.5 in the rotation for jobs where incomplete search results are worse than extra tokens and time.
