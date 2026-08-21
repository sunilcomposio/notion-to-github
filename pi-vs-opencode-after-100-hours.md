# Pi vs OpenCode: After 100 Hours, Which Open-Source Coding Agent Should You Use?

Open-source coding agents had a weird start to 2026.

In January, Anthropic suddenly blocked third-party tools from using Claude subscriptions. Overnight, with no warning.

OpenCode got hit the hardest. The team had to remove Claude login support after legal pressure. The commit message was literally just: “anthropic legal requests.”

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_1.png)

PI never really cared. It was never built around one provider anyway.

And that kinda brings us to the obvious question. If you’re using your own API keys anyway, which open-source agent do you actually pick?

OpenCode is sitting at **199K stars**, probably past 200K by the time you read this, with around 8 million monthly devs.

Then there’s Pi. Bare minimum by design. Four tools. A system prompt under 1,000 tokens. And somehow, top of our benchmark. Again.

> ℹ️ We recently covered [Pi Agent vs. Claude Code](https://composio.dev/content/pi-agent-vs-claude-code). Check that out if you're not sure whether Anthropic or Pi suits you better

The easiest way I’d frame it is this:

> 💁 OpenCode is VS Code (bit bloaty). Pi is NeoVim (bare minimum).

So which one do you go for? Let's get into the difference.

---

## TL;DR

| Category | Pi | OpenCode | Winner |
| --- | --- | --- | --- |
| Real tool use (our eval) | 21/30 tasks passed, \$0.078 per success | 19/30 tasks passed, \$0.119 per success | Pi |
| Total eval spend | \$1.64 for all 30 tasks | \$2.25 for all 30 tasks | Pi |
| Speed | 362.9s median per task, slowest of the bunch | 280.6s median per task | OpenCode |
| Context overhead | Under 1,000 tokens, prompt + tool defs | \~6,900 tokens per request | Pi |
| Out-of-the-box features | Four tools | Plan mode, MCP, LSP diagnostics, undo/redo, permissions, subagents, and a lot more | OpenCode |
| Model support | 20+ providers, 300+ models, mid-session switching | 75+ providers via AI SDK and `models.dev` | OpenCode |
| Cheap model access | BYOK or local, that's it | Zen gateway at cost, plus Go at \$10/month | OpenCode |
| Extensibility | TypeScript extensions running inside the agent process | Plugins, custom agents, skills, mostly config-driven | Pi |
| Sessions | Branchable session trees with fork and rewind | Linear log with undo/redo, but survives disconnects | Pi |
| Surfaces | Terminal | TUI, desktop app, web UI, IDEs via ACP, GitHub and GitLab CI | OpenCode |
| Permissions | Nothing. Full system access from prompt one, which isn't really my preferred style | allow/ask/deny per tool with glob patterns | OpenCode |
| Resource footprint | Lightweight, single process | 1GB+ RAM for a TUI is kinda crazy | Pi |

### In summary

**Scorecard: Pi 6, OpenCode 6. Dead tie.**

And honestly, that feels right. These two are built for pretty different kinds of people anyway.

> Remember the VS Code vs. Neovim thing I said earlier?

Pi is more like a platform you program. You get a tiny core and build your own harness around it in TypeScript.

OpenCode is more like a product you configure. You get something much closer to a full Claude Code replacement on day one, then tweak it through config.

Pi wins on efficiency, hackability, and, at least in our eval, raw results.

OpenCode wins on features, provider support, surfaces, and the simple fact that a normal person can pick it up and start using it without building half the setup themselves.

So yeah, the real tiebreaker is basically what kind of person you are. Keep reading.

---

## What is Pi?

> ℹ️ A minimal terminal coding agent that ships four tools and expects you to build the rest yourself.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_2.png)

