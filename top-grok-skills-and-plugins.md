# Top Grok Skills and Plugins

xAI released Grok Build in May, and it’s been improving steadily since then.

With support for spawning up to 8 parallel subagents and a recently added skills and plugins system, Grok is now a full-fledged ecosystem that rivals contenders like Claude Code and Codex.

However, unlike Claude or Codex, Grok comes with a superpower - it can scrape X data, the real-time engine behind every major announcement, quality content, trends and conspiracies, all with your X subscription.

Apart from that, Grok Build has also become popular in game communities, with people pairing it with game engines like Unity or Godot to build games. (more on this later in the blog)

For example:

Tetsuo on X built a new FPS shooter game in 1 hour:

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_1.png)

Another guy named Dan created a space combat game in C#:

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_2.png)

But what makes these builds and workflows is not just the prompts or agents; it’s the skills and plugins doing the heavy lifting.

This guide aims to cover some of the ones worth installing on your first install, why to install them, and how.

Let’s begin with a quick refresher on what skills and plugins are and why you should install them.

---

## What are Skills & Plugins & Why They Matter

The concept of skills and plugins is not new, yet people still interchange them quite a lot. Both extend the capabilities of Grok Build, and they are related, but not the same.

Skills are small, reusable instruction packs (usually a [SKILL.md](http://skill.md/) plus optional scripts) that turn Grok into a consistent specialist.

They eliminate repetitive long prompts, enforce high-quality practices like TDD or careful planning, cut token waste through focused behaviour, and deliver the same reliable results across projects.

On the other hand;

Plugins are larger, one-command packages that bundle one or more skills together with MCP servers, automation hooks, sub-agents and platform integrations that give Grok real superpowers.

With plugins, Grok Build can do live web research, control browsers, perform database operations, analyse production errors, and enable seamless deployments.

This is essential for complex agentic workflows and is easy to install, adopt, and share.

With that clarification done, let’s look at how to install skills and plugins in Grok Build before looking at some of the best skills and plugins you should check out/install first.

---

## How to install Skills & Plugins in Grok Build

Grok Build provides these ways to install skills and plugins.

- The marketplace - official ones listed on the marketplace, accessed using `/marketplace` or `/plugin`, least hassle
- Manual placement - put skills in `./.grok/skills/` or `~/.grok/skills/`, plugins in `./.grok/plugins/` or `~/.grok/plugins/`
- CLI command - use `grok plugin install <name> --trust`
- Config file - add extra paths in `~/.grok/config.toml` under `[skills]` or `[plugins]`
- npx installer - for skill packs (example: `npx skills@latest add <skill>`)
- Automatic compatibility - works with Claude Code skills and `~/.agents/skills/` without extra install

We will use any one of the method listed going forward. Now time to look at top skills and plugins.

---

## Top Skills to Use with Grok Build

These are the top skills I would install if I reinstall Grok Build. Most of them still live in my workspace.

### Matt Pocock Skills

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_3.png)

Matt Pocock Skills add proper engineering discipline to Grok Build. Similar to superpowers, it helps the agent plan better, avoid common failure modes, and follow structured processes instead of jumping straight into code.

It includes practical skills like `/grill-me` for questioning plans, `/tdd` for test-first development, `/diagnosing-bugs` for testing hypotheses before fixing, and handoff for clean session transfers.

This is still one of the highest-signal skill packs available across coding agents and one of my favourites.

You can install Matt Pocock Skills using:

```bash
npx skills@latest add mattpocock/skills
```

Then run `/setup-matt-pocock-skills` once inside Grok Build so it learns your project conventions.

---

### Caveman

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_4.png)

Caveman is an answer for those who want to reduce token waste on long sessions. It forces Grok to drop the unnecessary politeness and over-explanation that usually appears in agent responses like Claude Code, Codex, and so on.

In practice, it can cut output length by roughly 65% on average (range \~22–87% depending on task) while keeping the useful content intact. It's a small skill but one worth keeping enabled almost all the time.

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

### Whathappened

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_5.png)

If you are X-savvy or want real-time information of X in a structured way, whathappened is the answer.

