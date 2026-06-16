# Top 10 Skills to Use with OpenClaw To 100x Your Experience

OpenClaw is growing fast, and if you've used it for anything beyond general tasks, you've probably noticed the gap.

It handles most things, but specialized work like research, UI/UX design, writing, or reverse engineering? The output quality just isn't there.

That's the problem SKILLS solves!

It's a single file that plugs into OpenClaw and gives it deep, domain-specific context - purpose-built instructions for doing one thing exceptionally well. Better context in, better output out.

The catch? OpenClaw's own ClawHub now hosts over **13,000** community-built skills, and roughly 80% are garbage or outright malicious. Finding the good ones takes time.

I spent the last two weeks doing that work - cross-referencing ClawHub download rankings, community ratings, and real-world usage - and here's what's actually worth using.

---

## TL;DR- All the Skills at a Glance

| # | Skill | Category | Best For | Verified Install Command |
| --- | --- | --- | --- | --- |
| 1 | Composio | Coding Agents & IDEs | 1,000+ external tools like Github, Figma, Youtube, etc. | `npx skills add https://github.com/composiohq/skills --skill composio` |
| 2 | Capability Evolver | Coding Agents & IDEs | Long-running agents that improve automatically | `clawhub install kennyzir/capability-evolver-pro` |
| 3 | Reverse Engineering | Coding Agents & IDEs | Network analysis, protocol decoding | Search "reverse-engineering" on clawhub.ai |
| 4 | Frontend Design | Web & Frontend Dev | Production-grade UI, bold aesthetics | `clawhub install frontend-design` |
| 5 | Exa Web Search | Web & Frontend Dev | Developer-focused search, no API key needed | `clawhub install exa-web-search-free` |
| 6 | Deep Research Pro | Web & Frontend Dev | Multi-source research with citations | `openclaw skills install deep-research-pro` |
| 7 | N8N Workflow | DevOps & Cloud | Local workflow automation, no subscription | Search "n8n-workflow" on clawhub.ai |
| 8 | Vercel | DevOps & Cloud | Deploy and manage Vercel from chat | `clawhub install gora050/vercel-integration` |
| 8 | OpenAI Whisper | DevOps & Cloud | Local transcription, no third-party upload | Search "openai-whisper" on clawhub.ai |
| 9 | Agent Browser | Browser & Automation | Live web browsing, scraping, form-filling | `clawhub install agent-browser` |
| 10 | ByteRover | Browser & Automation | General-purpose task automation | `clawhub install byterover` |
| 11 | Eleven Labs Agent | Browser & Automation | Voice AI, automated calling fallback | `npx clawhub@latest install elevenlabs-agent` |
| 12 | GOG - Google Workspace | Productivity & Tasks | Gmail, Calendar, Drive in one install | `clawhub install gog` |
| 13 | Obsidian | Productivity & Tasks | Vault sync, notes through chat | `npx clawhub@latest install obsidian` |
| 14 | AgentMail | Productivity & Tasks | Standalone agent inbox, email triage | `clawhub install agentmail` |
| 15 | Home Assistant | Productivity & Tasks | Natural language local home control | Search "home-assistant" on clawhub.ai |

---

## How I Picked These Skills

Not every skill that looks impressive is actually useful day-to-day. Here's the filter I used:

- **Usefulness**: It solves a problem that nobody else is solving.
- **Download volume** - only skills with good  installs made the list
- **Community rating** - cross-referenced against ClawHub stars and community-vetted skills on Reddit and Twitter.
- **Activity** - skills with at least 3 months of active development (the 100/3 rule)
- **Security scan** - VirusTotal clean, SKILL.md reviewed manually

If it didn't pass all four, it didn't make the list.

---

## Read This Before Installing Anything

OpenClaw's growth attracted bad actors fast.

