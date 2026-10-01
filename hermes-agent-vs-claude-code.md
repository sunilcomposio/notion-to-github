# Hermes Agent vs Claude Code in 2026: Which Coding Agent Should You Use?

I started looking into Hermes Agent because I kept seeing people compare it with Claude Code.

At first, the comparison seemed simple:

- Claude Code was for coding, while 
- Hermes handled longer agent workflows, memory, scheduled jobs, and messaging.

But that explanation started to break as I looked deeper.

Claude Code now has: memory across sessions, scheduled tasks, cloud Routines, remote sessions, Slack, mobile access, subagents, plugins, and several other features that used to make Hermes look very different.

Hermes has changed too. It now has a larger skills system, several memory options, scheduled jobs, many model providers, messaging integrations, subagents, and a bundled skill that lets it call Claude Code for coding work.

So I went through both again.

The more I looked, the less useful a simple feature checklist became. They now overlap in many places, but they still approach the work differently.

This comparison focuses on the current differences and which one is the best pick.

---

## TL;DR

| Area | Claude Code | Hermes Agent |
| --- | --- | --- |
| Main use | Coding and software development | General agent work and coding |
| Models | Claude models | Many providers and local models |
| Memory | `CLAUDE.md` + Auto Memory | Memory files + session search + memory plugins |
| Skills | Yes | Yes, including agent-created skills |
| Scheduled work | Yes | Yes |
| Messaging | Slack, Channels, mobile, web | Telegram, Slack, Discord, WhatsApp, Signal, email, and more |
| Subagents | Yes | Yes |
| MCP | Yes | Yes |
| Plugins | Yes | Yes |
| Self-hosting | Limited to supported environments | Yes |
| Open source | No | MIT |
| Can use Claude Code | It is Claude Code | Hermes can call Claude Code |

The table shows why this comparison has become harder.

Many features now appear on both sides.

So instead of asking which tool has more features, I think it makes more sense to look at how each one expects you to work.

---

## What is Claude Code?

[Claude Code](https://claude.com/product/claude-code) is Anthropic's coding agent.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/hermes-agent-vs-claude-code/image_1.png)

- You can open it inside a repository and ask it to understand the code, edit files, run shell commands, write tests, inspect errors, work with git, and use other tools.
- The terminal is still an important interface. But Claude Code also works through VS Code, JetBrains, Desktop, the web, and mobile.
- The web version can continue running after you disconnect. You can also control local sessions remotely, send work from mobile, or use Claude through Slack for supported workflows.

Claude Code has moved far beyond the terminal.

Still, coding sits at the centre of how I would use it.

Usually, I start with a repository and a development task. Claude reads the project, plans the change, edits files, runs commands, tests the result, and gives me something I can review.

Hermes starts with a broader range of tasks.

## What is Hermes Agent?

Hermes is an open-source agent from Nous Research.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/hermes-agent-vs-claude-code/image_2.png)

- You can also use it from a terminal, but the agent can stay running through its gateway and continue across different sessions and interfaces.
- You can talk to the same Hermes setup from Telegram, Discord, Slack, WhatsApp, Signal, email, and several other platforms.
- It also has scheduled jobs, persistent memory, session search, skills, MCP, subagents, different execution backends, and support for many model providers.

This changes how I think about using it.

With Claude Code, my thought process is usually:

```yaml
I have a repository task. Can the agent do it?
```

With Hermes, the thought process can be broader:

```yaml
I have some work that needs research, tools, memory, scheduling, or coding. How should the agent handle it?
```

Coding can still be part of that work, but the main goal is to plan the right architecture for the task.

Hermes even has a bundled Claude Code skill for handing coding tasks to Claude Code. This detail matters later.

But first, it helps to look at the basic idea behind both tools.

## Philosophy: where does the work start?

This became the easiest way for me to separate them.

Claude Code usually starts with a repository.

I have some code. I want to build something, fix something, review something, or understand something. Then the agent works through that repository with me.

Hermes starts with a broader task.

Maybe the task needs code. Maybe it needs research, memory, a scheduled job, several tools, a message from Telegram, or some combination of them. This is why the tools can share many features and still feel different.

- Claude Code keeps development work close to the centre.
- Hermes treats coding as one type of work the agent can do.

This also explains why Hermes can hand a coding task to Claude Code instead of trying to handle every part itself.

Once I started looking at the tools this way, their architecture made more sense too.

---

## Agent architecture

Claude Code follows the development session closely.

