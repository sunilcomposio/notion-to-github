# Best GTM and Sales Skills for Claude / Codex

I have been using Claude Code a lot lately for sales work, and I keep hitting the same wall.

It reasons through tasks well, but it remembers nothing about how I work. Every account brief, recap, and outreach draft comes out generic, and I end up re-explaining my ICP and templates every few days.

Skills fix this. A skill is a `SKILL.md` file that Claude Code loads on its own. It keeps your format, standards, and context, so the same request produces the same output every time.

The problem is that most GTM skill bundles are poorly maintained or never trigger. I tested the popular ones and kept what worked. 

Here are the 10 worth installing if starting fresh.

### **TL;DR**

1. Composio Skill + CLI: The cleanest bridge for letting Claude or Codex act in Gmail, Slack, Notion, and 500+ apps.
2. sales-skills/sales: A large Claude sales router that maps plain-English requests to specialized GTM skills.
3. ColdIQ GTM Skills: 137 sales triggers and 34 email templates extracted from production outbound campaigns.
4. Vibe Prospecting plugin: Live B2B data for lead enrichment and list building inside Claude Code and Codex.
5. Goose Skills: Gooseworks' cross-agent GTM catalog with 108 skills and one installer for Claude, Codex, and Cursor.
6. AI Marketing Skills: Eric Siu's workflow-heavy Claude skill bundle for pipeline, outbound, experiments, and content ops.
7. meeting-notes-and-actions: A narrow Codex skill for turning rough call notes into follow-up, recap, and next steps.
8. meeting-insights-analyzer: A transcript-analysis skill for pulling objections, risks, and ownership from sales calls.
9. sales-communitytracker: A specific listening skill for buyer intent signals and competitor mentions across public communities.
10. issue-triage / linear: Two Codex skills that help pilot-heavy teams turn customer pain into tracked product work.

---

## Sales skills and plugins worth installing 

