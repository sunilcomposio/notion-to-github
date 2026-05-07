---
title: "10 Claude Cowork Plugins I Actually Use (And One I Turned Off After a Day)"
source: https://www.notion.so/10-Claude-Cowork-Plugins-I-Actually-Use-And-One-I-Turned-Off-After-a-Day-358f261a6dfe804b9c0cd1fab7216aa0
notion_page_id: 358f261a-6dfe-804b-9c0c-d1fab7216aa0
last_synced: 2026-05-07T08:58:51Z
---

I installed Claude Cowork the day it dropped. First week I was genuinely surprised. It finished things I'd half-started, cleaned up docs I'd been avoiding, handled tasks I'd normally context-switch three times to complete. 

Second week I realized it didn't know anything about how I actually work. It couldn't check Slack for me. It didn't know our CRM. Every task started with me explaining context it should have already had.

Then Anthropic released plugins. I spent a full weekend testing them. Some were really impressive. Some were fine.

Here are the ten worth actually installing, who they're for, and what they do and if they’re any good for your use cases.

One quick thing before we start: you can browse plugins from the Customize menu, and Anthropic keeps the collection on GitHub at `anthropics/knowledge-work-plugins`. Many plugins expose slash commands inside Cowork, though exact commands and connector setup can vary.

## Quick Summary: All Cowork Plugins at a Glance

1. **Enterprise Search:** Search Gmail, Slack, Notion, Drive, and Confluence simultaneously from one query.
2. **Sales:** Prospect research, CRM updates, deal risk flagging, and follow-up drafts from a single conversation.
3. **Legal:** First-pass contract review with GREEN/YELLOW/RED flagging before anything goes to counsel.
4. **Finance:** Variance analysis, reconciliation, and financial summaries connected to your data warehouse.
5. **Data:** Plain English to SQL. Your non-technical team members get answers without filing a data request.
6. **Marketing:** Competitive briefs, content drafts, and campaign performance in one place.
7. **HR:** Job descriptions, onboarding plans, and performance summaries that catch what you'd otherwise miss.
8. **Engineering:** ADRs, incident write-ups, and spec documents posted directly to GitHub or Confluence.
9. **Productivity:** Morning catch-up, meeting prep, and task updates across all your tools.
10. **Plugin Create:** Build your own plugin when none of the above fit your workflow.

---

## **What Are Claude Cowork Plugins?**

Plugins are pre-configured packages that bundle together skills, slash commands, connectors, and sub-agents for a specific job function. Instead of setting up each piece individually or re-explaining your workflow every session, you install a plugin once and Claude immediately knows the best practices, tools, and commands relevant to your role.

Think of it this way: base Cowork is a smart generalist. Plugins turn it into a specialist who already knows your domain.

Each plugin contains:

- **Skills:** domain knowledge Claude draws on automatically when relevant
- **Slash commands:** specific actions you trigger manually with /
- **Connectors:** MCP-based integrations that wire Claude to your actual tools
- **Sub-agents:** specialized agents that handle multi-step tasks within a workflow

---

### **What's the Difference Between a Plugin and a Skill?**

This comes up constantly so worth clearing up before we go further.<br>A **skill** is a single markdown file that teaches Claude how to do one specific thing. It fires automatically when relevant context appears. Skills are the building blocks.<br>A **plugin** is a complete package, skills, commands, connectors, and agents bundled together for a specific job function. When you install a plugin, you're installing an entire configured workflow, not just one instruction.<br>Think of it this way: a skill is a chapter. A plugin is the whole book, already indexed and ready to use.

---

---

### How to Install Claude Cowork Plugins

Installing a plugin takes under two minutes. Here's how:

**From the Cowork interface:**

1. Open the Claude Desktop app and switch to the Cowork tab
2. Click the **Customize** menu in the left sidebar
3. Browse available plugins and click install on the ones you want
4. Once installed, type `/` or click the `+` button to see available commands

**From the CLI:**