- You open a project, give Claude a task, and let it inspect files, run commands, make changes, and work through the repository.
- You can use that workflow through several surfaces now, including the terminal, IDEs, Desktop, web, and mobile.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/hermes-agent-vs-claude-code/image_3.png)

Hermes adds a persistent gateway around the agent.

- The gateway can keep running, and the same Hermes setup can continue working across sessions and messaging interfaces. This means the agent doesn't depend on a single terminal conversation.
- You can leave Hermes running and come back through Telegram, Slack, Discord, or another supported interface.
- You can run Hermes on your own machine, on a VPS, through Docker, over SSH, and through other supported environments such as Modal and Daytona.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/hermes-agent-vs-claude-code/image_4.png)

So I think about the architecture like this.

- Claude Code follows the coding session and repository.
- Hermes keeps the wider agent available across different sessions and systems.

That sounds like a design detail, but it can change the result quite a lot.

We saw this when we tested both with the same model.

---

## Composio benchmark

If Hermes and Claude Code use the same model, how much does the agent harness around that model matter?

This was what we were most curious about. So we tested eight agent harnesses under the same setup:

- Eight agent harnesses used the same Kimi K3 model.
- The model, provider, reasoning level, tools, application data, task instructions, and scoring stayed the same.
- The benchmark used 25 valid tasks across Gmail, Calendar, Sheets, GitHub, Slack, Notion, Linear, Airtable, and PagerDuty.

Here were the Hermes and Claude Code results:

| Harness | Passed | Median time | Tool calls | Estimated cost per success |
| --- | --- | --- | --- | --- |
| Hermes | 20/25 | 164.0s | 262 | \$0.46 |
| Claude Code | 19/25 | 330.5s | 293 | \$1.96 |

Hermes passed one more task in this test. It also had a lower median time and a lower estimated API cost per successful task.

But I would be careful with that result, since the test used Kimi K3 through OpenRouter, which increased token usage as well.

So this shows how both harnesses handled the same external model. It doesn't show how Claude Code performs when you run the latest Claude model inside Anthropic's own coding setup.

I think the more useful result is what happened across all eight harnesses: The same model produced pass rates from 68% to 88%.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/hermes-agent-vs-claude-code/image_5.png)

So choosing a model is only part of the setup. How the agent handles context, tools, retries, subagents, and stopping can change the result too.

This also shows that Hermes was the second-fastest harness overall, while Claude Code had the slowest median runtime and the highest estimated API cost in this particular Kimi K3 test.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/hermes-agent-vs-claude-code/image_6.png)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/hermes-agent-vs-claude-code/image_7.png)

And model choice is another area where Hermes and Claude Code separate clearly.

---

## Models, Context and Token usage

Claude Code is designed around Claude models. This gives Anthropic an obvious integration advantage:

- The company controls both the model and the coding harness, so it can develop them together.
- You can also run Claude through supported third-party infrastructure such as Amazon Bedrock, Google Vertex AI, and Microsoft Foundry.

Hermes takes a broader approach.

It can work with many model providers, OpenAI-compatible endpoints, local models, OpenRouter, and Nous Portal. In fact, Nous Portal gives access to more than 300 models.

That means:

-  I can use one model for coding and another for research.
- I can use a cheaper model for a repeated scheduled task.
- And I can change models without rebuilding the rest of the Hermes setup.

This matters if you like testing new models or matching different models to different jobs.

If I already know I want Claude for my development work, Claude Code keeps the decision much simpler.

The benchmark also gives us one useful lesson about context and token use.

> Even with the same model, the harness can change how the agent uses context, how many tool calls it makes, how long it works, and how often it retries.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/hermes-agent-vs-claude-code/image_8.png)

Based on the given graph, we can say that model choice and harness design both matter.

And once an agent stays around for longer, memory starts to matter too.

---

## Memory and skills

