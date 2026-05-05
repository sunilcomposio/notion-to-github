---
title: "Top 10 Design Skills for Claude Code and Codex"
source: https://www.notion.so/composio/top-design-skills-357f261a6dfe8044afd2e3ae99759268
notion_page_id: 357f261a-6dfe-8044-afd2-e3ae99759268
last_synced: 2026-05-05T17:44:12Z
---

Ask Claude Code or Codex to build a landing page and you get the same thing every time. Inter font. Purple gradient on white. Four cards in a grid. A faint hover state if you're lucky. Anthropic calls it distributive convergence. OpenAI's frontend team calls it high-frequency patterns from training data. The result is the same: every prompt produces the median, and the median is exhausted.

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/top-design-skills/22ca518fa1.jpg)

Skills are the workaround. They're SKILL.md files that ship with both Claude Code and Codex, loaded into context when relevant, and they override the model's default aesthetic instincts with explicit constraints. Some are aesthetic presets that fall apart the moment your project drifts from their assumed shape. Some encode the discipline that experienced designers carry in their heads. The good ones compound across a thousand prompts. The bad ones are decoration.

Below are ten skills worth installing across both agents, scored 1 to 10 from a senior designer's perspective. Senior designers care about constraints, feedback loops, and process discipline more than they care about how pretty the output looks. A 10 means the skill changes what gets produced even when nobody's prompting it carefully. A 7 means it works but reads as a band-aid.

---

## 1. [frontend-design (anthropic) and frontend-skill (openai)](https://github.com/anthropics/skills/tree/main/skills/frontend-design)

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/top-design-skills/d7126461a0.png)

the canonical starting point on both sides. anthropic's version has 277,000+ installs as of march 2026 and explicitly bans Inter, Roboto, Arial, and Space Grotesk. [openai's parallel skill](https://github.com/openai/skills/tree/main/skills/.curated/frontend-skill) for codex was published alongside gpt-5.4 with similar guardrails: no generic SaaS card grid as the first impression, no carousel without narrative purpose, no app UI made of stacked cards instead of a real layout.

### **what it does**

- Bans overused fonts (Inter, Roboto, Arial, Space Grotesk) before any code is written
- Forces the model to commit to an explicit aesthetic direction up front — brutalist, editorial, retro-futuristic, maximalist
- Codifies failure modes to reject: carousels without narrative, generic SaaS card grids, app UIs built from stacked cards
- Provides litmus checks — single visual anchor per screen, scannable headlines, motion only where it adds hierarchy

<callout icon="💡">
	**NB:** banning fonts is a band-aid for a training-data problem, not a design philosophy. seniors know taste comes from understanding constraints, not from a blocklist. still the right starting point because it's the floor everyone agrees on.
</callout>
install: `claude plugin add anthropic/frontend-design` / `frontend-skill` inside codex.

## 2. [impeccable](https://impeccable.style/) 

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/top-design-skills/7e36ef6e10.png)

Most design skills, including Anthropic's own frontend-design skill, collapse everything into one vocabulary, but a landing page and a dashboard live by contradictory rules. impeccable runs in two modes (brand and product) with commands like `typeset`, `colorize`, `bolder`, `quieter` that adjust based on which one you're in. Works with Cursor, Claude Code, Gemini CLI, and Codex CLI.

### **what it does**

- Runs in two distinct modes: brand (marketing, portfolios, editorial) and product (app UI, dashboards, internal tools)
- Bundles 23 commands in one skill: `typeset`, `colorize`, `animate`, `layout`, `bolder`, `quieter`, `delight`
- Ships with 27 deterministic anti-pattern detection rules across typography, color, layout, and motion
- Works across Claude Code, Cursor, Codex CLI, and Gemini CLI without separate configs

<callout icon="💡">
	**NB:** the brand-vs-product split is exactly how seniors actually think. a marketing site and an internal admin tool need opposite things, and most skills pretend they don't. one of the few that respects the distinction.
</callout>
install: `npx skills add pbakaus/impeccable`.

## 3. [figma implement-design](https://github.com/openai/skills/tree/main/skills/.curated/figma-implement-design) 

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/top-design-skills/00769c157f.png)

for when the figma file already exists and you want faithful translation instead of riffing. on codex it ships as `figma-implement-design`. on claude code there's a parallel public figma skill that gives Claude a more structured way to move from design to implementation so the gap between what was designed and what gets built stays as small as possible.

### **what it does**

- Translates Figma files into production code that respects your existing design system
- Handles multiple states per design (hover, selected, empty, loading) without separate prompts
- Pairs natively with Playwright to verify visual fidelity across viewport sizes
- Reduces the gap between what was designed and what gets shipped

<callout icon="💡">
	**NB:** the gap between design intent and shipped UI is where junior teams lose all their craft. anything that closes it is high value. pair with playwright for verification.
