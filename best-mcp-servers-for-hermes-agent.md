# Best MCP Servers for Hermes Agent in September 2026

A few days ago, I was using Hermes Agent on a project and wanted it to work beyond local files and the terminal. That led me into Hermes MCP and the growing number of servers available for GitHub, databases, browsers, and external apps.

I started testing a few, removing the ones that duplicated Hermes' native tools or skills, and keeping the ones that actually improved the workflow. That's when I realised choosing MCP servers for Hermes is less about popularity and more about fit.

With more than 100,000 MCP servers now tracked across the ecosystem, generic “best MCP” lists are not very useful here.

So this guide covers 21 MCP servers across seven categories that I would actually consider using with Hermes, along with setup, tool filtering, and when each one makes sense.

Let’s start with a brief refresher on MCP before moving into setup.

## What are MCP servers?

Model Context Protocol gives agents a standard way to discover and call tools from external systems.

An MCP server can expose GitHub operations, database queries, browser controls, CRM records, payment actions, or other capabilities through the same protocol.

Hermes acts as the MCP client.

You define servers under `mcp_servers` in `~/.hermes/config.yaml`, and Hermes discovers their tools and adds them to its normal tool registry.

You can also browse the Nous-reviewed catalogue with `hermes mcp` and install supported entries without building every configuration yourself.

That sounds straightforward, but Hermes gives you a few choices that change how I would build the stack.

## How Hermes handles MCP vs native tools and skills

I would keep native Hermes tools for capabilities that already live close to the agent. Filesystem work, terminal commands, and other built-in operations usually need less setup and avoid another MCP process or network connection.

Skills solve a different problem. They give Hermes instructions and procedures that load when needed, so a skill can be a better fit when you only need to teach the agent how to use a CLI or follow a repeatable workflow.

MCP becomes useful when an external service has a structured tool surface that Hermes should call directly. Hermes also lets you narrow that surface with `tools.include`, `tools.exclude`, and controls for MCP resources and prompts.

And once a workflow starts touching several authenticated SaaS apps, I would at least compare separate MCP servers with a managed layer such as Composio. It can reduce the OAuth and maintenance work, while focused MCP servers still give you tighter control over one important service.

That distinction matters because a server can look useful on a directory and still add very little to a real Hermes setup. So I ranked them on more than popularity.

## How I ranked these MCP servers in September 2026

No single score tells you whether an MCP server belongs in Hermes.

I used these signals together:

- **MCP Toplist score:** version count, release and commit activity, GitHub stars, and listing age all contribute to its ranking.
- **Official ownership:** I preferred first-party servers with a maintained official option.
- **Hermes fit:** Hermes catalogue presence, clean stdio or HTTP support, and useful tool controls all helped.
- **Maintenance:** active repositories, current documentation, and recent releases mattered.
- **Tool surface:** smaller or filterable servers are easier for an agent to use.
- **Authentication:** OAuth, restricted credentials, read-only modes, and clear permission documentation helped.
- **Real utility:** the server needed to give Hermes a meaningful new capability.
- **Security:** servers with broad access received more scrutiny, especially for communication, finance, and production data.

MCP Toplist currently aggregates several major MCP directories, and its latest snapshot tracks more than 106,000 servers.

With that filter in place, we can move from the ecosystem to the servers I would actually consider installing.

## Best MCP servers by category

## Automation

Automation is a good place to start because it shows the difference between giving Hermes another tool and giving it an entire workflow engine.

These three solve different parts of that problem.

These two solve different parts of that problem.

| Server | Best for | Hermes setup |
| --- | --- | --- |
| **Composio** | Multi-app actions and workflows | Remote MCP |
| **Browser Use** | Browser automation | Remote or local MCP |

### 1. Composio

**When you need Hermes to work across many apps from one MCP connection.**

Composio Connect gives Hermes access to 1,500+ apps through one hosted MCP server. Instead of loading every app action into context, it exposes seven meta-tools that let the agent search for tools, connect accounts, get schemas, and execute actions when needed.

That makes it useful for tasks that span services. Hermes can find the required actions at runtime, and Composio handles the connected accounts and execution.

