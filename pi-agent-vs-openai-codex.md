# Pi Agent vs OpenAI Codex: Which Coding Agent Should You Use in 2026?

In February, OpenAI ran a Super Bowl ad for Codex.

An actual Super Bowl ad. For a coding agent.

🎥 [Watch on YouTube — Watch on YouTube](https://youtu.be/aCN9iCXNJqQ?si=oFQs1twLZWF0XdHQ)

[Mario Zechner](https://x.com/badlogicgames) took pretty much the opposite marketing strategy with Pi. He gave it a name you can barely Google on purpose, hoping nobody would find it, use it, or start opening issues on GitHub.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_1.png)

You can guess how both plans went.

Codex now has around **20 million active users**. And the agent that was supposed to have zero users is sitting at around **100k GitHub stars** and **1.3 million npm downloads a week**.

Both marketing plans failed successfully. Lol.

But there is an even better contrast between the two.

OpenAI used early versions of GPT-5.3-Codex to help debug, train, and build Codex itself. So in a very literal sense, **Codex helped build Codex**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_2.png)

Pi is almost the exact opposite. Pi gives you the smallest possible base and expects **you** to build the agent you want around it.

One agent helps build itself. The other gives you the pieces and says, "go build yours."

That is what makes this comparison interesting.

Let's get into it.

---

## TL;DR

| Category | Pi | Codex | Winner |
| --- | --- | --- | --- |
| Real tool use, our eval | **21/30 tasks passed** | **20/30 tasks passed** | Pi |
| Tokens per task, our eval | 924,990 avg | **383,722 avg** | Codex |
| Context control | Under 1,000 tokens, visible and replaceable | Managed by Codex for you | Pi |
| Model freedom | 20+ providers, hundreds of models, mid-session switching | Mostly OpenAI models, tightly integrated with Codex | Pi |
| Out-of-box features | Four tools and a text box | Sandbox, cloud, subagents, hooks, skills, MCP | Codex |
| Sandboxing | Nothing by default | OS-level sandboxing on by default | Codex |
| Autonomy | Mostly interactive and developer-led | Cloud tasks, parallel attempts, long unattended runs | Codex |
| Extensibility | TypeScript extensions can change the runtime itself | Hooks, skills, MCP, SDKs | Pi |
| Sessions | Branchable session trees | Resume, fork, archive | Pi |
| Team workflows | Mostly whatever you build | GitHub, GitLab, CI, Slack, IDEs | Codex |
| Product completeness | A harness you shape yourself | A complete coding agent platform | Codex |

### In summary

**Scorecard: Codex 6, Pi 5.**

Codex is the more complete product. You get a real sandbox, cloud execution, proper team workflows, parallel agents, and models that OpenAI can train specifically around the Codex harness.

Pi is the more transparent and hackable one. You can see what is happening, swap models whenever you want, rewrite parts of the runtime, and somehow this tiny four-tool agent still beat Codex by one task in our eval.

But there was one result I really did not expect.

Pi is supposed to be the token-efficiency guy. It has a system prompt and tool definitions under roughly 1,000 tokens, while bigger coding agents carry much more scaffolding around every request.

Yet in our run, **Codex used around 2.4x fewer tokens per task than Pi**.

That sounds completely backwards at first.

It makes sense once you look at how both agents actually work.

---

## Skipping the "What is Pi / Codex?" part this time

If you clicked on a Pi vs Codex post in 2026, I am assuming you already have at least some idea what these tools are.

And honestly, I have written Pi's origin story enough times already. You can check out [Pi vs Claude Code](https://composio.dev/content/pi-agent-vs-claude-code) or [Pi vs OpenCode](https://composio.dev/content/pi-vs-opencode).

So I am not going to copy-paste all of that again.

The one-line version is enough.

[Pi](https://pi.dev/) is Mario Zechner's open-source coding agent built around four tools: `read`, `write`, `edit`, and `bash`. It is now under [Earendil](https://github.com/earendil-works/pi), stays MIT licensed, and is designed to be modified.

[Codex](https://github.com/openai/codex) is OpenAI's coding agent platform. It runs locally in your terminal and IDE, but also stretches into cloud agents, Git workflows, desktop apps, subagents, and a bunch of other surfaces.

That's enough background. The differences are more interesting anyway.

> 💁 If you're already deep into Codex, I also put together a list of [the best Codex plugins and tools](https://composio.dev/blog/best-codex-plugins).

---

## 1. Philosophy: Own the Harness vs Not

This is the biggest difference between Pi and Codex. Most of the other differences in this post are basically side effects of this one.

Pi starts with the assumption that frontier coding models already know how to behave like coding agents. They understand Git, shell commands, tests, files, repository search, and normal software engineering workflows because they have already been trained heavily on that stuff.

🎥 [Watch on YouTube — Watch on YouTube](https://youtu.be/RjfbvDXpFls?si=8L_MyK_OdbBSjneW)

So Pi asks a pretty simple question: why keep explaining all of that to the model?

From that point of view, every extra tool schema, planning abstraction, hidden instruction, and giant system prompt can become context overhead. Pi keeps the base harness tiny and lets you add complexity only when you actually need it.

Codex is also pretty shell-first, so it is not like OpenAI decided the model needs 50 specialized tools for everything.

The difference is what happens around the loop.

Codex takes responsibility for much more of the execution environment. It handles sandboxing, permissions, context management, cloud environments, background jobs, parallelism, Git workflows, and a lot of the stuff Pi simply leaves to you.

| Question | Pi | Codex |
| --- | --- | --- |
| What are you getting? | A programmable agent harness | A complete engineering agent |
| Who controls the prompt? | You | Mostly OpenAI |
| Who controls the workflow? | Mostly you | Mostly the product |
| Who handles isolation? | You | Codex |
| Who chooses the model? | You | Mostly OpenAI |
| Something is missing? | Build it | Use an extension point or integration |

There is one line that sums up the whole thing pretty well:

> ℹ️ **Codex is configurable. Pi is programmable.**

Codex gives you hooks, skills, MCP, SDKs, subagents, and plenty of ways to customize what it does. But those are still extension points OpenAI decided to expose.

Pi lets your TypeScript run inside the harness itself. You can change what the agent does, what tools it has, what goes into context, how sessions behave, or even replace built-in pieces.

That difference is not a huge deal if all you want is "fix this bug."

It becomes a very big deal once you start building agents yourself.

---

## 2. Agent Architecture

Pi's internal architecture is boring. And I mean that as a compliment.

At the center, it is basically the standard agent loop:

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_3.png)

The main pieces are pretty easy to understand.

`pi-ai` handles models and providers. `pi-agent-core` handles the actual agent loop and state. Then `pi-coding-agent` gives you the CLI, sessions, extensions, and the rest of the user-facing stuff.

Pi also exposes useful controls like `steer()`, which lets you change direction while a run is active, and `followUp()`, which queues another message for when the current run finishes.

And because the same agent can run through the TUI, RPC, JSON streams, or directly through the SDK, you are not really locked into Pi's own terminal interface either.

Codex starts with a similar loop, but then puts a lot more machinery around it.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_4.png)

The open-source Codex repo is a pretty serious Rust project. There is a reusable `codex-core`, a Ratatui terminal interface, headless execution for automation, an app server using JSON-RPC, state and protocol crates, MCP support, and platform-specific sandboxing.

Even editing is more opinionated.

Codex pushes edits through `apply_patch` using its own patch format, and OpenAI trains its models around that workflow. So the model is not just generally good at coding; it is trained to work inside the exact environment Codex gives it.

Pi's editing setup is much simpler. `edit` performs targeted replacements, `write` writes files, and bash handles pretty much everything else.

Neither approach is automatically better.

Pi's architecture is easier to understand and modify. Codex's architecture is much more serious if your goal is running agents as a reliable product.

> 💁 Codex is more impressive as systems engineering. Pi is more interesting as an idea.

---

## 3. The Benchmark: Same Model, Same agentic tasks, Different Harness

This is where things got fun.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_5.png)

We used the same setup as our [Pi vs OpenCode comparison](https://composio.dev/content/pi-vs-opencode): **30 hard agentic tool-use tasks** against real apps.

Every harness used the same model, **DeepSeek V4 Pro (0813)** at max reasoning, through our hosted MCP router.

Same model. Same tasks. Same verifier checks.

So whatever changes between runs is mostly coming from the harness.

| Harness (DeepSeek V4 Pro, hard 30 set) | Tasks passed | Cost per shared success | Avg tokens per task |
| --- | --- | --- | --- |
| **Pi Agent** | **21/30 (70%)** | \$0.031 | 924,990 |
| **Codex** | **20/30 (66.7%)** | \$0.031 | **383,722** |
| DeepSeek Harness | 20/30 (66.7%) | \$0.028 | 88,562 |
| OpenCode | 19/30 (63.3%) | \$0.032 | 710,140 |
| Claude Code | 19/30 (63.3%) | \$0.074 | 649,900 |

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_6.png)

Pi topped the board again with **21/30**, while Codex landed one task behind at **20/30**. On tasks both harnesses successfully completed, the cost was basically a dead tie at **\$0.031 per shared success**.

Then I looked at the token column.

Pi averaged **924,990 tokens per task**.

Codex averaged **383,722**.

That means Codex used roughly **2.4x fewer tokens** in this run, even though Pi is the tool built around having almost no fixed context overhead.

At first this looked completely wrong.

Then it clicked.

Pi saves tokens **before the task starts**. Its prompt and tool definitions are tiny, and that same small prefix can stay cache-friendly across turns.

Codex seems to save tokens **during the task**. It reads less, takes fewer turns, follows a more trained-in workflow, and knows when to stop earlier.

Pi averaged 16.3 turns in our run. It kept going.

So the better way to put it is:

> **Lean overhead does not automatically mean lean sessions.**

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_7.png)

