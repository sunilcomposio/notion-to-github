# GLM 5.2 vs Kimi K2.7: Open-weight agentic coding and tool use head-to-head

It was first Minimax with their m2 and then Kimi last week with Kimi K2.7 code and now Zhipu has unveiled their best GLM 5.2. What a time to be alive as an open-weight model enthusiast.

Absolute bonkers that all these Chinese labs are pushing the open-weight frontiers when Anthropic and OpenAi are doing away with their subsidized cost. You can’t not love them enough. Truly the saviour of token poors.

I have already covered Minimax m2 and Kimi 2.7 earlier and in this post I will just be talking about GLM 5.2 and Kimi 2.7. Kimi is currently holds crown for the best open-weight model but if the words to believed GLM might just take the throne.

And that is why we’re here. I have a standard criteria for comparison. There are two rounds.

1. Testing the models on toughest terminal bench questions for agentic coding tasks
2. And a long-running workflows with Composio tool router for real-world automation tasks

So, let’s go.

## TL;DR

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/glm-5-2-vs-kimi-k2-7-open-weight-agentic-coding-and-tool-use-head-to-head/image_1.png)

If you want the quick version, here is what happened:

- Agentic terminal coding ended 5/10 for both models. GLM 5.2 and Kimi K2.7 tied, but they passed different tasks.
- The coding run was cheaper with GLM 5.2: \$4.96 total for its 5 solves, or \$0.99 per solve. Kimi K2.7 was \$5.86 total and \$1.17 per solve.
- Across 22 Composio tasks using Gmail, Slack, Drive, GitHub, Calendar, Reddit, Notion, and web search, GLM 5.2 averaged 0.800 and Kimi K2.7 averaged 0.775.
- Kimi K2.7 cost less on the tool-use run: \$1.78 total versus GLM 5.2 at \$2.55.
- In this run, GLM 5.2 cost less for coding and scored a bit higher on tool quality. Kimi K2.7 cost less on tools.

Terminal-Bench was a split result. Both landed at 5/10, but they passed different tasks. On Composio workflows, GLM 5.2 was ahead by 0.025 points, 0.800 to 0.775. Costs split by workload, GLM on coding and Kimi on tools.

## Intro to GLM 5.2