</callout>
## 4. [playwright / webapp-testing](https://github.com/ComposioHQ/awesome-codex-skills/tree/master/webapp-testing)

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/top-design-skills/bc7b191c57.png)

with playwright wired in, Codex can open the app in a real browser, compare the implementation to your screenshots for different screen sizes, and iterate on layout or behavior until the result is closer to the target. claude code gets the same loop through MCP, where Claude Code opens a headless browser, navigates through the application, takes screenshots of every page, and then runs the design review skill against those screenshots autonomously.

### **what it does**

- Opens the app in a real headless browser and screenshots every page automatically
- Compares implementation against reference screenshots across multiple viewports
- Iterates on layout and behavior until visuals match the target without manual review
- Verifies state transitions, navigation flows, and responsive breakpoints

<callout icon="💡">
	**NB:** the visual feedback loop is the single highest-leverage thing you can give an agent doing design work. without it, you're trusting a blind model. with it, the model can grade itself. nothing else in this list comes close.
</callout>
## 5. [designer skills collection (owl-listener)](https://github.com/Owl-Listener/designer-skills) 

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/top-design-skills/92fb59f5a9.png)

63 design skills and 27 commands organised into 8 plugins covering research, systems, strategy, UI, interaction design, prototyping and testing, design ops, and the everyday toolkit stuff like writing rationale and building case studies. Commands chain skills together so /discover runs a full research discovery cycle, /strategize builds a UX strategy from vision through to metrics, and /handoff generates a developer handoff package with measurements, behaviours, edge cases, and a QA checklist.

### **what it does**

- 63 skills across 8 plugins: research, systems, strategy, UI, interaction, prototyping, design ops, toolkit
- 27 commands chain skills into full workflows — `/discover`, `/strategize`, `/handoff`
- Generates dev handoff packages with measurements, behaviors, edge cases, and QA checklists
- Encodes scaffolding for writing design rationale and building case studies

<callout icon="💡">
	**NB:** strong on process, weaker on taste. the value is in the unglamorous parts (handoff specs, rationale writing, QA checklists) that juniors skip and seniors live by.
</callout>
install: `/plugin marketplace add Owl-Listener/designer-skills`.

## 6. [theme-factory (composio's awesome-codex-skills)](https://github.com/ComposioHQ/awesome-codex-skills/tree/master/theme-factory)

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/top-design-skills/bc7b191c57.png)

Creates reusable theme tokens and palettes. cleanest way to stop the agent from inventing one-off color values every component.

### **what it does**

- Generates reusable theme tokens — colors, spacing scales, typography ramps
- Outputs CSS variable systems with consistent naming conventions
- Produces palette variations (light, dark, high-contrast) from a single source
- Stops the agent from inventing one-off hex values for every new component

<callout icon="💡">
	**NB:** theme tokens are foundational. a system without them is just a mood board. surprised this skill isn't more widely installed.
</callout>
## 7. [design process pack (julian oczkowski)](https://medium.com/@julian.oczkowski/7-claude-code-design-skills-that-follow-a-real-design-process-b871b8673d05)

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/top-design-skills/5756d06f7a.png)

7 skills that follow a professional design workflow from vague idea to working, accessible, reviewed frontend. requirements gathering → design brief → information architecture → design tokens → task decomposition → frontend generation → review. the requirements skill is the unglamorous hero: the LLM becomes relentless and stress-tests your requirements before a single line of code gets written, and you might spend 20 minutes answering questions, which is the point.

### **what it does**

- 7 skills covering the full flow: requirements gathering, design brief, IA, tokens, task decomposition, generation, review
- Requirements skill stress-tests your input with 20+ minutes of questions before any code is generated
- Design review skill runs autonomously when paired with Playwright MCP
- Output mirrors a real professional design workflow rather than a vibe-coded prompt loop

<callout icon="💡">
	**NB:** the requirements-first ordering is the whole game. juniors prompt-first, seniors spec-first. this skill enforces the discipline that experienced designers carry in their heads.
</callout>
## 8. [composio connect](https://composio.dev/) 

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/top-design-skills/8f21dd2851.png)

Connects Codex to apps like GitHub, Slack, and Notion. Lets it work beyond your local repo and across the apps you already use. Useful when your work lives across tools, not just inside the repo. for designers this means the agent can pull assets and specs from figma, sync tokens to linear, post review screenshots to slack.

### **what it does**

- Connects Codex and Claude Code to 1000+ apps via the Composio CLI — GitHub, Slack, Notion, Figma, Linear, and more
- Pulls Figma assets and design specs directly into the agent's context
- Syncs design tokens across Linear, Notion, and your repo without manual copy-paste
- Posts review screenshots and progress updates to Slack channels automatically