Pi's harness can be tiny while the model still spends a million tokens wandering around your task.

Codex can carry more product machinery while the actual trajectory ends up shorter.

That was probably my favorite result from this entire comparison because it kills the easy narrative for both sides.

Pi still passed more tasks.

Codex still used way fewer tokens.

Nobody gets the clean victory screenshot. 🤷‍♂️

> ⚠️ **NOTE:** Codex's standalone cost-per-success was not fully comparable in this eval because cost data was incomplete for some cells, so I am using shared-success cost here. Also, this is one 30-task eval using one model. Don't turn it into universal law.

### Databricks found the other side of the story

Our test is also not the only interesting harness comparison out there.

Databricks benchmarked coding agents against its own multi-million-line codebase using real engineering work across Python, Go, TypeScript, Scala, Rust, protobuf, and Bazel.

Their conclusion was pretty interesting:

> "In many cases, simple harnesses like Pi performed best on our workloads."

Databricks co-founder Matei Zaharia later summarized one of the results on X: Pi achieved the same success rate as some vendor harnesses while costing around **2x less**, largely because it sent less input to the model.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_8.png)

That does not contradict our result as much as it first seems.

It just shows that **harness efficiency depends on the workload, model, and trajectory**.

Sometimes Pi's tiny context wins hard. Sometimes a tightly trained harness like Codex helps the model finish in far fewer steps.