Quick recap if you missed our [Pi vs Claude Code](https://composio.dev/blog/pi-agent-vs-claude-code) post.

[Pi](https://pi.dev/) is Mario Zechner's answer to harness bloat. He got tired of Claude Code changing under him, so he built an agent where the model gets `read`, `write`, `edit`, and `bash`, a system prompt of a few hundred tokens, and nothing else. No MCP. No permissions. No plan mode. No subagents. All on purpose.

It now lives under [Earendil](https://github.com/earendil-works/pi) after his April blog post "I've sold out," and the core stays open-source.

The real product is the extension system. TypeScript files run inside the same process as the agent loop, and Pi can read its own source code. So when you need a missing feature, you literally ask Pi to build it.

That design is also why Peter Steinberger picked Pi as the engine for OpenClaw.

> 💁 **Fun Fact:** Pi has "only" 94k stars, but the npm package pulls around 1.3 million downloads a week. Growing crazy fast among developers.

Not everyone is fond of VS Code. 🤷‍♂️

---

## What is OpenCode?

> ℹ️ The most starred open source coding agent in the world, and the closest thing to an open, model agnostic Claude Code.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_3.png)

[OpenCode](https://opencode.ai) is basically an open-source, model-agnostic coding agent that tries to give you the full Claude Code experience without locking you to Anthropic

It is built by the team behind SST, which rebranded to Anomaly in 2026 and moved the repo to `anomalyco/opencode`.

The numbers are crazy for a free tool. Around **199k stars**. Roughly 8 million monthly active developers. Cloudflare publishes official OpenCode setup docs for its own engineers.

Can you believe this free thing is doing over \$25M ARR? It's just the tool that's free, there's subscription plans they've introduced from where they make tons of money.

The origin story is crazier than Pi's. The original agent, called TermAI, was created by Kujtim Hoxha back in 2024. [Dax Raad](https://x.com/thdxr) and Adam Elmore did the OpenCode rebrand and made it popular. Then Charm hired Hoxha and pulled the repo into its own org. That causes rewritten git history, banned contributors, the whole drama. After the backlash, Charm renamed its fork **Crush**, and the Dax and Adam project kept the OpenCode name as a from scratch rewrite.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_4.png)

Not anything interesting if you don't care about "history". Lol.

So when anyone says OpenCode in 2026, they mean the Anomaly one.

> 💁 **Fun fact:** For the 1.0 release, the team threw out their entire Go based TUI and wrote a new framework called OpenTUI in Zig and SolidJS, because the old one "had performance and capability issues." Can't believe Golang had performance issues at that scale.

A coding agent team writing their own Zig rendering engine for the terminal is the most 2026 thing I can think of. Crazy stuff.

---

## 1. Philosophy: Pi vs OpenCode

Our earlier comparisons were open vs closed. This one is open vs open.

Both teams agree on the big stuff. The harness should be MIT licensed. You should bring your own model. Nobody should be vendor-locked in.

They completely disagree on how much harness should exist.

OpenCode's philosophy is that the harness is a real product. Ship plan mode, permissions, LSP integration, MCP, a desktop app, a web UI, IDE support, GitHub actions. Give people everything Claude Code has, minus the lock in.

Pi's bet is that most of that is overhead (not a feature, what OpenCode believes). Frontier models have been RL trained so hard on coding that they already know what a coding agent is. Every tool schema and every injected instruction is context you pay for on every request. So the harness should be close to zero, and you add back only what you personally need.

The [disler comparison](https://github.com/disler/pi-vs-claude-code/blob/main/PI_VS_OPEN_CODE.md) has the cleanest one liner on this.

> "Pi gives you control at the runtime level. OpenCode gives you control at the configuration level."

And yes, there is real beef here. Zechner has publicly called out OpenCode's compaction for pruning tool results and breaking prompt caching.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_5.png)

He is not a fan of their Vercel AI SDK dependency either. More on that below.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_6.png)

> 💁 If the "just use bash" philosophy works for you, this might go well: [Top 10 CLI Tools to Level Up Codiing Agents](https://composio.dev/blog/top-10-cli-tools-for-claude-code). No MCPs, plain raw CLI tools get most of the work done.

---

## 2. Agent Architecture: Pi vs OpenCode

Underneath, both use the same basic loop: read the task, call the model, run tools, feed results back, repeat.

The architecture around that loop is where they differ.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_7.png)

### OpenCode: client + server

OpenCode splits into a persistent local server and separate clients.

The server handles:

- agent logic
- LLM calls
- tool execution
- SQLite session storage
- MCP servers

The TUI, desktop app, web UI, and IDE integrations all talk to that same server.

This makes sessions more safe to terminal crashes or SSH drops, and the same server can also run headlessly in CI. The downside is a heavier footprint, with people reporting **1GB+ RAM usage** for the TUI setup.

You read it right. Read more about it here: [OpenCode is pretty resource inefficient](https://news.ycombinator.com/item?id=47465584)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_8.png)

### Pi: single process

Pi keeps everything in one process:

- provider layer
- agent loop
- CLI

It is all TypeScript and small enough to understand without going through a huge codebase.

Pi supports four run modes:

- interactive TUI
- print mode for scripts
- RPC over stdin/stdout
- SDK mode for embedding

That SDK is also how OpenClaw embeds Pi.

### OpenCode's way of LSP

I'd call it a star feature of OpenCode, the way it handles LSP (Language Server Protocol).

OpenCode can run real language servers such as `gopls` and `typescript-language-server` and feed diagnostics back into the agent after edits.

So the flow looks like this:

> model edits code → LSP reports an error → model sees it → model fixes it

That gives OpenCode fast compiler and type feedback without needing to run the full test every time.

Pi does not have this out of the box.

### Pi's Session trees

Pi way of handling sessions is kinda interesting.

Pi stores sessions as `JSONL`, with every entry pointing to a parent. That means the session history is a tree instead of one linear log.

```plain text
A → B → C
     ↘ D → E
     ↘ F → G
```

You can:

- use `/tree` to jump back to an earlier point
- use `/fork` to branch from any past message
- keep multiple debugging paths from the same point

OpenCode uses a linear history with `/undo` and `/redo`, which is simpler but less flexible.

### Context compaction

OpenCode uses a hidden system agent to summarize the conversation when context gets tight, then hides older messages from the active context.

The criticism is that this can remove useful tool outputs and hurt prompt caching.

Pi also supports compaction, but the implementation is replaceable through extensions, which fits its more hackable architecture.

---

## 3. The benchmark. Same model, different harness

Now for the part I was most curious about.

We ran **30 hard agentic tool-use tasks** against real apps. Every harness used the same model, **DeepSeek V4 Pro (0813)** at max reasoning, through our hosted MCP router.

Same model. Same tasks. Same tools. Same verifier checks.

So whatever gap shows up here is mostly the harness.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_9.png)

Here is the full result.

| Harness (DeepSeek V4 Pro, hard 30 set) | Tasks passed | Cost per success | Cost per shared success | Avg tokens per task | Avg turns |
| --- | --- | --- | --- | --- | --- |
| **Pi Agent** | **21/30 (70%)** | **\$0.078** | \$0.031 | 924,990 | 16.3 |
| Codex | 20/30 (66.7%) | n/a\* | \$0.031 | 383,722 | n/a |
| DeepSeek Harness | 20/30 (66.7%) | \$0.076 | \$0.028 | 88,562 | 0.9 |
| **OpenCode** | **19/30 (63.3%)** | **\$0.119** | \$0.032 | 710,140 | 13.1 |
| Claude Code | 19/30 (63.3%) | n/a\* | \$0.074 | 649,900 | 12.1 |
| Hermes Agent | 18/30 (60%) | n/a\* | \$0.037 | 113,894 | 6.5 |

> ℹ️ \*Not comparable because cost measure was incomplete for some cells. Shared success only counts tasks every harness passed.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_10.png)

### Pi actually topped the board

Pi finished first with **21/30 tasks passed**, while OpenCode landed at **19/30**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_11.png)

It was also cheaper across the full run.

- **Pi** spent \$1.64
- **OpenCode** spent \$2.25
- **Pi cost per success** was \$0.078
- **OpenCode cost per success** was \$0.119

That last number looks rough for OpenCode, but there is a catch.

On tasks both harnesses actually passed, they were basically tied. Pi came in at **\$0.031 per shared success**, while OpenCode was **\$0.032**.

So OpenCode was not really more expensive when it worked. It just failed more tasks, and **failed runs still burn tokens**.

### Pi was also painfully slow

Pi had the best pass rate, but it was also the **slowest harness in the entire run**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_12.png)

