---
title: "OpenCode Skills"
source: https://www.notion.so/composio/OpenCode-Skills-35df261a6dfe804b8e65f09a2c9f6798
notion_page_id: 35df261a-6dfe-804b-8e65-f09a2c9f6798
last_synced: 2026-05-11T13:38:38Z
---

Skills are perhaps the simplest yet most effective layer when it comes to usefulness. We’ve had MCPs, plugins, and a dozen other markdown-file-driven approaches, but in terms of pure ROI, Skills are the best.

They work because they meet the model where it already is. Instead of forcing a new interface, a new protocol, or a heavy abstraction, Skills give the model a compact, reusable set of instructions it can pull from at the right moment. That sounds almost too basic, but that is exactly why it works.

The underrated part is that Skills are also easy to reason about. A team can open one, read it, edit it, and immediately understand what behavior it is supposed to produce. That makes them much easier to maintain than systems that hide logic behind APIs, chains, or orchestration layers.

I switch between coding harnesses, at work it’s Claude Code and for personal stuff it’s always OpenCode. 

And over time, I’ve ended up using a bunch of Skills inside OpenCode for everything from routine coding tasks to oddly specific workflows. [Skills for ClaudeCode](https://composio.dev/content/top-claude-skills) is same as OpenCode these are just markdown files anyway.

So I collated some of the best OpenCode Skills I’ve found. Some are genuinely useful, some are surprisingly clever, and a few are just plain esoteric (you can ignore them).

Also, to pair with some of there skills I use [Composio CLI.](https://dashboard.composio.dev) A very minimalist CLI that let’s your agent authenticate with external apps like [Figma](https://composio.dev/toolkits/figma/framework/opencode), [Linear ](https://composio.dev/toolkits/linear/framework/opencode)etc for a seamless workflow.

## Summary

1. Vercel React Best Practices: React and Next.js performance rules from Vercel Engineering.
2. Composio Skills: Give OpenCode one interface for SaaS tools, auth, CLI actions, and MCP.
3. Anthropic Skill Creator: Create, test, benchmark, and improve your own SKILL.md packages.
4. Anthropic Frontend Design: Build less generic frontend UI with stronger visual direction.
5. Anthropic MCP Builder: Plan, implement, test, and evaluate MCP servers in TypeScript or Python.
6. Cloudflare Skills: Use Workers, D1, R2, Vectorize, Workers AI, Wrangler, and Agents SDK.
7. Replicate Skills: Find, compare, run, and prompt AI models on Replicate.
8. Hugging Face hf-cli: Use the modern Hugging Face Hub CLI for models, datasets, Spaces, and jobs.
9. Anthropic Webapp Testing: Test local web apps with Playwright scripts and server lifecycle helpers.

## What is an OpenCode skill?

An Agent Skill is a folder with a SKILL.md file that OpenCode or any agent can discover and load through its native skill tool. The file uses YAML frontmatter for fields like name and description, then Markdown instructions that tell the agent how to perform a repeatable task.

OpenCode looks for skills in project and global locations, including .opencode/skills, \~/.config/opencode/skills, .claude/skills, and .agents/skills. The agent initially sees the skill names and descriptions, then loads full instructions only when a task matches or you ask for a skill directly.

## Best OpenCode skills

### 1. [Obra Superpowers](https://github.com/obra/superpowers): A complete agentic software development methodology with composable skills

Superpowers turns OpenCode from a fast code generator into a disciplined engineer. It depends on a bootstrap plugin that injects a "1% rule" into every session — if there's even a slim chance a skill applies, the agent must invoke it. It stands out because it ships a full methodology (brainstorm → plan → TDD → subagent execution → review) rather than a single narrow skill, and the skills auto-trigger from the moment you start a session.

- Activates `brainstorming` before any creative work to extract a real spec
- Enforces red/green/refactor TDD with the `test-driven-development` skill
- Breaks features into 2–5 minute tasks via `writing-plans`
- Runs `subagent-driven-development` with two-stage code review
- Includes `systematic-debugging`, `using-git-worktrees`, and `verification-before-completion`
- Skills auto-update on session start via a separate `superpowers-skills` repo

**install the plugin in OpenCode**

Add to your `opencode.json`:

json

```bash
{ "plugin": ["superpowers@git+https://github.com/obra/superpowers.git"] }
```
**restart OpenCode and verify**

```bash
opencode
# prompt: Tell me about your superpowers
```
**invoke a skill directly**

```bash
opencode
# prompt: Use the brainstorming skill to help me design a new auth flow.`
```
[Visit](https://github.com/obra/superpowers) · [repository](https://github.com/obra/superpowers) · [OpenCode install guide](https://github.com/obra/superpowers/blob/main/docs/README.opencode.md) · [skills repository](https://github.com/obra/superpowers-skills) · [marketplace](https://github.com/obra/superpowers-marketplace)

### 2. [Vercel React Best Practices](https://github.com/vercel-labs/agent-skills/tree/main/skills/react-best-practices): React and Next.js performance rules from Vercel Engineering

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/opencode-skills/ba6ac58e15.png)

This skill gives OpenCode a concrete rulebook for React and Next.js performance work. It depends on the Agent Skills format and Vercel’s compiled rule files. It stands out because the rules are prioritized by impact, not grouped as a loose style guide.

- Flags fetch waterfalls and late awaits
- Guides bundle-size refactors
- Checks RSC and SSR serialization patterns
- Covers memoization and re-render rules
- Points to per-rule files with examples

**install the skill**

```shell
npx skills add https://github.com/vercel-labs/agent-skills --skill react-best-practices
```
**restart OpenCode and invoke it**

```shell
opencode
# prompt: Use the vercel-react-best-practices skill to review this Next.js page.
```
[Visit](https://github.com/vercel-labs/agent-skills/tree/main/skills/react-best-practices) · [repository](https://github.com/vercel-labs/agent-skills) · [skills directory](https://github.com/vercel-labs/agent-skills/tree/main/skills)

### 3. [Composio Skills](https://github.com/ComposioHQ/skills/tree/main/skills/composio): Use SaaS tools, auth, CLI actions, and MCP from OpenCode

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/opencode-skills/9795677d98.png)

If I had to pick one OpenCode skill to install first, this is it. Not because I work adjacent to it, but because every other OpenCode skill on this list does one thing. Composio is the connective tissue between them.<br><br>OpenCode is strong at editing code and following repo-local instructions. What it doesn't do natively is talk to the 1000+ SaaS apps you actually use via MCP and CLI: GitHub issues, Linear tickets, Slack threads, Stripe customers, Salesforce records, Jira sprints, and Notion pages. Composio gives your agent a single interface to all of them. No separate auth flows, no token juggling, no rewriting integrations every time an API changes.

- Installs a composio SKILL.md for OpenCode-compatible agents
- Uses composio search, link, execute, and listen CLI commands
- Supports SDK setup with composio init
- Documents CLI and agent-building workflows
- Covers MCP, Tool Router, triggers, and automation tags

**install the Composio skill**

```shell
npx skills add composiohq/skills
```
**install and authenticate the Composio CLI**

```shell
curl -fsSL https://composio.dev/install | bash
composio login
composio whoami
```
**initialize Composio in an app project**

```shell
composio init
```
[Visit](https://github.com/ComposioHQ/skills/tree/main/skills/composio) · [repository](https://github.com/ComposioHQ/skills) · [SKILL.md](https://github.com/ComposioHQ/skills/blob/main/skills/composio/SKILL.md) · [documentation](https://docs.composio.dev)

---

### 4. [Vault Daydream](https://github.com/glebis/claude-skills/tree/main/daydream): Mine your Obsidian vault for non-obvious connections between notes (esoteric pick)

This one's strange in the best way. Vault Daydream is a multi-agent skill that mimics the brain's default mode network — the thing that fires when you're zoning out and a random connection between two unrelated memories suddenly surfaces. It's based on Gwern's "LLM Daydreaming" essay, which proposed that LLMs don't have breakthrough insights partly because they lack background processing. This skill is one of the few working implementations of that idea you can actually point at your own notes.

- Samples random pairs of notes from your Obsidian vault (last 120 days by default)
- Dispatches Sonnet subagents as the *generator* to find non-obvious links between pairs
- Dispatches Haiku subagents as the *critic*, scoring on novelty, coherence, and usefulness
- Filters insights below a 7.0/10 average score before surfacing them
- Writes accepted daydreams back into your vault as new notes with backlinks
- Costs roughly \$0.40–0.50 per run (around 50 pairs) on Claude Code

**install the skill**

```bash
git clone https://github.com/glebis/claude-skills.git
cp -r claude-skills/daydream ~/.claude/skills/
# Then edit instructions.md to point VAULT_ROOT at your Obsidian vault
```
**restart your agent and invoke it**

```bash
claude
# prompt: /daydream
```
[Visit](https://github.com/glebis/claude-skills/tree/main/daydream) · [repository](https://github.com/glebis/claude-skills) · [Gwern's original essay](https://gwern.net/ai-daydreaming)

## 5. [Anthropic Skill Creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator): Create, test, benchmark, and improve [SKILL.md](http://SKILL.md) files

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/opencode-skills/d7126461a0.png)

Skill Creator is Anthropic’s own skill for making and improving other skills. It depends on test prompts, eval metadata, benchmark aggregation scripts, and a review viewer. It is most useful when you want OpenCode behavior to become a reusable package instead of a one-off prompt.

- Drafts SKILL.md frontmatter and instructions
- Creates realistic trigger and task evals
- Runs with-skill and baseline comparisons
- Aggregates pass rate, timing, and token data
- Optimizes skill descriptions for triggering

**install the skill**

```shell
npx skills add https://github.com/anthropics/skills --skill skill-creator
```
**restart OpenCode and invoke it**

```shell
opencode
# prompt: Use the skill-creator skill to turn our release workflow into a reusable skill.
```
[Visit](https://github.com/anthropics/skills/tree/main/skills/skill-creator) · [repository](https://github.com/anthropics/skills) · [SKILL.md](https://github.com/anthropics/skills/blob/main/skills/skill-creator/SKILL.md)

---

### 6. [Anthropic Frontend Design](https://github.com/anthropics/skills/tree/main/skills/frontend-design): Build production-grade UI with a stronger visual point of view

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/opencode-skills/d7126461a0.png)

Frontend Design gives OpenCode a design brief before it writes UI code. [Design skills](https://composio.dev/content/top-design-skills) ensures the AI agents adheres to specific guidelines and do not default to their purple gradient slop gen. It depends on the agent choosing an aesthetic direction and then implementing working HTML, CSS, JavaScript, React, or Vue. It is useful when your default agent output keeps producing generic dashboards and landing pages.

- Forces a named aesthetic direction before coding
- Covers typography, color, motion, and layout
- Discourages generic AI UI patterns
- Supports React, Vue, HTML, CSS, and JS
- Maps implementation complexity to design intent

**install the skill**

```shell
npx skills add https://github.com/anthropics/skills --skill frontend-design
```
**restart OpenCode and invoke it**

```shell
opencode
# prompt: Use frontend-design to redesign this onboarding page.
```
[Visit](https://github.com/anthropics/skills/tree/main/skills/frontend-design) · [repository](https://github.com/anthropics/skills) · [SKILL.md](https://github.com/anthropics/skills/blob/main/skills/frontend-design/SKILL.md)

---

### 7. [Anthropic MCP Builder](https://github.com/anthropics/skills/tree/main/skills/mcp-builder): Build MCP servers with protocol, SDK, and evaluation guidance

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/opencode-skills/d7126461a0.png)

MCP Builder guides OpenCode through the full lifecycle of a Model Context Protocol server. It depends on MCP protocol docs plus TypeScript SDK or Python FastMCP references. The differentiator is that it treats eval creation as part of the server build, not an afterthought. [MCP with OpenCode](https://composio.dev/content/mcp-with-opencode) is a genuinely powerful combination giving your agent access to gated external data sources.

The Plugin

- Balances API coverage with workflow tools
- Recommends TypeScript and streamable HTTP defaults
- Uses Zod or Pydantic schemas for tools
- Adds MCP Inspector testing steps
- Defines 10 read-only evaluation questions

**install the skill**

```shell
npx skills add https://github.com/anthropics/skills --skill mcp-builder
```
**restart OpenCode and invoke it**

```shell
opencode
# prompt: Use mcp-builder to create a TypeScript MCP server for our internal API.
```
[Visit](https://github.com/anthropics/skills/tree/main/skills/mcp-builder) · [repository](https://github.com/anthropics/skills) · [SKILL.md](https://github.com/anthropics/skills/blob/main/skills/mcp-builder/SKILL.md)

---

### 8. [Cloudflare Skills](https://github.com/cloudflare/skills/tree/main/skills/cloudflare): Build Workers, storage, AI, networking, security, and IaC on Cloudflare

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/opencode-skills/b062f3769b.png)

Cloudflare’s platform skill gives OpenCode a product map for Cloudflare development. It depends on Cloudflare docs retrieval because limits, config, pricing, and APIs change. It is useful when a task spans Workers, Pages, KV, D1, R2, Vectorize, Workers AI, Wrangler, and deployment choices.

- Routes tasks through Cloudflare product decision trees
- Covers Workers, Pages, D1, R2, KV, and Durable Objects
- Includes Workers AI, Vectorize, Agents SDK, and AI Gateway
- Points agents to current docs and changelogs
- Pairs with Cloudflare remote MCP servers

**install Cloudflare skills**

```shell
npx skills add https://github.com/cloudflare/skills
```
**restart OpenCode and invoke it**

```shell
opencode
# prompt: Use the cloudflare skill to design a Workers + D1 architecture for this API.
```
[Visit](https://github.com/cloudflare/skills/tree/main/skills/cloudflare) · [repository](https://github.com/cloudflare/skills) · [SKILL.md](https://github.com/cloudflare/skills/blob/main/skills/cloudflare/SKILL.md)

---

### 9. [stop-slop](https://github.com/hardikpandya/stop-slop): Strip AI tells out of docs, READMEs, and release notes (bonus pick)

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/opencode-skills/3ca7a809e0.png)

This one's a bonus. It's not a coding skill at all. stop-slop gives OpenCode a rulebook for cleaning up the obvious AI-written tone in prose, depending on a SKILL.md plus three reference files (phrases, structures, examples) that load on demand. It's worth installing if you use OpenCode for docs and writing as much as code.

- Removes filler phrases, throat-clearing openers, and em-dash reveals
- Breaks formulaic structures like binary contrasts and dramatic fragmentation
- Enforces active voice with a real human subject in every sentence
- Includes a 1–10 scoring rubric across five dimensions with a 35/50 revision threshold
- Pairs naturally with READMEs, blog posts, release notes, and commit messages

**install the skill**

bash

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/hardikpandya/stop-slop.git ~/.agents/skills/stop-slop
```
**restart OpenCode and invoke it**

```bash
opencode
# prompt: Use stop-slop to clean up the AI tone in this README draft.
```
[Visit](https://github.com/hardikpandya/stop-slop) · [repository](https://github.com/hardikpandya/stop-slop) · [SKILL.md](https://github.com/hardikpandya/stop-slop/blob/main/SKILL.md)

---

### 10. [Anthropic Webapp Testing](https://github.com/anthropics/skills/tree/main/skills/webapp-testing): Test local web apps with Playwright and server helpers

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/opencode-skills/d7126461a0.png)

Webapp Testing gives OpenCode a repeatable browser-testing workflow for local apps. It depends on Python Playwright scripts and a bundled server lifecycle helper. It stands out because it tells the agent to inspect the rendered page first, then act with selectors it discovered.

- Runs native Python Playwright scripts
- Starts one or more local servers with with_server.py
- Waits for networkidle before DOM inspection
- Captures screenshots and browser logs
- Uses a reconnaissance-then-action workflow

**install the skill**

```shell
npx skills add https://github.com/anthropics/skills --skill webapp-testing
```
**restart OpenCode and invoke it**

```shell
opencode
# prompt: Use webapp-testing to verify the checkout form at http://localhost:5173.
```
[Visit](https://github.com/anthropics/skills/tree/main/skills/webapp-testing) · [repository](https://github.com/anthropics/skills) · [SKILL.md](https://github.com/anthropics/skills/blob/main/skills/webapp-testing/SKILL.md)

---

## End Note

Install the narrow skills that match your repo, then use Composio as the integration layer when OpenCode needs to leave the filesystem and act across GitHub, Slack, Linear, Jira, Notion, Salesforce, or other SaaS tools.

**Try Composio →**[https://dashboard.composio.dev](https://dashbaord.composio.dev)

*End note: Treat skills like small, versioned playbooks. Install only what you will use, and keep them updated as your workflows evolve.*