So yeah, your coding agent is not just a pretty terminal wrapped around a model anymore. The harness can change both cost and capability in very real ways.

---

## 4. Context Management

Pi treats context like something you should be able to see and control yourself.

The default system prompt and tool definitions stay under roughly **1,000 tokens**. Very little gets injected behind your back, and if you hate the default prompt, you can replace it.

You can also change compaction, inject your own context, filter history, add custom memory, or build a completely different setup through extensions.

Codex takes the opposite approach.

Its `ContextManager` tracks token usage, compacts old history, keeps cache-friendly prefixes, and lets the model discover repository context through shell commands when it needs something.

You mostly do not think about it.

That is the practical difference.

With Pi, **context engineering becomes part of your workflow**. With Codex, **context engineering becomes OpenAI's problem**.

Neither is automatically wrong.

Not everyone wants to spend an afternoon debugging their custom compaction logic because their agent decided reading `node_modules` twice was a personality trait.

Pi users absolutely will though. That's basically the target audience. Lol.

### Pi's session tree is genuinely excellent

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_9.png)

This is probably one of my favorite things Pi does.

Most coding agents treat a session like a normal chat history: message after message in one straight line.

Pi stores sessions as a tree.

```plain text
A → B → C
     ↘ D → E
     ↘ F → G
```

Every entry keeps an ID and parent ID, so you can jump backwards and continue from an earlier point without destroying what happened after it.

You can use `/tree` to navigate around, fork another session, explore another debugging path, and still keep your old branch.

That sounds like nerdy implementation detail until you have a two-hour coding-agent session go completely off the rails.

Being able to return to "the point before the agent decided to rewrite half the codebase" is extremely useful.

> 💁 A real production use case for linked lists. All that LeetCode grind finally paid off. 😆

Codex has resume and fork support too, but I still prefer Pi's tree model.

It feels like the cleaner abstraction.

---

## 5. Models

This one is not really close.

Pi's `pi-ai` layer can talk to a huge number of providers: OpenAI, Anthropic, Google, Groq, xAI, DeepSeek, Kimi, OpenRouter, local models through llama.cpp or vLLM, and plenty more.