- **Pi median time** was 362.9 seconds
- **OpenCode median time** was 280.6 seconds
- **Claude Code median time** was 181.8 seconds

Pi also averaged **16.3 turns per task**, compared with OpenCode's **13.1**, and pushed more raw tokens than anyone else.

The difference is that its fixed overhead stays tiny.

The system prompt plus tool definitions stay under 1,000 tokens, and the prefix barely changes between turns. That makes a lot of that repeated context land as cache hits.

### The task-by-task split is more interesting

| Task | Pi | OpenCode |
| --- | --- | --- |
| CRM migration archive | ✅ 10/10 | ❌ 3/10 |
| Slack action items | ✅ 5/5 | ❌ 2/5 |
| Vendor directory | ✅ 11/13 checks | ❌ 7/12 |
| Handover audit | ✅ 11/11 | ❌ 10/11 |
| PagerDuty timeline audit | ✅ 10/10 | ❌ 9/10 |
| Renewal screen | ❌ 5/11 | ✅ 11/11 |
| Sponsorship screen | ❌ 4/11 | ✅ 11/12 |
| Sheets to Airtable tracker | ❌ 8/10 | ✅ 10/10 |

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_13.png)

Pi did better on the **audit and census-style tasks**, where patiently reading everything and checking every detail matters.

