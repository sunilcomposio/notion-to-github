---
title: "Hermes Skills"
source: https://www.notion.so/composio/Hermes-Skills-35ff261a6dfe80e09b64d51fdfb00f79
notion_page_id: 35ff261a-6dfe-80e0-9b64-d51fdfb00f79
last_synced: 2026-05-13T13:06:19Z
---

Hermes agents are running riot on the internet. It's the only agent with a built-in learning loop: it creates skills from experience, improves them during use, searches its own past conversations, and builds a deepening model of who you are across sessions.

I've been running Hermes for most of my growth and marketing work for the past 15 days. Before that, I was on OpenClaw. The reason I switched comes down to one thing: predictability. Hermes just works. OpenClaw gave me some genuinely painful experiences in its heyday, and I got tired of fighting it.

It's a fast-growing project, close to 150k GitHub stars and a seriously vibrant community. And like Claude Code or OpenClaw, you can augment Hermes with skills to handle more work with less friction.

A few skills in particular have noticeably improved how Hermes runs for me. These ten are the survivors, the ones I'd reinstall on a fresh Hermes setup before I did anything else. Most came up repeatedly across r/ClaudeAI, the Hermes Discord, and the various `awesome-claude-skills` GitHub lists. A few I just personally lean on for marketing work and won't shut up about.

## **A few notes before we dive in:**

- **Format.** Hermes follows the [agentskills.io](https://agentskills.io/) open standard — the same `SKILL.md` format [Claude Code](https://your-domain.com/claude-skills), [Codex CLI](https://your-domain.com/codex-cli-skills), Cursor, [OpenCode](https://your-domain.com/opencode-skills), and friends use.
- **Install location.** Hermes loads skills from `~/.hermes/skills/` (per-user) or `./skills/` (per-project).
- **The skills below are model-invoked.** You don't call them. The agent decides when to load them based on the `description:` in each skill's frontmatter.

## TL;DR:

1. **Obra Superpowers:** Adds operating discipline to Hermes with brainstorming, TDD, systematic debugging, subagents, and verification gates.
2. **Composio Universal CLI + Skill:** Connects Hermes to Gmail, Sheets, Slack, CRMs, and 1,000+ SaaS tools without hand-rolling OAuth.
3. **Obsidian Skills:** Turns your Obsidian vault into a read-write knowledge base Hermes can search, update, and build on.
4. **Defuddle:** Cleans web pages into token-efficient reader-mode markdown before Hermes reads them.
5. **Taste-Skill:** Stops generic AI-looking UI by pushing Hermes toward stronger visual taste and more varied design.
6. **SEO + GEO Skills:** Gives Hermes a growth workflow for keyword research, content gaps, SEO optimization, and AI-engine visibility.
7. **Humanizer:** Rewrites AI-flavored copy so it sounds more natural, specific, and closer to your voice.
8. **Claudeception:** Helps Hermes turn hard-won solutions into reusable skills it can retrieve later.
9. **Playwright Skill:** Teaches Hermes production-grade Playwright testing patterns instead of brittle tutorial-style tests.
10. **Reflexion:** Adds a self-review loop so Hermes critiques and improves its own output before handing it over.

## Top Hermes Skills in 2026

## 1. Obra Superpowers: The Operating Discipline

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/hermes-skills/8b561dea3f.png)

If you install one skill pack, make it this. [`obra/superpowers`](https://github.com/obra/superpowers) by Jesse Vincent isn't really a skill. It's a complete software-development methodology bolted onto your agent. It overrides the default "rush to produce output" behavior that every LLM ships with.

What you actually get:

- **`brainstorming`**: Before any code, the agent teases out a real spec from the conversation, shows it back in chunks short enough to read, and waits for sign-off.
- **`test-driven-development`**: Strict RED-GREEN-REFACTOR. Deletes any code written before a failing test exists. Yes, really.
- **`systematic-debugging`**: Fires on any bug, test failure, or unexpected behavior *before* the agent proposes a fix. Kills the symptom-patching reflex.
- **`subagent-driven-development`**: Pairs beautifully with Hermes' native subagents. Dispatches a fresh agent per task with two-stage review.
- **`verification-before-completion`**: Forces fresh verification commands before the agent claims success. Has saved me from a dozen "all done!" moments that weren't.

```bash
npx skills add obra/superpowers --target ~/.hermes/skills

# Or clone directly
git clone <https://github.com/obra/superpowers.git> ~/.hermes/skills/superpowers
```
## **2. Composio Universal CLI + Skill: The Integration Layer**

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/hermes-skills/a056e1c255.png)

