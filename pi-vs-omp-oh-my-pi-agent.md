# Pi vs OMP (Oh My Pi Agent): Everything You Need to Know

You already know how Pi was born. Mario Zechner got fed up with Claude Code's bloat, deleted everything, and shipped an agent with four tools and a system prompt under 1,000 tokens. Less is more, that's the whole idea.

Then on December 31, 2025, a security researcher named [Can Bölük](https://x.com/_can1357), a guy who spent years breaking some of the hardest software protections out there, and decided to fork Pi.

Not just that. \\he added an LSP client, an actual debugger, a browser, a Python kernel, subagents, and roughly 80,000 lines of Rust, basically a whole freaking IDE.

Then he named it Oh My Pi, as in [Oh My Zsh](https://ohmyz.sh/), because it is the fully loaded version of an agent that keeps things empty on purpose.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_1.png)

> This sort of feels like we're moving in circles. I mean, the whole principle of Pi is to be minimal, so if you put everything back in, what makes it much different from something like OpenCode? 🤔

Fair question. OMP actually has a pretty good answer.

The extra stuff here is not features for the sake of features, it is a very specific bet on how agents should edit code, and there is a benchmark behind it.

> ℹ️ This is the third post in this series. We already covered [Pi Agent vs Claude Code](https://composio.dev/content/pi-agent-vs-claude-code) and [Pi vs OpenCode](https://composio.dev/content/pi-vs-opencode). Check those out if you are still deciding between the big names.

Let's go through the technicalities of each agent, and at the end, we will also put the parent and the fork through our benchmark on the same model (deepseek-v4-flash).

---

## TL;DR

| Category | Pi | OMP | Winner |
| --- | --- | --- | --- |
| Real tool use (our eval) | 20/30 tasks passed | 17/30 tasks passed | Pi |
| Cost per success | \$0.028 | \$0.103 | Pi |
| Speed | 132.2s median per task | 272.4s median, slowest of all eight harnesses | Pi |
| Token appetite | 558,885 avg tokens per task | 742,283 avg tokens per task | Pi |
| Out-of-the-box tooling | Four tools | 31+ tools, LSP, debugger, browser, Python kernel | OMP |
| Edit reliability on cheap models | Plain string edits | Hashline, took one model from 6.7% to 68.3% | OMP |
| Debugging | Tell it to printf, literally | Real DAP debuggers, breakpoints on live processes | OMP |
| Non-code files | Bash your way through | One `read` tool for SQLite, PDFs, notebooks, archives | OMP |
| Model support | 20+ providers, 300+ models | 40+ providers, plus role-based routing per task type | OMP |
| Config migration | You start fresh | Auto-inherits your `.claude`, `.cursor`, `.codex` config | OMP |
| Simplicity | Small enough | A monorepo plus \~80k lines of Rust | Pi |
| Session model | Branchable trees, it invented them | Branchable trees, it inherited them | Tie |

### In summary

**Scorecard: OMP 6, Pi 5, one tie.**

And yet the benchmark says Pi. The fork with more of everything, more tools, more tokens, more Rust, lost the actual test to the four-tool parent, 17 to 20, at nearly four times the cost per success.

That said, OMP was still the second-best harness in our entire eight-harness run. It beat Claude Code, Codex, and OpenCode on the same model.

So this one is genuinely not a crazy win either way. OMP's tooling is real and useful. It just costs you tokens, time, and money on every single task, and on this benchmark, the extra cost did not pay off.

---

## What is OMP?

> ℹ️ A fork of Pi that puts a whole IDE on top of the minimal core.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_2.png)

[Oh My Pi](https://omp.sh/), command `omp`, is a much more loaded fork of Pi. It keeps the same core idea, but adds a lot more tooling around it.

Can Bölük forked `pi-mono` on the very last day of 2025, and the project has been moving insanely fast ever since. It has had multiple releases a week, sometimes several in one day, more than **500 releases**, 12,000+ commits, and over **26k GitHub stars** under his company Stencil Labs.

It is still MIT licensed, still free, and installable with one curl command.

The main features:

- **Hashline edits.** Uses short content hashes instead of exact text matches.
- **LSP and debugger.** Adds diagnostics, semantic renames, and real DAP debugging.
- **Native Rust core.** About 80,000 lines of Rust for grep, glob, bash, parsing, and PTY work.
- **31+ built in tools.** Reads SQLite, PDFs, DOCX, spreadsheets, notebooks, archives, and more.
- **Model routing.** Use different models for small tasks, hard tasks, planning, and fallbacks.
- **Config import.** Reads Claude, Cursor, Codex, and Gemini configs on first run.

> 💁 Pi starts with almost nothing. OMP comes with most of the extra tooling already built in.

---

## What is Pi?

> ℹ️ A minimal terminal coding agent that ships four tools and expects you to build the rest yourself.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_3.png)

Quick recap if you missed the last two posts.

[Pi](https://pi.dev/) is Mario Zechner's answer to harness bloat. The model gets `read`, `write`, `edit`, and `bash`, plus a system prompt that is only a few hundred tokens long. That is basically it. No MCP. No permissions. No plan mode. All of that is intentional.

His reasoning is pretty simple. Frontier models have been trained so heavily on coding agent behavior that they already know what a coding agent is supposed to do.

Pi can run `rg` through bash when it needs ripgrep. It can also write and load TypeScript extensions inside its own process. If you need something, build it or use what's already in the system sort of philosophy.

It now lives under [Earendil](https://github.com/earendil-works/pi) MIT licensed, and the core remains open source.

Related: [Pi Agent vs Claude Code](https://composio.dev/content/pi-agent-vs-claude-code) and [Pi vs OpenCode](https://composio.dev/content/pi-vs-opencode)

---

## 1. Philosophy: Pi vs OMP

This is the fun part, because for once in this post this is not open vs closed or indie vs corporate.

Pi's bet is simple. The model already knows how to be a coding agent. Every tool schema and every extra instruction takes up context on every request, so the harness should stay as close to zero as possible.

OMP makes almost the opposite bet.

Bölük wrote a whole blog post about it that went viral, **the harness problem**. His argument is that the stuff around the model matters almost as much as the model itself. Bad coding results can sometimes come from the harness and its edit tools, not just the model.

His proof was a benchmark where he kept the models the same and only changed the format.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_4.png)

Grok Code Fast 1 went from a **6.7% success rate to 68.3%**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_5.png)

Same model. Only the harness changed.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_6.png)

Across 16 models, hashline added roughly 15 percentage points on average compared with the standard edit format.

So both projects are answering the same basic question:

> How much harness does a model actually need?

They just have completely different answers.

Zechner says almost none. Bölük says probably way more than you think, as long as the harness is actually useful and not just bloated (and I kinda agree with Bölük).

> 💁 Both sides have real evidence. Anthropic cut 80% of Claude Code's system prompt without losing performance. Hashline made a weak model about 10 times better without changing the model.

So, what do you trust more? Simplicity or simplicity with a few more features?

---

## 2. Architecture: what the fork actually changed

Underneath, both agents run the same basic loop because **OMP is literally a fork of Pi**.

Read task, call model, run tools, feed results back, repeat.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_7.png)

What OMP changed is basically everything around that loop.

### The edit tool

Pi edits files the normal way. The model copies the old text and gives the new text. This works well with frontier models, but cheaper models may not be good with that.

OMP's hashline gives every line a tiny 2 or 3 character content hash when the agent reads a file. The model can then say something like "replace line `2:f1`" instead of copying the whole line back.

If the file changed, stale edits get rejected before they touch anything. Whitespace also stops being such a pain.

It is a smart fix for one of the most common agent failure modes. Weaker models get the biggest boost from it, which is exactly the kind of setup OMP seems built for.

### The native core

Pi is a single TypeScript process and small enough that you can actually read through it.

> ℹ️ Here's the code if you're interested: [Pi Core](https://github.com/earendil-works/pi/tree/main/packages/agent)

OMP has a TypeScript frontend on top of a Rust engine. Grep, glob, bash, syntax parsing, and the terminal renderer all run inside the process.

That makes a lot of common operations fast, but the harness is also much bigger and harder to read.

### The tools

Pi ships 4 tools (`read`, `write`, `edit` and `bash`). OMP ships 31 and probably will add more...

Some of the more interesting ones:

- `debug` attaches DAP debuggers to live processes
- `lsp` handles semantic renames across a codebase
- `eval` runs a persistent Python or Bun kernel
- `read` opens common code and document formats
- Stream control can stop, correct, and retry generation

> Same engine underneath. One is just super small on purpose. The other adds useful stuff. Both work based on your taste.

---

## 3. The benchmark: same model, parent vs fork

This is probably what you are here for.

We ran **30 hard agentic tool use tasks** against real apps through our [hosted MCP](https://composio.dev/mcp) router. Eight harnesses, all using the same model, **DeepSeek V4 Flash**, with a 900 second limit per task.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_8.png)

Same model, same tasks, same tools, same verifier checks.

So whatever gap shows up here mostly comes down to the harness.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_9.png)

Here is how the two did:

| Harness (DeepSeek V4 Flash, hard 30 set) | Tasks passed | Median time | Avg tokens per task | Total cost | Cost per success |
| --- | --- | --- | --- | --- | --- |
| Pi | 20/30 (66.7%) | 132.2s | 558,885 | \$0.56 | \$0.028 |
| OMP | 17/30 (56.7%) | 272.4s | 742,283 | \$1.75 | \$0.103 |

Pi (the parent) won.

Three more tasks passed, around a quarter of the cost per success, and less than half the median time.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_10.png)

> ⚠️ **NOTE**: Pi ran on high reasoning instead of max, and 24 of its 30 trials went through the official DeepSeek API instead of OpenRouter. The cost numbers especially should be read with those two catches in mind.

But OMP deserves real credit here.

17/30 made it the **second best harness out of all eight**. It beat Claude Code at 16, Codex at 16, DeepAgents at 16, Hermes at 15, and OpenCode at 14 on the exact same model.

So the extra tooling clearly has value. OMP still finished second overall. It just lost this comparison to Pi. 🤧

### The task by task split is where it gets good

Both agents passed the same 16 core tasks and failed the same 9 brutal ones.

The whole 20 vs 17 gap comes down to five tasks.

| Task | Pi | OMP |
| --- | --- | --- |
| Slack action items | ✅ | ❌ |
| Sheets approval update | ✅ | ❌ |
| Errand intake | ✅ | ❌ |
| Reimbursement audit | ✅ | ❌ |
| GCal free/busy batch | ❌ | ✅ |

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_11.png)