OpenCode did better on the **longer screening and multi-step workflows**.

Which is kinda perfect.

Pi wins by being small. OpenCode wins when the workflow benefits from more stuff around the model.

After all this non-sense, the eval ended up being spot on for both of them. 🤧

We did a complete breakdown in this tweet. You might want to [check it out](https://x.com/composio/status/2090069397050097864).

🐦 [View post on X](https://x.com/composio/status/2090069397050097864)

<unknown url="https://app.notion.com/p/3c2f261a6dfe80faa9e9fa1d1663a1ee#3c2f261a6dfe806bbeb7e66c8253d5e6" alt="tweet"/>

> ⚠️ **NOTE** Pi's request went through OpenRouter, while most other harnesses hit the DeepSeek API directly. Costs are normalized to native rates in the report, but keep that in mind when comparing exact numbers.

---

## 4. Token overhead

This is basically the argument both tools were built around, so let's put some numbers next to it.

The most cited comparison here is the [Systima study](https://systima.ai/blog/claude-code-vs-opencode-token-overhead) from July, which also ended up doing [700+ points on Hacker News](https://news.ycombinator.com/item?id=48883275).

They put a logging proxy between the harness and the model, then measured how much context gets sent **before the model even sees your prompt**.

| Harness | Fixed overhead per request |
| --- | --- |
| Claude Code (measured by Systima) | \~32,800 tokens |
| OpenCode (measured by Systima) | \~6,900 tokens |
| Pi | Under 1,000 tokens |

OpenCode already looks pretty good here. Around **4.7x less overhead than Claude Code**.

Then Pi shows up with **under 1,000 tokens**, including the tool definitions.

That makes it roughly **7x leaner than OpenCode**.

> 💁 Everyone is somebody's bloatware. 🗿

Matei Zaharia [said on X](https://x.com/matei_zaharia/status/2074943619610649059) the reason was pretty simple. Pi had the same success rate with **2x less cost.**

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_14.png)

Our eval points in the same direction too. Pi had the best pass rate and the lowest total spend.

But there is an important catch here.

OpenCode has had some pretty ugly token cases too. There is a [GitHub issue](https://github.com/anomalyco/opencode/issues/8234) where asking something as basic as "what's the tech stack in this project" burned **13,088 tokens** on a fresh project.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_15.png)

And Pi is not crazy cheap in every sense either. In our eval, it actually pushed the **highest raw token count of all six harnesses**, mostly because it took more turns to finish tasks.

So yeah, tiny fixed overhead helps a lot. But **lean overhead does not always mean lean sessions**.

Worth keeping that distinction in mind.

---

## 5. Pricing and the ban

Both tools are free and MIT licensed. Fork them, modify them, ship them.

The difference starts after install.

### Pi

- BYOK for pretty much any provider
- local models through Ollama, vLLM, or llama.cpp
- OAuth login where providers still allow it
- no paid CLI tier at all

> 💁 Pi is basically just the harness. You bring the model and pay whoever is serving it.

### OpenCode

OpenCode does the same BYOK thing, but it also has its own model options.

- **BYOK** where the provider bills you directly
- **Zen** with curated models sold on a pay-as-you-go basis
- **Go** with open coding models bundled into a cheap monthly plan

Go starts at **\$5 for the first month, then \$10/month**, with models like GLM, Kimi K3, Qwen, MiniMax, and DeepSeek.

> ⚠️ Double check the pricing. OpenCode changes this stuff pretty often.

And honestly, that Go plan is hard to ignore.

For \$10/month, it is probably one of the cheapest legit ways to run capable open models through a coding agent right now.

Pi does not really have an equivalent. Its version of "cheap" is running local models, which is useful, but it is not the same thing.

### Then Anthropic happened

In January 2026, Anthropic started rejecting Claude Pro and Max OAuth tokens when they were used outside Claude Code.

The error was pretty clear. The credential was **"only authorized for use with Claude Code."**

OpenCode had been using those subscription tokens by mimicking the Claude Code client, so this hit them directly.

Then came the legal requests, the Claude login code got removed, and we got the now-famous commit message from the intro.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_16.png)

George Hotz called the move **"a huge mistake."**

OpenAI reportedly went in the other direction and opened Codex subscription access to third-party harnesses, including OpenCode and Pi.

Lol.

The important part for this comparison is pretty simple now.

If you want Claude in either Pi or OpenCode, you are basically paying API rates. OpenCode also gives you Zen as another route.

---

## 6. Extensibility

Both are highly customizable, but the extension model is very different.

### OpenCode

OpenCode is mostly config driven through `opencode.json`, plugins, agents, skills, and MCP.

You can add:

- custom agents with their own model, prompt, permissions, and step limits
- JS or TS plugins with lifecycle hooks like `tool.execute.before`
- custom tools
- local and remote MCP servers
- skills from `.opencode/skills` and `.claude/skills`

```json
{
  "permission": {
    "bash": {
      "git *": "allow",
      "rm *": "deny",
      "*": "ask"
    }
  }
}
```

### Pi

Pi extensions are TypeScript modules loaded directly into the single agent process.

They can:

- rewrite or block tool calls
- replace built-in tools
- filter history
- replace compaction
- persist custom session state
- add TUI components
- probably many more...

That gives Pi much **deeper runtime control** than OpenCode.

> ℹ️ OpenCode plugins extend the harness. Pi extensions can modify the harness itself.

The tradeoff is security. Pi extensions run in process with full system access, so they need more trust than OpenCode's plugin model.

---

## 7. Permissions and safety

This is probably the biggest gap between the two.

OpenCode has a proper permission system. Every tool can be set to `allow`, `ask`, or `deny`, with glob patterns and per-agent overrides. The defaults are decent too. `.env` reads are denied by default, and the Plan agent asks before edits or bash commands.

What it does **not** have is real sandboxing. There is no container or OS-level isolation built in.

Pi goes the other way and basically ships no permission layer at all. From the first prompt, the agent runs with your full user privileges. 🥶

That is intentional. Zechner's say is that once an agent can write files and execute code, permission prompts only get you so far. Pi instead expects you to isolate the whole process using Docker, a microVM like Gondolin, or a policy sandbox like OpenShell.

> 💁 There is also an optional `damage-control.ts` extension that checks bash commands against YAML rules. Helps a bit.

So the actual difference is pretty simple.

> ℹ️ OpenCode gives you tool-level guardrails. Pi expects you to handle isolation outside the agent.

---

## Which one should you pick?

**Pick OpenCode if**

- You want something close to a full Claude Code replacement without building half the setup yourself
- You want plan mode, permissions, MCP, undo and redo, and LSP support out of the box
- You want the \$10 Go plan for cheap access to capable open models
- You work in typed languages and want LSP diagnostics fed straight back into the agent
- You are setting this up for a team and actually need proper `allow`, `ask`, and `deny` controls

**Pick Pi if**

- You care about token cost at scale. In our eval, the same model passed more tasks and cost less with Pi
- You want a harness small enough that you can actually read and understand the whole thing
- You like branching while debugging. Pi's session trees are really good for that
- You run local or smaller models where the difference between \~1,000 and \~6,900 tokens of fixed overhead actually matters
- You would rather change the harness in TypeScript than spend time configuring someone else's abstraction

The benchmark kinda gives away the answer anyway.

Pi did better on the careful audit-style tasks. OpenCode did better on longer, more structured workflows.

So there probably isn't one winner for everything.

Both are free. Try both and keep the one that fits how you work. ✌️

---

## Conclusion

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-vs-opencode-after-100-hours/image_17.png)

After all that, I still don’t think there’s one obvious winner here. The scorecard ends **6-6**.

OpenCode wins on adoption, features, and ease of use. Pi won our benchmark with **21/30 tasks vs 19/30**, lower total spend, and way less fixed overhead.

The bigger difference is still the same one we started with:

> **OpenCode gives you configuration control. Pi gives you runtime control.**

If you just want to install something and start working, pick **OpenCode**.

If you want to understand and modify the harness itself, pick **Pi**.

And if cost per successful task starts mattering at scale, keep an eye on Pi. 🤷‍♂️