You can even switch models in the middle of the same session.

That gives you some interesting workflows.

You could use an expensive model for planning, switch to something cheap for mechanical edits, try a local model for sensitive code, or swap providers the moment somebody changes pricing or OAuth policies again.

There is a lot of freedom there.

Codex technically supports alternative compatible endpoints in some setups too, but I do not think that is the real comparison.

The full Codex experience is very clearly built around OpenAI models.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_10.png)

More importantly, OpenAI can train those models specifically around Codex itself.

They can train the model around the tools, patch format, sandbox behaviour, context strategy, and the exact kind of long-running software engineering work they want Codex to perform.

That gives Codex something Pi cannot fully copy: **model-harness co-training**.

Pi's argument is that a sufficiently good frontier model should work through a simple generic interface.

Codex's argument is basically: sure, but what if I train the model specifically for my interface?

That's a pretty strong advantage too.

So the result here is simple.

**Pi wins model freedom. Codex wins model-harness integration.**

Personally, I value the freedom more, but I can see why OpenAI's approach works so well for longer autonomous tasks.

---

## 6. Extensibility

Codex is already very extensible by normal coding-agent standards.

It supports MCP, hooks, skills, subagents, SDKs, headless execution, GitHub automation, and a bunch of other integrations. For most developers, that is already more customization than they will ever touch.

Pi still goes deeper.

The reason is simple: Pi extensions run **inside the agent process**.

An extension can register tools, replace built-in tools, add commands, intercept executions, transform messages, inject context, change persistence, rewrite compaction behaviour, register providers, and even modify the TUI.

At that point, calling them "plugins" almost undersells what is happening.

You are modifying the runtime.

And this is where Pi starts making a lot more sense as an agent framework rather than just another terminal coding tool.

OpenClaw could use Pi as the underlying agent runtime. Shopify built `pi-autoresearch`, an autonomous optimization loop, around the same extension system.

Pi's SDK also exposes enough of the loop that you can embed it into another app without treating the CLI as some external black box.

> ℹ️ **Codex gives you extension points. Pi gives you the runtime.**

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_11.png)

> This is a Pi extension, not a separate agent framework. That's kinda the whole point

There is obviously a downside.

Pi's value grows with the amount of effort you put into it.

If you just want to open a terminal, type "fix this issue," and move on with your day, you probably do not care that you can replace the compaction algorithm with 80 lines of TypeScript.

Codex has much better defaults for that person.

> 💁 **Fun fact:** the [pi-skills](https://github.com/badlogic/pi-skills) repo is compatible with Claude Code and Codex CLI too. Somehow the competing coding agents are slowly standardizing on the same skill format and [AGENTS.md](http://agents.md/).

If you are actually building agent infrastructure, though, Pi wins this one pretty easily.

---

## 7. Sandboxing and Permissions

This is easily Pi's weakest part of the comparison.

Pi intentionally ships without a built-in sandbox or permission system.

If you run Pi normally, the agent can generally do whatever your user account can do. If you want real isolation, you are expected to provide it yourself using something like Docker, devcontainers, microVMs, nsjail, or another proper system-level boundary.

There is a real philosophy behind this.

Zechner's argument is that once an agent can write arbitrary code and run that code, permission prompts can easily become security theatre. The actual security boundary should live outside the agent at the OS or container level.

And honestly, I agree with a decent part of that.

The funny part is that Codex implemented the OS-level isolation itself.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_12.png)

On macOS, Codex uses Seatbelt.

On Linux, it uses technologies including Landlock and seccomp.

On Windows, it uses restricted tokens and ACLs.

The sandbox applies across the process tree, and networking can also be restricted depending on the execution mode. Cloud tasks get isolated environments too.

So this is not just another agent asking you "Allow command?" 40 times until you stop reading the popup.

Codex actually has a security architecture around execution.

> 💁 If I am rolling a coding agent out to 50 engineers, "everyone please remember to put Pi inside the right container" is not a security policy.

If you already isolate every agent yourself, Pi's design may be perfectly fine for you.

For most people and especially teams, Codex's default is simply better.

For me, Codex wins this part fair and square.

---

## 8. Pricing and Cost

Pi itself costs nothing.

It is MIT licensed, so your bill mostly comes down to the model and whatever infrastructure you decide to use.

```plain text
Pi cost = $0 + model usage + your infrastructure
```

That gives Pi an extremely low cost floor.

You can run a cheap hosted model, use an open-weight model locally, pay for Opus or GPT when you need something stronger, or mix all of them in the same workflow.

