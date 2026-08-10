# Pi Agent vs Claude Code in 2026: Everything You Need to Know

While researching for this post, I found out something funny. Pi, the most interesting coding agent of 2026, was built by a guy who got fed up with Claude Code. 🥶

[Mario Zechner](https://x.com/badlogicgames), the creator of libGDX, was a hardcore Claude Code user. He built [cchistory](https://mariozechner.at/posts/2025-08-03-cchistory/) just to track its system prompt changes, and even [patched the binary](https://mariozechner.at/posts/2025-08-06-cc-antidebug/) to add features Anthropic had not shipped.

Then he quit and wrote his own agent. Four tools, a tiny system prompt, no MCP, no permissions. He named it Pi so nobody could Google it. What a Chad! 🗿

That small project now has 85k+ GitHub stars, powers OpenClaw, and outscored Claude Code on our own tool use benchmark at a fifth of the cost.

So I put the two side by side. The default everyone uses, against the minimal harness built as a rejection of it.

Let's go!

## TL;DR

| Category | Claude Code | Pi | Winner |
| --- | --- | --- | --- |
| Tool price | Needs a paid plan or API key | Free, MIT licensed | Pi |
| Real tool use (our eval) | 16/30 tasks passed, \$0.195 per success | 20/30 tasks passed, \$0.028 per success | Pi |
| Predictable billing | \$20/month flat on Pro | Pay per token, bills can spike | Claude Code |
| Model support | Claude models only but it’s built for it | 20+ providers, 300+ models, mid session switching | Pi |
| Out of the box features | Sub agents, teams, plan mode, MCP, skills, checkpoints | Four tools and a text box | Claude Code |
| Context discipline | Auto compaction, silent context management | You see and control every token | Pi |
| Extensibility depth | Shell hooks, MCP, skills, plugins | TypeScript extensions running inside the agent process | Pi |
| Multi agent workflows | Native sub agents and Agent Teams | Spawn pi via bash, or install a community extension | Claude Code |
| Permissions and guardrails | Deny by default, five modes, sandboxing | Full system access from the first prompt | Claude Code |
| Surfaces | Terminal, VS Code, JetBrains, desktop, web, mobile, Slack | Terminal | Claude Code |
| Session model | Linear conversation log | Branchable session trees with fork and rewind | Pi |

#### In Summary:

**Scorecard: Pi 6, Claude Code 5.**

Pi is the cheaper, more flexible, and more transparent harness, especially if you pay per token, switch models, or want full control. Claude Code remains the better daily driver for most people because it ships with stronger guardrails, predictable subscription pricing, and a polished feature set out of the box. The practical answer is to use Claude Code for everyday work and Pi for custom workflows, local models, and cost-sensitive tasks.

And here is the twist. The scorecard says Pi, but almost everyone who reviews both, including the biggest Pi fans, still runs Claude Code as their daily driver. I will explain why is that in a minute.

---

## What is Pi?

> ℹ️ A minimal, open source terminal coding agent that ships four tools and expects you to build the rest yourself.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-claude-code/image_1.png)

[Pi](https://pi.dev/) is the coding agent Mario Zechner released in late 2025, now maintained under [Earendil](https://github.com/earendil-works/pi). Zechner joined Earendil in April 2026 in a blog post he titled "I've sold out," and the core stayed MIT licensed.

The whole product fits in literally one sentence. The model gets `read`, `write`, `edit`, and `bash`, a system prompt of a few hundred tokens, and nothing else.

His reasoning: frontier models have been RL trained so hard on coding agent behavior that they already know what a coding agent is. You do not need 10,000 tokens of instructions to remind them. Need ripgrep? The model runs `rg` through bash. Need GitHub? It runs `gh`. Need a browser? Ask Pi to write itself a browser tool.

> 💁 If you're on the same philosophy, this might be worth reading: [Top 10 CLI Tools to Level-Up Claude Code](https://composio.dev/content/top-10-cli-tools-for-claude-code). No MCPs, nothing. Plain raw Bash tools gets most of the work done.

That last part is the actual product. Pi extensions are TypeScript files that run inside the same process as the agent loop, and Pi can read its own source and docs, so the standard workflow when you need a missing feature is to ask Pi to build it.

Everything Zechner refused to ship, the community shipped as extensions within weeks. Todo lists, plan mode, sub agents, permission gates, even an MCP adapter.

---

## What is Claude Code

> ℹ️ Anthropic's batteries included coding agent, and the default the entire agent category gets measured against

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-claude-code/image_2.png)

