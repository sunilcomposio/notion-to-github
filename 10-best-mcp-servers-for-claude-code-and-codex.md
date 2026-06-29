# 10 Best MCP Servers for Claude Code and Codex

MCPs had been the talk of the town for the better part of 2025 and every one was going gaga over how good the paradigm was. But then something changed. AI models became really good at coding, much less hallucinations, and the rise of coding harnesses and sandboxes. Which meant agents can effectively write code themselves reliably to call API endpoints using CLI tools. Tool calling became second fiddle. For GitHub, Claude Code could wire gh CLI in a sandbox to chain complex actions and for repeatable flows have a [Skill.md](http://Skill.md) file.

Anthropic introduced Programmatic tool calling, Cloudflare introduced Code mode, Composio introduced CLI tool for SaaS integrations.

However, all said and done MCP still makes a lot of sense when you are interacting with services that lack a well-designed CLI tools or the ones that are hard to interact with codes.

## MCP vs CLI: When to choose what?

Use a **CLI with Skills **when:

- The service already has a mature CLI.
- The workflow needs to be repeatable in CI.
- You need composability of actions.
- The task is mostly execution: deploy, migrate, test, lint, scan, generate types.
- The agent can get clean JSON output from the command.
- You want the workflow to remain visible as scripts in your repo.

Use an **MCP server** when:

- You want narrower permissions than giving the agent your full shell environment. This can be fatal.
- When you need complete audit trail of your coding agents. What was executed, when, and how.
- The service does not have a good CLI.
- The agent needs live, authenticated context from an external system.
- The output is easier to consume as structured tool results than terminal text.
- The workflow spans messy product, design, browser, production, or organizational context.
- The agent needs OAuth-backed access without manually handling tokens and API glue.

So, keeping that in mind I curated a list of MCP servers for Claude Code and Codex that actually make sense in 2026 and when to use it’s CLI version over MCP.

## TL;DR: MCP servers covered in this post

| MCP server | Best for | When to use it |
| --- | --- | --- |
| **Composio MCP Gateway** | Multi-app orchestration | Use it as the default when your workflow spans Linear, GitHub, Slack, Sentry, Notion, Gmail, Drive, Calendar, or other SaaS tools. |
| **Chrome DevTools MCP** | Real-time browser debugging | Use it for screenshots, console logs, network traces, DOM inspection, and performance debugging. |
| **Context7 MCP** | Fresh framework docs | Use it when writing code against fast-moving libraries like Next.js, Tailwind, or other APIs where model knowledge may be stale. |
| **Playwright MCP** | Visual UI and E2E testing | Use it when the agent needs to click through flows, inspect browser state, and verify UI changes. |
| **Figma MCP** | Design-to-code context | Use it when frontend work depends on Figma frames, design tokens, spacing, typography, and layout details. |
| **Sentry MCP** | Production debugging | Use it when the agent needs real stack traces, errors, releases, traces, and production issue context. |
| **Google Workspace MCP** | Team docs and communication context | Use it to pull requirements, approvals, schedules, docs, sheets, and emails into the coding workflow. |
| **Linear MCP** | Ticket-driven engineering | Use it when the agent needs to read issues, understand acceptance criteria, update status, or sync work with tickets. |
| **Atlassian MCP** | Jira and Confluence workflows | Use it for enterprise teams where specs, tickets, comments, and planning live in Jira or Confluence. |
| **Notion MCP** | Product specs and internal knowledge | Use it when PRDs, launch plans, meeting notes, decision logs, and engineering docs live in Notion. |
| **Supabase MCP** | Backend and database work | Use it for schema work, SQL, logs, RLS, auth, and Supabase project context. |
| **Neon MCP** | Postgres branching | Use it for isolated database branches, migration testing, query inspection, and safe DB experimentation. |
| **Vercel MCP** | Deployments and frontend hosting | Use it for deployments, preview environments, environment variables, domains, and project monitoring. |
| **Cloudflare MCP** | Edge infrastructure | Use it for Workers, KV, D1, R2, Pages, DNS, and edge deployment context. |
| **AWS MCP** | Cloud inspection with IAM controls | Use it for structured AWS resource inspection, debugging, and controlled cloud context. |
| **GKE / Kubernetes MCP** | Cluster inspection | Use it for pods, workloads, events, rollout status, and read-only Kubernetes debugging. |

## How to Install MCP Servers in Claude Code and Codex

Claude Code uses the `claude mcp` command.

For a remote MCP server:

```bash
claude mcp add --transport http <name> <url>
```

Example:

```bash
claude mcp add --transport http notion <https://mcp.notion.com/mcp>
```

For a local command-based server:

```bash
claude mcp add <name> <command> <args...>
```

Example:

```bash
claude mcp add playwright npx @playwright/mcp@latest
```

Then open Claude Code and run:

```plain text
/mcp
```

Use `/mcp` to authenticate, inspect, and manage connected MCP servers.

Useful commands:

```bash
claude mcp list
claude mcp remove <name>
```

---

## How to Install MCP Servers in Codex

Codex usually uses `~/.codex/config.toml`.

For a remote MCP server:

```toml
[mcp_servers.notion]
url = "<https://mcp.notion.com/mcp>"
```

Then authenticate:

```bash
codex mcp login notion
```

For a local command-based server:

```toml
[mcp_servers.playwright]
command = "npx"
args = ["@playwright/mcp@latest"]
```

You can also add servers from the CLI:

```bash
codex mcp add posthog --url <https://mcp.posthog.com/mcp>
codex mcp add playwright -- npx @playwright/mcp@latest
```

Useful commands:

```bash
codex mcp list
codex mcp login <name>
codex mcp remove <name>
```

## Best MCP servers for Claude Code and Codex in 2026

---

---

### 1. [Composio MCP Gateway](https://dashboard.composio.dev): Connect any app to Claude Code and Codex

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/10-best-mcp-servers-for-claude-code-and-codex/image_1.png)