In late January 2026, security researchers at Koi Security uncovered **ClawHavoc** - a coordinated attack campaign that planted 341 malicious skills on ClawHub using typo-squatting (subtle misspellings of legitimate tools, like "clawhubb" instead of "clawhub").

These backdoored skills distributed the Atomic Stealer malware and exfiltrated SSH keys, API tokens, and browser session cookies from thousands of installs.

A Snyk audit found a separate 13.4% of skills flagged for critical issues, including prompt injection and exposed API keys.

ClawHub responded by partnering with VirusTotal for automated scanning and removing 2,400+ suspicious skills.

Every skill page now shows a Skill Card with a VirusTotal result. But the risk is still real.

**Before installing any skill from ClawHub:**

1. Visit the ClawHub page and confirm the VirusTotal scan says **Benign**
2. Read the `SKILL.md` file yourself - it's plain text, no excuses for skipping it
3. Apply the **100/3 rule**: only install skills with 100+ downloads and 3+ months of activity
4. Never run `clawhub install <skill>` from a link someone sent you - always find it yourself on clawhub.ai first

> **Note:** OpenClaw ships 53 first-party bundled skills that carry zero registry risk. If you're just getting started, those are the safest place to begin before going into ClawHub.

---

## How To Install Skills in OpenClaw

There are two main ways. Here are both.

### ClawHub

ClawHub is the central community registry for OpenClaw skills. First install the CLI globally:

```bash
npm i -g clawhub
```

Then install any skill with:

```bash
clawhub install <skill-slug>
```

Skills install to `./skills/` in your current directory by default (or your configured OpenClaw workspace). Always restart OpenClaw after installing - skills are snapshotted at session start.

> **Security rule:** Before running any install command, go to the skill's ClawHub page, verify the VirusTotal scan says **Benign**, and read the SKILL.md. Never install from a link without checking this yourself.

### Skills.sh

Skills.sh is a curated alternative maintained by Vercel with stricter vetting. Fewer options, but higher signal-to-noise. Here's how to use it:

```bash
cd .openclaw
npx skills add <owner/repo> --skill <skill-name>
```

Select OpenClaw from the list (press `space` to select, `enter` to finalize), then verify in your dashboard's Skills section.

With that covered, here are the skills worth installing.

---

## Top OpenClaw Skills in 2026

---

## Coding Agents, Engineering & IDEs

The real demand in the OpenClaw coding agents and IDEs space is for genuine developer infrastructure. Most developers use:

| Skill | What It Does |
| --- | --- |
| **Composio** | Connects agents to external services without custom auth - GitHub, Slack, Gmail, and 1,000+ more via a single integration |
| **OpenClaw coding agent skill** | Configurations that make autonomous agents actually useful inside a real codebase, covering PR and CI workflows |
| **Reverse Engineering** | Full visibility into undocumented protocols and binary traffic for network analysis and protocol decoding |

Let’s look at each of them in detail below.

---

### 1. Composio

Personally, I use this one the most, and it genuinely delivers.

It gives OpenClaw access to **1,000+ external tools** through a single integration framework - GitHub, Slack, Gmail, Notion, Linear, Jira, HubSpot, Salesforce, and hundreds more - without writing a single custom authentication pipeline. Composio manages all OAuth flows and token refreshes for each one.

Whether you're building autonomous agents or traditional apps, this skill handles the heavy lifting. It's the one install that makes every other workflow easier.

**Install:**

```bash
# Via Skills.sh (recommended)
cd .openclaw
npx skills add https://github.com/composiohq/skills --skill composio --yes

# Or via Composio's own installer
curl -fsSL https://composio.dev/install | bash -s -- --agent
```

Once installed, authenticate specific services:

```bash
composio link github
composio link gmail
composio whoami  # verify
```

**Use Cases:**