```shell
claude plugin marketplace add anthropics/knowledge-work-plugins
claude plugin install sales@knowledge-work-plugins
```
### **Uploading a custom plugin:**

If you've built your own plugin or downloaded one, go to Customize → upload the plugin file directly.

Searches across all your connected work tools simultaneously Slack, Teams, Outlook, Jira, everything.

Once installed, plugins activate automatically. Skills fire when relevant, slash commands become available in your session, and connected tools stay authorized across sessions, you only authorize once.

**Note:** If you're on a Team or Enterprise plan, your admin may have restricted which plugins you can install or auto-installed specific ones for your organization.

---

## Claude Cowork with Composio for Secure Connectors

Most plugins ship with Claude Connectors that let you connect Claude to data sources like Google Drive, Notion, and GitHub. However, what works for an individual quickly breaks down at enterprise scale. 

Connectors authenticate per-user but offer little visibility into what Claude actually does once connected — which records were read, which actions were taken, which data left the building. 

It works for individual use cases. However, for a 500-person company deploying Claude across legal, finance, and engineering teams, the absence of an audit trail is the difference between a sanctioned rollout and a security incident waiting to happen.

Composio addresses this gap by operating as a separate governance layer or an [MCP gateway](https://composio.dev/mcp-gateway) between AI tools and the [SaaS apps](https://composio.dev/toolkits) they connect to.

Check out here at [dashboard.composio.dev](https://dashboard.composio.dev)

## Best Plugins for Claude Cowork in 2026

### **1. Enterprise Search Plugin for Claude Cowork**

I asked it a dumb test question: "What's the oldest unanswered customer question in our Slack?" It came back with a thread from 11 days ago where three people had tagged support but nobody replied. I answered it in 90 seconds. That alone was worth the install. The more tools you have connected, the more useful it gets.

**What it does:**

- Searches across all your connected work tools simultaneously Slack, Teams, Outlook, Jira, everything. 
- Deduplicates results and synthesizes answers with source citations
- Handles queries like "what did we decide about X last month" across email, chat, and docs
- Works best when more connectors are authorized, each new connection makes it stronger

**Key connectors:** [Google Drive](https://composio.dev/toolkits/googledrive), [Gmail](https://composio.dev/toolkits/gmail), [Slack](https://composio.dev/toolkits/slack), [Notion](https://composio.dev/toolkits/notion), [Microsoft 365](https://composio.dev/toolkits/dynamics365), [Confluence](https://composio.dev/toolkits/confluence)

**How to install:**

1. Open Claude Desktop → Cowork tab → Customize
2. Search "Enterprise Search" → click Install
3. Connect your tools from the Connectors menu
4. Type `/search` to run your first query

**Resource: **[github.com/anthropics/knowledge-work-plugins/tree/main/enterprise-search](https://github.com/anthropics/knowledge-work-plugins/tree/main/enterprise-search)

---

### **2. Sales Plugin for Claude Cowork**

I hate logging meeting notes in HubSpot. This plugin writes the note, creates the follow-up task, and updates the deal stage from one conversation. It caught that a prospect mentioned a budget cut and automatically flagged the deal as at risk, better than what I would have written myself. The /sales:prospect-research command pulled funding, news, and recent LinkedIn activity before a cold outreach. I walked in knowing the CEO had just posted about hiring a head of sales. Made the conversation completely different.

(Note: Salesforce integration may require Enterprise API access depending on your plan. HubSpot and Pipedrive may be easier to connect, but exact requirements depend on your workspace and CRM plan.)

**What it does:**

- Writes CRM notes, creates follow-up tasks, and updates deal stages from your conversation
- Researches prospects before calls, funding history, recent news, LinkedIn activity
- Flags deal risks automatically based on what was said in the conversation
- Drafts personalized follow-up emails from your call notes

**Key connectors:** HubSpot, Salesforce, Pipedrive, Slack

**How to install:**

1. Open Claude Desktop → Cowork tab → Customize
2. Search "Sales" → click Install
3. Connect your CRM via the Connectors menu
4. Key commands: `/sales:pipeline-review`, `/sales:forecast`, `/sales:follow-up`

**Resource: **[github.com/anthropics/knowledge-work-plugins/tree/main/sales](https://github.com/anthropics/knowledge-work-plugins/tree/main/sales)

---

### **3. Legal, Contract Review Before It Goes to Counsel**

I sent a vendor NDA through it before passing it to our lawyer. She came back and said it caught the two problem clauses but missed a jurisdiction issue she had to add herself. Then she said "honestly not bad" and moved on. She still reviewed it properly, but she spent seven minutes instead of twenty, because she already knew where to look.

**What it does:**

- Reviews contracts clause by clause using GREEN/YELLOW/RED flagging
- Triages NDAs into key terms, unusual clauses, and recommended changes
- Handles compliance checks, vendor agreement reviews, and risk assessments
- Separates standard boilerplate from the parts that actually need human attention

**Key connectors:** Slack, Box, Egnyte, Microsoft 365, Jira, Pramata

**How to install:**

1. Open Claude Desktop → Cowork tab → Customize
2. Search "Legal" → click Install
3. Connect your document storage (Box, Egnyte, or M365) via Connectors
4. Key commands: `/legal:review-contract`, `/legal:triage-nda`, `/legal:risk-assessment`

**Resource: **[github.com/anthropics/knowledge-work-plugins/tree/main/legal](https://github.com/anthropics/knowledge-work-plugins/tree/main/legal)

---

### **4. Finance, I Don't Work in Finance and Still Used It**

I run a small side consulting thing. Quarterly I have to reconcile expenses across three credit cards and a business bank account. Normally that's a Saturday morning of cursing. I connected the Finance plugin to my BigQuery read-only, asked for a variance analysis, and it spat out a spreadsheet with actual vs expected by category. Then I asked it to turn that into a slide deck for my partner. It did. I did not touch Excel or PowerPoint. That's not nothing.

**What it does:**

- Reconciles accounts and prepares journal entries for close workflows
- Runs variance analysis across actual vs expected spend by category
- Connects directly to data warehouses for live financial data
- Works with Excel/PowerPoint cross-app workflow, analysis straight to presentation

**Key connectors:** [Snowflake](https://composio.dev/toolkits/snowflake), Databricks, BigQuery, Slack, Microsoft 365

**How to install:**

1. Open Claude Desktop → Cowork tab → Customize
2. Search "Finance" → click Install
3. Connect your data warehouse via the Connectors menu (read-only recommended)
4. Key commands: `/finance:reconciliation`, `/finance:variance-analysis`, `/finance:close-status`

**Resource: **[github.com/anthropics/knowledge-work-plugins/tree/main/finance](https://github.com/anthropics/knowledge-work-plugins/tree/main/finance)

---

### **5. Data and Analytics Plugin for Cowork**

Our PM can't write SQL. I don't mean that as a dig. She just shouldn't have to. She asked me one day "what's our 30 day retention for users who finished onboarding last quarter?" I showed her how to type that exact sentence into Claude with the Data plugin connected. It wrote the query, ran it, and built a chart. She didn't need me. That felt like a small promotion for her and a small demotion for me, which is exactly the point.

Read-only mode is configurable. Don't let anyone tell you otherwise.

**What it does:**

- Translates plain English questions into SQL queries and runs them
- Builds charts and dashboards from query results automatically
- Supports complex joins, statistical analysis, and data validation
- Configurable read-only mode to prevent accidental writes to production

**Key connectors:** Snowflake, Databricks, BigQuery, Hex, Amplitude, Jira

**How to install:**

1. Open Claude Desktop → Cowork tab → Customize
2. Search "Data" → click Install (or install from [claude.com/plugins/data](https://claude.com/plugins/data))
3. Connect your data warehouse via Connectors, set read-only access first
4. Key commands: `/data:write-query`, `/data:analyze`

**Resource: **[github.com/anthropics/knowledge-work-plugins/tree/main/data](https://github.com/anthropics/knowledge-work-plugins/tree/main/data) · Also on [claude.com/plugins/data](https://claude.com/plugins/data)

---

### **6. Marketing research plugin for Cowork**

Our content person spends every Monday morning pulling competitor news. I told her to try /marketing:competitive-brief for our main rival. It pulled their last three product launches, a pricing change, and a recent exec interview into a structured brief. She said "that's basically what I do manually until 10am." Now she starts at 10am and spends the first hour on actual strategy.

Some integrations are still being rolled out. Check Anthropic's connector docs for the latest on what's available for your stack.

**What it does:**

- Generates competitive briefs from recent news, product launches, and positioning changes
- Drafts content in your brand voice connected to your content calendar
- Pulls campaign performance data and surfaces actionable insights
- Can publish drafts to WordPress, configure a review step before enabling this

**Key connectors:** Google Analytics, HubSpot, Slack, WordPress, Notion

**How to install:**

1. Open Claude Desktop → Cowork tab → Customize
2. Search "Marketing" → click Install
3. Connect Google Analytics and HubSpot via Connectors
4. Key commands: `/marketing:competitive-brief`, `/marketing:content-brief`, `/marketing:metrics-review`

**Resource: **[github.com/anthropics/knowledge-work-plugins/tree/main/marketing](https://github.com/anthropics/knowledge-work-plugins/tree/main/marketing)

---

### **7. HR Plugin for Cowork**

We were hiring a customer support lead. I used /hr:job-description and fed it our standard template plus a few notes about the role. The plugin added a line about "Spanish fluency preferred" that I had not mentioned. I almost deleted it. Then I remembered our support volume from Latin America had doubled in six months. I left it in. The candidate we hired spoke Spanish. That line is why she applied. I would have forgotten it entirely.

The onboarding plan feature is fine. But that one catch is why I keep the plugin around.

**What it does:**

- Drafts job descriptions that go beyond generic templates, catches requirements you might overlook
- Generates offer letters, onboarding plans, and performance review summaries
- Keeps hiring documentation consistent in structure and tone across roles
- Covers the full employee lifecycle from recruitment through performance management

**Key connectors:** Slack, Microsoft 365, Google Drive

**How to install:**

1. Open Claude Desktop → Cowork tab → Customize
2. Search "HR" → click Install
3. Connect Slack and Google Drive via Connectors
4. Key commands: `/hr:job-description`, `/hr:onboarding-plan`, `/hr:performance-summary`

**Resource: **[https://github.com/anthropics/knowledge-work-plugins/tree/main/human-resources](https://github.com/anthropics/knowledge-work-plugins/tree/main/human-resources)

---

### **8. Engineering and documentation plugin for Claude Cowork**

I asked our lead engineer what he thought of this plugin. He said "I used it to write an ADR for our database migration and posted it to Confluence without opening Confluence. I didn't hate the experience." That's engineering praise. Trust me.

The /engineering:incident-writeup command turns a Slack post-mortem into a structured document. We used it after a small outage last week. Someone just typed what happened, Claude made it look official, and we moved on.

**What it does:**

- Generates architecture decision records (ADRs) from technical context and notes
- Turns Slack post-mortems into structured incident write-ups
- Writes spec documents and posts directly to GitHub wikis, Confluence, or Jira
- Generates release notes and internal engineering updates from commit or PR context

**Key connectors:** GitHub, Jira, Confluence, Slack, Linear

**How to install:**

1. Open Claude Desktop → Cowork tab → Customize
2. Search "Engineering" → click Install
3. Connect GitHub and Confluence via Connectors
4. Key commands: `/engineering:write-adr`, `/engineering:draft-spec`, `/engineering:incident-writeup`

**Resource: **[github.com/anthropics/knowledge-work-plugins/tree/main/engineering](https://github.com/anthropics/knowledge-work-plugins/tree/main/engineering)

---

### **9. Productivity plugin for Claude Cowork**

I wake up to 40 Slack messages, 12 emails, and a calendar that's already changed twice. The /update command scans everything from the last 12 hours and tells me what actually needs attention. Yesterday it surfaced a client who'd moved a deadline in a late night email, I would have missed that until the afternoon. Before a hard conversation last week, /meeting-prep pulled relevant docs, recent Slack threads, and attendee context. Changed how I opened.

**What it does:**

- Scans email, Slack, and calendar overnight and surfaces what actually needs action
- Prepares meeting briefs from calendar links, docs, context, attendee background
- Creates and updates tasks based on what came in since your last session
- Works for end-of-day summaries too, not just morning catch-ups

**Key connectors:** Slack, Gmail, Google Calendar, Notion, Jira, Asana, Microsoft 365

**How to install:**

1. Open Claude Desktop → Cowork tab → Customize
2. Search "Productivity" → click Install
3. Connect Slack, Gmail, and Google Calendar via Connectors
4. Key commands: `/update`, `/meeting-prep`, `/competitive-brief`

---

### **10. Plugin Create meta-plugin for Claude Cowork**

I tried to build a plugin for my client QBR process, pull metrics from BigQuery, grab last quarter's Notion notes, draft a QBR deck in one command. It worked about 80%. The Notion connector was finicky and I spent an hour on a permission issue. Haven't shipped it yet. But I know exactly what I'm building next quarter.

<br>That's when I hit the real limit. A full QBR workflow needs to update Salesforce, create a Google Slide, and send a Slack reminder. It stopped feeling like a plugin and started feeling like integration work. That's when I started using Composio. it gives Claude access to 1,000+ apps through a single integration so I can chain actions across tools without building every connector myself. I still start with the Cowork plugins. But when a workflow needs to cross apps or actions the official set doesn't cover, Composio is the next layer I add.

**What it does:**

- Walks you through building a custom plugin from scratch using Claude itself
- Generates plugin structure, skills, connectors, slash commands, from plain English descriptions
- File-based output (markdown and JSON), no coding required for basic workflows
- Pairs with Claude Code for teams wanting to build more sophisticated plugins programmatically

**How to install:**

1. Open Claude Desktop → Cowork tab → Customize
2. Search "Plugin Create" → click Install
3. No connectors needed upfront, you configure them as part of building your plugin
4. Type `/plugin-create` to start building

**Resource: **[github.com/anthropics/knowledge-work-plugins/tree/main/cowork-plugin-management](https://github.com/anthropics/knowledge-work-plugins/tree/main/cowork-plugin-management)

---

## Community Plugins and Tools Worth Knowing About

Beyond Anthropic's official collection, there's a growing directory of community and partner-built plugins at [claude.com/plugins](https://claude.com/plugins). A few worth checking:

### **CoworkPowers, Systematic Knowledge Work Workflows**

This plugin adds four systematic workflow commands that tackle knowledge work, drafting communications, making decisions, preparing for meetings, in a structured way that gets better over time.

The **compound** feature is what makes it different. Every completed task feeds patterns, templates, and preferences back into the system. The next similar task is faster and more accurate because it learned from the last one.

**What it does:**

- `/workflow-research` – runs parallel agents to gather context before any work begins, including a context-gatherer and stakeholder-mapper
- `/workflow-work` – executes the plan with specialized agents once research is complete
- `/workflow-review` – multi-agent quality review from multiple perspectives before output is finalized
- `/workflow-compound` – extracts patterns and preferences from completed tasks so future work improves automatically

**How to install:**

```plain text
# Option 1 – from marketplace
/plugin install coworkpowers@coworkpowers

# Option 2 – from GitHub
git clone https://github.com/nabeelhyatt/coworkpowers.git
claude --plugin-dir ./coworkpowers
```
Or download ZIP from GitHub → Cowork → Plugins → + → Upload Plugin → drag and drop ZIP.

No connectors required to start. Works with web search and local files alone. Connect MCPs for richer context.

**Resources:** [github.com/nabeelhyatt/coworkpowers](https://github.com/nabeelhyatt/coworkpowers)

---

### **MSApps Plugin Collection, 26 Production-Ready Plugins**

The largest open-source plugin marketplace for Claude. 26 plugins built on the **SOSA security framework** (Supervised Orchestrated Secured Agents), a four-pillar methodology that addresses the trust problem most community plugins ignore: hardcoded credentials, no confirmation on destructive actions, no concept of agent boundaries.

**Standout plugins from the collection:**

- `whatsapp-mcp` – Send messages, search conversations, manage business outreach. Includes persistent memory that learns each contact's communication style across sessions.
- `google-drive-upload` – Upload files to Google Drive directly from a Cowork conversation.
- `youtube-transcription` – Transcribe any YouTube video and work with the content.
- `wordpress` – Manage posts, users, and WooCommerce from Claude.
- `apollo` – Prospect research and outreach via Apollo.
- `mac-cleanup` – Fix disk-full errors in Cowork VMs and Claude Code sandboxes.
- `sosa-compliance-checker` – Audits all your installed plugins against the SOSA security framework, not just MSApps ones.

**How to install:**

bash

```plain text
# Add the marketplace
/plugin marketplace add MSApps-Mobile/claude-plugins

# Install any individual plugin
/plugin install google-drive-upload@msapps-plugins
/plugin install whatsapp-mcp@msapps-plugins
/plugin install youtube-transcription@msapps-plugins
```
**Resources:** [github.com/MSApps-Mobile/claude-plugins](https://github.com/MSApps-Mobile/claude-plugins) \| [msapps.mobi/plugins](https://msapps.mobi/plugins)

### **What I Actually Run Right Now**

Enterprise Search, Sales, Data, and Productivity. That's my daily stack. Legal is installed but I only use it when a contract shows up. Finance stays because of my side thing. HR stays because of the Spanish fluency thing. The others come and go.

The thing that surprised me most: the Data plugin is the one non-technical people on my team ask about. They don't want to learn SQL. They just want answers. That's the real use case Anthropic nailed.

One honest limitation: the plugins are open source and free to install, but the setup for some connectors is not trivial. Don't install Enterprise Search and expect magic if none of your tools are connected yet. Start with clean access to the tools your team already uses.

Start with one plugin. Use it for a week. Then add another. The base Claude Cowork experience is smart. But with the right plugins, it stops feeling like a generic assistant and starts feeling like someone who's been on your team for months.

My rule now: use Cowork plugins for common role-based workflows, and use Composio when the workflow needs to cross tools, APIs, or actions the default plugin set does not cover.

I didn't believe that until I tried it myself. Now I do.

---

#### The Full Breakdown

<table header-row="true">
<tr>
<td>Plugin</td>
<td>Best For</td>
<td>Key Connectors</td>
<td>Plugin Availability</td>
</tr>
<tr>
<td>Enterprise Search</td>
<td>Searching across all your tools at once</td>
<td>Google Drive, Slack, Notion, Gmail, M365</td>
<td>All paid plans</td>
</tr>
<tr>
<td>Sales</td>
<td>Prospect research, CRM updates, deal management</td>
<td>HubSpot, Salesforce, Pipedrive</td>
<td>All paid plans</td>
</tr>
<tr>
<td>Legal</td>
<td>Contract review, NDA triage, compliance checks</td>
<td>Box, M365, Pramata, Egnyte</td>
<td>All paid plans</td>
</tr>
<tr>
<td>Finance</td>
<td>Reporting, reconciliation, variance analysis</td>
<td>Snowflake, BigQuery, M365</td>
<td>All paid plans</td>
</tr>
<tr>
<td>Data</td>
<td>Natural language queries, dashboards, analysis</td>
<td>Snowflake, BigQuery, Hex, Amplitude</td>
<td>All paid plans</td>
</tr>
<tr>
<td>Marketing</td>
<td>Campaigns, content briefs, performance reporting</td>
<td>HubSpot, Google Analytics, WordPress</td>
<td>All paid plans</td>
</tr>
<tr>
<td>HR</td>
<td>Job descriptions, onboarding, performance docs</td>
<td>Slack, M365, Google Drive</td>
<td>All paid plans</td>
</tr>
<tr>
<td>Engineering</td>
<td>ADRs, specs, documentation to GitHub/Confluence</td>
<td>GitHub, Jira, Confluence, Slack</td>
<td>All paid plans</td>
</tr>
<tr>
<td>Productivity</td>
<td>Daily task updates, meeting prep, briefings</td>
<td>Slack, Gmail, Notion, Jira, Calendar</td>
<td>All paid plans</td>
</tr>
<tr>
<td>Plugin Create</td>
<td>Building custom plugins for your specific workflow</td>
<td>Anything you configure</td>
<td>All paid plans</td>
</tr>
</table>
### Final Take

The way I’d think about Claude Cowork plugins is simple: don’t install ten of them and hope your work magically gets easier.

Pick the one annoying workflow you keep putting off. The thing you do every week, even though some part of you knows it should not still be this manual. Maybe it’s meeting prep. Maybe it’s CRM updates. Maybe it’s pulling numbers for someone who asks the same question every month.

Start there.

That’s where these plugins actually make sense. Not as a giant “AI transformation” thing, but as a small fix for the parts of work that quietly eat your day. The best ones do not feel flashy. They just remove a few extra tabs, a few repeated explanations, and a few moments where you would have had to stop what you were doing to hunt for context.

Just be careful with access. Connect only what the plugin really needs, test it on low-risk work first, and do not assume every connector is ready for your exact setup on day one.

For me, the value became obvious when Claude stopped feeling like a chatbot I had to brief from scratch and started feeling like something that understood where my work already lived. That is the real difference.

---

## FAQs

### What's the difference between a Claude Cowork plugin and a skill?

A **skill** is a single markdown file that teaches Claude how to do one specific thing — it fires automatically when relevant context appears. A **plugin** is a complete package: skills, slash commands, connectors, and sub-agents bundled together for a specific job function. The article's framing: a skill is a chapter, a plugin is the whole book — already indexed and ready to use. You install a plugin once and Claude immediately knows the best practices, tools, and commands relevant to that role, instead of you re-explaining your workflow every session.

### Which Cowork plugins should I install first?

The author's daily stack is **Enterprise Search, Sales, Data, and Productivity** — those four come up over and over. Legal stays installed but only fires when a contract shows up. Finance and HR earn their slot through specific moments (a quarterly side-business reconciliation, a job description that surfaced a Spanish-fluency requirement nobody had thought to add). The article's actual recommendation: don't install ten plugins and hope work gets easier. Pick the one annoying weekly workflow you keep putting off — meeting prep, CRM updates, the same number-pull every month — and start there.

### Do I need Composio if Claude Cowork plugins already have connectors built in?

For individual use, the built-in connectors are fine. The article makes the case that they break down at enterprise scale: connectors authenticate per-user but offer little visibility into what Claude actually does once connected — which records were read, which actions were taken, which data left the building. For a 500-person company rolling Cowork across legal, finance, and engineering, that absence of an audit trail is the difference between a sanctioned rollout and a security incident waiting to happen. Composio acts as a separate governance layer / [MCP gateway](https://composio.dev/mcp-gateway) for that case — and the author also reaches for it when a workflow needs to cross apps in ways the official plugin set doesn't cover (e.g., a full QBR that spans BigQuery, Notion, Salesforce, Google Slides, and Slack).

### Are Cowork plugins worth installing if I'm not on a paid plan or my admin has restricted them?

Plugins are open source and free to install, but Cowork itself is on Anthropic's paid tiers, and Team or Enterprise plan admins can restrict which plugins users can install or auto-install specific ones for the whole org. Practical reading: if you're on a paid plan as an individual, you're free to install whatever's useful. If you're inside an Enterprise tenant, check what's already provisioned before going hunting — your IT team may have already curated a set, and the value of plugins like Enterprise Search and Data scales with how many tools are properly authorized, which is usually an admin call anyway.
