# Top Grok Skills and Plugins

xAI released Grok Build in May, and it’s been improving steadily since then.

With support for spawning up to 8 parallel subagents and a recently added system for skills and plugins, Grok is now a full-fledged ecosystem that rivals contenders like Claude Code and Codex.

However, unlike Claude or Codex, Grok comes with a superpower - it can scrape X data, the real-time engine behind every major announcement, quality content, trends and conspiracies, all with your X subscription.

This gives Grok Build a unique edge for research-heavy workflows. You can track launches as they happen, pull insights from real conversations, spot emerging trends, and turn that live context into apps, agents, or automated workflows.

Access to real-time data is only one part of the equation, though. To make that information useful, Grok needs the right tools to search, process, design, code, and take action across different platforms.

But with so many options in place for a single need, it's hard to find the right one.

This guide aims to cover which ones are worth installing on your first install, why to install them, and how to install them.

Let’s begin with a quick refresher on what skills and plugins are and why you should install them.

---

## What are Skills & Plugins & Why They Matter

The concept of skills and plugins is not new, yet people still often interchange them. Both extend the capabilities of Grok Build and are related but not the same.

**Skills **are small, reusable instruction packs (usually a [SKILL.md](http://skill.md/) plus optional scripts) that turn Grok into a consistent specialist.

They eliminate repetitive, long prompts, enforce high-quality practices such as TDD or careful planning, reduce token waste through focused behaviour, and deliver the same reliable results across projects.

On the other hand;

**Plugins **are larger, one-command packages that bundle one or more skills with MCP servers, automation hooks, sub-agents, and platform integrations, giving Grok real superpowers.

With plugins, Grok Build can do live web research, control browsers, perform database operations, analyse production errors, and enable seamless deployments.

This is essential for complex agentic workflows and is easy to install, adopt, and share.

With that clarification done, let’s look at how to install skills and plugins in Grok Build before looking at some of the best skills and plugins you should check out/install first.

Related: [Best OpenCode Skills](https://composio.dev/content/10-best-opencode-skills-that-are-actually-useful-in-2026)

---

## <span discussion-urls="discussion://3abf261a-6dfe-80f0-b061-d1e11e9add51/83aa4232-f2fe-4199-9b33-19ddd911d654/3acf261a-6dfe-80a1-8145-001cdeb0762a">How to install Skills & Plugins in Grok Build</span>

Grok Build provides these ways to install skills and plugins.

### Skills

The Official ones listed on the marketplace can be accessed using `/marketplace` inside Grok Build itself.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_1.png)

Or you can try manual placement

Put skills in `./.grok/skills/` or `~/.grok/skills/`

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_2.png)

or add an extra path in the `~/.grok/config.toml` under `[skills]` .

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_3.png)

### Plugins

Grok Plugins can be installed using:

```plain text
 grok plugin install <name> --trust
```

and then verify the plugin by doing `/plugin` .

If it fails, use the Grok Build Marketplace to add it as a plugin.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_4.png)

Or you can add extra paths in `~/.grok/config.toml` under  `[plugins]`

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_5.png)

> For both skills and marketplace,  Grok Build comes with automatic compatibility support for `.claude/skills` and `agent/skills`.  Just put skills there and let grok build pick it up.

### Alternative

For non-official skills packs like [skills.sh](http://skills.sh) use:

```plain text
npx skills@latest add <skill>
```

We will use any one of the methods listed going forward. Now time to look at top skills and plugins.

---

## Top Skills to Use with Grok Build CLI in 2026

These are the top skills I would install if I reinstall Grok Build. Most of them still live in my workspace.

### 1. <span discussion-urls="discussion://3abf261a-6dfe-80f0-b061-d1e11e9add51/3acf261a-6dfe-80af-99c3-fd98ae107dca/3acf261a-6dfe-8032-875b-001cc5845138">Composio CLI: Power Grok with 1000+ apps from GitHub, Linear, to Figma, Canva.</span>

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_6.png)