<callout icon="💡">
	**senior take:** not a craft skill, but design rarely lives in one tool. seniors spend half their time stitching across figma, notion, linear, and slack. anything that automates that stitching frees up time for the actual thinking.
</callout>
## 9. [excalidraw diagram skill](https://github.com/coleam00/excalidraw-diagram-skill) 

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/top-design-skills/6b9bd427b9.png)

most agent-generated diagrams are placeholder garbage. this one isn't. The Excalidraw skill generates a diagram you can actually share, not a placeholder you'd redraw yourself.

### **what it does**

- Generates Excalidraw-format diagrams that open natively in Excalidraw, not just screenshots
- Supports flowcharts, system architecture, sequence diagrams, and ER diagrams
- Produces `.excalidraw` files you can share, edit, and version-control
- Works across Claude Code and Codex via the standard SKILL.md format

<callout icon="💡">
	**senior take:** narrow scope, high quality output. a senior would rather have one skill that produces shareable diagrams than ten that produce pretty mermaid charts no one reads.
</callout>
install: `npx skills add https://github.com/coleam00/excalidraw-diagram-skill --skill excalidraw-diagram`.

## 10. [accesslint](https://github.com/accesslint/claude-marketplace) 

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/top-design-skills/043a69cff6.png)

High skill quality, focused scope. Useful for any UI/UX engineer who takes accessibility seriously, design system teams that need to enforce WCAG compliance, and developers preparing for accessibility audits. handles contrast checks, semantic HTML review, color-only-signal detection.

### **what it does**

- Color contrast checks against WCAG AA and AAA thresholds
- Semantic HTML review — heading hierarchy, ARIA usage, form label associations
- Detects color-as-only-signal patterns in status indicators, links, and error states
- Flags accessibility regressions before shipping instead of after a failed audit

<callout icon="💡">
	**senior take:** the unsexy stuff that separates seniors from juniors. nobody installs this until they fail an audit, and by then they've shipped a dozen accessibility regressions. install it before you need it.
</callout>
install: `git clone https://github.com/accesslint/claude-marketplace.git && cp -r claude-marketplace/plugins/accesslint/skills/* ~/.claude/skills/`.

---

**summary scoreboard for skim readers:** playwright/webapp-testing 10, impeccable 9, accesslint 9, oczkowski's pack 9, theme-factory 8, designer skills collection 8, figma implement-design 8, frontend-design 7, composio connect 7, excalidraw 7.


---

## FAQs

### Why does every AI-generated landing page look the same?

It's a training-data problem the article calls "distributive convergence" (Anthropic's term) or "high-frequency patterns from training data" (OpenAI's). Models default to the median of what they've seen — Inter font, purple-on-white gradients, four-card grids, faint hover states — because that's what dominates their training corpus. Skills work around this by injecting explicit constraints into context that override those defaults: banning specific fonts, forcing a committed aesthetic direction, or rejecting failure modes like generic SaaS card grids before any code is written.

### Which design skill should I install first if I only pick one?

**Playwright / webapp-testing** — it scored 10/10 in the article and the reasoning is simple: the visual feedback loop is the single highest-leverage thing you can give an agent doing design work. Without it, the model is shipping blind and you're doing review by hand. With it, the model opens the app in a real browser, screenshots every page across viewports, compares to reference, and iterates until visuals match — the model effectively grades itself. Nothing else on the list comes close on impact.

### What's the difference between frontend-design and Impeccable — should I run both?

**Frontend-design** (Anthropic) and its Codex twin **frontend-skill** (OpenAI) are the floor — they ban overused fonts, force an explicit aesthetic commitment, and codify failure modes. Useful, but the article calls font bans "a band-aid for a training-data problem, not a design philosophy" and scores it 7/10. **Impeccable** scored 9/10 because it does what those skills don't: separates **brand mode** (marketing, portfolios, editorial) from **product mode** (apps, dashboards, internal tools) — exactly how senior designers actually think. A landing page and an admin tool need opposite things. Run frontend-design as your floor, then layer Impeccable on top for the brand-vs-product split.

### Which design skills should a senior designer prioritize, and which are overrated?

The article's senior-lens scoreboard: **install first** — Playwright/webapp-testing (10), Impeccable (9), Accesslint (9), Oczkowski's design process pack (9). These are the ones that change output even when nobody's prompting carefully — feedback loops, brand/product split, accessibility discipline, and requirements-first ordering. **Solid mid-tier** — theme-factory (8), Owl-Listener's collection (8), figma implement-design (8). **Useful but band-aid-shaped** — frontend-design (7), Composio Connect (7), excalidraw (7). The senior take throughout: aesthetic-preset skills compound across a thousand prompts only when they encode discipline (constraints, feedback loops, process). Skills that just decorate output read as band-aids.