[GLM 5.2 is Zhipu](https://z.ai/blog/glm-5.2) AI's open-weight model, pitched around agents, with native tool calling and coding work emphasized over pure chat.

On OpenRouter, it sits at \$1.40 per million input tokens and \$4.40 per million output tokens. The output side is the part I watch with coding agents, because verbose plans, patch diffs, logs, and retries can pile up fast. At \$4.40 per million output tokens, it is cheap enough to test seriously, but I would still keep an eye on chatty runs.

## Intro to Kimi K2.7

[Kimi K2.7 is Moonshot](https://www.kimi.com/resources/kimi-k2-7-code) AI’s coding-focused Kimi model, available on OpenRouter as `moonshotai/kimi-k2.7-code`. Moonshot is selling it on native tool calling and code work. Moonshot also frames K2.7 as a step up from K2.6. That puts it in the same lane as GLM 5.2 when tool calls and code edits happen in one loop.

Pricing is lower than GLM 5.2 on OpenRouter: \$0.74 per million input tokens and \$3.50 per million output tokens, compared with GLM 5.2 at \$1.40 input and \$4.40 output. Agent loops burn output tokens fast, so I care about that \$3.50 number more than I would in a normal chat test. K2.7 gives me a little more tolerance for verbose agent loops before the bill starts annoying me.

## How I tested this

I ran two evaluations with the same harness each time. The model swap was the OpenRouter ID: GLM 5.2 `z-ai/glm-5.2` or Kimi K2.7 `moonshotai/kimi-k2.7-code`.

- I used Terminal-Bench 2.0 for terminal coding, with opencode, an agent wrapper that turns model messages into terminal actions, on Daytona cloud sandboxes, which are isolated cloud Linux environments, through OpenRouter.
- I ran the tool-use side as a Composio suite over Gmail, Google Calendar, GitHub, Slack, Google Drive, Reddit, Notion, and web search, also through OpenRouter.

#### Terminal Bench

Terminal-Bench is a benchmark of hard, real command-line tasks. In concrete terms, the model might have to write a compressor, recover a password, reverse a path tracer, fix a vulnerability, or build around tensor parallelism.

The benchmark labels in this subset cover cryptanalysis, vulnerability fixing, LLM inference batching, grouping model requests so they run efficiently, interpreter work, password recovery, path-tracing reversal, regex/chess logic, PyTorch parallelism, and compression. Each task runs in its own isolated Linux sandbox, and an automated test decides whether the final state passes or fails. I used a 10-task hard subset rather than the full Terminal-Bench set. Here, “solved” means the task’s automated test passed.

An agent drives the shell loop for the model: it reads the terminal state, asks the model for the next action, runs the command, sends the output back, and repeats. One step is one full command-output round trip. When a task goes past 100 steps, the model has worked through a long chain of terminal actions instead of answering once and stopping.

Each Terminal-Bench task has a built-in time limit. I doubled that limit for both models after a first pass at the standard limit cut them off while they were still making progress. The longer budget kept early timeouts from dominating the result.

#### Tool-use automation workflows

For the Composio run, the tasks hit real connected accounts across Gmail, Google Calendar, GitHub, Slack, Google Drive, Reddit, Notion, and web search. Each model got Composio’s tool-router meta tools, helper tools that let the agent find and invoke the right app action during the run. A separate GPT-5.5 judge graded each finished task from 0 to 1 based on whether the model actually did the job against the account contents, including live web checks for public claims. A 1 means the task was fully correct, a 0 means it failed, and partial scores mean it got some of the required work right.

Tasks that only hit provider errors, Daytona setup hiccups, or tool-router stalls were excluded for both models, so both scores used the same task set.

## Test 1: Agentic terminal coding

I ran the 10-task hard Terminal-Bench 2.0 subset on the same opencode agent setup, with both models getting the doubled time budget. Each row is pass/fail against the task’s automated tests.

| Task | GLM 5.2 | GLM cost | Kimi K2.7 | Kimi cost |
| --- | --- | --- | --- | --- |
| feal-differential-cryptanalysis | ✅ | \$0.10 | ✅ | \$0.18 |
| fix-code-vulnerability | ✅ | \$0.08 | ❌ | \$0.02 |
| llm-inference-batching-scheduler | ✅ | \$0.87 | ✅ | \$0.35 |
| make-mips-interpreter | ❌ | \$2.07 | ❌ | \$0.86 |
| password-recovery | ✅ | \$0.33 | ✅ | \$0.14 |
| path-tracing-reverse | ❌ | \$0.98 | ❌ | \$1.75 |
| regex-chess | ❌ | \$0.00 | ✅ | \$2.32 |
| torch-pipeline-parallelism | ❌ | \$0.14 | ❌ | \$0.06 |
| torch-tensor-parallelism | ❌ | \$0.01 | ✅ | \$0.13 |
| write-compressor | ✅ | \$0.38 | ❌ | \$0.04 |
| **Total** | **5/10** | **\$4.96** | **5/10** | **\$5.86** |

Both models solved 5/10. The shared passes were `feal-differential-cryptanalysis`, a FEAL block-cipher cryptanalysis task; `llm-inference-batching-scheduler`, a task about batching inference requests for an LLM server; and `password-recovery`, which requires recovering a password. GLM 5.2’s extra passes were `fix-code-vulnerability` and `write-compressor`. Kimi K2.7’s were `regex-chess`, a regex/chess logic task, and `torch-tensor-parallelism`, a PyTorch tensor-parallelism task for splitting tensor work across devices.

The tasks neither model passed were `make-mips-interpreter`, `path-tracing-reverse`, and `torch-pipeline-parallelism`, which cover building an interpreter for MIPS assembly, reversing path-tracing output, and PyTorch pipeline parallelism, where work is split into stages. Both models failed those three even under the longer time limit. I read the result as a rough tie on this hard terminal subset.

GLM 5.2 reached the same 5/10 result for less money: \$4.96 total versus \$5.86 for Kimi K2.7.

## Test 2: Real tool use

The tool-use run used real jobs against connected accounts: summarize a month of email by sender, count a week of Slack by channel, organize a thousand Drive files, look up a GitHub repo, and draft outreach from web research.

I ran 22 tasks across Gmail, Google Calendar, GitHub, Slack, Google Drive, Reddit, Notion, and web search. Each task was scored from 0 to 1 by GPT-5.5 using the final result, the task requirements, and web verification for public claims.

| Task | GLM 5.2 | Kimi K2.7 | GLM cost | Kimi cost |
| --- | --- | --- | --- | --- |
| Last commit on a GitHub repo | 1.00 | 0.45 | \$0.032 | \$0.018 |
| GitHub repo stats lookup (Composio) | 0.98 | 1.00 | \$0.017 | \$0.008 |
| Summarize last 10 unread emails by sender | 0.98 | 0.98 | \$0.028 | \$0.016 |
| Categorize 3 years of calendar events | 0.95 | 0.82 | \$0.132 | \$0.080 |
| Organize 10 recent Drive files | 0.92 | 0.75 | \$0.023 | \$0.013 |
| Next 5 calendar events as a reminder | 0.90 | 0.86 | \$0.108 | \$0.038 |
| Productivity report from Calendar + GitHub + Slack | 0.90 | 0.92 | \$0.091 | \$0.226 |
| Top 10 Reddit posts on LLMs | 0.90 | 0.86 | \$0.109 | \$0.050 |
| Summarize the last 20 Slack messages | 0.88 | 0.75 | \$0.077 | \$0.024 |
| Summarize a month of inbox by sender | 0.86 | 0.90 | \$0.048 | \$0.066 |
| San Francisco weather from web search | 0.85 | 0.95 | \$0.018 | \$0.012 |
| Organize 1,000 Drive files by type and date | 0.82 | 0.96 | \$0.091 | \$0.052 |
| Top 5 Reddit posts on Python | 0.82 | 0.62 | \$0.070 | \$0.018 |
| List 10 recent Notion pages | 0.78 | 0.72 | \$0.027 | \$0.016 |
| Research report on microservices | 0.78 | 0.82 | \$0.212 | \$0.138 |
| Top 10 web results on Docker | 0.78 | 0.45 | \$0.059 | \$0.006 |
| Daily tech digest from email + web news | 0.65 | 0.86 | \$0.157 | \$0.058 |
| Scrape YC \$20M+ startups, draft outreach | 0.65 | 0.62 | \$0.452 | \$0.519 |
| Company intel report from web search | 0.55 | 0.82 | \$0.241 | \$0.136 |
| Frontend trends from web + Reddit | 0.55 | 0.68 | \$0.199 | \$0.106 |
| Trending GitHub repos in Rust/Go/Python | 0.55 | 0.65 | \$0.123 | \$0.020 |
| Compare Rust, Go, and Zig from web research | 0.55 | 0.62 | \$0.239 | \$0.159 |
| **Average score / total cost** | **0.800** | **0.775** | **\$2.55** | **\$1.78** |

The averages were 0.800 for GLM 5.2 and 0.775 for Kimi K2.7. The gap here was small: 0.025 points over 22 tasks. Kimi’s total cost was lower, \$1.78 versus \$2.55.

GLM 5.2 had the largest task-level gaps. Last commit on a GitHub repo was 1.00 vs 0.45, and Top 10 web results on Docker was 0.78 vs 0.45. GLM was also ahead on the two calendar tasks I tested, with Categorize 3 years of calendar events at 0.95 vs 0.82 and Next 5 calendar events as a reminder at 0.90 vs 0.86.

Kimi K2.7 led on Summarize a month of inbox by sender, 0.90 vs 0.86, and did better on Daily tech digest from email + web news, 0.86 vs 0.65. It also won Organize 1,000 Drive files by type and date, 0.96 vs 0.82, plus Company intel report from web search, 0.82 vs 0.55. Task-level leads bounced around, which is why the 22-task average ended close.

For SaaS-heavy agent work, I’d pick between GLM 5.2 and Kimi K2.7 based on price and the tasks in the queue. In this run, the GLM-friendly work was GitHub lookups, calendar work, the Docker search task, and Reddit post ranking. The Kimi-friendly work was Drive cleanup, the month-of-inbox summary, the daily tech digest, and company/web research, with a lower total run cost.

## Cost

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/glm-5-2-vs-kimi-k2-7-open-weight-agentic-coding-and-tool-use-head-to-head/image_2.png)

Cost here means the per-run price OpenRouter billed in USD, summed per task. I used the charged amount instead of a token-count estimate.

On Terminal-Bench, total spend was \$4.96 for GLM 5.2 and \$5.86 for Kimi K2.7. They had the same number of solves, so GLM 5.2 was cheaper: \$0.99 per solved task versus \$1.17 for Kimi K2.7.

Composio went the other way: \$2.55 for GLM 5.2 and \$1.78 for Kimi K2.7.

The public price sheet explains part of the split. GLM 5.2 is billed at \$1.40 per million input tokens and \$4.40 per million output tokens. Kimi K2.7 is cheaper per token at \$0.74 input and \$3.50 output. Terminal-Bench favored GLM 5.2 on cost for the same solves. In Composio, Kimi K2.7’s lower per-token price made it cheaper.

Caveat: these were small task counts, and OpenRouter’s billed prices can differ a bit from a vendor’s own API. I would treat these as rough run costs rather than exact pricing claims.

## Final Verdict

I see this as a near tie. GLM 5.2 and Kimi K2.7 both solved 5/10 on Terminal-Bench, then GLM 5.2 led the Composio tool-use run at 0.800 vs 0.775. Cost flipped by test: GLM 5.2 was cheaper on coding, Kimi K2.7 was cheaper on tools.

Terminal-Bench’s equal 5/10 score hides which tasks each model passed. Both solved FEAL block-cipher cryptanalysis, LLM inference batching scheduler, and password recovery. GLM 5.2 alone got code-vulnerability fixing and compressor writing; Kimi K2.7 alone got regex/chess logic and PyTorch tensor parallelism. At the same pass count, GLM’s bill was lower: \$4.96 for its 5 solves vs Kimi’s \$5.86.

Composio was closer. GLM 5.2 averaged 0.800 across the tool suite while Kimi K2.7 averaged 0.775. The difference was 0.025 points. GLM looked stronger on GitHub/calendar tasks and the Docker web search task, while Kimi looked better on Gmail and Drive cleanup. Kimi was cheaper here, \$1.78 vs GLM’s \$2.55.

### When you’ll notice the difference

If your agent mostly does hard terminal coding, I’d treat them as interchangeable on raw success rate and pick based on the kinds of tasks you care about. GLM 5.2 was cheaper in this run and won different coding tasks than Kimi, so I’d try both against your repo before standardizing.

For SaaS-tool orchestration, I’d lean GLM 5.2 when you care about the small average-score edge, especially for GitHub and calendar workflows or the Docker-style web search task. I’d lean Kimi K2.7 when Gmail-heavy workflows or lower tool-use cost matter more.

Caveat: this was only 10 Terminal-Bench tasks and 22 Composio tasks. Terminal-Bench also ran at 2x the standard agent time budget, so leaderboard comparisons would be misleading. A few tasks hit only upstream HTTP errors or rate limits, 504/429 responses, Daytona setup hiccups, or tool-router stalls; I excluded them for both models. With a 5/10 result on this Terminal-Bench subset, I would still run both models on your actual workflow before picking a default.

Want to build this kind of agent setup without hand-rolling auth for every app? Composio connects agents to 1,000+ apps with auth and tools.

[Get started with Composio](https://composio.dev/)