[Composio](https://composio.dev/) provides Grok Build with access to more than [1,000 applications ](https://composio.dev/toolkits)via a single remote MCP connection. Instead of loading every integration action into the model’s context, it exposes seven meta-tools that let the agent find tools, initiate authorisation, and execute actions when needed.

This makes it useful for workflows involving applications such as  [GitHub](https://composio.dev/toolkits/github/framework/grok-build), [Linear](https://composio.dev/toolkits/linear/framework/grok-build), [Jira](https://composio.dev/toolkits/jira/framework/grok-build), [Figma](https://composio.dev/toolkits/figma/framework/grok-build), and other external services. When an application has not been connected, Composio can generate an OAuth authorisation link for the user.

You can install Composio using:

```bash
grok mcp add --transport http composio https://connect.composio.dev/mcp
```

Then type `/mcp` , select Composio and complete the OAuth flow.

> Note: In WSL you can’t access the browser directly, so copy-paste the produced URL and configure it.

Alternatively;

You can type `/mcp`  inside Grok Build, in the mcp window press a to add a new mcp. Add the  `https://connect.composio.dev/mcp`  and initiate the OAuth flow

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_7.png)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_8.png)

Or install the Composio CLI directly. It's a CLI for everything Composio that can handle authentication management, tool calling, bash scripting and everything in-between.

This gives a more composable way for Grok CLI to work with Composio toolkits.

You can install it with:

```plain text
curl -fsSL https://composio.dev/install | bash
composio login
```

Complete the OAuth flow and then add the composio-cli skill

```plain text
composio --install-skill composio-cli claude
```

This ensures that composio-cli, used by Grok Build, follows the correct instructions and doesn’t hallucinate.

> The reason I put this one at the top is because , it offers the skill, plugins and mcp all bundled together under one ecosystem, so one time config is all you need.

### 2. Matt Pocock Skills: Add disciplined planning, TDD, debugging, and handoff workflows.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_9.png)