Whathappened turns the real-time X data into clean, structured briefings instead of raw noise and summarises what happened, maps public opinion, surfaces live debates, and pulls key receipts,  all using Grok’s built-in X tools.

> Note : This skill only works properly inside Grok Build.

You can install whathappened using:

```bash
npx skills add kunchenguid/whathappened
```

and then use: `/whathappened <query>`

---

### XActions Skills

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_6.png)

XActions takes `whathappend` ability to the next level by doing more than just reading X. It packages scraping, monitoring, and automation capabilities into ready-to-use agent skills.

You can scrape profiles, followers, threads, monitor accounts, or run simple automation tasks without needing the official X API. This is one of the cleaner ways people have started packaging Grok’s X advantage.

You can install XActions Skills by cloning the repository and placing the skills you want under your skills folder:

```bash
git clone <https://github.com/nirholas/XActions>
```

Then copy the relevant skill folders into `~/.grok/skills/` or your project’s `.grok/skills/`.

---

### Wangnov/grok-skills

Haven’t tried it yet, but on my to-do list.

`Wangnov/grok-skills` is especially useful when your workflow needs both research and media generation. It combines web and X research with image generation, video generation, and basic ffmpeg post-processing.

Everything runs through a logged-in Grok session, so you avoid extra API costs for media tasks. It’s a practical all-in-one skill for research-plus-assets work.

You can install Wangnov/grok-skills using:

```bash
npx skills add Wangnov/grok-skills
```

You can learn more at: [https://github.com/Wangnov/grok-skills](https://github.com/Wangnov/grok-skills)

---

### Agentic-Code-Review & Repo-Health-Check

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_7.png)

These two skills from the `awesome-grok-build` starter kit are especially useful for reviewing code properly and getting oriented in unfamiliar repositories.

agentic-code-review focuses on correctness, security, tests and regression risk. repo-health-check helps you quickly understand a new codebase and propose the smallest safe first change. Both work well with Plan Mode.

You can install them by cloning the community kit and copying the skill folders:

```bash
git clone <https://github.com/DominikTobureto/awesome-grok-build>
```

Then place the skill folders into your `.grok/skills/` directory. Learn more at : [https://github.com/DominikTobureto/awesome-grok-build](https://github.com/DominikTobureto/awesome-grok-build)

---

### GodotPrompter

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_8.png)

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

### Hyperframes

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_9.png)

Hyperframes is for those who want a Grok Build or similar agent to create and edit videos by writing HTML, CSS and JavaScript instead of using traditional timeline editors.

It ships a full set of agent skills that teach the correct patterns for planning compositions, writing valid HyperFrames HTML, adding animations, linting, previewing and rendering.

The main entry skill is `/hyperframes`, which routes “make me a video” requests to the right workflow.

You can install Hyperframes skills using:

```bash
npx skills add heygen-com/hyperframes
```

and use it with `/hyperframes`  in grok build. Learn more at: [https://hyperframes.heygen.com/](https://hyperframes.heygen.com/)

---

### Remotion Skills

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_10.png)

Remotion is similar to Hyperframes in that it allows you to create motion graphics and programmatic videos using React and TypeScript.

It teaches the agent Remotion best practices like compositions, animations, sequencing, rendering and project structure

This makes the output clean and production-ready instead of generic React code that happens to render video.

You can install Remotion Skills using:

Bash

```plain text
npx skills add remotion-dev/skills
```

You can learn more at: [https://www.remotion.dev/docs/ai/skills](https://www.remotion.dev/docs/ai/skills)

> Note: Remotion skill work best inside an existing Remotion project, so better first create it.

---

---

## Top Plugins to Use with Grok Build

Skills are great; some even perform tasks, but for a seamless experience, plugins are mandatory. These are the ones that still reside directly in my skills.

### Composio

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_11.png)