You are not locked into one pricing model.

Codex is bundled into ChatGPT plans and also supports API-backed usage.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_13.png)

The interesting part is that the Codex price is not really buying only model tokens. You are also getting the sandbox, cloud environments, integrations, parallel execution, agent infrastructure, and the rest of the product around it.

So I would not force a fake winner here.

If you want **maximum control over what you spend**, Pi is better.

If you want **one subscription and a full coding-agent product**, Codex can actually be really good value.

And as our benchmark showed, raw token price is not the only thing that matters anyway.

A cheap model that needs three times as many turns can very quickly stop being cheap.

> ⚠️ Agent pricing is incredibly workload-dependent. Always look at cost per completed task, not just price per million tokens.

---

## What the Internet Thinks

Pi has a very specific kind of fan.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_14.png)

These are the people saying things like, "I replaced the compaction logic," "I switched providers halfway through my session," or "I made Pi write a new tool for itself."

You know exactly the type of person I'm talking about.

The praise around Pi is very consistent. People love the transparency, model freedom, session branching, predictable context, hackability, and the fact that very little happens without you being able to inspect it.

The complaints are basically the same qualities viewed from the other side.

Pi has fewer guardrails, expects more work from you, deliberately refuses to add features other coding agents treat as standard, and gives you enough control to create either an amazing custom workflow or a TypeScript-powered crime scene.

Codex attracts a slightly different crowd.

Users tend to praise how good it is at difficult tasks, its methodical debugging, long-running autonomy, cloud execution, sandboxing, and the fact that it fits into an OpenAI subscription they may already use.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_15.png)

Its biggest complaints are often [not about coding ability](https://community.openai.com/t/codex-rate-limits-discussion-thread/1378553) at all.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_16.png)

People complain about usage limits, unclear quotas, changes between releases, and occasional regressions where an update suddenly makes the agent feel worse than it did yesterday.

The simplest way I can put it is:

> **Pi gets more nerd love. Codex is for regular devs.**

Pi's biggest fans usually want to understand or modify the harness.

Codex users often want the exact opposite. They want the harness to disappear so they can hand over a task and review the result later.

That difference tells you a lot about who each tool is really for.

---

## Which One Should You Pick?

### Pick Pi if:

- You want to use **multiple model providers** or local models.
- You care about exactly what enters the model's context.
- You want a harness you can actually understand and modify.
- You are building your own agent workflows or an agent product.
- You already isolate agents through containers or another real sandbox.
- You would rather write 50 lines of TypeScript than wait for a vendor to ship your feature.

### Pick Codex if:

- You want to give an agent a task and **leave it alone**.
- You care about sandboxing and safer defaults.
- Your workflow revolves around GitHub, pull requests, CI, and team review.
- You want cloud execution and parallel agents without building the infrastructure yourself.
- You already pay for ChatGPT and want a complete coding-agent product.
- You want the model and harness to be optimized together.

> 💁 **What I do:** you also do not necessarily have to pick one ecosystem. Pi can use OpenAI models too, so you can keep Pi's transparent harness for interactive work and still keep Codex around for the cloud jobs Pi was never really designed for.

That split makes more sense to me than forcing one tool to do everything.

When I want to stay close to the agent, control the model, inspect the context, or mess with the harness itself, Pi is great.

When I want to give something a clear task, let it disappear for a while, run the tests, and come back with work I can review, Codex makes more sense.

---

## Conclusion

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/pi-agent-vs-openai-codex/image_17.png)

The scorecard says **Codex 6, Pi 5**.

And I think that is fair, but the raw score makes the gap look bigger than it really is.

Pi is still a four-tool agent with a sub-1,000-token harness going against one of the most heavily engineered coding-agent platforms in the market. The fact that Pi not only keeps up but actually passed **21/30 against Codex's 20/30** in our eval is kinda ridiculous.

At the same time, Codex is solving a bigger problem.

Pi is basically:

> How little software can I put between myself and the model?

Codex:

> How much software do I need around the model so I can stop watching it?

And I think that is the real choice here.

If I am **hacking on the agent itself, switching models, running local inference, or building weird custom workflows**, I would pick Pi.

If I am **handing off real engineering tasks, running work in the cloud, working with a team, or I simply want decent safety defaults without building them myself**, I would pick Codex.

And if you just read an entire 3,000-word article about harness overhead, session trees, `apply_patch`, and Linux sandboxing, there is a pretty good chance you are going to install both anyway.

Who are we kidding? 😴