You know this one. [Claude Code](https://code.claude.com/docs/en/overview) started as Boris Cherny's internal AppleScript toy at Anthropic in September 2024, hit research preview in February 2025, and became the fastest scaling developer product anyone has ever measured.

It ships everything: 10+ built in tools, sub agents, Agent Teams, plan mode, MCP as both client and server, Agent Skills, plugins, hooks, checkpoints with rewind, a permission system with five modes, and it runs in the terminal, VS Code, JetBrains, a desktop app, the browser, and your phone.

More than 80% of Anthropic's own engineers use it daily, and around "90% of Claude Code's code is now written by Claude Code". 🥴

Related: [Claude Code vs Codex](https://composio.dev/content/claude-code-vs-openai-codex)

---

## 1. Philosophy: Pi vs Claude Code

This comparison only makes sense once you understand that Pi exists because of Claude Code.

Zechner was an early Claude Code user. His complaint was never that it was bad. His complaint was that it kept changing underneath him. The system prompt and tool definitions shifted on every release, which broke his workflows and changed model behavior.

So the two tools represent opposite answers to the same question: how much harness does a frontier model actually need?

Anthropic's answer is interesting because it is drifting toward Pi's. Boris Cherny's team deletes harness code every time a new model ships, and in July 2026 Anthropic removed over 80% of Claude Code's system prompt for the Claude 5 generation models with no measurable loss on their coding evals. The bet behind Claude Code is that Anthropic trains the model and the harness together, so scaffolding today can become model behavior tomorrow.

Pi's answer is that the scaffolding was never load bearing in the first place.

> 💁 Anthropic quietly deleting most of its own system prompt is about the strongest validation of Pi's thesis you could ask for. Zechner just got there a year early.

---

## 2. Agent Architecture: Claude Code vs Pi

Both run the same loop underneath. Read the task, call the model, execute tool calls, feed results back, repeat until the model stops asking for tools.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-claude-code/image_3.png)

The difference is everything wrapped around that loop.

- **Claude Code** gives the model Read, Write, Edit, Bash, Glob, Grep, WebSearch, WebFetch, NotebookEdit, TodoWrite, there's a lot.... and the Task tool that spawns typed sub agents with their own isolated context windows. Plan mode forces a read only explore and plan phase before any file gets touched.
- **Pi** gives the model four tools, plus optional read only `grep`, `find`, and `ls` behind a `-tools` flag. There is no plan mode, but `pi --tools read,grep,find,ls` gets you a read only session in one line. There are no sub agents, and they say that if you want one, tell Pi to run itself through bash, ideally in tmux so you can actually watch it work.

The system prompt gap is the main highlight:

|  | System prompt | Built in tools |
| --- | --- | --- |
| Claude Code (pre Claude 5 era) | \~10,000 to 14,000 tokens by community measurement | 10+ |
| Claude Code (current) | Cut by over 80% for Claude 5 gen models | 10+ |
| Pi | Under 1,000 tokens including tool definitions | 4 |

One less obvious architectural difference: Pi stores sessions as trees, not logs. Every message carries a parent id, so under the hood this is basically a linked list that learned to branch.

> A real use case for linked lists, in the wild. All that leetcode grind had something useful. 🤡

So `/tree` lets you jump back to any earlier point in the conversation and branch from there, and `/fork` spins a new session off any past message. Claude Code got checkpoints and rewind in 2.0, which covers the common case, but Pi's tree model is the better design.

---

## 3. The benchmark: same model, different harness

We ran 30 hard agentic tool use tasks against real apps, eight harnesses, every single one wired to the same model, DeepSeek V4 Flash, through our hosted MCP router, with a 900 second ceiling per task.

Same model, same tasks, same tools. The only thing that changes is the harness, so whatever gap shows up is the wrapper, not the model.

Here is how our two contenders did:

| Harness (DeepSeek V4 Flash, hard 30 set) | Tasks passed | Median time | Avg tokens per task | Total cost | Cost per success |
| --- | --- | --- | --- | --- | --- |
| Claude Code | 16/30 (53.3%) | 122.7s | 741,659 | \$3.12 | \$0.195 |
| Pi | 20/30 (66.7%) | 132.2s | 558,885 | \$0.56 | \$0.028 |

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-claude-code/image_4.png)