For this blog, “skills” mean things Claude Code or Codex can install and run: [SKILL.md](http://skill.md/) repos, skill libraries, Codex registry entries, and app-action bridges with a real install path.

I tested them against real sales jobs - prospecting, lead enrichment, meeting prep, transcript review, and actions in Gmail, Slack, and Linear. (used composio as orchestrator)

Every pick is a working install command, current docs, and a maintainer you can trust.

### 1. [Composio CLI + Skills](https://dashboard.composio.dev): External app actions for Claude and Codex, from shell commands to live handoffs

ComposioHQ’s `connect-apps` skill is the one I would install the day a workflow touches Gmail or Slack. Writing the follow-up is easy. Actually sending the email, posting the handoff in Slack, and creating the HubSpot or Linear update is where most agent demos fall apart.

- Sends Gmail or Outlook messages from the shell
- Posts Slack, Discord, or Teams updates after a call
- Creates issues or tasks in GitHub, Jira, or Linear
- Writes pages into Notion and rows into Sheets or Airtable
- Supports dry-run execution for safer first tests

**install the CLI**

```bash

curl -fsSL https://composio.dev/install | bash

```

**log in and link apps**

```bash

composio login

composio link gmail

composio link slack

composio link hubspot

```

**Test a safe first action**

```bash

composio execute GMAIL_SEND_EMAIL --dry-run -d '{"recipient_email":"you@example.com","subject":"recap","body":"draft recap"}'

```

[Awesome Codex Skills](https://github.com/ComposioHQ/awesome-codex-skills) · [Awesome Claude Skills](https://github.com/ComposioHQ/awesome-claude-skills)

---

### 2. [sales-skills/sales](https://github.com/sales-skills/sales): A router-first Claude sales pack with hundreds of GTM playbooks

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/best-gtm-and-sales-skills-for-claude-codex/og-sales-skills-sales.png)

A Claude-oriented repo built around one router, `/sales-do`, plus a long catalog of sales and marketing skills. I would start here if the team wants one install and one entry point, because remembering 40 skill names is exactly the sort of overhead that kills adoption.

- Routes plain-English GTM asks through `/sales-do`
- Generates cold outbound sequences from a segment or ICP
- Handles CRM, enrichment, note-taking, and social-listening jobs
- Covers webinar, influencer, email, ad, and directory workflows
- Installs with one command, then triggers after restart

**Install the router skill**

```bash

npx skills add sales-skills/sales --skill sales-do

```

**Restart and invoke it**

```plain text

/sales-do build a stakeholder map and 3-touch outbound sequence for CFOs at Series C fintech companies

```

[Releases](https://github.com/sales-skills/sales/releases)

---

### 3. ColdIQ GTM Skills: outbound playbooks from real campaigns

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/best-gtm-and-sales-skills-for-claude-codex/image_1.png)

Built by ColdIQ, an agency that runs outbound for clients. The skills come from real campaigns, not theory: 

> 7 master skills, 52 sub-skills, 137 sales triggers, 34 email templates, and Clay enrichment workflows. 

Each master skill routes your request to the right sub-skill.

- 137 sales triggers and 34 cold email templates
- Covers Clay enrichment, list building, and signal-based selling
- LinkedIn ads and organic content skills included
- Browsable directory at coldiq.com

**install a master skill**

```javascript

npx skills add https://github.com/sachacoldiq/ColdIQ-s-GTM-Skills --skill cold-email

```

Do not install everything. Follow this flow instead: 

- SDR teams: take cold email and list building. 
- Signal-led or founder-led teams: take signal-based-selling and the Clay skill. 
- Paid-led teams: take the LinkedIn skills. 

Using two or three skills is enough.

Skill page: [github.com/sachacoldiq/ColdIQ-s-GTM-Skills](https://github.com/sachacoldiq/ColdIQ-s-GTM-Skills) \| [coldiq.com/skills-directory](https://coldiq.com/skills-directory)

---

### 4. Vibe Prospecting plugin: live data for lead enrichment

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/best-gtm-and-sales-skills-for-claude-codex/image_2.png)

Skills need data. This plugin gives Claude Code and Codex live access to 150M+ companies and 800M+ contacts from Explorium, so the agent looks up titles and emails instead of guessing them.

- Builds prospect lists from ICP filters
- Fills missing emails, phones, and titles in CRM records
- Handles ABM lists, lead scoring, and CSV cleaning
- Shows a preview and cost estimate before any fetch

**install and log in**

```bash

npx @vibeprospecting/vpai@latest login

```

**run a first list build**

```bash

build a list of VP Sales at US SaaS companies, 100–500 employees, that raised in the last 12 months — preview 5 rows first

```

Benefit: You see five sample rows and a cost estimate before anything is fetched, so a bad filter costs nothing.

Skill page: [vibeprospecting.ai/product/plugin](https://www.vibeprospecting.ai/product/plugin)

---

### 5. [Goose Skills](https://github.com/gooseworks-ai/goose-skills): A cross-agent GTM catalog with 108 skills and one install path

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/best-gtm-and-sales-skills-for-claude-codex/og-gooseworks-ai-goose-skills.png)

Gooseworks built one of the cleaner mixed-agent catalogs I found. If a team has Claude Code on one laptop and Codex on another, this is easier to live with than maintaining separate repos and separate instructions. It sounds boring. It is. That is why it works.

- Installs to Claude Code, Codex, Cursor, or all detected agents
- Adds access to a catalog of 108 GTM skills
- Covers sales, marketing, SEO, lead gen, and competitive intel
- Let users call a skill by name after install
- Supports updates through the same Gooseworks CLI

**install for Claude**

```bash

npx gooseworks install --claude

```

**install for Codex**

```bash

npx gooseworks install --codex

```

**update the catalog**

```bash

npx gooseworks update

```

[Skill catalog](https://skills.gooseworks.ai)

---

### 6. [AI Marketing Skills](https://github.com/ericosiu/ai-marketing-skills): Workflow-heavy Claude skills for pipeline, outbound, and growth ops

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/best-gtm-and-sales-skills-for-claude-codex/og-ericosiu-ai-marketing-skills.png)

Eric Siu’s repo is closer to an operating kit than a prompt pack. The useful part is that the folders are opinionated about outputs: pipeline recovery, outbound optimization, scorecards, quote mining. A marketplace-sized bundle usually loses here because reviews get sloppy and trigger quality drops.

- Runs `Deal Resurrector` and `Trigger Prospector` sales workflows
- Includes `Cold Outbound Optimizer` and `Lead Pipeline` tools
- Adds `Weekly Scorecard` and experiment workflows for growth teams
- Ships category-level `SKILL.md` files for Claude Code use
- Supports scripts, scoring logic, and env-based automation

**clone the repo**

```bash

git clone https://github.com/ericosiu/ai-marketing-skills.git

cd ai-marketing-skills

```

**Copy one category skill into Claude**

```bash

cp sales-pipeline/SKILL.md .claude/skills/sales-pipeline.md

```

**Run a workflow example**

```bash

cd growth-engine

pip install -r requirements.txt

cp .env.example .env

```

---

### 7. [meeting-notes-and-actions](https://github.com/ComposioHQ/awesome-codex-skills): A Codex skill for turning rough call notes into clean follow-up

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/best-gtm-and-sales-skills-for-claude-codex/og-composiohq-awesome-codex-skills.png)

ComposioHQ’s Codex registry includes this narrow meeting skill, and I would take that narrowness over a giant bundle every time for post-call work. It misses broader account planning. Fine. The win is that reps get one recap shape to review before anything goes to a customer.

- Turns pasted notes into customer follow-up drafts
- Creates internal recap bullets for Slack or docs
- Builds next-step checklists with owners and deadlines
- Installs into `~/.codex/skills` through the Codex installer
- Relies on Codex auto-triggering from skill descriptions

**install from the Codex registry**

```bash

python ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo ComposioHQ/awesome-codex-skills --path meeting-notes-and-actions

```

**manual install fallback**

```bash

git clone https://github.com/ComposioHQ/awesome-codex-skills.git

cp -R awesome-codex-skills/meeting-notes-and-actions ~/.codex/skills/

```