- Building chat-based AI agents that securely access external services like Gmail or Slack via isolated MCP sessions - *"Create an agent that monitors my Gmail and summarises unread emails every morning."*
- Developing multi-tenant apps that need programmatic access across multiple user accounts - *"Set up a direct execution flow for managing GitHub repos across multiple users."*
- Triggering event-driven automations - *"Create a workflow that fires whenever a new Stripe payment is received."*

**ClawHub:** [github.com/ComposioHQ/openclaw-composio-plugin](https://github.com/ComposioHQ/openclaw-composio-plugin) \| **Skills.sh:** `composiohq/skills`

---

### 2. Capability Evolver

If you run OpenClaw on recurring workflows, this is the first skill to install.

It's the most-downloaded skill on ClawHub by a wide margin, in fact, more than double the second-place skill. Once you use it, it's obvious why.

Capability Evolver analyzes your agent's runtime logs, detects error patterns and inefficiencies, and generates structured improvement proposals.

It runs entirely locally - no API key, no external calls, no hallucination risk. The engine uses deterministic pattern matching rather than an LLM, which means results are reproducible and sub-100ms.

The compounding effect is real. Tasks your agent struggled with in week one become routine by week four.

**Important note:** Capability Evolver expands *what* your agent can do through deterministic log analysis - no LLM, no external dependencies, reproducible results every time.

> **Security note:** The original `capability-evolver` by `@autogame-17` was flagged in a GitHub security report (issue #95) for undisclosed data exfiltration to Feishu/ByteDance. ClawHub marked it suspicious. Use the verified clean version below - same functionality, VirusTotal Benign, ClawScan Benign, Static analysis Benign.

**Install:**

```json
clawhub install kennyzir/capability-evolver-pro
```

**Use Cases:**

- Automatically improving agent performance on long-running, repeating tasks - no manual intervention required
- Reducing errors and context drift on complex multi-step workflows by letting the agent self-calibrate
- Ideal for anyone running OpenClaw continuously rather than in one-off sessions

**ClawHub page:** [clawhub.ai/kennyzir/capability-evolver-pro](http://clawhub.ai/kennyzir/capability-evolver-pro)

---

### 3. Reverse Engineering

Ever wanted to reverse-engineer a tool, app, or web service to understand exactly how it communicates under the hood? This skill is built for that.

It turns OpenClaw into an elite network analyst - capturing raw traffic, dissecting unknown protocols, and translating binary data into clean documentation and custom parsers.

Whether it's security research, debugging, or system interoperability, this skill gives you full visibility into any network communication.

**Use only on systems you're authorized to analyze.**

**Install:**

```bash
# Search ClawHub for the current verified build
clawhub search "reverse engineering"
# Then: clawhub install <verified-slug>
```

**Use Cases:**

- Capturing and analyzing raw network traffic to uncover vulnerabilities or undocumented endpoints - *"Capture and analyze traffic from this app and identify any undocumented endpoints."*
- Developing custom Wireshark dissectors (Lua) and Python parsers to map complex binary structures - *"Write a Wireshark Lua dissector for this binary protocol sample."*
- Performing entropy analysis and TLS fingerprinting (JA3/JA3S) - *"Run entropy analysis on this packet capture and identify the encryption layer."*

Visit: [https://www.skills.sh/wshobson/agents/protocol-reverse-engineering](https://www.skills.sh/wshobson/agents/protocol-reverse-engineering)

---

## <span discussion-urls="discussion://379f261a-6dfe-8052-acf0-da078ca88987/381f261a-6dfe-808c-8f9f-c48523ed085f/381f261a-6dfe-8088-9302-001c7c264105">Web & Frontend Dev</span>

Most OpenClaw web and frontend skills produce the same result: Inter font, purple gradient, safe layout.

<span discussion-urls="discussion://379f261a-6dfe-8052-acf0-da078ca88987/381f261a-6dfe-807b-a39f-d918e0ab849d/381f261a-6dfe-8044-8053-001cb5dd75bc">The tools that are actually worth using, which go further than static generation</span>.

| Skill | What It Does |
| --- | --- |
| **Frontend Design** | Production-grade UI with intentional aesthetics and bold typography instead of AI defaults |
| **Exa Web Search** | Developer-focused search that surfaces actual documentation and GitHub repos instead of SEO noise |
| **Deep Research Pro** | Multi-source research with citations and confidence ratings when being wrong has real consequences |

Let’s look at each of them in detail below

---

### 4. Frontend Design

It's by Anthropic. That alone makes it worth noting - it's actively maintained, well-tested, and the SKILL.md is clean.

OpenClaw can build frontend apps out of the box, but the output is generic - the usual Inter font, purple gradients, and safe layouts that all look the same.

This skill forces OpenClaw into a master-level design mindset: bold aesthetic direction, intentional typography, and production-grade interfaces before a single line of code is written.

It explicitly targets what it calls "AI slop" aesthetics.

**Install:**

```bash
clawhub install frontend-design
```

Or pull from the Anthropic skills repo directly:

```bash
npx skills add https://github.com/anthropics/claude-code --skill frontend-design --yes
```

**Use Cases:**

- Prototyping distinctive landing pages that don't look like every other AI-generated template - *"Build me a landing page with a brutalist aesthetic for a SaaS product."*
- Translating abstract brand directions into functional, production-ready code - *"Design a site that feels like an editorial fashion magazine - dark, high contrast, typographic."*
- Breaking out of cookie-cutter design ruts - *"Redesign this hero section. Avoid any standard layouts or default fonts."*

**ClawHub / PIAX listing:** [piax.org/skills/anthropics-skills/frontend-design](https://www.piax.org/skills/anthropics-skills/frontend-design) \| **GitHub:** [github.com/anthropics/claude-code/tree/main/skills/frontend-design](https://github.com/anthropics/claude-code)

---

### 5. Exa Web Search (Free)

OpenClaw supports browsing, but for technical work, general search just doesn't cut it. It surfaces SEO blogs over actual documentation.

Exa's neural search index is built differently - it pulls from GitHub repos, technical docs, and coding forums rather than the general web, and supports category filtering by "research paper," "code," "news," and more.

There are a few Exa skill variants on ClawHub. `exa-web-search-free` is the no-key option, and it holds 24,000+ downloads.

If you want the full 9-tool suite, `web-search-exa` (v2.0.0, March 2026) covers deep research, structured extraction, and advanced filters.

**Install:**

```bash
# Free version - no API key
clawhub install exa-web-search-free

# Full version - requires EXA_API_KEY
clawhub install web-search-exa
```

**Use Cases:**

- Pulling current coding documentation and API references - *"Find the latest React 19 docs on server components."*
- Searching specifically for technical solutions, not SEO content - *"Search for the best open-source alternatives to Stripe's API."*
- Finding real-world GitHub implementations - *"Find me a GitHub repo that implements JWT authentication in Node.js."*

**ClawHub page:** [clawhub.ai search: exa-web-search-free](https://clawhub.ai/) \| **LLMBase:** [llmbase.ai/openclaw/exa-web-search-free](https://llmbase.ai/openclaw/exa-web-search-free/)

---

### 6. Deep Research Pro

Best for situations where being wrong has real consequences: market research, due diligence, competitive analysis.

It's the most-downloaded AI research skill on ClawHub. No API key required - it uses DuckDuckGo search under the hood.

What sets this apart from just asking your agent a question is that Deep Research Pro asks 1–2 clarifying questions up front (goal, depth, angle), then conducts multi-source research and delivers cited reports with a transparent methodology.

It tells you what it found, what it's uncertain about, and how confident it is in each claim. That transparency is its top feature.

**Install:**

```bash
openclaw skills install deep-research-pro
```

**Use Cases:**

- Running rigorous market research with source citations and confidence ratings - *"Research the top five competitors in the AI agent space and flag anything you're uncertain about."*
- Producing analyst-grade reports with methodology transparency - *"Write a research brief on enterprise SaaS churn trends, note your confidence on each claim."*
- Replacing expensive consultant hours with first-pass research on new topics or markets

**ClawHub page:** [clawhub.ai/parags/deep-research-pro](https://clawhub.ai/parags/deep-research-pro)

---

## DevOps & Cloud

This is the category where vetting matters most - everything here touches privileged system access. The practical use cases that dominate this space are:

| Skill | What It Does |
| --- | --- |
| **N8N Workflow** | Connects OpenClaw to your local N8N instance for multi-step workflow automation without a cloud subscription |
| **Vercel** | Translates plain conversational commands into Vercel CLI actions - deploy, manage builds, and debug without touching the terminal |
| **OpenAI Whisper** | Runs transcription locally so your audio never leaves your system - meetings, videos, and subtitles offline |

> **Security note:** Apply the 100/3 rule, read the SKILL.md line by line, and always check VirusTotal before installing anything in this category.

Here are the details of the above skills, the ones that are worth installing.

---

### 7. N8N Workflow

You've probably used N8N - and if so, you already know that running enterprise-level automations on the cloud burns cash fast.

This skill connects OpenClaw directly to your local N8N instance, letting you spin up, manage, and trigger complex multi-step workflows using plain chat.

No expensive subscription, no manual dashboard. Because the entire process runs locally, your data stays private.

> **Note for production use:** Security experts recommend limiting the agent to webhook-only N8N access rather than full N8N API control, to avoid prompt injection attacks overriding workflow permissions.

**Install:**

```bash
# Search for the current verified build on ClawHub
clawhub search "n8n workflow"
# Then: clawhub install <verified-slug>
```

**Use Cases:**

- Triggering an automated sequence to post LinkedIn updates the moment a new podcast goes live - *"When a new podcast episode drops, draft and post a LinkedIn update automatically."*
- Setting up multi-app workflows through conversation - *"Create a workflow that saves every Gmail attachment to Dropbox and notifies me on Slack."*
- Automating recurring data tasks - *"Every Monday at 9 am, pull last week's analytics and email me a summary."*

**ClawHub:** Search `n8n-workflow` on [clawhub.ai](https://clawhub.ai/) \| Requires a running local N8N instance

---

### 8. Vercel

If deploying to Vercel is already part of your workflow, this skill eliminates the need for the terminal entirely.

It connects OpenClaw to the Vercel CLI, translating plain conversational commands into the exact CLI actions needed to deploy, manage, and debug your projects.

In short, all the operations you'd normally type are replaced by a single chat message.

**Install:**

```bash
clawhub search "vercel"
# Then: clawhub install <verified-slug>
```

**Use Cases:**

- Deploying new projects using natural language - *"Deploy this project to Vercel."*
- Managing builds and rollbacks without touching the CLI - *"Roll back to the last stable deployment."*
- Debugging failed builds on the fly - *"Why did my last deployment fail? Show me the build logs."*

**ClawHub:** [https://clawhub.ai/gora050/vercel-integration](https://clawhub.ai/gora050/vercel-integration)

---

### 8. OpenAI Whisper

If you're a developer and content creator, accurate transcriptions are already part of your workflow. The problem is that uploading sensitive audio to third-party services isn't always an option.

This skill runs OpenAI's Whisper model locally - fast, accurate transcriptions without your audio ever leaving your system.

**Install:**

```bash
clawhub search "whisper"
# Then: clawhub install <verified-slug>
```

**Use Cases:**

- Transcribing audio and video files completely offline - *"Transcribe this audio file locally."*
- Converting meeting recordings into clean text - *"Transcribe this meeting recording and clean it up."*
- Generating subtitles without a paid transcription tool - *"Transcribe this video and format the output as subtitles."*

**ClawHub:** [https://clawhub.ai/steipete/openai-whisper](https://clawhub.ai/steipete/openai-whisper) \| Requires a local Whisper installation + OpenAI API key

---

## Browser & Automation

This is the tightest signal-to-noise category on ClawHub - most of what's here is genuinely maintained and genuinely useful. Most automation-focused users reach for:

| Skill | What It Does |
| --- | --- |
| **Agent Browser** | Full headless browser control - navigate, click, scrape, fill forms, and capture screenshots on the live web |
| **ByteRover** | General-purpose automation for tasks that fall between more specialized skills - the catch-all capability layer |
| **Eleven Labs Agent** | Voice AI with a fail-safe calling mechanism that pivots to a real phone call when text communication fails |

Here are the skills worth installing in this category.

---

### 9. Agent Browser

Agent Browser gives OpenClaw a full headless browser - navigate websites, click buttons, fill forms, take screenshots, record video, extract data, and manage sessions.

58,600+ downloads. A Rust-based headless browser automation CLI with Node.js fallback, maintained by @TheSethRose.

It's what turns your agent from a text processor into something that can actually operate on the internet.

Without this skill, your agent is blind to the live web.

**Install:**

```bash
clawhub install agent-browser
```

> **Alternative:** For cloud-browser automation with anti-detect profiles and CAPTCHA solving, check out the Browser Use integration (`npx skills add https://github.com/browser-use/browser-use --skill browser-use`).

**Use Cases:**

- Monitoring competitor pricing pages and alerting you to changes - *"Check these three competitor pricing pages daily and tell me if anything changes."*
- Scraping structured data from any website - *"Pull all the job listings from this page and give me a table."*
- Completing multi-step web forms - *"Fill out and submit this contact form using my standard details."*

**ClawHub page:** [clawhub.ai/TheSethRose/agent-browser](https://clawhub.ai/TheSethRose/agent-browser) \| **GitHub:** [github.com/TheSethRose/Agent-Browser-CLI](https://github.com/TheSethRose/Agent-Browser-CLI)

---

### 10. ByteRover

If you repeatedly find yourself doing similar but varied tasks that don't have a dedicated skill, ByteRover is the skill you need.

The Swiss-army knife of OpenClaw skills - a multi-purpose task handler that ranks consistently in ClawHub's top downloads.

It combines utility and development capabilities into a single skill, useful for automation and processing tasks that don't fit neatly into a more specialized tool.

**Install:**

```bash
clawhub install byterover
```

**Use Cases:**

- Handling general-purpose automation tasks that fall between more specialized skills
- Flexible task processing for developers who need a catch-all capability layer
- Bridging gaps in your skill stack without installing five different single-purpose tools

**ClawHub:** [https://clawhub.ai/byteroverinc/byterover](https://clawhub.ai/byteroverinc/byterover)

---

### 11. Eleven Labs Agent

If you're into voice AI or your job involves any calling workflow, this one is a game-changer.

It integrates directly with the ElevenLabs CLI, giving OpenClaw an actual voice.

The most interesting feature is the fail-safe mechanism: if your agent fails to send a text message or email, it automatically pivots to making a real phone call instead.

That's a fundamentally different capability class - turning a chat-based AI into something that can actually pick up the phone.

**Install:**

```bash
npx clawhub@latest install elevenlabs-agent
```

Set your API key:

```bash
export ELEVENLABS_API_KEY=your_key_here
```

**Use Cases:**

- Fallback calling when text-based communication fails - *"If the email fails, call them instead."*
- Automating voice-based tasks like making reservations - *"Call and book a table for 2 at 7 pm."*
- Generating voiced summaries for hands-free productivity - *"Read out my task list for today."*

**ClawHub:** [https://clawhub.ai/pennyroyaltea/elevenlabs-agents](https://clawhub.ai/pennyroyaltea/elevenlabs-agents) \| Requires your ElevenLabs API key

---

## Productivity & Tasks

These are the daily-driver tools: note-taking, daily summaries, task logging, and inbox management. Most productivity-focused users rely on:

| Skill | What It Does |
| --- | --- |
| **GOG workspace unifier** | Brings Gmail, Calendar, Drive, Docs, and Sheets under one install instead of five separate setups |
| **Obsidian vault sync** | Connects your agent directly to your knowledge base - read, write, link, and tag notes through chat |
| **AgentMail** | Handles inbox triage, draft generation, and send approval workflows via a standalone agent inbox |
| **Home Assistant** | Natural language control of your local smart home setup with no cloud dependency |

Details on the skill mentioned in the table

---

### 12. GOG - Google Workspace

It's a bundled skill that ships with OpenClaw, meaning you may already have it.

The most-downloaded productivity skill on ClawHub by a significant margin (184,900+ downloads).

GOG gives your OpenClaw agent full access to Gmail, Google Calendar, Drive, Docs, Sheets, and Contacts - through a single integration.

In short, instead of six separate skill setups, one install handles your entire Google universe.

**Install:**

```bash
# Via ClawHub
clawhub install gog

# Or via native OpenClaw command (if bundled)
openclaw skills install gog
```

One-time OAuth setup (required):

```bash
gog auth credentials /path/to/client_secret.json
gog auth add you@gmail.com --services gmail,calendar,drive,contacts,sheets,docs
gog auth list  # verify
```

**Use Cases:**

- Summarising unread emails, drafting responses, and managing inbox labels through conversation - *"Summarise everything in my inbox that needs a reply today."*
- Scheduling meetings and checking your week - *"What does my Thursday look like? Block two hours for deep work in the morning."*
- Finding and updating Drive documents without opening a browser - *"Find the Q2 report in Drive and give me the key numbers."*

**ClawHub page:** [clawhub.ai/steipete/gog](https://clawhub.ai/steipete/gog) \| **LLMBase:** [llmbase.ai/openclaw/gog](https://llmbase.ai/openclaw/gog/)

---

### 13. Obsidian

If your knowledge base lives in Obsidian, this is the skill that finally connects it to your agent.

It lets OpenClaw read, write, link, and tag notes inside your vault through plain conversation - no manual file hunting, no context switching. The skill works directly on your vault's Markdown files via `obsidian-cli`, so changes show up in Obsidian instantly.

Built by @steipete (OpenClaw's creator), which means it's actively maintained and the SKILL.md is clean.

**Install:**

```bash
npx clawhub@latest install obsidian
```

One-time vault setup (required):

```bash
# Point OpenClaw to your vault's absolute path
export OBSIDIAN_VAULT_PATH="/path/to/your/vault"
```

**Use Cases:**

- Searching and summarising notes through natural language - *"Summarise everything I've written about AI agents in the last month."*
- Creating new notes from chat - *"Log this as a daily note and tag it #meeting."*
- Finding connections between notes - *"What notes are linked to my 'OpenClaw setup' note?"*

**ClawHub page:** [clawhub.ai/steipete/obsidian](http://clawhub.ai/steipete/obsidian) \| **GitHub:** [github.com/openclaw/skills/tree/main/skills/steipete/obsidian](http://github.com/openclaw/skills/tree/main/skills/steipete/obsidian)

---

### 14. AgentMail

If your agent needs to send, receive, or triage email independently - not just through your personal Gmail - AgentMail is the dedicated skill for it.

It gives OpenClaw its own programmatic email inbox: create disposable addresses, send messages, receive and read replies, and manage threads through chat. Where GOG connects to your existing Gmail account, AgentMail gives your agent a standalone inbox it fully controls - useful for outreach workflows, automated follow-ups, and approval chains.

Maintained by the AgentMail team and available via both ClawHub and Skills.sh.

**Install:**

```bash
clawhub install agentmail
```

After installing, add your API key to `~/.openclaw/openclaw.json`:

```json
{
  "skills": {
    "entries": {
      "agentmail": {
        "enabled": true,
        "env": {
          "AGENTMAIL_API_KEY": "your-api-key-here"
        }
      }
    }
  }
}
```

Get your API key from the AgentMail Console at [agentmail.to](http://agentmail.to) .

**Use Cases:**

- Creating disposable inboxes for outreach campaigns - *"Create a new inbox for this product launch and monitor it for replies."*
- Automating follow-up sequences with draft approval - *"Draft a follow-up to everyone who hasn't replied in 3 days and send after I approve."*
- Receiving and triaging inbound email through conversation - *"What came into the support inbox today? Summarise and flag anything urgent."*

**ClawHub page:** [clawhub.ai/agentmail-to/agentmail](http://clawhub.ai/agentmail-to/agentmail) \| **Docs:** [agentmail.to/integrations/openclaw](http://agentmail.to/integrations/openclaw)

---

### 15. Home Assistant

Personally, I haven't used it, but a few friends say it's game-changing for home automation - and the use cases make it obvious why.

It connects OpenClaw directly to a local Home Assistant setup, letting you control your entire smart home through natural language. No rigid routines, no cloud dependency, no data leaving your network.

**Install:**

```bash
clawhub search "home assistant"
# Then: clawhub install <verified-slug>
```

**Use Cases:**

- Controlling smart lights, locks, and appliances with natural language - *"Turn off all the lights in the living room"* or *"Lock the front door."*
- Creating dynamic AI-driven home automation routines - *"Every night at 10 pm, dim the bedroom lights and lock all doors."*
- Checking all connected devices in one place - *"Which devices are currently on?"*

**OpenCalw GitHub Skill:** [https://github.com/developmentcats/homeassistant-assist](https://github.com/developmentcats/homeassistant-assist)\| Requires a running local [Home Assistant](https://www.home-assistant.io/) instance

---

## Extra Skills Worth Knowing

Beyond the main list, here are a few more that came up repeatedly in the community:

| Skill | What It Does | Install |
| --- | --- | --- |
| WhatsApp CLI (Wacli) | Drafts, approves, and sends WhatsApp messages via natural language | `clawhub install wacli` |
| GitHub | PR triage, CI status, issue management via the `gh` CLI | `clawhub install steipete/github` |
| Obsidian | Reads and writes to your Obsidian vault - notes, tags, links | `clawhub install obsidian` |
| Bird (Twitter/X) | Searches keywords, checks feeds, pulls social data into chat | Search `bird` on clawhub.ai |
| YouTube Summarizer | Extracts and summarises YouTube transcripts | Search `youtube-summarizer` on clawhub.ai |
| GOG (Obsidian alt) | If you use Google over Obsidian for knowledge management | `clawhub install gog` (already listed above) |

---

## Summary

OpenClaw skills are plugins that add tools, knowledge, and workflows to enhance your AI agent. Here's what to keep in mind:

- **Installation:** The correct CLI command is `clawhub install <skill-slug>` or `openclaw skills install <skill-slug>` for bundled skills. Always verify the VirusTotal result on clawhub.ai first.
- **Security:** The ClawHavoc attack in early 2026 is a real reminder that not every skill on ClawHub is safe. Always verify the VirusTotal scan, read the SKILL.md, and apply the 100/3 rule (100+ downloads, 3+ months active).
- **Start bundled:** OpenClaw ships 53 first-party skills with zero registry risk. Get comfortable with those - especially `gog`, `github`, and `summarize` - Before going deep into ClawHub.
- **Stack intentionally:** The best OpenClaw setups aren't the biggest. Each skill you add is a new set of instructions your agent follows. Keep it tight and purposeful.

If you're only installing one skill, make it **Composio** - this one unlocks 1,000+ services without worrying about tool selection, OAuth pipelines, or context rot.

If you're building for the long term, use it with **Capability Evolvert.** This covers breadth, depth, and continuous improvement - and it compounds over time.