The failure is the best part of the whole report I'd say.

OMP usually did not fail because it misunderstood the task. It failed because it kept doing too much.

On one task, it burned through 1.22 million tokens and 861 seconds just to end with, "I need one more page that was truncated." On another, it found the right entries but reported the wrong row numbers. And on the reimbursement task, it used 1.8 million tokens, almost hit the time limit, and passed only 3 of 13 tests.

Pi's mistakes were much simpler. On the GCal task, it booked 90 minute events instead of the required 60 minute slots. OMP got that one right.

> ℹ️ We have done a complete breakdown in our tweet, check it out: [Agent Harness Test.](https://x.com/composio/status/2086814488162972027?s=20)

## 4. OMP talks too much

This also matches what a lot of OMP users have noticed.

OMP averaged **742,283 tokens per task** in our run, compared to 558,885 for Pi. Same model, same tasks, 33% more tokens, and fewer passes.

Fabio Akita came to a similar conclusion in [his review](https://akitaonrails.com/en/2026/05/25/first-impressions-using-oh-my-pi-and-opencode/). The extra tools are useful, especially for projects with lots of different file types, but OMP often does more work than it needs to.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_12.png)

That seems to be the tradeoff.

Hashline clearly helps cheaper models, and OMP still beat five other harnesses in our benchmark. But all those extra tools also mean more context, more tool calls, and a lot more chance of higher cost.

Pi had four tools and usually just finished the task.

> 💁 OMP gives the model more ways to solve a problem. Most of the times it also gives it many ways to waste time.

## 5. Pricing and models

Both are free and MIT licensed. You can fork them, change them, ship them. Nice and simple for once.

### Pi

- BYOK for 20+ providers and 300+ models
- Local models through Ollama, vLLM, and llama.cpp
- OAuth login where supported

### OMP

- BYOK for 40+ providers
- OAuth login for several coding plans
- Local models and custom OpenAI compatible endpoints
- Model routing for different jobs

OMP supports more of basically everything here, and model routing is a genuinely useful cost feature that Pi does not have built in.

One warning though, and this applies to both.

> ⚠️ After the Anthropic OAuth removal we covered in the [last post](https://composio.dev/content/pi-vs-opencode), using subscription logins through third party harnesses is kinda sus with some providers. Bölük himself reportedly got his Gemini account disabled while running benchmarks.

If you care about keeping the account safe, API keys are the best route.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_13.png)

---

## 6. Extensibility

Both use the same basic extension system, so there is not much to compare here.

OMP keeps Pi's TypeScript extensions, hooks, and full runtime access. It can even write and reload its own extensions during a session.

The main difference is the starting point.

Pi starts small and lets you add what you need. OMP starts with most things already built in, then lets you turn things off or modify them.

So if you want something simple and easy to understand, Pi makes more sense.

If you want more built in from day one, OMP gives you that.

---

## Which one should you pick?

**Pick Pi if:**

- You want better speed and cost in our benchmark
- You mostly work with normal source code
- You want something small and easy to understand

**Pick OMP if:**

- You work with PDFs, spreadsheets, SQLite, notebooks, and other file types
- You want things like a debugger, LSP, and more reliable edits on cheaper models
- You want more tools built in and do not mind tuning them

You can also just run both.

They use the same session format and extension system, and OMP can import your existing configs on first launch.

---

## Conclusion

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/image_14.png)

The scorecard says OMP 6, Pi 5.

The benchmark says Pi 20, OMP 17.

So yeah, pick whichever number matters more to you. 🤷‍♂️

Bölük is right that the harness matters. OMP beat Claude Code, Codex, and OpenCode with a budget model. Hashline and the extra tools are useful, and it still beat Claude Code, Codex, and OpenCode on the same model.

At some point, more harness starts costing tokens and time without improving the result. OMP's Slack run is a pretty good example of that.

OMP added almost everything and came second. Pi has 4 bare tools and still came first.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-omp-oh-my-pi-agent/gif_1.gif)

> Sometimes the original just wins. ✌️