Composio MCP is different from most servers on this list.

Most MCP servers connect your agent to one app: Linear, Slack, GitHub, Notion, Gmail, etc. Composio acts more like a **tool router** across many apps. Instead of installing and configuring 20 separate MCP servers, you connect Composio once and let the agent discover, authenticate, and execute actions across supported SaaS tools.

This makes the most sense when your workflow crosses multiple apps.

Example:

```markdown
Read the Linear ticket, check the related Sentry issue, 
find the Slack discussion, create a GitHub branch, and update the Notion 
launch doc.
```

Claude Code or Codex could do this with separate MCPs, CLIs, and API scripts. But Composio makes the workflow cleaner by handling auth, tool discovery, schemas, and cross-app execution from one place.

**Why it makes sense over installing everything separately**

The problem with too many MCP servers is context bloat. Your agent gets a huge list of tools, many of which are irrelevant to the current task.

Composio avoids that by exposing a smaller set of meta-tools. The agent searches for the right integration, checks whether the app is connected, asks you to authenticate if needed, fetches the schema, and then executes the action.

That is a better pattern for large multi-app workflows.

**Use it for**

- Connecting Claude Code or Codex to many SaaS apps from one place.
- Managing OAuth, API keys, scopes, and token refresh.
- Discovering tools dynamically instead of preloading every app.
- Running cross-app workflows.
- Executing actions in parallel across multiple connected tools.
- Avoiding one-off MCP setup for every service.

**Installation for Claude Code**

```markdown
claude mcp add --scope user --transport http composio https://connect.composio.dev/mcp
```

This will start an OAuth flow. Complete the authentication and you’re done.

Connect any app you want by asking in the chat interface.

```markdown
Connect to Gmail and Notion via Composio
```

Click on the auth link and finish the authentication and Codex can now talk to your apps.

**Installation for Codex**

```markdown
codex mcp add composio --url https://connect.composio.dev/mcp
```

Similar to Claude Code this will start an OAuth flow. Complete the authentication and you’re done.

The connection process is same as Claude Code.