[Matt Pocock Skills](https://github.com/mattpocock/skills) adds proper engineering discipline to Grok Build.

Similar to superpowers, it helps the agent plan more effectively, avoid common failure modes, and follow structured processes rather than jumping straight into code.

It includes practical skills like `/grill-me` for questioning plans, `/tdd` for test-first development, `/diagnosing-bugs` for testing hypotheses before fixing, and handoff for clean session transfers.

This is still one of the highest-signal skill packs available across coding agents and one of my favourites.

You can install Matt Pocock Skills using:

```bash
npx skills@latest add mattpocock/skills
```

Then run `/setup-matt-pocock-skills` once inside Grok Build so it learns your project conventions.

Related: [Top Codex Skills](https://composio.dev/content/top-codex-skills)

---

### 3. Caveman: Cut token waste with terse, high-signal agent responses.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_10.png)

[Caveman](https://github.com/juliusbrussee/caveman) is an answer for those who want to reduce token waste on long sessions. It forces Grok to drop the unnecessary politeness and over-explanation that usually appears in agent responses like Claude Code, Codex, and so on.

In practice, it can cut output length by roughly 65% on average (ranging from \~22–87% depending on the task) while keeping the useful content intact. It's a small skill but one worth keeping enabled almost all the time.

You can install Caveman using:

```bash
npx skills@latest add JuliusBrussee/skills --skill caveman
```

<br>Once installed, toggle it with:  `/caveman` or by saying "talk like caveman"; turn it off with "normal mode."

Companion commands include

- `/caveman-commit` (terse commit messages), 
- `/caveman-review` (one-line PR comments), and 
- `/caveman-stats` (session savings).

---

### 4. Whathappened: Turn real-time X conversations into structured briefings.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_11.png)

If you are X-savvy or want real-time information about X in a structured way, what happened is the answer.

[WhatHappened](https://github.com/kunchenguid/whathappened) turns real-time X data into clean, structured briefings rather than raw noise, summarises what happened, maps public opinion, surfaces live debates, and pulls key receipts, all using Grok’s built-in X tools.

> Note : This skill only works properly inside Grok Build.

You can install whathappened using:

```bash
npx skills add kunchenguid/whathappened
```

and then use: `/whathappened <query>`

---

### 5. XActions Skills: Scrape, monitor, and automate X workflows without the official API.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_12.png)

XActions takes `whathappend` ability to the next level by doing more than just reading X. It packages scraping, monitoring, and automation capabilities into ready-to-use agent skills.

You can scrape profiles, followers, threads, monitor accounts, or run simple automation tasks without needing the official X API. This is one of the cleaner ways people have begun to package Grok’s X advantage.

You can install XActions Skills by cloning the repository and placing the skills you want under your skills folder:

```bash
git clone <https://github.com/nirholas/XActions>
```

Then copy the relevant skill folders into `~/.grok/skills/` or your project’s `.grok/skills/`.

---

### 6. Wangnov/grok-skills: Combine web/X research with image, video, and ffmpeg workflows.

Haven’t tried it yet, but on my to-do list.

`Wangnov/grok-skills` is especially useful when your workflow needs both research and media generation. It combines web and X research with image generation, video generation, and basic ffmpeg post-processing.

Everything runs through a logged-in Grok session, so you avoid extra API costs for media tasks. It’s a practical all-in-one skill for research-plus-assets work.

You can install Wangnov/grok-skills using:

```bash
npx skills add Wangnov/grok-skills
```

You can learn more at: [https://github.com/Wangnov/grok-skills](https://github.com/Wangnov/grok-skills)

---

### 7. Agentic-Code-Review & Repo-Health-Check: Review code and understand unfamiliar repos safely.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_13.png)

These two skills from the `awesome-grok-build` starter kits are especially useful for properly reviewing code and getting oriented in unfamiliar repositories.

[agentic-code-review](https://github.com/DominikTobureto/awesome-grok-build/blob/main/.grok/skills/agentic-code-review/SKILL.md) focuses on correctness, security, tests and regression risk. repo-health-check helps you quickly understand a new codebase and propose the smallest, safe-first change. Both work well with Plan Mode.

You can install them by cloning the community kit and copying the skill folders:

```bash
git clone <https://github.com/DominikTobureto/awesome-grok-build>
```

Then place the skill folders into your `.grok/skills/` directory. Learn more at: [https://github.com/DominikTobureto/awesome-grok-build](https://github.com/DominikTobureto/awesome-grok-build)

---

### 8. GodotPrompter: Give Grok better Godot, GDScript, scenes, and signal context.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_14.png)

If you are building games with Godot and want Grok Build to understand Godot-specific patterns, project structure, and common workflows.

It gives the Grok/Cursor agent better context around GDScript, scenes, signals, and Godot conventions so the suggestions stay more accurate and less generic.

This is one of the cleanest game-engine-focused plugins currently available for Grok Build.

You can install GodotPrompter using:

```plain text
grok plugin install jame581/GodotPrompter --trust
```

Then enable it with:

```plain text
grok plugin enable godot-prompter
```

Learn more at: [https://github.com/jame581/GodotPrompter](https://github.com/jame581/GodotPrompter)

---

### 9. Hyperframes: Create and edit programmatic videos with HTML, CSS, and JavaScript.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_15.png)

[Hyperframes ](https://hyperframes.heygen.com/)is for those who want a Grok Build or similar agent to create and edit videos using HTML, CSS, and JavaScript rather than traditional timeline editors.

It ships a full set of agent skills that teach the correct patterns for planning compositions, writing valid HyperFrames HTML, adding animations, linting, previewing and rendering.

The main entry skill is `/hyperframes`, which routes “make me a video” requests to the right workflow.

You can install Hyperframes skills using:

```bash
npx skills add heygen-com/hyperframes
```

and use it with `/hyperframes`  in grok build. Learn more at: [https://hyperframes.heygen.com/](https://hyperframes.heygen.com/)

---

### 10. Remotion Skills: Build production-ready motion graphics and videos with React and TypeScript.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_16.png)

[Remotion ](https://github.com/remotion-dev/remotion)is similar to Hyperframes in that it allows you to create motion graphics and programmatic videos using React and TypeScript.

It teaches the agent Remotion best practices like compositions, animations, sequencing, rendering and project structure

This makes the output clean and production-ready, rather than generic React code that happens to render video.

You can install Remotion Skills using:

```plain text
npx skills add remotion-dev/skills
```

You can learn more at: [https://www.remotion.dev/docs/ai/skills](https://www.remotion.dev/docs/ai/skills)

> Note: Remotion skill work best inside an existing Remotion project, so better first create it.

Related: [Top Design Skills](https://composio.dev/content/top-design-skills)

## Top Plugins to Use with Grok Build

Skills are great; some even perform tasks, but for a seamless experience, plugins are mandatory. These are the ones that still reside directly in my skills.

---

### 11. Firecrawl: Search, scrape, crawl, and extract clean data from websites.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_17.png)

[Firecrawl ](https://www.firecrawl.dev/)is especially useful for research, document retrieval, competitive analysis, and data collection from websites.

It gives Grok Build live access to the web through search, scraping, crawling, website mapping, structured extraction, and browser interaction.

It can render JavaScript-heavy pages, handle common anti-bot restrictions, and return content as clean Markdown or structured data.

You can install Firecrawl using:

```plain text
/marketplace
```

Then search for `firecrawl` and press `i` to install it.

> Note: Firecrawl may request authentication when you first use its hosted MCP server.

---

### 12. Superpowers: Add structured engineering workflows for planning, TDD, and debugging.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_18.png)

[Superpowers ](https://github.com/obra/superpowers)is for those who want their agent to plan carefully, validate its work, and follow a more disciplined development process instead of immediately generating code.

It adds structured software engineering workflows to Grok Build, and the current implementation includes test-driven development, systematic debugging, collaboration patterns, and repeatable engineering processes.

I personally use this before switching to Matt Pocock's skills.

You can install it with:

```bash
grok plugin install superpowers --trust
```

> Note: Review the plugin before using `--trust`, since that option skips the interactive trust prompt.

---

### 13. Exa: Get fast, high-quality agent-oriented web search results.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_19.png)

[Exa ](https://exa.ai/)provides fast, high-quality agent-oriented search. It works particularly well as a complement to Firecrawl.

Exa's API is purpose-built for LLMs, so results come back structured and filtered rather than cluttered with ads and navigation, fast enough for an agent to search mid-task without breaking flow.

For teams, that means less time and token spend per lookup, so search-heavy steps (competitor checks, source verification, quick fact lookups) stop being a bottleneck inside the coding session itself

Use Exa for quick, accurate retrieval, and switch to Firecrawl for full-page scraping or site crawling.

You can install the Exa plugin using:

```plain text
/plugin
```

Then search for `exa` and press `i`to install it.

---

### 14. Vercel: Let Grok manage deployments, environment variables, logs, and domains.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_20.png)

[Vercel ](https://github.com/vercel/vercel-plugin)is for those who want their agents to deploy their projects on the Vercel platform on their behalf. It gives Grok direct control over deployments, environment variables, build logs, and domains.

It also keeps Grok aware of current Vercel features, which reduces outdated suggestions.

For teams, this pairing of live platform knowledge with real deploy/env/log access leads to fewer review cycles spent catching agent suggestions that no longer reflect how Vercel actually works.

This fixes a common issue with agent integrations: context drift.

You can install the Vercel plugin using:

```plain text
/plugin
```

Then search for `vercel` and install it using `i` .

---

### 15. Cloudflare: Build and deploy Workers, Durable Objects, and edge apps with platform-aware guidance.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_21.png)

[Cloudflare ](https://github.com/cloudflare/skills)is for those scenarios when part of your stack lives on the other side of the world. It provides skills for Workers, Durable Objects, and related Cloudflare tooling.

The plugin covers the entire Cloudflare developer platform: Workers, Durable Objects, the Agents SDK, MCP servers, Wrangler CLI, and web performance, functioning as a skill library that maps Cloudflare concepts directly to prompts.

This makes scaffolding and deploying edge projects noticeably smoother inside Grok Build for business and working with it easier.

You can install Cloudflare skills or plugins using:

```plain text
/plugin
```

Then search for `cloudflare` and install it using `i`

> Note: Don’t grt confused by the name, if you go to official repo , its given as a plugins.

---

### 16. Chrome DevTools: Debug frontend issues through live browser inspection, traces, and network data.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_22.png)

[Chrome DevTools ](https://github.com/ChromeDevTools/chrome-devtools-mcp)is useful for frontend debugging and performance work. It lets Grok control a live browser session. (not on WSL)

You can record performance traces, inspect network requests, evaluate JavaScript, and take DOM snapshots without leaving the agent workflow.

Under the hood, this runs on the official Chrome DevTools MCP server, which lets a coding agent control and inspect a live Chrome browser and acts as a Model Context Protocol server, giving the assistant access to the full power of Chrome DevTools for reliable automation, in-depth debugging, and performance analysis.

This means that instead of an engineer manually opening DevTools, reproducing the issue, and reporting back what they saw, Grok can drive the same browser session directly and return a trace, a failing request, or a DOM state as evidence.

You can install the Chrome DevTools plugin using:

```plain text
/plugin
```

Then search for `chrome-devtools` and install it using `i`

Related: [Top Claude Code Plugins](https://composio.dev/content/top-claude-code-plugins)

---

### 17. Sentry: Pull production errors and stack traces into Grok for faster fixes.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_23.png)

[Sentry ](https://github.com/getsentry/sentry-for-ai)closes the loop between local development and production.

It lets Grok pull real error data and stack traces. Combined with Seer-powered analysis, it helps turn production issues into concrete fixes faster.

This works through Sentry's official Grok plugin, which connects Grok to Sentry via the Sentry MCP server, providing real production issue-debugging context, code review with Sentry data, and monitoring configuration- on top of SDK setup for any platform.

This means less engineer time spent context-switching between logs, code, and chat to reconstruct what broke, and a shorter gap between an alert firing and a fix landing.

You can install the Sentry plugin using:

```plain text
/plugin
```

Then search for `sentry` and install it. Part of the official Grok Build Marketplace.

---

### 18. Unity MCP + CLI: Drive the Unity editor and iterate on game projects from Grok.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_24.png)

If you are like me and like to build games in [Unity](https://github.com/IvanMurzak/Unity-MCP) and want Grok Build to actually drive the editor and project instead of just writing C# in isolation.

People are already using this combination to let Grok open scenes, modify GameObjects, work with the Asset Store, and iterate on playable prototypes much faster. It is currently one of the most practical ways to pair Grok Build with Unity.

You can set it up by installing the Unity MCP server and connecting it through Grok’s MCP system, then pairing it with a simple Unity-focused skill that teaches the agent your project conventions.

```plain text
# 2. In Unity, open Package Manager → Add package from git URL
https://github.com/CoderGamester/mcp-unity.git
```

After the package is installed, open **Tools → MCP Unity → Server Window** and click **Force Install Server**.

Then connect it to Grok Build with:

```plain text
grok mcp add unity
```

> Note: There is no single official marketplace plugin yet. Most people combine the Unity MCP with grok build and a lightweight custom skill for best results.

---

---

## Conclusion

Skills give Grok better judgment and consistency. Plugins give Grok real tools and reach, but don’t install them all at once.

Start with a solid foundation:

- Cross-app workflows: Composio CLI or MCP
- Engineering discipline → Matt Pocock Skills + Superpowers
- Token control → Caveman
- X advantage → what happened or XActions
- Web power → Firecrawl + Exa
- Game development → GodotPrompter or Unity MCP setup

Then add the platform plugins that match the work you actually do (Vercel, Cloudflare, Sentry, etc.).

Use both skills and plugins, but do it with a clear understanding of the use case. Only then does Grok Build start to feel like a real system instead of just another coding agent.

---

### Sources {toggle="true"}

	- Official docs: [https://docs.x.ai/build/features/skills-plugins-marketplaces](https://docs.x.ai/build/features/skills-plugins-marketplaces)
	- Plugin Marketplace announcement: [https://x.ai/news/grok-plugin-marketplace](https://x.ai/news/grok-plugin-marketplace)
	- Firecrawl roundup: [https://www.firecrawl.dev/blog/best-grok-plugins](https://www.firecrawl.dev/blog/best-grok-plugins)
	- Matt Pocock Skills: [https://github.com/mattpocock/skills](https://github.com/mattpocock/skills)
	- Superpowers: [https://github.com/obra/superpowers](https://github.com/obra/superpowers)
	- whathappened: [https://github.com/kunchenguid/whathappened](https://github.com/kunchenguid/whathappened)
	- XActions: [https://github.com/nirholas/XActions](https://github.com/nirholas/XActions)
	- Wangnov/grok-skills: [https://github.com/Wangnov/grok-skills](https://github.com/Wangnov/grok-skills)
	- awesome-grok-build: [https://github.com/DominikTobureto/awesome-grok-build](https://github.com/DominikTobureto/awesome-grok-build)