Pi passed more tasks than any of the eight harnesses we tested, and did it at the lowest cost of the whole bunch. Cost per successful task is the brutal column: **3 cents against 19**.

Claude Code posted the fastest median time. It just burned 741,659 tokens per task getting there, against Pi's 558,885. The overhead is the story, again.

And yes, that is Claude Code running DeepSeek. That is the whole point of the setup. Only the harness changes.

For the curious, Codex ran the same and landed at 16/30 for \$1.29 total. Right between the two.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-claude-code/image_5.png)

> ⚠️ **NOTE: **Pi ran on high reasoning instead of max, and 24 of its 30 trials went through the official DeepSeek API instead of OpenRouter. Make sure to read the exact numbers with that in mind.

---

## 4. Pricing: Claude Code vs Pi

### Claude Code

- Pro, \$20/month or \$17/month annually, includes Claude Code
- Max 5x at \$100/month, Max 20x at \$200/month

> ⚠️ Guys, make sure to double check the pricing. It changes frequently.

### Pi

- The tool costs nothing. MIT license, fork it, ship it
- You bring your own API keys for any provider, or run local models for free through Ollama, vLLM, or llama.cpp
- OAuth login for subscriptions where providers allow it

On paper Pi wins on price and Claude Code wins on predictability. A \$20 flat rate is genuinely hard to beat for a solo dev using Claude models all day, and per token bills on heavy Pi usage can hit hundreds of dollars a month. I usually prefer the subscription system.

Anthropic locked its subscription OAuth tokens to Claude Code and [Claude.ai](http://claude.ai/) only. Third party harnesses got cut off, starting with a silent server side block in January, formalized in the docs by February, fully enforced by April. OpenClaw, OpenCode, Cline, and yes, Pi, all lost the ability to run on your Claude Pro or Max subscription.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-claude-code/image_6.png)

So the practical situation today: running Claude models through Pi means API rates. Running them through Claude Code means your cheap subscription.

DHH called the move "very customer hostile," Lol.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-claude-code/image_7.png)

OpenAI's Codex team publicly went the opposite direction and encouraged third party harness usage.

> 💁 If your workflow is Claude models all day every day, the subscription math alone mostly settles this comparison. Pi's cost advantage is real on API keys and irrelevant on a Max plan you already pay for.

---

## 5. Models: one provider vs all of them

Claude Code runs Claude. Opus, Sonnet, and Haiku tiers, switchable with `/model`, with thinking effort levels, tuned end to end because Anthropic controls the whole stack.

That vertical integration is why the model feels so at home in the harness. It was trained in it.

Pi runs everything. The `pi-ai` layer normalizes four API shapes across 20+ providers, from Anthropic and OpenAI to Groq, Cerebras, Mistral, OpenRouter, and local models. Over 300 models total. You can switch models mid session with `Ctrl+P`, and Pi converts thinking traces between providers.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-claude-code/image_8.png)

The local model story deserves its own mention. Claude Code's giant prompt overhead makes it a poor fit for small local models, which is exactly why Simon Willison flagged Pi as the lightweight option for that use case.

---

## 6. Extensibility

Both tools are deeply customizable.

Claude Code's model is external and config driven. Hooks are shell scripts that fire on 14 lifecycle events and talk JSON over stdin. MCP servers are separate processes speaking a protocol. Skills are markdown instruction packs with lazy loading. Plugins bundle all of it into installable packages with official and community marketplaces behind a single `/plugin` command.

```json
{
  "permissions": {
    "allow": ["Bash(npm run test *)", "Bash(git status *)"],
    "deny": ["Read(./.env)", "Bash(curl *)"]
  }
}
```

Pi's model is internal. Extensions are TypeScript modules loaded into the same runtime as the agent loop, hooking 25 event types. They can block or rewrite tool calls before they run, override the built in tools entirely, inject context, replace the compaction logic, persist custom state into the session file, and draw their own TUI widgets. Claude Code's hooks can approve or reject what the agent does. Pi's extensions can change what the agent is.

The tradeoff is trust. Claude Code's marketplace plugins go through some checks and run outside the core process. Pi packages run with full system access, and its own docs tell you to review them before installing.