---

### 8. [meeting-insights-analyzer](https://github.com/ComposioHQ/awesome-codex-skills): Transcript analysis for recurring objections, risks, and ownership gaps

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/best-gtm-and-sales-skills-for-claude-codex/og-composiohq-awesome-codex-skills.png)

This Codex skill is for teams drowning in transcripts. Generic summarizers flatten everything into the same bland recap. 

The transcript analyzer skill within the skill pack earns its place when you need the repeatable stuff, recurring objections, risk patterns, and missing owners before a manager review or forecast call.

- Pulls themes and follow-ups from transcript files
- Flags call risks and behavioral patterns
- Extracts objection clusters for coaching or messaging
- Runs inside the Codex skill system, no separate app required
- Works on exported notes or pasted transcript text

**install the skill**

```bash

python ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo ComposioHQ/awesome-codex-skills --path meeting-insights-analyzer

```

**Check that Codex can see it**

```bash

ls ~/.codex/skills/meeting-insights-analyzer

head ~/.codex/skills/meeting-insights-analyzer/SKILL.md

```

---

### 9. [sales-communitytracker](https://github.com/sales-skills/sales): Public-community listening for buyer intent and competitor mentions

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/best-gtm-and-sales-skills-for-claude-codex/og-sales-skills-sales.png)

This one is a specialized skill pack. 

CommunityTracker is for founder-led sales, product-led motions, and scrappier teams that pull demand from public conversations instead of waiting on form fills. 

I was skeptical at first. Then I looked at how many real GTM teams are already mining Reddit, LinkedIn, X, and Slack for intent.

- Tracks mentions across Reddit, Slack, Discord, LinkedIn, X, GitHub, and HN
- Surfaces intent-scoring and share-of-voice workflows
- Feeds public discussion into prospecting research
- Lives inside the broader `sales-skills/sales` install
- Invokes well through the `/sales-do` router

**install the parent bundle**

```bash

npx skills add sales-skills/sales --skill sales-do

```

**route a listening task**

```plain text

/sales-do find high-intent buyer signals for payroll software across Reddit, LinkedIn, and founder communities

```

---

### 10. [issue-triage / linear](https://github.com/ComposioHQ/awesome-codex-skills): Post-sale handoff skills for turning friction into tracked product work

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/best-gtm-and-sales-skills-for-claude-codex/og-composiohq-awesome-codex-skills.png)

I am keeping these paired because the job is one path: transcript or pilot feedback, then triage, then a tracked issue. 

This is for post-sale, pilots, and expansion motions, not cold prospecting. If customer pain keeps dying in Slack threads, this pair fixes a real leak.

- Triages Linear or Jira backlogs from the terminal
- Creates and manages Linear issues and workflows
- Turns escalation notes into tracked product follow-up
- Pairs well with transcript and meeting-note skills
- Helps account teams close the loop after pilots

**install issue triage**

```bash

python ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo ComposioHQ/awesome-codex-skills --path issue-triage

```

**Install the Linear skill**

```bash

python ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo ComposioHQ/awesome-codex-skills --path linear

```

---

## Also worth a look ( bonus)

Two repos with skills and plugins almost made the list, but with a few caveats 

-  [Extruct's gtm-skills](https://github.com/extruct-ai/gtm-skills) and [gtm-cowork-skills](https://www.extruct.ai/blog/cowork-gtm-skills/) cover outbound and deal execution well, but the prompts assume their stack (Granola transcripts, MEDDPICC, and Attio) needs editing before they fit your workflow. 
- The [Claude GTM Plugin](https://github.com/manojbajaj95/claude-gtm-plugin) is the largest bundle around and has a useful /bootstrap interview, but it leans more toward marketing than sales. If your motion fits, both are solid.

---

## Write a CLAUDE.md for sales

One habit I always enforce on everyone on my team is to put a CLAUDE.md in the project root. It should only contain:

- ICP, 
- deal stages, 
- methodology, 
- tone rules, 
- and guardrails like "never send without review." 

This makes the skills read it before they run, so everything comes out in the desired voice. A simple ten-minute hack, and with this, every install above gets better. 

---

## Wrap up

A few practical caveats before I wrap:

- Do not install all ten skills at once. This trigger’s quality lives in each skill's description field. In fact, [Anthropic's skills guidance](https://github.com/anthropics/skills) identifies under-triggering and over-triggering as the most common failures. Read the `SKILL.md` before you trust the install.
- Watch overlap. ColdIQ, Goose, and the router pack each ship a cold email skill. Keep one and remove the rest, or they compete for the same trigger.
- Be careful with write access. Once an agent can touch Gmail, Slack, or Linear, you are testing live operations, not drafts. Use dry runs and connect the fewest apps. 
- Best to use Composio as an orchestration layer for secure auth, optimized tool calls, and parallel processing.

My blunt recommendation is simple: start with one router, one meeting skill, and one app-action bridge. If you install five overlapping bundles, you will spend next week debugging triggers instead of moving deals.

[**Try Composio →**](https://dashboard.composio.dev)

<empty-block/>