Managed and Hosted Composio MCP Server: [Composio Claude Code](https://composio.dev/toolkits/composio/framework/claude-code) \| [Composio Codex](https://composio.dev/toolkits/composio/framework/codex)

### 2. Chrome DevTools MCP - Real-Time Browser Debugging

[![ChromeDevTools/chrome-devtools-mcp](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/10-best-mcp-servers-for-claude-code-and-codex/og-chromedevtools-chrome-devtools-mcp.png)](https://github.com/ChromeDevTools/chrome-devtools-mcp)

This one I actually used to build [Documint](https://documint-demo.vercel.app/).

The problem is that AI writes the frontend code completely blind. It can't see what the CSS actually renders like, and it certainly can't inspect network requests or read your console errors in real time.

Chrome DevTools MCP fixes the issue by offering direct browser debugging to the IDE.

This means that ai agents could now take screenshots, inspect the network waterfall, read console logs with source maps, and verify his own work before asking you for feedback.

After careful inspection, I found that most of the PDF data is split across pages because there is no enforced schema, so I added one, and Chrome DevTools helped me handle the boundary calculations.

For me, this is the game-changer for frontend-heavy projects. Instead of the AI guessing what a form looks like, it can now see it in real time.

**Why it makes sense over CLI/code**

You can write Puppeteer scripts. You can run tests. You can dump HTML. But browser debugging is interactive, visual, and stateful. Chrome DevTools MCP gives the agent access to the same runtime signals a frontend engineer uses every day.

**Usage**

- Verify the DOM structure and CSS layout by having the AI take screenshots after each code change.
- Debug network issues by inspecting requests, response bodies, and headers directly.
- Analyze performance traces and get actionable suggestions for optimization.
- Handle form interactions, button clicks, and user flows in real time without writing Puppeteer code.

**Installation Claude**

```shell
claude mcp add chrome-devtools npx chrome-devtools-mcp@latest
```

**Installation Codex**

Add this to your Codex configuration file (**`~/.codex/config.toml`**)

```shell
[mcp_servers.chrome-devtools]
command = "npx"
args = ["chrome-devtools-mcp@latest"]
```

**Setup**

Chrome DevTools MCP requires an active Chrome instance with remote debugging enabled. Start Chrome with:

```shell
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome \
  --remote-debugging-port=9222 \
  --user-data-dir=/tmp/chrome-mcp-profile
```

Learn more at: [https://github.com/ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp)

### 3. Context7 - AI Framework Optimizer

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/10-best-mcp-servers-for-claude-code-and-codex/image_2.png)

AI models are trained on old data. If you ask them to write code for Next.js 15 or Tailwind v4, they will confidently hallucinate outdated APIs and break your app.

This server fixes that by fetching real-time, version-specific documentation and injecting only the relevant snippets into your context window.

**Usage**

- Eliminate framework hallucinations for bleeding-edge libraries.
- Fetch real-time quick start guides and documentation.
- Prevent context window bloat by only pulling the exact functions needed, unlike massive web scrapes.

**<span discussion-urls="discussion://369f261a-6dfe-80fc-bed7-d99e8b7b13d6/cf997f7c-4ffa-4c51-be4c-ca47401ac2f1/36df261a-6dfe-80fe-a3b9-001c1eb73b62">Prompt</span>**

**Installation Claude**

```shell
claude mcp add --scope user --header "CONTEXT7_API_KEY: YOUR_API_KEY" --transport http context7 https://mcp.context7.com/mcp
```

Get your api key at: [context7.com/dashboard](https://context7.com/dashboard) (you need to log in)/signup → Profile → API Keys → Copy & Paste)

**Installation Codex**

Add this to your Codex configuration file (**`~/.codex/config.toml`** )

```shell
[mcp_servers.context7]
url = "https://mcp.context7.com/mcp"
http_headers = { "CONTEXT7_API_KEY" = "YOUR_API_KEY" }
```

Then validate

```javascript
codex mcp list
```

You can explore more at: [https://github.com/upstash/context7-mcp](https://github.com/upstash/context7-mcp)

---

### 4. Playwright MCP - Visual UI/UX Testing Suite

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/10-best-mcp-servers-for-claude-code-and-codex/image_3.png)

We all know, AI writes UI code completely blind. It guesses what the CSS will look like, which usually results in broken layouts.

This server spins up a real headless browser and lets AI navigate to your local server, click buttons, read console errors, and visually verify its own work. Great for frontend development work.

**Usage**

- Automate end-to-end testing without writing manual test scripts.
- Visually verify DOM elements and layout structures.
- Debug client-side React errors by having the AI read the actual browser console.

**Installation Claude**

```shell
claude mcp add playwright npx @playwright/mcp@latest
```

**Installation Codex**

Add this to your Codex configuration file (**`~/.codex/config.toml`**** )**

```shell
[mcp_servers.playwright]
command = "npx"
args = ["@playwright/mcp@latest"]
```

Learn more at: [https://playwright.dev/docs/getting-started-mcp](https://playwright.dev/docs/getting-started-mcp)

### 5. Figma MCP: **Design Context That Agents Can Actually Use**

If you are a frontend engineer, you know the pain of translating design mocks into pixel-perfect frontend code, as it involves endless back-and-forth toggling between Figma and your IDE.

This server allows the AI to pull exact layout data, colors, typography, and spacing directly from a Figma file URL.

This is exactly the kind of problem MCP is good at. Figma is visual, structured, authenticated, and not naturally CLI-friendly.

**Why it makes sense over CLI/code**

There is no equivalent of figma inspect this frame and give me the useful implementation context in a normal developer workflow. You can use the Figma API, but the agent would have to write custom code, handle auth, parse design JSON, and decide what matters.

Figma MCP turns that messy design graph into agent-usable context.

**Usage**

- Extract exact design systems (hex codes, shadow tokens, font scales) from a design file.
- Generate implementation-ready React or Tailwind components directly from a node ID.
- Keep frontend builds perfectly consistent with the designer's intent.

**Installation Claude**

```plain text
claude plugin install figma@claude-plugins-official
```

After installing, open Claude Code and run `/plugin`, select `figma`, and complete the authentication flow.

**Installation Codex**

Add this to your Codex configuration file (**`~/.codex/config.toml`**)

```plain text
codex mcp add figma --url https://mcp.figma.com/mcp
```

When prompted, authenticate with Figma.

I use this one almost every day. Learn more at: [https://www.figma.com/mcp-catalog/](https://www.figma.com/mcp-catalog/)

Managed and Hosted Figma MCP Server: [Figma Claude Code](https://composio.dev/toolkits/figma/framework/claude-code) \| [Figma Codex](https://composio.dev/toolkits/figma/framework/codex)

### 6. Sentry MCP - For Serious Production Debugging

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/10-best-mcp-servers-for-claude-code-and-codex/image_4.png)

Sentry MCP is a must-have if your agent works on production software.

The repo tells the agent what the code is supposed to do. Sentry tells it what the code is actually doing in the wild.

When a production error comes in, the useful context is scattered: stack traces, releases, suspect commits, user impact, frequency, environment, breadcrumbs, request data, and sometimes traces. Without Sentry MCP, you usually copy-paste the stack trace into the agent and hope it has enough context.

With Sentry MCP, the agent can inspect the issue directly, map the stack trace to the codebase, understand when it started, and propose a fix based on live production evidence.

**Why it makes sense over CLI/code**

Production debugging is context-heavy. The agent needs to understand which errors matter, where they occur, how often they happen, and what changed around the same time. Also, for enterprises audit logs are important.

Sentry MCP gives the agent structured production context without forcing you to manually paste logs and screenshots into the chat.

**Usage**

- Pull live production stack traces into the IDE.
- Investigate errors by release, environment, and frequency.
- Correlate production issues with recent code changes.
- Generate targeted hotfixes.
- Verify that a fix addresses the actual failing path.

**Installation Claude**

```plain text
claude plugin marketplace add getsentry/sentry-mcp
claude plugin install sentry-mcp@sentry-mcp
```

**Installation Codex**

Add this to your Codex configuration file (**`~/.codex/config.toml`**)

```plain text
[mcp_servers.sentry]
command="npx"
args= ["-y","mcp-remote@latest","https://mcp.sentry.dev/mcp"]
```

On first use, it will open the OAuth flow and prompt you to connect your Sentry account. Once done now it can pull issues, traces, and production errors directly from Sentry.

Get your debugging pipeline started by visiting: [https://github.com/sentry/mcp-server](https://github.com/sentry/mcp-server)

Managed and Hosted Sentry MCP Server: [Sentry Claude Code](https://composio.dev/toolkits/sentry/framework/claude-code) \| [Sentry Codex](https://composio.dev/toolkits/sentry/framework/codex)

## Team Context MCP Server

### 7. Google Workspace MCP - Context Assembly from Team Docs

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/10-best-mcp-servers-for-claude-code-and-codex/image_5.png)

This server gives your agent access to Gmail, Google Drive, Docs, Sheets, Slides, Calendar, Chat, and Forms through a single interface. It's the most complete Google Workspace MCP available for retrieving context from multiple files

I felt its need when, I was building a feature and my agent asked me “if we have a budget approval for the expense”.

I had to leave the IDE, search through all the gmail, fetch the multiple expense excel sheet and paste each one in the agent context.

At that time it was not present, but now google did it and we have gog workspace.

**Usage**

- Search Gmail across multiple labels and threads to find requirements or past decisions.
- Fetch and read Google Docs to inject design specs, requirements, or architectural decisions into the context.
- Query Google Sheets to pull budget data, feature matrices, or roadmaps.
- Check Calendar availability to avoid scheduling conflicts in automated workflows.
- Create Docs and Sheets on the fly to draft proposals or generate reports.

**Installation Claude**

```html
claude mcp add workspace-mcp uvx workspace-mcp --transport streamable-http --tools gmail drive calendar docs sheets
```

Set your Google OAuth credentials:

Use [Composio integrations](https://compoaio.dev/toolkits) if you don’t want to create and maintain personal client ID and secret.

```html
export GOOGLE_CLIENT_ID="your_client_id.apps.googleusercontent.com"
export GOOGLE_CLIENT_SECRET="your_client_secret"
```

**Installation Codex**

Add this to your Codex configuration file (**`~/.codex/config.toml`**)

```html
[mcp_servers.google-workspace]
command = "uvx"
args = ["workspace-mcp", "--transport", "streamable-http", "--tools", "gmail", "drive", "calendar", "docs", "sheets"]
env = {
  "GOOGLE_CLIENT_ID" = "your_client_id.apps.googleusercontent.com",
  "GOOGLE_CLIENT_SECRET" = "your_client_secret"
}
```

Learn more at: [https://github.com/taylorwilsdon/google_workspace_mcp](https://github.com/taylorwilsdon/google_workspace_mcp)

Managed and hosted Google Services MCP servers: [https://composio.dev/toolkits?search=google](https://composio.dev/toolkits?search=google)

### 8. Linear MCP - Ticket Tracking

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/10-best-mcp-servers-for-claude-code-and-codex/image_6.png)

If you're working in a team, you know managing project boards distracts from actually writing code.

You have to read a ticket, switch contexts, write the code, and then go back to update the board.

This server lets the AI read your backlog, understand the constraints, write the code, and update the ticket status for you.

**Usage**

- Read ticket requirements and acceptance criteria directly into the AI's context.
- Update task statuses automatically when a PR is opened.
- Sync your local development workflow tightly with your project management board.

**Installation Claude**

```plain text
claude mcp add--transport http linear-server https://mcp.linear.app/mcp
```

Then open Claude Code and run:

```plain text
/mcp
```

Select Linear and complete the authentication flow.

**Installation Codex**

Add this to your Codex configuration file (**`~/.codex/config.toml`**)

```plain text
[features]
experimental_use_rmcp_client=true

[mcp_servers.linear]
url="https://mcp.linear.app/mcp"
```

Then authenticate:

```plain text
codex mcp login linear
```

Learn more at [https://linear.app/docs/mcp](https://linear.app/docs/mcp)

Managed and Hosted Linear MCP Server: [Linear Claude Code](https://composio.dev/toolkits/linear/framework/claude-code) \| [Linear Codex](https://composio.dev/toolkits/linear/framework/codex)

### **9. Atlassian MCP - Jira and Confluence for Enterprise Context**

Atlassian MCP is the Linear alternative for teams that live in Jira and Confluence.

In many companies, the real source of truth is not the GitHub issue. It is a Jira ticket linked to a Confluence spec, connected to an incident note, tied to a release plan, and discussed across comments.

That context is painful to move manually into an agent session. Atlassian MCP lets Claude Code and Codex search and summarize Jira, Jira Service Management, Confluence, Bitbucket, and Compass without leaving the IDE.

This is especially useful in larger teams where engineering work is surrounded by process, compliance, approvals, and documentation.

**Why it makes sense over CLI/code**

Jira and Confluence have APIs, but wiring them manually is tedious. The useful context is spread across issues, comments, pages, links, statuses, projects, and permissions.

Atlassian MCP turns that into a structured interface the agent can use directly.

For agents, this is exactly where MCP shines: authenticated organizational context that is too messy to fetch cleanly with a few shell commands.

**Usage**

- Read Jira tickets and acceptance criteria before coding.
- Search Confluence for specs, decision records, and architecture docs.
- Summarize long tickets or planning pages.
- Create Jira work items from meeting notes or bug reports.
- Update ticket status and add implementation comments.
- Pull Bitbucket context if your team uses Atlassian’s code tooling.

**Installation Claude**

```javascript
claude mcp add --transport http atlassian https://mcp.atlassian.com/v1/mcp/authv2`
```

Then run:

```javascript
/mcp
```

Complete the Atlassian OAuth flow.

**Installation Codex**

Add this to \~/.codex/config.toml:

```javascript
[mcp_servers.atlassian]
url = "https://mcp.atlassian.com/v1/mcp/authv2"
```

Then authenticate:

```javascript
codex mcp login atlassian
```

Learn more: [**https://support.atlassian.com/atlassian-rovo-mcp-server/docs/getting-started-with-the-atlassian-remote-mcp-server/**](https://support.atlassian.com/atlassian-rovo-mcp-server/docs/getting-started-with-the-atlassian-remote-mcp-server/)

Managed and hosted MCP servers for Atlassian Services: [https://composio.dev/toolkits?search=atlassian](https://composio.dev/toolkits?search=atlassian)

### **9. Notion MCP - Product Specs and Internal Knowledge**

Notion MCP is useful when your team’s product and engineering knowledge lives in Notion.

This includes PRDs, launch plans, engineering specs, meeting notes, user research, decision logs, roadmap docs, onboarding notes, and internal wikis.

Agents are already good at reading code. But code usually tells you what exists, not why it exists. Notion MCP gives the agent access to the surrounding product context that shaped the code in the first place.

If you regularly paste Notion pages into Claude Code or Codex before asking for implementation help, this MCP is worth considering.

**Why it makes sense over CLI/code**

Notion is structured, but not in a way that is pleasant to access from a normal coding workflow.

You can use the Notion API, but agents then need to deal with page IDs, blocks, permissions, databases, and formatting. Notion MCP gives them a more natural way to search, read, and update workspace content.

The value is not raw API access. The value is turning internal knowledge into usable coding context.

**Usage**

- Search product specs before implementing a feature.
- Pull meeting notes and decision logs into context.
- Draft technical plans from product docs.
- Generate release notes or project updates.
- Update implementation notes after work is complete.
- Create docs from completed agent work.

**Installation Claude**

```javascript
claude mcp add --transport http notion https://mcp.notion.com/mcp
```

**Installation Codex**

Add this to \~/.codex/config.toml:

```javascript
[mcp_servers.notion]
url = "https://mcp.notion.com/mcp"
```

Then authenticate:

```javascript
codex mcp login notion
```

Learn more: [**https://developers.notion.com/guides/mcp/overview**](https://developers.notion.com/guides/mcp/overview)

Managed and Hosted Notion MCP Server: [Notion Claude Code](https://composio.dev/toolkits/notion/framework/claude-code) \| [Notion Codex](https://composio.dev/toolkits/notion/framework/codex)

## **Stack-Dependent MCP Servers**

These MCP servers are not must-installs. They are worth it only when they match the stack you actually use.

This is where people often over-install MCPs. Supabase, Neon, Vercel, Cloudflare, AWS, and Kubernetes all have strong CLIs or APIs. Claude Code and Codex can use those directly.

The case for MCP here is cleaner platform context: structured results, narrower permissions, OAuth/IAM guardrails, fewer brittle shell commands, and safer inspection of live state.

Use these MCPs when the agent repeatedly needs to inspect platform state, debug deployments, reason about database branches, check logs, or understand infra before editing code.

Use CLIs with Skills, when the workflow is repeatable and belongs in scripts or CI.

### **11. Supabase MCP: Supabase-Aware Backend Work**

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/10-best-mcp-servers-for-claude-code-and-codex/image_7.png)

For a backend/data engineer, writing complex SQL migrations and manually managing database schemas wastes a ton of time.

This server serves as the brain of the app’s backend, with AI handling everything from creating tables to configuring Row Level Security (RLS) policies directly.

If you are dealing with serious production issues, adding this one can greatly help fix them.

**Usage**

- Automate the full database and schema setup from plain-English descriptions.
- Write and analyze complex SQL joins and views.
- Read database logs to instantly find and fix failed queries.

**Installation Claude**

```bash
claude mcp add --scope project --transport http supabase "https://mcp.supabase.com/mcp"
```

**Installation Codex**

Add this to your Codex configuration file (**`~/.codex/config.toml`**** )**

```shell
[mcp_servers.supabase]
url = "https://mcp.supabase.com/mcp"
```

Managed and Hosted Supabase MCP Server: [Supabase Claude Code](https://composio.dev/toolkits/supabase/framework/claude-code) \| [Supabase Codex](https://composio.dev/toolkits/supabase/framework/codex)

---

### 12. Neon MCP - Database Branching for AI-Driven Development

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/10-best-mcp-servers-for-claude-code-and-codex/image_8.png)

Think of it as your AI-native database management. With this one, your agent can provision, branch, migrate, and query databases with zero human intervention.

I have used it for some of the projects, and it really does its job well. Here is a sample example of how you can use this one:

Run a migration query on a production branch. agent + neon mcp then :

- creates an isolated branch of production, 
- runs the migration on that branch, 
- creates sample data, 
- runs performance tests, 
- and shows the results. 

If I approve, it applies the migration to production. If something breaks, I just reset the branch, and the production database is untouched.

**Usage**

- Create isolated database branches for each feature or AI task.
- Run SQL migrations on branches before applying them to production.
- Execute queries and analyze query plans to optimize performance.
- Manage database users and authentication through natural language.
- Spin up temporary databases for testing and instantly destroy them.

**Installation Claude**

bash

```shell
npx neonctl init --agent claude
```

This automatically sets up OAuth and creates an API key. Claude will guide you through the rest.

**Installation Codex**

Add this to your Codex configuration file (**`~/.codex/config.toml`**)

```shell
[mcp_servers.neon]
command = "npx"
args = ["-y", "@neondatabase/mcp-server-neon", "start", "YOUR_NEON_API_KEY"]
```

**Setup**

Get your Neon API key from the dashboard at [https://console.neon.tech](https://console.neon.tech/) → Account → API Keys

Then, to test, point the agent to a full-stack project and prompt:

```plain text
Create a branch of my production database, add a users table with email and hashed password columns, create 100 sample users, run a query to check indexes, and reset to parent if anything fails.
```

Learn more at: [https://neon.com/docs/ai/neon-mcp-server](https://neon.com/docs/ai/neon-mcp-server)

Managed and Hosted Neon MCP Server: [Neon Claude Code](https://composio.dev/toolkits/neon/framework/claude-code) \| [Neon Codex](https://composio.dev/toolkits/neon/framework/codex)

---

### **13. **Vercel MCP - Deploy from Your IDE

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/10-best-mcp-servers-for-claude-code-and-codex/image_9.png)

A must install for those who constantly deploy their apps to vercel.

Vercel MCP lets agents deploy your frontend, manage environments, and run deployments directly from the IDE without touching Git.

The agent writes the feature, deploys to preview, runs E2E tests, and if they pass, promotes to production. All from inside my editor.

No more context-switching. No more dashboard checking. The agent handles the entire pipeline while you focus on what feature to add next.

**Usage**

- Deploy your app from the IDE without touching Git or CI/CD dashboards.
- Manage preview environments and promote changes to production.
- Configure environment variables and secrets.
- Monitor deployment status and rollback if issues arise.

**Installation Claude**

```bash
claude mcp add vercel https://mcp.vercel.com
```

Authenticate with Vercel token:

```bash
export VERCEL_TOKEN="your_token"
```

**Installation Codex**

Add this to your Codex configuration file (**`~/.codex/config.toml`**)

```bash
[mcp_servers.vercel]
url = "https://mcp.vercel.com"
```

Learn more at: [https://vercel.com/docs/mcp](https://vercel.com/docs/mcp)

Managed and Hosted Vercel MCP Server: [Vercel Claude Code](https://composio.dev/toolkits/vercel/framework/claude-code) \| [Vercel Codex](https://composio.dev/toolkits/vercel/framework/codex)

## 14. Cloudflare MCP: Edge Context

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/10-best-mcp-servers-for-claude-code-and-codex/image_10.png)

Cloudflare MCP gives ai agents access to Workers (serverless functions), KV (key-value storage), D1 (SQLite), and R2 (object storage) for deploying truly global infrastructure.

This means your AI agent can deploy to the edge, manage workers, configure KV stores, and handle database operations through natural language.

**Usage**

- Deploy serverless functions to the edge across 300+ data centers.
- Manage key-value storage for caching and sessions.
- Configure D1 SQLite databases for edge-native data.
- Handle R2 object storage for files and media.
- Set up Cloudflare Pages for static site deployment.

**Installation Claude**

```bash
claude mcp add cloudflare npx @cloudflare/mcp
```

Authenticate:

```bash
wrangler login
```

**Installation Codex**

Add this to your Codex configuration file (**`~/.codex/config.toml`**)

```bash
[mcp_servers.cloudflare]
command = "npx"
args = ["@cloudflare/mcp"]
```

Learn more at: [https://developers.cloudflare.com/mcp/](https://developers.cloudflare.com/mcp/)

Managed and Hosted Cloudflare MCP Server: [Cloudflare Claude Code](https://composio.dev/toolkits/cloudflare/framework/claude-code) \| [Cloudflare Codex](https://composio.dev/toolkits/cloudflare/framework/codex)

---

### **15. AWS MCP - Cloud Context With IAM Guardrails**

AWS MCP is useful when you want agents to inspect AWS through structured tools with IAM controls and auditability.

It is not a replacement for the AWS CLI, CDK, Terraform, or CloudFormation. It is best for controlled cloud inspection.

**Use it for**

- Inspecting AWS resources.
- Debugging infra issues.
- Pulling AWS service guidance.
- Giving agents narrower access.
- Auditing agent cloud interactions.

**Setup**

First authenticate with AWS:

```javascript
aws login
aws sts get-caller-identity
```

**Installation Claude**

```javascript
claude mcp add aws-mcp uvx mcp-proxy-for-aws@latest \
  https://aws-mcp.us-east-1.api.aws/mcp \
  --metadata AWS_REGION=us-west-2
```

Change AWS_REGION=us-west-2 to your region.

**Installation Codex**

Add this to \~/.codex/config.toml:

```javascript
[mcp_servers.aws_mcp]
command = "uvx"
args = [
  "mcp-proxy-for-aws@latest",
  "https://aws-mcp.us-east-1.api.aws/mcp",
  "--metadata", "AWS_REGION=us-west-2"
]
startup_timeout_sec = 60
```

**CLI alternative**

Use aws, CDK, Terraform, or CloudFormation for repeatable infra workflows.

Learn more: [**https://docs.aws.amazon.com/agent-toolkit/latest/userguide/getting-started-aws-mcp-server.html**](https://docs.aws.amazon.com/agent-toolkit/latest/userguide/getting-started-aws-mcp-server.html)

### **16. GKE / Kubernetes MCP - Cluster Inspection**

GKE or Kubernetes MCP is useful because cluster state is live and complex.

Agents can use kubectl, but a read-only MCP gives them a cleaner and safer way to inspect pods, workloads, events, services, deployments, and cluster health.

**Use it for**

- Inspecting failing pods.
- Checking rollout status.
- Reading workload metadata.
- Reviewing cluster events.
- Debugging infra vs code issues.

**Installation Claude**

```javascript
claude mcp add --transport http gke https://container.googleapis.com/mcp
```

Authenticate with your Google Cloud identity when prompted.

For safety, prefer read-only scope:

```javascript
https://www.googleapis.com/auth/container.read-only
```

**Installation Codex**

Add this to \~/.codex/config.toml:

```javascript
[mcp_servers.gke]
url = "https://container.googleapis.com/mcp"
```

Then authenticate:

```javascript
codex mcp login gke
```

For safety, use a separate Google Cloud identity for agents and grant read-only GKE access first.

**CLI alternative**

Use kubectl, gcloud, Helm, and CI/CD for controlled cluster operations.

Learn more: [**https://docs.cloud.google.com/kubernetes-engine/docs/how-to/use-gke-mcp**](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/use-gke-mcp)

---

---

## How to pick: when to use what

The easiest mistake is to install every MCP server that looks useful. Don’t do that. MCP servers are powerful, but every extra server adds tools, schemas, auth surfaces, and context overhead. The best setup is usually a small default layer plus task-specific servers that you enable only when the work demands it.

For most Claude Code and Codex workflows, start with **Composio MCP Gateway** as the default. It gives you one managed entry point for many apps instead of forcing your agent to juggle a dozen separate MCP servers.

That matters because the bottleneck is rarely “can the agent call a tool?” The real bottleneck is orchestration: finding the right app, checking auth, picking the right action, executing safely, and keeping the context window clean.

Use **Composio MCP Gateway** when:

- Your workflow touches more than one app: Linear + GitHub + Slack, Sentry + Notion + GitHub, Gmail + Drive + Calendar, etc.
- You want managed OAuth, API keys, scopes, token refresh, and connection handling instead of one-off setup for every service.
- You want the agent to discover the right tools at runtime instead of preloading every possible MCP into context.
- You care about auditability, secure auth, parallel execution, and reducing tool bloat.
- You are building repeatable agent workflows for a team, not just hacking together a one-off local setup.

Use a **single-purpose MCP server** when the task is deep inside one product and that product’s context is the whole job:

- **Chrome DevTools or Playwright** for browser debugging, screenshots, console logs, and UI verification.
- **Figma** when the agent needs design tokens, frames, layout data, and implementation details from a file.
- **Sentry** when the agent needs production errors, traces, releases, and stack traces.
- **Linear, Jira, or Notion** when the source of truth is a ticket, spec, roadmap, or internal doc.
- **Supabase, Neon, Vercel, Cloudflare, AWS, or GKE** when the task depends on live platform state, deployments, logs, databases, branches, or infrastructure context.

Use the **CLI instead of MCP** when the workflow is mostly deterministic and belongs in your repo:

- Tests, linting, builds, migrations, deploy scripts, type generation, and repeatable CI jobs.
- Anything that should be reviewed as code and rerun by humans or CI.
- Services with mature CLIs that return clean JSON and do not require messy cross-app context.

A good rule of thumb:

1. If the workflow spans multiple apps, use **Composio MCP Gateway** first.
2. If the workflow requires deep, live context from one product, add that product’s MCP server only for that task.
3. If the workflow is repeatable execution, use CLI + Skills.
4. If the server bloats context more than it improves decisions, turn it off.

The strongest setup is not “all MCPs always on.” It is **Composio as the orchestration layer**, a few specialist MCPs for high-context jobs, and CLIs for repeatable engineering execution.

Example:

1. Use **Composio MCP Gateway** to coordinate the workflow across Linear, Sentry, GitHub, Slack, and Notion.
2. Pull the Linear bug and related Sentry issue into context.
3. Let the agent write the fix, open the GitHub PR, and update the Notion implementation note.
4. Send a Slack update when the PR is ready.

This keeps the agent focused, avoids context bloat, and still gives it access to the systems where the real engineering context lives.