**Install<br><br>**You do not need to manually edit the Hermes MCP configuration. You can point your Hermes agent to [composio.dev/hermes](https://composio.dev/hermes)

Paste this into Hermes:

```plain text
Set up Composio from composio.dev/hermes
```

Hermes reads the setup instructions and adds Composio as an HTTP MCP server.

When Hermes needs an app for the first time, Composio starts the OAuth flow. You approve the connection in your browser, and that connection stays available for later sessions.<br><br>Or if you want to go the old way

Add your Composio API key to `~/.hermes/.env`, then add the remote server to `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  composio:
    url: "https://connect.composio.dev/mcp"
    headers:
      x-consumer-api-key: "${COMPOSIO_API_KEY}"
```

Hermes supports remote MCP URLs, custom headers, and `${VAR}` references from its environment file.

Hermes even supports per-server allowlists, so only the selected MCP tools enter its tool registry, so you can use seven meta-tools provided by composio.

Or you can narrow tool access further in Hermes if you do not need the remote workbench or Bash tools:

```yaml
tools:
  include:
    - COMPOSIO_SEARCH_TOOLS
    - COMPOSIO_GET_TOOL_SCHEMAS
    - COMPOSIO_MANAGE_CONNECTIONS
    - COMPOSIO_WAIT_FOR_CONNECTIONS
    - COMPOSIO_MULTI_EXECUTE_TOOL
```

**When to use**

- Hermes needs to work across several SaaS applications.
- You want one MCP connection instead of configuring many app servers.
- You want tools to load when Hermes needs them.
- You want Composio to handle app authentication and connected accounts.

**When not to use**

- One direct MCP already covers the task.
- Hermes only needs a local CLI or built-in tool.
- You want every integration and credential layer to run on your own infrastructure.

**Official source:** Composio Connect documentation and Hermes MCP documentation.

### 2. Browser Use

**High-level browser automation when a proper API does not exist.**

Browser Use currently sits near the top of the MCP Toplist and provides both hosted and local MCP options. It can take a browser task from Hermes and handle the interaction instead of making Hermes reason through every page action itself.

I would keep it for sites where browser automation is actually necessary. If an API or first-party MCP exists, I would usually start there.

**Install**

Hosted:

```yaml
mcp_servers:
browser_use:
url:"https://api.browser-use.com/mcp"
headers:
x-browser-use-api-key:"${BROWSER_USE_API_KEY}"
```

Local:

```yaml
mcp_servers:
browser_use:
command:"uvx"
args:
-"--from"
-"browser-use[cli]"
-"browser-use"
-"--mcp"
```

**Tool filter**

```yaml
tools:
include:
- browser_task
- monitor_task
```

**When to use**

- The website has no usable API.
- Hermes needs to complete a multi-page browser task.
- You want high-level browser delegation.

**When not to use**

- A structured API already exists.
- The browser contains unrelated sensitive sessions.
- You only need browser debugging.

That last case naturally leads to developer tooling, where browser control serves a very different purpose.

## Developer tooling

For coding agents, I would avoid installing every development MCP I can find. Hermes already has strong local capabilities, so an MCP should give it information or state that is otherwise hard to reach.

These three clear that bar.

| Server | Best for | Hermes setup |
| --- | --- | --- |
| **GitHub MCP** | Repositories, issues, PRs | Remote or local |
| **Chrome DevTools MCP** | Runtime browser debugging | Local |
| **Playwright MCP** | Browser testing | Local |

### 4. GitHub MCP Server

**Highest-value external MCP for a coding-focused Hermes profile.**

GitHub maintains its own MCP server for repositories, issues, pull requests, Actions, search, and other GitHub operations. It also supports toolsets, read-only modes, and individual tool controls.

That works well with Hermes because you can restrict the server first and then apply another allowlist inside Hermes.

**Install**

```yaml
mcp_servers:
github:
url:"https://api.githubcopilot.com/mcp/"
auth:
type: oauth
```

**Tool filter**

```yaml
tools:
include:
- search_code
- list_issues
```

**When to use**

- Hermes works across several repositories.
- Issues and PRs are part of the coding loop.
- You want structured GitHub operations.

**When not to use**

- `git` and `gh` already cover everything.
- Hermes only works inside one local repository.
- A CLI skill gives you the same workflow with less tool surface.

**Official source:** GitHub MCP Server repository.

### 5. Chrome DevTools MCP

**One of the strongest options for debugging a running web application.**

Chrome DevTools MCP currently ranks at the top of MCP Toplist. It lets coding agents inspect and control a live Chrome session, including runtime state and performance information.

That makes it more useful than generic browser automation when Hermes is debugging something that already exists in the browser.

**Install**

```yaml
mcp_servers:
chrome_devtools:
command:"npx"
args:
-"-y"
-"chrome-devtools-mcp@latest"
```

**Tool filter**

```bash
hermes mcp configure chrome_devtools
```

Keep only the inspection and debugging operations you use regularly.

**When to use**

- You debug front-end applications.
- Runtime browser information matters.
- Hermes needs access to DevTools state.

**When not to use**

- You only need simple navigation.
- Playwright tests already show the problem.
- The connected browser profile contains sensitive unrelated accounts.

**Official source:** Chrome DevTools documentation.

### 6. Playwright MCP

**Structured browser interaction from the team that maintains Playwright.**

Microsoft’s Playwright MCP exposes browser interaction through structured page state and accessibility snapshots. It works well for navigation, testing, forms, browser state, and repeatable web interactions.

There is one useful caveat. Microsoft now points coding agents toward the Playwright CLI plus skills for some workflows because that approach uses less context.

So I would install the MCP when persistent browser state or richer browser introspection actually helps.

**Install**

```yaml
mcp_servers:
playwright:
command:"npx"
args:
-"-y"
-"@playwright/mcp@latest"
```

**Tool filter**

```yaml
tools:
include:
- browser_navigate
- browser_snapshot
- browser_click
```

**When to use**

- Hermes needs persistent browser state.
- Browser testing forms part of the development loop.
- Structured page inspection is useful.

**When not to use**

- Playwright CLI already solves the workflow.
- Hermes only needs a few scripted checks.
- Chrome DevTools MCP already covers the same task.

Developer tools mostly affect code and browsers. Commerce MCPs cross into customer data, orders, and money, so I would reduce the exposed surface even further.

## Commerce and shopping

| Server | Best for | Hermes setup |
| --- | --- | --- |
| **Shopify Storefront MCP** | Product discovery and carts | Remote |
| **Stripe MCP** | Payments and billing | Remote |
| **PayPal MCP** | PayPal merchant workflows | Local or remote |

### 7. Shopify Storefront MCP

**First-party access to product and shopping flows without broad Shopify Admin access.**

Shopify provides Storefront MCP endpoints for stores. They can expose catalogue search, products, policies, carts, and related customer-facing commerce operations.

That makes Storefront MCP a better starting point than giving Hermes broad Admin API permissions when the agent only needs to help someone shop.

**Install**

```yaml
mcp_servers:
shopify_storefront:
url:"https://YOUR_STORE.myshopify.com/api/mcp"
```

For UCP catalogue tools:

```yaml
mcp_servers:
shopify_catalog:
url:"https://YOUR_STORE.myshopify.com/api/ucp/mcp"
```

**Tool filter**

```yaml
tools:
include:
- search_catalog
- lookup_catalog
- get_product
```

**When to use**

- Hermes needs product information.
- You are building a shopping assistant.
- Admin access is unnecessary.

**When not to use**

- You need store administration.
- Inventory management is the primary workflow.
- Direct application code already handles the storefront interaction.

**Official source:** Shopify Storefront MCP documentation.

### 8. Stripe MCP

**First-party access to one of the most common billing and payment systems.**

Stripe hosts an official remote MCP server and supports OAuth-based client access. That gives Hermes structured access to Stripe operations without requiring an unofficial wrapper.

Payments still deserve a very small allowlist. Start with read operations, and add writes only after the workflow is clear.

**Install**

```yaml
mcp_servers:
stripe:
url:"https://mcp.stripe.com"
auth:
type: oauth
```

**Tool filter**

```bash
hermes mcp configure stripe
```

Start with customers, invoices, and other required read operations.

**When to use**

- Billing support needs current Stripe data.
- Hermes works with subscriptions or invoices.
- You want Stripe’s hosted MCP.

**When not to use**

- Reports already exist in your warehouse.
- Hermes does not need direct payment-system access.
- The available permissions are broader than the workflow.

**Official source:** Stripe AI and MCP documentation.

### 9. PayPal MCP

**First-party PayPal merchant operations with sandbox support.**

PayPal maintains an MCP implementation for merchant workflows such as invoicing. It supports local setup and separates sandbox from production environments.

That separation makes it practical to test the agent before exposing real financial actions.

**Install**

```yaml
mcp_servers:
paypal:
command:"npx"
args:
-"-y"
-"@paypal/mcp"
-"--tools=all"
env:
PAYPAL_ACCESS_TOKEN:"${PAYPAL_ACCESS_TOKEN}"
PAYPAL_ENVIRONMENT:"SANDBOX"
```

**Tool filter**

```yaml
tools:
include:
- list_invoices
- get_invoice
```

**When to use**

- Your business already uses PayPal.
- Hermes needs invoice or merchant context.
- You want a sandbox-first setup.

**When not to use**

- Stripe already covers the payment workflow.
- Hermes only needs exported financial data.
- You have no reason to expose merchant operations.

Money is the obvious sensitive surface, but communication MCPs can expose an equally important asset: private company conversations.

## Communication

I would rarely connect a broad communication server to the same Hermes profile that handles production systems or financial actions.

Profiles are useful here.

| Server | Best for | Hermes setup |
| --- | --- | --- |
| **Slack MCP** | Team conversations | Remote |
| **Google Workspace MCP** | Gmail and Workspace | Remote |
| **Microsoft Teams MCP** | Microsoft 365 teams | Remote |

### 10. Slack MCP

**Gives access to the system where many teams already keep day-to-day context.**

Slack provides an official MCP server for supported AI clients. It can search messages and files, retrieve conversations, work with channels, and perform supported communication actions.

That makes it useful for research and team context, but I would enable message sending separately from search and retrieval.

**Install**

Connect the Slack remote MCP through its supported OAuth flow, then register it in the Hermes profile that needs Slack.

**Tool filter**

```bash
hermes mcp configure slack
```

Start with search and retrieval tools.

**When to use**

- Important company knowledge lives in Slack.
- Hermes needs current conversation context.
- You want it to assist with team communication.

**When not to use**

- You only need a static export.
- Hermes does not need to send messages.
- The same profile has unrelated high-risk tools.

**Official source:** Slack MCP documentation.

### 11. Google Workspace MCP

**MCP endpoints for Gmail and other Workspace services.**

Google now provides remote MCP servers for Workspace services including Gmail, Drive, Calendar, Chat, and People through its Developer Preview program.

I would still connect only the service the workflow needs. For a communication agent, Gmail is a much cleaner starting point than enabling the whole Workspace surface.

**Install**

```yaml
mcp_servers:
gmail:
url:"https://gmailmcp.googleapis.com/mcp/v1"
auth:
type: oauth
```

**Tool filter**

```bash
hermes mcp configure gmail
```

Keep search and read actions first, then add drafts or writes when the workflow requires them.

**When to use**

- Gmail is central to the workflow.
- You want Google’s own remote MCP.
- Your environment can use the current preview.

**When not to use**

- You need several unrelated SaaS apps at once.
- You cannot use the current preview.
- Another existing integration already covers the same Gmail actions.

This is one place where a managed layer such as Composio can make sense if Gmail is only one of many apps Hermes needs. I would compare that against maintaining separate OAuth flows for each service rather than defaulting to it.

**Official source:** Google Workspace MCP documentation.

### 12. Microsoft Teams MCP

**Teams access for Microsoft 365 environments.**

Microsoft maintains a remote MCP server for Teams through its MCP catalogue. It can expose chats, channels, users, and messages through Microsoft Graph.

It fits best when the organisation already uses Microsoft identity, Graph permissions, and Teams as its main communication layer.

**Install**

```yaml
mcp_servers:
teams:
url:"https://agent365.svc.cloud.microsoft/agents/tenants/YOUR_TENANT_ID/servers/mcp_TeamsServer"
auth:
type: oauth
```

**Tool filter**

```bash
hermes mcp configure teams
```

Keep message and channel reads narrow before enabling writes.

**When to use**

- Your company runs on Teams.
- Microsoft Graph permissions already exist.
- Hermes needs current internal communication context.

**When not to use**

- You only need email.
- Teams does not contain important workflow state.
- The agent has no reason to communicate on your behalf.

Once Hermes can read customer conversations and team discussions, CRM access becomes much more useful because it adds business context.

## Sales and marketing

| Server | Best for | Hermes setup |
| --- | --- | --- |
| **HubSpot MCP** | CRM workflows | Remote |
| **Salesforce DX MCP** | Salesforce data and development | Local |
| **Linear MCP** | Customer and product follow-through | Remote |

### 13. HubSpot MCP

**Official CRM access with OAuth and live sales data.**

HubSpot provides a remote MCP server for CRM information including contacts, deals, engagements, and related objects.

That gives a Hermes sales agent the current CRM state instead of forcing it to work from exports or stale context.

**Install**

```yaml
mcp_servers:
hubspot:
url:"https://mcp.hubspot.com"
auth:
type: oauth
```

**Tool filter**

```bash
hermes mcp configure hubspot
```

Start with contact and deal reads. Add CRM updates only when the agent’s job requires them.

**When to use**

- HubSpot is your main CRM.
- Hermes needs current sales context.
- Contact and deal information drives other workflows.

**When not to use**

- You only need a couple of HubSpot actions alongside many other apps.
- The team has not defined allowed CRM writes.
- Another existing managed integration already handles HubSpot cleanly.

**Official source:** HubSpot remote MCP documentation.

### 14. Salesforce DX MCP

**Why it ranks: strong official tooling for controlled Salesforce access.**

Salesforce maintains its Salesforce DX MCP Server and groups capabilities into toolsets for data, metadata, users, testing, and other Salesforce operations.

The server can expose many tools, so Salesforce recommends enabling only the toolsets you need. That advice fits Hermes particularly well.

**Install**

```yaml
mcp_servers:
salesforce:
command:"npx"
args:
-"-y"
-"@salesforce/mcp@latest"
-"--orgs"
-"DEFAULT_TARGET_ORG"
-"--toolsets"
-"data,users"
-"--allow-non-ga-tools"
```

**Tool filter**

Use Salesforce toolsets first, then apply Hermes filtering on top.

```yaml
tools:
include:
- run_apex_test
```

**When to use**

- Salesforce is central to the business.
- You need Salesforce-specific metadata or development operations.
- You want precise control over available toolsets.

**When not to use**

- You only need a few standard CRM actions.
- The Salesforce CLI workflow is already enough.
- Hermes does not need direct Salesforce access.

**Official source:** Salesforce DX MCP Server.

### 15. Linear MCP

**Why it ranks: useful when customer requests need to turn into actual product or engineering work.**

Linear is not a CRM, but it often sits immediately after one. Its official MCP server lets Hermes find, create, and update issues, projects, comments, and related work.

Linear also provides a read-only endpoint, which I would start with for research and status workflows.

**Install**

Read and write:

```yaml
mcp_servers:
linear:
url:"https://mcp.linear.app/mcp"
auth:
type: oauth
```

Read-only:

```yaml
mcp_servers:
linear:
url:"https://mcp.linear.app/mcp/readonly"
auth:
type: oauth
```

**Tool filter**

```yaml
tools:
include:
- list_issues
- get_issue
```

**When to use**

- Customer feedback becomes product work.
- Hermes needs project status.
- You want a read-only starting point.

**When not to use**

- You need a real CRM.
- Your sales process never touches Linear.
- Project data already exists somewhere else Hermes can access.

CRM systems tell Hermes what the business is doing. Database MCPs let it inspect the data underneath those workflows.

## Data

| Server | Best for | Hermes setup |
| --- | --- | --- |
| **MCP Toolbox for Databases** | Multiple database engines | Local |
| **Supabase MCP** | Supabase development | Remote |
| **Snowflake MCP** | Enterprise analytics | Remote |

### 16. MCP Toolbox for Databases

**Why it ranks: one maintained server that covers a wide range of databases.**

Google’s MCP Toolbox for Databases ranks inside the current MCP Toplist top 10 and supports PostgreSQL, BigQuery, MySQL, SQL Server, Oracle, MongoDB, Redis, Snowflake, and other systems.

It also supports predefined toolsets, which help you expose a smaller database surface to Hermes.

**Install**

For PostgreSQL:

```yaml
mcp_servers:
postgres:
command:"npx"
args:
-"-y"
-"@toolbox-sdk/server"
-"--prebuilt=postgres/data"
-"--stdio"
```

**Tool filter**

```yaml
tools:
include:
- list_tables
- execute_sql
```

For production systems, I would prefer restricted database users and predefined queries where possible.

**When to use**

- You work with several database engines.
- Schema inspection is a regular agent task.
- You want a maintained general-purpose database MCP.

**When not to use**

- Hermes only needs one fixed query.
- A business-level API already exposes safer operations.
- You cannot give Hermes restricted database credentials.

**Official source:** Google MCP Toolbox for Databases.

### 17. Supabase MCP

**Why it ranks: one of the most useful first-party MCPs for modern application backends.**

Supabase maintains a remote MCP server for projects, tables, SQL, logs, branches, configuration, and other development surfaces.

It also supports feature groups and project scoping, so you can reduce what a specific Hermes profile can reach.

**Install**

```yaml
mcp_servers:
supabase:
url:"https://mcp.supabase.com/mcp?features=database,debugging,development"
auth:
type: oauth
```

**Tool filter**

```bash
hermes mcp configure supabase
```

Then keep only the operations the agent needs.

**When to use**

- Your application runs on Supabase.
- Hermes works on backend development.
- You want project-scoped access.

**When not to use**

- Hermes only needs application-level data.
- You can't safely restrict production access.
- A narrow PostgreSQL MCP already solves the problem.

**Official source:** Supabase MCP documentation.

### 18. Snowflake-managed MCP

**Why it ranks: managed MCP access inside an enterprise data platform.**

Snowflake’s managed MCP server is generally available and runs within Snowflake’s environment. It can expose SQL execution along with Cortex Analyst, Cortex Search, Cortex Agents, and custom tools.

That makes it more suitable for existing Snowflake deployments than adding an unrelated database wrapper beside the warehouse.

**Install**

After creating the server in Snowflake:

```yaml
mcp_servers:
snowflake:
url:"${SNOWFLAKE_MCP_URL}"
auth:
type: oauth
```

**Tool filter**

```bash
hermes mcp configure snowflake
```

Keep only the Snowflake operations required by that workflow.

**When to use**

- Analytics already live in Snowflake.
- You need governed warehouse access.
- Cortex services form part of the workflow.

**When not to use**

- You only need a local database.
- A business API already answers the question.
- Your Snowflake role gives Hermes more access than it needs.

**Official source:** Snowflake managed MCP documentation.

The moment a data workflow reaches payments, expenses, or accounting records, I would separate it from the general data profile. That brings us to finance.

## Finance

| Server | Best for | Hermes setup |
| --- | --- | --- |
| **QuickBooks Online MCP** | Accounting | Local |
| **Brex MCP** | Spend and expenses | Remote |
| **Stripe MCP** | Billing and payments | Remote |

### 19. QuickBooks Online MCP

**Broad accounting access from an Intuit-maintained implementation.**

The QuickBooks Online MCP server exposes QuickBooks entities and financial reports through a local stdio server.

It also supports controls that disable write, update, and delete operations. I would use those before relying only on Hermes-level filtering.

**Install**

```yaml
mcp_servers:
quickbooks:
command:"node"
args:
-"/absolute/path/to/quickbooks-online-mcp-server/dist/index.js"
env:
QUICKBOOKS_CLIENT_ID:"${QUICKBOOKS_CLIENT_ID}"
QUICKBOOKS_CLIENT_SECRET:"${QUICKBOOKS_CLIENT_SECRET}"
QUICKBOOKS_ENVIRONMENT:"sandbox"
QUICKBOOKS_DISABLE_WRITE:"true"
QUICKBOOKS_DISABLE_UPDATE:"true"
QUICKBOOKS_DISABLE_DELETE:"true"
```

**Tool filter**

```bash
hermes mcp configure quickbooks
```

**When to use**

- Accounting workflows live in QuickBooks.
- Hermes needs invoice or reporting information.
- You can begin with read-oriented access.

**When not to use**

- You only need one exported report.
- You want a fully hosted server.
- OAuth maintenance is not worth the workflow.

**Official source:** QuickBooks Online MCP Server.

### 20. Brex MCP

**First-party access to company spend and expense data.**

Brex offers an official MCP server in early access for company spending, cards, expenses, reimbursements, and related information.

Its MCP access follows the authenticated user’s Brex permissions, which gives you an existing permission model to build on.

**Install**

```yaml
mcp_servers:
brex:
url:"https://api.brex.com/mcp"
auth:
type: oauth
```

**Tool filter**

```bash
hermes mcp configure brex
```

Begin with transaction and expense reads.

**When to use**

- Your company already runs Brex.
- Finance agents need current spend information.
- Existing Brex roles provide useful boundaries.

**When not to use**

- You cannot use the current early-access MCP.
- QuickBooks contains all the information you need.
- The workflow does not require direct spend-system access.

**Official source:** Brex MCP documentation.

### 21. Stripe MCP

**Billing data often belongs in both commerce and finance workflows.**

Stripe appears twice because the same system serves different jobs. A commerce profile may care about customers and payment operations, while a finance profile may focus on invoices, subscriptions, and billing activity.

I would keep those as separate Hermes profiles or separate allowlists rather than exposing every Stripe capability everywhere.

**Install**

```yaml
mcp_servers:
stripe:
url:"https://mcp.stripe.com"
auth:
type: oauth
```

**Tool filter**

```bash
hermes mcp configure stripe
```

Keep the finance profile focused on billing and reporting operations.

**When to use**

- Stripe drives billing.
- Finance needs current subscription or invoice state.
- Direct Stripe data is required.

**When not to use**

- Your warehouse already contains enough reporting data.
- Hermes does not need direct billing-system access.
- The available credentials remain too broad.

**Official source:** Stripe MCP documentation.

By this point, the pattern is clearer. The problem is no longer finding another MCP server. It is deciding how many separate servers you actually want Hermes to maintain.

## Starter packs for Hermes users

I would not start by installing one server from every category.

These three packs are closer to how I would build the stack in practice.

### Minimal Safe Start

Use this when you are trying MCP with Hermes for the first time.

Start with:

1. GitHub
2. One project or data server
3. One external business service

For example:

```yaml
mcp_servers:
github:
url:"https://api.githubcopilot.com/mcp/"
auth:
type: oauth
tools:
include:
- search_code
- list_issues

linear:
url:"https://mcp.linear.app/mcp/readonly"
auth:
type: oauth

playwright:
command:"npx"
args:
-"-y"
-"@playwright/mcp@latest"
```

This gives Hermes a useful external surface without turning every interaction into a large tool-selection problem.

If you later find yourself adding several separate SaaS connections mainly for authentication and basic actions, that's when I would compare a managed integration layer such as Composio against maintaining each MCP separately.

### Developer Power Pack

Use this when Hermes acts mainly as a coding agent.

I would start with:

- GitHub MCP
- Chrome DevTools MCP
- Linear MCP
- Playwright only when persistent browser interaction is useful

You may not need both Chrome DevTools and Playwright in every session. And if Playwright CLI plus a skill solves the workflow, that is often the smaller setup.

The main idea is to make every development MCP earn its place.

### Full Business Agent Pack

For a business agent, I would design the stack around systems rather than apps.

A practical version could be:

- n8n for durable workflows
- One database MCP
- One communication or CRM MCP
- One finance MCP only when the agent genuinely needs finance access

If the agent has to authenticate separately with Gmail, Slack, HubSpot, Notion, Linear, and several similar SaaS tools, a managed layer such as Composio can reduce the connection and token-refresh work.

I would still keep databases, browsers, and other high-control systems on focused integrations where that gives you better boundaries.

That is also why performance starts to matter quickly.

## Performance and multi-MCP reality on Hermes

The first few MCP servers usually feel harmless. The problem appears when each server brings another group of tool schemas into the agent’s decision space.

Hermes gives you filtering controls because this can affect both context use and tool-selection quality.

A few practices make a large difference:

- **Keep only the servers that profile uses.**
- **Use ****`tools.include`**** aggressively.**
- **Use server-side filtering too.** GitHub toolsets, Salesforce toolsets, Supabase feature groups, and Zapier action selection all help.
- **Disable MCP resources and prompts when you do not need them.**

```yaml
tools:
resources:false
prompts:false
```

- **Prefer a skill or CLI for simple procedures.**
- **Avoid exposing the same capability through several servers.**
- **Expect remote MCP latency and failures.** The MCP endpoint and the target API both become part of the request path.
- **Keep broad SaaS access consolidated when separate servers add more maintenance than control.**

This matters even more with smaller local models. A model that already struggles to select between ten similar tools usually will not improve when you give it fifty.

So before adding another MCP, I would check whether the current set is also safe.

## Security patterns specific to Hermes

Every MCP server extends what Hermes can reach. The server's credentials determine how much damage a mistaken action can cause.

Hermes has several controls worth using:

- Separate profiles by job. A coding profile, finance profile, and always-on messaging profile should not automatically share the same MCP set.
- Prefer `tools.include`. Start from a small allowlist.
- Use server-level restrictions too. Hermes filtering should complement OAuth scopes, read-only endpoints, database roles, and application permissions.
- Scope filesystem access to individual projects.
- Keep secrets in `.env` rather than prompts or checked-in YAML.
- Prefer read-only credentials when writes are unnecessary.
- Separate production and sandbox environments.
- Review catalogue manifests before installation.
- Treat remote MCP operators as part of the trust boundary.
- Review OAuth scopes again when the agent’s job changes.

The same rule applies if you use a managed integration service for SaaS authentication. It can reduce credential handling and token-refresh work, but you still need to control which accounts, users, and actions the agent can reach.

Once those boundaries are clear, choosing between MCP, native Hermes capabilities, and a managed integration becomes much easier.

## MCP vs native Hermes vs skills vs managed integrations

|  | Native Hermes | Skills / CLI | Direct MCP | Managed integration layer |
| --- | --- | --- | --- | --- |
| **Best for** | Core local actions | Repeatable procedures | One focused external service | Several SaaS apps |
| **Setup effort** | Lowest | Low | Medium | Low to medium |
| **Context impact** | Existing Hermes surface | Usually low | Depends on exposed tools | Depends on loading model |
| **Authentication** | Local/provider specific | CLI specific | Per server | Managed |
| **Maintenance** | Hermes updates | Skill and CLI | Every server | Service-managed |
| **Security control** | Hermes config | CLI permissions | Hermes + server scopes | App scopes + provider controls |
| **Reliability** | Fewer external dependencies | Depends on API or CLI | MCP + target service | Provider + target service |
| **Best use case** | Files and terminal | Known procedures | GitHub, database, Stripe | Cross-app SaaS workflows |

The choice I keep coming back to is the smallest one that solves the job.

## Conclusion

After going through the current MCP ecosystem, that is probably the biggest change in how I think about Hermes MCP setups.

The useful question is not “How many good MCP servers can I install?” It is “Which few capabilities are important enough to stay exposed to this agent?”

For a developer profile, GitHub is an easy starting point, and Chrome DevTools or Playwright can come next when the workflow needs them.

For automation, n8n stands out because Hermes already has a reviewed catalogue path. For data, a focused database server usually adds more value than a long list of generic tools.

Then I would let Hermes native tools and skills handle everything they already do well.

If the workflow later grows across many authenticated SaaS applications, a managed layer such as Composio can be worth adding rather than another five individual MCP setups.

Start small, measure what Hermes actually calls, remove what it ignores, and review scopes whenever the agent’s job changes.

That usually gives you a better Hermes setup than any “install these 50 MCP servers” list can.

## Sources {toggle="true"}

Hermes MCP documentation: [Nous Research Hermes MCP guide](https://github.com/NousResearch/hermes-agent/blob/main/website/docs/user-guide/features/mcp.md)

Hermes CLI and catalog: [Hermes CLI reference](https://github.com/NousResearch/hermes-agent/blob/main/website/docs/reference/cli-commands.md)

MCP rankings: [MCP Toplist](https://mcptoplist.com/)

GitHub MCP: [GitHub MCP Server](https://github.com/github/github-mcp-server)

Chrome DevTools MCP: [Chrome DevTools for agents](https://developer.chrome.com/docs/devtools/agents/get-started)

Playwright MCP: [Microsoft Playwright MCP](https://github.com/microsoft/playwright-mcp)

Shopify MCP: [Shopify Storefront MCP](https://shopify.dev/docs/apps/build/storefront-mcp/servers/storefront)

Stripe MCP: [Stripe AI](https://github.com/stripe/ai)

PayPal MCP: [PayPal MCP Server](https://developer.paypal.com/ai-tools/mcp-server)

Slack MCP: [Slack MCP guide](https://slack.com/help/articles/48855576908307-Guide-to-the-Slack-MCP-server)

Google Workspace MCP: [Google Workspace MCP](https://developers.google.com/workspace/guides/configure-mcp-servers)

Microsoft MCP servers: [Microsoft MCP catalog](https://github.com/microsoft/mcp)

HubSpot MCP: [HubSpot MCP documentation](https://developers.hubspot.com/docs/apps/developer-platform/build-apps/integrate-with-the-remote-hubspot-mcp-server)

Salesforce MCP: [Salesforce DX MCP](https://github.com/salesforcecli/mcp)

Linear MCP: [Linear MCP documentation](https://linear.app/docs/mcp)

Database MCP: [Google MCP Toolbox for Databases](https://github.com/googleapis/mcp-toolbox)

Supabase MCP: [Supabase MCP documentation](https://supabase.com/docs/guides/ai-tools/mcp)

Snowflake MCP: [Snowflake managed MCP](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-mcp)

QuickBooks MCP: [QuickBooks Online MCP Server](https://github.com/intuit/quickbooks-online-mcp-server)

Brex MCP: [Brex MCP documentation](https://www.brex.com/support/using-brex-in-ai-apps)

Zapier MCP: [Zapier MCP](https://zapier.com/mcp)

Browser Use: [Browser Use](https://github.com/browser-use/browser-use)

Composio Hermes integration: [Composio for Hermes](https://composio.dev/hermes)