Hermes ships with around 40 built-in tools. The moment you want to [draft a Gmail](https://composio.dev/toolkits/gmail/framework/hermes-agent), [update a Google Sheet](https://composio.dev/toolkits/googlesheet/framework/hermes-agent), [post to Slack](https://composio.dev/toolkits/slack/framework/hermes-agent), or hit any of [1,000+ SaaS apps](https://composio.dev/toolkits/), you hit the wall.

[Composio](https://composio.dev/) is how you get past it without spending a day wiring OAuth.

You have two ways in, and you can either use the CLI (recommended) or the MCP server:

- **The CLI route:** Install the `composio` command, then the agent calls it like any other shell tool. Lighter on tokens and better for multi-step chaining.
- **The MCP route:** Point Hermes at `https://connect.composio.dev/mcp`, and Tool Router dynamically loads only the tools the task needs. No 200-tool context bloat.

These two essentially do the same thing but different approaches.

The [Composio skill](https://github.com/ComposioHQ/awesome-claude-skills) teaches the agent the right way to use Composio, tool discovery, authentication, using remote workbench. The CLI has everything the agent needs to execute tasks in your connected apps.

With these your Hermes agent will be able to interact with any apps you’d like

```bash
# Install the Composio CLI
curl -fsSL https://composio.dev/install | bash

# Authenticate one time
composio login
```
For the MCP path, drop this into your Hermes config at `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  composio:
    url: "https://connect.composio.dev/mcp"
    headers:
      x-consumer-api-key: "YOUR_COMPOSIO_API_KEY"
    connect_timeout: 60
    timeout: 180
```
**Resources:**

- Awesome skills catalog: [github.com/ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills)
- Composio docs: [docs.composio.dev](https://docs.composio.dev/)
- Get an API key: [dashboard.composio.dev](https://dashboard.composio.dev/)
- Browse toolkits: [composio.dev/toolkits](https://composio.dev/toolkits)

## **3. **Obsidian Skills — your second brain becomes the agent's first brain

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/hermes-skills/a45d454da5.png)

If you do any knowledge work, this one is non-negotiable. [`kepano/obsidian-skills`](https://github.com/kepano/obsidian-skills) — written by Steph Ango (Obsidian's CEO) — gives your agent full read-write access to your vault.

What's in the pack:

- **`obsidian-cli`** — read, create, search, append, manage tasks and properties from the command line.
- **`obsidian-markdown`** — teaches the agent Obsidian Flavored Markdown.
- **`json-canvas`** — work with `.canvas` files for visual maps.
- **`obsidian-bases`** — database-like views over your notes.
- **`defuddle`** — extract clean markdown from web pages.

The unlock is bidirectional. Hermes can pull context from your notes ("what did I write about that campaign last month?") *and* push new findings back. It stops being a chatbot you copy-paste in and out of.

If you want to push your notes to other tools — auto-create [Notion](https://composio.dev/toolkits/notion/framework/hermes-agent) pages from meeting notes, or save research to [Google Drive](https://composio.dev/toolkits/googledrive/framework/hermes-agent) — wire those in through Composio (#2).

bash

```bash
git clone https://github.com/kepano/obsidian-skills.git ~/.hermes/skills/obsidian-skills
```
**Resources:**

- Repo: [github.com/kepano/obsidian-skills](https://github.com/kepano/obsidian-skills)
- Obsidian CLI docs: [help.obsidian.md/cli](https://help.obsidian.md/cli)
- Steph Ango's blog: [stephango.com](https://stephango.com/)
- Community discussion in r/ObsidianMD: [reddit.com/r/ObsidianMD](https://reddit.com/r/ObsidianMD)

---

### 4. Defuddle — token-efficient web reading

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/hermes-skills/04e84d160a.png)

Defuddle ships as part of the Obsidian pack but earns its own slot. Every web page Hermes fetches is 90% navigation, ads, related-content sidebars, and footer junk. You pay tokens for all of it.

Defuddle runs the page through a reader-mode extractor before the agent ever sees the content. Same information, often 5-10x fewer tokens. For research-heavy days this is a permanent context-window win.

bash

```bash
npm install -g defuddle-cli
```
**Resources:**

- Repo: [github.com/kepano/defuddle](https://github.com/kepano/defuddle)
- npm package: [npmjs.com/package/defuddle-cli](https://npmjs.com/package/defuddle-cli)
- Compare against Readability.js: [github.com/mozilla/readability](https://github.com/mozilla/readability)

---

### 5. Taste-Skill — stop the generic UI slop

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/hermes-skills/a6e299c44b.png)

Every LLM has the same defaults: Inter font, purple gradient, three-column hero, "Get started" button, soft shadow on every card. It works, but it screams "AI made this."

[`Leonxlnx/taste-skill`](https://github.com/Leonxlnx/taste-skill) is built specifically to override those defaults. It's a senior-UI/UX-engineer skill with three tunable knobs:

- **`DESIGN_VARIANCE`** (1–10) — Perfect symmetry → artsy chaos
- **`MOTION_INTENSITY`** (1–10) — Static → cinematic
- **`VISUAL_DENSITY`** (1–10) — Art gallery airy → pilot cockpit packed

The pack also ships variants: `gpt-taste`, `soft-skill`, `minimalist-skill`, `brutalist-skill`, and `image-to-code-skill` for a "generate → analyze → code" pipeline. For landing pages, this is the difference between something you'd ship and something you'd start over on.

```bash
npx skills add https://github.com/Leonxlnx/taste-skill --target ~/.hermes/skills
```
**Resources:**

- Repo: [github.com/Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill)
- Author's other work: [github.com/Leonxlnx](https://github.com/Leonxlnx)
- Companion redesign skill: [github.com/Leonxlnx/taste-skill/tree/main/skills/redesign-skill](https://github.com/Leonxlnx/taste-skill/tree/main/skills/redesign-skill)

---

### 6. SEO + GEO Skills — 20 skills made for growth work

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/hermes-skills/131d05e20b.png)

This is the closest thing to "marketing in a box" for agent skills. [`aaron-he-zhu/seo-geo-claude-skills`](https://github.com/aaron-he-zhu/seo-geo-claude-skills) ships 20 skills covering the whole SEO/GEO lifecycle:

- **Research** — keyword research, competitor analysis, content-gap analysis
- **Build** — SEO content writer, GEO content optimizer, meta-tag optimizer, schema generator
- **Optimize** — on-page auditor, technical SEO checker, content-quality auditor
- **Monitor** — rank tracking, backlink analyzer, performance reporter

The GEO half ("Generative Engine Optimization" — getting cited by ChatGPT, Perplexity, AI Overviews instead of just ranking on Google) is the genuinely novel bit. There's a `/seo:geo-drift-check` command that validates whether your GEO score actually matches AI-engine citations.

Zero dependencies. Hook up [Google Search Console](https://composio.dev/toolkits/googlesearchconsole/framework/hermes-agent) or an SEO tool like [Ahrefs](https://composio.dev/toolkits/ahrefs/framework/hermes-agent) through Composio (#2) and it gets much more powerful.

```bash
npx skills add aaron-he-zhu/seo-geo-claude-skills --target ~/.hermes/skills
```
**Resources:**

- Repo: [github.com/aaron-he-zhu/seo-geo-claude-skills](https://github.com/aaron-he-zhu/seo-geo-claude-skills)
- CORE-EEAT framework explainer: in the repo's `/docs` folder
- CITE Domain Rating framework: in the repo's `/docs` folder
- GEO research reference: [Princeton GEO paper](https://arxiv.org/abs/2311.09735)
- Google Search Console: [search.google.com/search-console](https://search.google.com/search-console)

---

### 7. Humanizer — remove the AI tells from your writing

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/hermes-skills/0e6beefdc7.png)

Every piece of agent-generated copy has the same problem: it reads like agent-generated copy. The em-dashes. The "It's not just X, it's Y." The lists of three. The signposting headers. Readers feel it before they can name it.

[`blader/humanizer`](https://github.com/blader/humanizer) is a skill engineered specifically to detect and rewrite those patterns. You can paste a sample of your own writing first and it'll voice-match to your sentence rhythm, word choices, and quirks. Or just run it raw and get text that doesn't scream "ChatGPT did this."

bash

`git clone https://github.com/blader/humanizer.git ~/.hermes/skills/humanizer`

For me, this is the skill I run on every blog draft and every cold email before it leaves the building.

**Resources:**

- Repo: [github.com/blader/humanizer](https://github.com/blader/humanizer)
- Siqi Chen's other skills: [github.com/blader](https://github.com/blader)
- Background on AI-writing detection: [arxiv.org/abs/2303.07205](https://arxiv.org/abs/2303.07205)
- Companion: [github.com/blader/Claudeception](https://github.com/blader/Claudeception) (see #8)

---

### 8. Claudeception — Hermes learns from itself

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/hermes-skills/0166f85fbc.png)

[`blader/Claudeception`](https://github.com/blader/Claudeception) is the skill that makes Hermes' built-in learning loop noticeably sharper. When the agent solves a non-obvious problem — a weird debugging session, a workaround that took trial-and-error to find, a project-specific pattern it figured out — it writes that knowledge back as a new skill *with a trigger description optimized for future retrieval*.

The agent doesn't just save the solution; it saves it under the exact phrasing that'll surface it the next time the same kind of problem comes up. The idea comes from academic work on skill libraries for AI agents (Voyager, Wang et al., 2023).

It pairs really well with Hermes' native memory loop. The native loop captures *what happened*; Claudeception captures *what worked, why, and how to retrieve it later*.

```bash
git clone https://github.com/blader/Claudeception.git ~/.hermes/skills/claudeception
mkdir -p ~/.hermes/hooks
cp ~/.hermes/skills/claudeception/scripts/claudeception-activator.sh ~/.hermes/hooks/
chmod +x ~/.hermes/hooks/claudeception-activator.sh
```
**Resources:**

- Repo: [github.com/blader/Claudeception](https://github.com/blader/Claudeception)
- Original continuous-learning version: [github.com/blader/claude-code-continuous-learning-skill](https://github.com/blader/claude-code-continuous-learning-skill)
- Voyager paper (the academic basis): [arxiv.org/abs/2305.16291](https://arxiv.org/abs/2305.16291)
- Skill-library theory primer: [voyager.minedojo.org](https://voyager.minedojo.org/)

---

### 9. Playwright Skill — 70+ production-tested testing patterns

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/hermes-skills/edb082b826.png)

[`testdino-hq/playwright-skill`](https://github.com/testdino-hq/playwright-skill) is the most thorough Playwright pack I've seen. It covers 70+ patterns including E2E flows, Page Object Model, CI/CD integration, test migrations, and CLI usage.

The reason this earns a slot: it actually teaches the agent the production patterns that don't break six months later. If Hermes is writing tests for your app, you want it writing them like a senior QA engineer wrote them, not like a tutorial copy-paste. Wire results into [Linear](https://composio.dev/toolkits/linear/framework/hermes-agent) or [GitHub Issues](https://composio.dev/toolkits/github/framework/hermes-agent) through Composio and the loop closes — tests fail, tickets file themselves.

```bash
npx skills add testdino-hq/playwright-skill --target ~/.hermes/skills
npm install -g playwright
playwright install chromium
```
**Resources:**

- Repo: [github.com/testdino-hq/playwright-skill](https://github.com/testdino-hq/playwright-skill)
- Playwright docs: [playwright.dev](https://playwright.dev/)
- POM pattern reference: [playwright.dev/docs/pom](https://playwright.dev/docs/pom)
- TestDino blog (case studies): [testdino.com/blog](https://testdino.com/blog)

---

### 10. Reflexion — self-refinement loop

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/hermes-skills/b3ff2064a3.png)

The last slot was a toss-up; this won because it compounds. [`NeoLabHQ/reflexion`](https://github.com/NeoLabHQ/reflexion) implements a self-refinement loop: the agent reflects on its previous output, identifies what's weak or wrong, and corrects itself before showing you the result.

It sounds like it'd be slow. It is, slightly. But the output quality jump on anything substantive — code, copy, analysis — is real. For one-shot trivia it's overkill; for anything you'd actually ship, it's the difference between "first draft" and "second draft" without you having to ask for the second draft.

bash

```bash
npx skills add NeoLabHQ/reflexion --target ~/.hermes/skills
```
Pair it with `verification-before-completion` from Superpowers (#1) and you get a double-tap quality gate: the agent reflects on its output, fixes the obvious problems, then runs real verification before claiming done.

**Resources:**

- Repo: [github.com/NeoLabHQ/reflexion](https://github.com/NeoLabHQ/reflexion)
- Original Reflexion paper (Shinn et al., 2023): [arxiv.org/abs/2303.11366](https://arxiv.org/abs/2303.11366)
- NeoLabHQ's sibling skills (SDD, DDD, code-review): [github.com/NeoLabHQ](https://github.com/NeoLabHQ)
- Self-Refine paper (related approach): [arxiv.org/abs/2303.17651](https://arxiv.org/abs/2303.17651)

---

---

### A few honest caveats before I wrap

- **Don't install everything blind.** Skills shape the agent's default behavior. The more you stack, the more decisions get made for you. Run with the top 3 (Superpowers, Composio, Obsidian) for a week before adding the rest.
- **The descriptions matter more than the bodies.** When a skill doesn't fire when you expect it to, the fix is almost always in the frontmatter `description:`, not the instructions below.
- **Vet anything you grab from a random repo.** Skills run real code in your agent's environment. The community has been good about flagging bad actors, but the bar to publish is low. Read the SKILL.md before you install.

That's the stack. If you find something that earns its slot ahead of any of these, I want to hear about it.

---