Earlier, I would focus on [CLAUDE.md](http://CLAUDE.md) when discussing memory. For a quick refresher:

- [Claude.md](http://Claude.md) is used for project instructions, commands, conventions, architecture notes, and other useful context.
- Claude loads that information again when you start another session.

But now Claude Code also has Auto Memory.

Claude can save its own notes from your corrections, preferences, project decisions, and other information that may help in future sessions.

Auto Memory is stored per repository and shared across worktrees.

So Claude Code gets two useful memory layers, and both can come back in future conversations.

| Memory | Who writes it | Main use |
| --- | --- | --- |
| `CLAUDE.md` | You | Instructions and project rules |
| Auto Memory | Claude | Learnings and patterns |

Hermes handles memory differently:

- It starts with `MEMORY.md` and `USER.md`.
- `MEMORY.md` stores things the agent learns about its environment, projects, conventions, and previous work.
- `USER.md` stores information about how you prefer to work with the agent.
- Then Hermes keeps previous conversations separately.
- It can search them with `session_search`.

So useful information doesn't always need to stay in the small main memory files. Hermes can search an older session when it needs something again.

Hermes can even integrate with external memory providers such as Mem0, Honcho, Hindsight, Supermemory, and OpenViking.

For me, scope is the useful difference.

- Claude Code memory follows development work and repositories closely.
- Hermes tries to maintain useful context for the wider agent.

Then skills add another layer: Both tools support reusable skills.

- In Claude Code, I could create a skill to review an API endpoint, prepare a release, check a database migration, or follow a pull request process.
- Hermes also uses skills for repeated procedures. But Hermes can create and improve those skills from its own work.

If it solves a difficult task and finds a useful procedure, it can store that process as a skill and use it again later.

That can be useful when work spans several tools.

For example, a skill could explain how to collect information from GitHub, compare it with Linear, prepare a report, and send the result to Slack.

That wider workflow also affects cost.

---

## Pricing

The pricing models are quite different.

- Claude Code comes with supported Claude plans. You pay Anthropic for access, and your available usage depends on the plan you choose.
- Hermes itself is free because the project uses the MIT license. But running Hermes can still cost money. You may pay for a model API, a VPS, cloud execution, search APIs, or other external tools. 
- However, you can also run local models and avoid model API charges for those tasks in both.

So I would not describe Hermes as simply cheaper. The real cost depends on the models and infrastructure you choose.

The benchmark gives one useful cost example, but it should stay separate from subscription pricing.

> The plot shows estimated Kimi K3 API cost for the shared benchmark slice, using OpenRouter list prices.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/hermes-agent-vs-claude-code/image_9.png)

Hermes gives you more choice over where that cost comes from, unlike Claude Code, which bundles it all at a fixed price.

That choice also appears in how both tools can be extended.

---

## Extensibility

Both tools can grow far beyond their default setup.

| Feature | Claude Code | Hermes |
| --- | --- | --- |
| Instructions | `CLAUDE.md` | Skills |
| Memory | Auto Memory | Memory providers |
| Skills | Skills | Skills |
| Hooks | Hooks | Through plugins / workflows |
| MCP | MCP | MCP |
| Plugins | Plugins | Plugins |
| Subagents | Custom subagents | Subagents |
| Multi-agent work | Agent teams | Agent delegation |
| IDE support | IDE integrations | Coding-agent integrations |
| Scheduling | Routines | Cron jobs |
| Messaging | Channels | Messaging adapters |
| Model choice | Claude models | Different model providers |
| Execution | Claude Code environments | Different execution backends |

[Claude Code plugins ](https://composio.dev/content/top-claude-code-plugins)can also package skills, agents, hooks, and MCP servers together.

The difference comes back to what you are extending.

- With Claude Code, most of the extension system stays close to software development.
- With Hermes, those extensions belong to a wider agent that can also do development work.

Once either agent can run commands, connect to external services, and stay active for long periods, control matters.

## Permissions, security, and control

Claude Code fits well when I want a managed development environment with permissions, hooks, IDE support, and review workflows around the repository. Those controls are part of why I use it for active development work besides Codex.

Hermes gives you more control over the agent runtime itself.

It is MIT-licensed, so you can run it on your own machine, on a VPS, through Docker, over SSH, or through other supported execution backends.

Claude Code stays inside Anthropic's product and model ecosystem, while Anthropic also provides enterprise controls and supported hosted environments.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/hermes-agent-vs-claude-code/image_10.png)

Hermes lets you control more of the agent infrastructure yourself. But there is an important detail here.

Self-hosting Hermes does not mean every part of the workflow stays local.

> If you connect Hermes to Claude, OpenAI, a hosted MCP server, or another cloud service, that service still receives the information it needs for that request.

So I would look at the complete data path instead of treating self-hosting as one switch.

The same wider-agent design appears again when we get to scheduling and where you can actually talk to the agent.

## Scheduling and surfaces

I originally treated scheduled jobs as a clear Hermes feature.

That is no longer accurate.

Claude Code now has several ways to schedule work.

- Inside a running session, `/loop` can repeatedly check something such as a release branch, CI status, or review comments.
- Claude Code also supports scheduled tasks on Desktop.
- Then there are cloud Routines.

For a quick refresher:

- A Routine can contain a prompt, repository, environment, connectors, permissions, and triggers.
- Cloud Routines run through Anthropic's infrastructure, so they do not depend on your laptop staying awake.
- They can use connectors too.

This means now Claude Code can handle unattended work.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/hermes-agent-vs-claude-code/image_11.png)

Hermes has its own built-in cron system as well, quite ahead of Claude Code:

- A job can run once or on a schedule.
- It can start a fresh agent session, use a selected model, attach skills, and send the result somewhere when it finishes.
- A job can send its result back to where it started, save it locally, or send it to platforms such as Telegram or Discord.
- You can also use it for silent monitoring, where Hermes checks something regularly and stays quiet while everything works. It sends a message only when it finds a problem.

This is where I would separate the two.

- Claude Code Routines make sense when the repeated work stays close to development.
- Hermes cron makes more sense when a scheduled job needs to move between tools, models, scripts, messaging platforms, or coding agents.

The same pattern appears in the interfaces:

- Claude Code is much easier to use away from the terminal now.
- You can start or monitor cloud work from mobile, use Remote Control for local sessions, send tasks through Dispatch, or work through Slack. 
- Channels can also receive events from systems such as Telegram, Discord, CI systems, or your own server.

Hermes covers more messaging platforms directly.

Its integrations include Telegram, Discord, Slack, Google Chat, WhatsApp, Signal, SMS, email, Mattermost, Matrix, DingTalk, Feishu/Lark, WeCom, WeChat, and iMessage through BlueBubbles.

You can leave Hermes running on a server and talk to it through the apps you already use. This also works with cron. This means:

> I can ask Hermes from Telegram to run something every morning and send the result back to Telegram.

So where would I actually use each one?

---

## Which should you pick?

I will use Claude Code when most of the work happens inside a repository. Its tools, model integration, permissions, hooks, IDE support, remote sessions, and review workflow fit this type of work well.

I would reach for Hermes when the job continues outside the repository. Coding can still be part of the job.

Simple comparison table to get you started

| Use Claude Code when... | Use Hermes when... |
| --- | --- |
| Most of the work happens inside a repository | The job continues outside the repository |
| You are building a feature | You want to run research every morning |
| You are fixing bugs or refactoring files | You need scheduled checks or monitoring |
| You are writing tests or debugging CI | You want context across several projects |
| You are reviewing pull requests | You want to use different models for different tasks |
| You are exploring or understanding a large codebase | You want to work through Telegram, Slack, or other apps |
| You are running repeated repository maintenance | You need several external tools in one workflow |
| You want tight IDE, hooks, permissions, and review support | You want reusable skills, your own server, or other agents |

In a nutshell:

- Claude Code fits repository-heavy development work very well.
- Hermes makes more sense when coding is only one part of a larger workflow.

And that leads to the setup I find most interesting.

---

## Using Hermes and Claude Code together

Hermes includes a Claude Code skill by default.

The skill tells Hermes how to launch the Claude Code CLI and delegate coding work.

Hermes can handle this in two ways:

- For a simple task, it can run Claude Code in print mode, wait for the result, and continue.
- For longer work, it can start an interactive Claude Code session and keep communicating with it.

This means you can send Hermes instructions from Telegram:

```yaml
Check the new support issues from today. Find anything that looks like a real bug. Reproduce the most important one, fix it, run the tests, and send me the result.
```

So the workflow will split as:

- Hermes can collect the issues, keep the context, decide what needs coding, and then call Claude Code for the repository work.
- Claude Code can inspect the code, make the changes, and run the tests.
- Then Hermes can continue with the result.

This changes how we approach the task

No need to choose one and leave the other out.

Hermes can keep the longer-running context and manage the wider job. Then Claude Code can handle the serious repository work when the task reaches that point.

In fact, this is the best way to use Hermes, rather than comparing; it's the best of both worlds.

Hope this establishes the core workflow for new usage and now for the final takeaway.

---

## Conclusion

After going through both Claude Code & Hermes, I wouldn't choose based on which one has the longer feature list, since there is too much overlap now.

I would look at where the work starts:

- If I open a repository because I need to build or fix something, Claude Code makes sense.
- If I start with a wider task that may involve research, memory, external tools, messages, scheduled work, and some coding along the way, I would reach for Hermes.

And the workflow doesn't have to end there.

- Hermes can maintain long-running context and handle the broader task.
- When it reaches serious repository work, it can pass that part to Claude Code.

Claude Code has expanded far beyond its original terminal workflow. Hermes has become much more capable at coding too.

But I still tend to think about Claude Code from the repository outward, while Hermes starts with the agent and whatever work I give it.

For me, that is the useful difference to consider for the modern comparison.