[Composio](https://composio.dev/) gives Grok Build access to more than 1,000 applications through a single remote MCP connection. Instead of loading every integration action into the model’s context, it exposes seven meta-tools that let the agent find tools, initiate authorisation, and execute actions when needed.

This makes it useful for workflows involving applications such as Gmail, Slack, HubSpot, GitHub, Google Calendar, and other external services. When an application has not been connected, Composio can generate an OAuth authorisation link for the user.

You can install Composio using:

```bash
grok mcp add --transport http composio <https://connect.composio.dev/mcp>
```

Then type `/mcp`, select Composio, and complete the OAuth flow.

> Note: In WSL you can’t access the browser directly, so copy-paste the produced URL and configure it.

---

### Firecrawl

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_12.png)

Firecrawl is especially useful for research, documentation retrieval, competitive analysis, and collecting data from websites.

It gives Grok Build live access to the web through search, scraping, crawling, website mapping, structured extraction, and browser interaction. It can render JavaScript-heavy pages, handle common anti-bot restrictions, and return content as clean Markdown or structured data.

You can install Firecrawl using:

```plain text
/marketplace
```

Then search for `firecrawl` and press `i` to install it.

> Note: Firecrawl may request authentication when you first use its hosted MCP server.

---

### Superpowers

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_13.png)

Superpowers is for those who want their agent to plan carefully, validate its work, and follow a more disciplined development process instead of immediately generating code.

It adds structured software-engineering workflows to Grok Build, and the current implementation covers test-driven development, systematic debugging, collaboration patterns, and repeatable engineering processes.

I personally use this before switching to Matt Pocock's skills.

You can install it with:

```bash
grok plugin install superpowers --trust
```

> Note: Review the plugin before using `--trust`, since that option skips the interactive trust prompt.

---

### Exa

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_14.png)

Exa provides fast, high-quality agent-oriented search, better than Grok Search itself. It works particularly well as a complement to Firecrawl.

Use Exa when you need quick and accurate retrieval, and switch to Firecrawl when you need full-page scraping or site crawling.

You can install Exa using:

```plain text
/plugin
```

Then search for `exa` and press `i`to install it.

---

### Vercel

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_15.png)

Vercel is for those who want their agents to deploy their project on their behalf on the Vercel platform. It gives Grok direct control over deployments, environment variables, build logs, and domains.

It also keeps Grok aware of current Vercel features, which reduces outdated suggestions.

You can install Vercel using:

```plain text
/plugin
```

Then search for `vercel` and install it using `i` .

---

### Cloudflare

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_16.png)

Cloudflare is for those scenarios when part of your stack lives on the other side of the world. It provides skills for Workers, Durable Objects, and related Cloudflare tooling.

This makes scaffolding and deploying edge projects noticeably smoother inside Grok Build.

You can install Cloudflare using:

```plain text
/plugin
```

Then search for `cloudflare` and install it.

---

### Chrome DevTools

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_17.png)

Chrome DevTools is useful for frontend debugging and performance work. It lets Grok control a live browser session. (not on WSL)

You can record performance traces, inspect network requests, evaluate JavaScript, and take DOM snapshots without leaving the agent workflow.

You can install Chrome DevTools using:

```plain text
/plugin
```

Then search for `chrome-devtools` and install it.

---

### Sentry

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_18.png)

Sentry closes the loop between local development and production. It lets Grok pull real error data and stack traces. Combined with Seer-powered analysis, it helps turn production issues into concrete fixes faster.

You can install Sentry using:

```plain text
/plugin
```

Then search for `sentry` and install it. Part of the official Grok Build Marketplace.

---

### Unity MCP + CLI

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/top-grok-skills-and-plugins/image_19.png)

If you are like me and like to build games in Unity and want Grok Build to actually drive the editor and project instead of just writing C# in isolation.

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

- Cross-app workflows: Composio MCP
- Engineering discipline → Matt Pocock Skills + Superpowers
- Token control → Caveman
- X advantage → what happened or XActions
- Web power → Firecrawl + Exa
- Game development → GodotPrompter or Unity MCP setup

Then add the platform plugins that match the work you actually do (Vercel, Cloudflare, Sentry, etc.).

Use both skills and plugins, but do it with a clear understanding of the use case. Then only Grok Build starts feeling like a real system instead of just another coding agent.

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