> The sharpest criticism of Pi's ecosystem, from [thevinter's "Bad Vibes From Pi"](https://www.thevinter.com/blog/bad-vibes-from-pi), lands exactly here: a lot of those community extensions are themselves vibecoded, so that's something to watch-out for.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-claude-code/image_9.png)

Related: [Best Claude Code Plugins](https://composio.dev/content/top-claude-code-plugins)

---

## 7. Permissions and safety

This is the widest gap between the two tools, and neither side considers it a flaw.

Claude Code is deny by default. Every risky action asks first, and you can loosen from there through five permission modes, allow and deny rules, and OS level sandboxing for bash.

When Claude Code has security incidents, and it has had real ones with CVEs attached, they get found, patched, and written up because millions of people are using it.

Pi has no permission system. First prompt, full user privileges, go.

Zechner's argument is that agent security is mostly "theater" 🫩, because once a tool can write code and run code the game is already over. Pi's answer to safety is isolation instead of prompts: run it in Docker, in a micro VM, or behind a sandbox, and there is an opt in extension that checks bash commands against YAML rules if you want a middle ground.

One of them requires you to have read the docs, and the other protects the "intern" who did not.

> There isn't much to discuss here. You probably already know how things work by now...

---

## What the Internet thinks

Maybe worth seeing what the internet thinks of Pi and Claude Code by now?

It's more of what we are discussing so far. The famous endorsements is around Pi, the daily usage clusters around Claude Code, and both facts are true.

Let me show you the Pi love first, because it is a lot.

Peter Steinberger built OpenClaw on top of Pi and quote tweeted Zechner with "Mario is the real one we don't deserve." Armin Ronacher wrote that Pi is [the coding agent he uses "almost exclusively"](https://lucumr.pocoo.org/2026/1/31/pi/).

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-claude-code/image_10.png)

Theo from [t3.gg](http://t3.gg/) did a whole episode about getting Pi pilled. His take was simple: "The more minimal, the better." Wes Bos and Scott Tolinski spent a Syntax episode arguing Claude Code is overkill for most work.

🎥 [Watch the demo — Watch on YouTube](https://www.youtube.com/watch?v=3DNkDIVKtK8)

My favorite line comes from [IndyDevDan](https://agenticengineer.com/the-only-claude-code-competitor), who calls Pi the only real Claude Code competitor: "Claude Code is the starter pack. Pi is the endgame."

About 80% of his work still goes through Claude Code. Only 20% goes through Pi. The endgame guy is still playing the starter pack. Internet is wild!

Claude Code does not need loud fans anyway. It has \$2.5B in revenue (might increase, researched through Claude itself), 141k GitHub stars, most of the enterprise market, and Microsoft reportedly using it inside their own teams. The numbers are just enough, lol. 🤧

---

## Which one should you pick?

**Pick Claude Code if:**

- You want something that works at full power
- You live on Claude models, because the \$20 to \$200 subscriptions are the cheapest way to run them and Pi is locked out of that pricing
- You are deploying to a team and need permissions, SSO, spend caps, and audit trails
- You want sub agents, plan mode, and multi surface access without building anything

**Pick Pi if:**

- You pay per token at scale, since less context per task is real money, a fifth of the cost in our eval
- You want to mix providers, run open models, or go fully local
- You want to read, own, and modify every part of your harness, MIT license and all
- You are the kind of engineer who would rather build the exact workflow you want than configure someone else's

**Do what most reviewers actually do:** run both. Claude Code as the primary builder on your subscription, Pi as the cheap second opinion, the local model runner, and the place where your custom workflows live. The two tools are converging anyway.

Claude Code keeps deleting scaffolding, and the Pi community keeps rebuilding Claude Code's features as extensions. 🤡

---

## Conclusion

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-claude-code/image_11.png)

The scorecard reads Pi 6, Claude Code 5. And honestly, Pi deserves those wins. It is cheaper per task, open source, works with any model. What more do you want?

But here is the thing. When I sit down with a bug and just want it fixed, I open Claude Code. It works right away, the guardrails just works, the \$20 plan keeps the bill consistent, and the model just feels at home in it.

No thinking about the tool itself.

That is the whole story really. Even the biggest Pi fans ship most of their work through Claude Code. It's a secret. 🤫

Zechner was right that these agents need way less scaffolding than we thought. Anthropic deleting 80% of its own system prompt basically admitted it. But being right and being the tool everyone opens every day are two different things, and Claude Code still owns the second one for me.

So use Claude Code for the day job. Keep Pi around for when you want to tinker, go local, or save some money. ✌️
