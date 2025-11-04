While most agentic coding tools like Codex, Cursor, and Windsurf are adding SDKs and plugin APIs, Anthropic’s **Claude Code** is trying to do something a bit different. They’ve been quietly building a complete stack - skills for domain context, Plugins for modular workflows, and MCPs for tool integrations, all connected through one environment.

I wanted to see how that actually works when you build something real. 

So I picked a project I’ve been planning for a while. **Luno is** a personal finance platform. It includes payment integrations, cron jobs for bill reminders, an agentic chatbot (wired with **Tool Router** for calling tools like Gmail, Notion, Stripe, etc, integrated inside the app), household sharing, subscription tracking, and analytics.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_1.png)

The goal was simple: test the entire Claude Code setup. Skills, Plugins, MCP servers, Sub Agents, and slash commands, and see if it really helps speed up real-world development or just adds more setup overhead.

## TL;DR

Built [**Luno **](https://github.com/rohittcodes/luno)in 2-3 days using Claude Code's full stack. Setup took a day (creating Skills, configuring Rube MCP). After that, features were shipped in 30-60 minutes instead of 8-10 hours of manual work. Cost: $12.67 for Claude Code usage (~15.5M input, 174k output tokens) + a Cursor Pro account for routine CRUD. Context7 MCP was critical in setup and development by pulling the right docs in‑session.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_2.png)

The infrastructure works, but requires upfront investment. 

Skills taught Claude: Code: my patterns for workflows; Rube MCP connected everything through one server; dev‑toolkit plugin handled security/testing/reviews. Tool Router powers the agentic chatbot. It would usually take 2-3 months and 200+ hours. 

You can find the repository [here](https://github.com/rohittcodes/luno), don't forget to drop a star!

**Quick look: Here’s what the dashboard looks like:**

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_3.png)

## Day One: The Setup

I started by creating Skills for CC. Not because I love documentation, but because I was tired of having to explain the same patterns over and over. "Use TanStack Query for data fetching." "RLS policies for multi-tenant data." "Error boundaries here, not there."

I asked Claude Code to generate Skills for my workflow:

- Feature development patterns

- Database architecture (Supabase)

- [Tool Router](https://docs.composio.dev/docs/tool-router/quick-start) integration with AI SDK

- Analytics pipeline patterns

- Design-to-code workflow

They're just markdown files in `.claude/skills/`. Nothing fancy. But here's the thing: once I had them, Claude stopped generating code that looked like it came from a tutorial. It started generating code that looked like *my* code.

Then I set up [**Rube MCP**](https://rube.app/). The problem with MCPs is that they eat your context window; multiple servers = less space for the model to think. Rube connects to 500+ apps through a single MCP server. GitHub, Linear, Figma, Supabase, all through one connection. It manages a sandbox environment for tool actions and stores data there, so your context window stays free. In parallel, Context7 MCP removed a ton of context‑switching by fetching authoritative docs directly in the session.

I already had my [dev-toolkit plugin](https://github.com/rohittcodes/claude-plugin-suite) from last month (when Anthropic dropped plugin support). 16 specialised agents, 10+ slash commands, MCP integrations. Things like `/security-scan` for OWASP reviews, `/test` for running tests with coverage reports. I wanted to stress-test it on something real.

Setup took a day. Then I started building.

## Building Luno: My personal finance management (The Messy Part)

I gave Claude a prompt: "Build the database schema for a personal finance platform. Transactions, accounts, categories, budgets, goals, household sharing with invitations."

It generated the complete **Supabase schema**. Foreign keys, indexes, RLS policies. The schema made sense because the Skills taught me how to structure databases. But then I looked closer, and it forgot the indexes on the household invitations table. The `token` and `email` Columns needed indexes for performance. Had to point that out.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_4.png)

### **First lesson:** Skills help consistency, but you still need to review.

For the UI, I passed Claude a Figma design link to get some ideas and build on top of it. I'd already set up my theme using tweakcn, so the implementation was based on that existing setup rather than an exact match of the Figma design. The designs don't match, but the UI came out clean and consistent.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_5.png)

Authentication was manual, and **Supabase Auth** handles most of it anyway.

Then I hit a wall: cost. Claude's pricing was adding up fast. I was generating a lot of code, and at ~$3 per million tokens, it gets expensive quickly. So I switched to Cursor for routine feature work, transaction management, budget tracking, and basic CRUD. Cursor's $20/month subscription made more sense for that stuff.

I came back to Claude Code when I needed to integrate Composio's** **[**Tool Router**](https://docs.composio.dev/docs/tool-router/quick-start)** **with the AI SDK for the chatbot. The docs weren't clear on some patterns, and I kept getting the integration wrong. I used Context7 MCP to fetch the actual AI SDK docs and Tool Router examples.

### What is [Tool Router](https://docs.composio.dev/docs/tool-router/quick-start) (and why use it)?

**Tool Router** exposes connected apps as callable tools for your AI agent, without hand‑wiring each integration. You connect once (per user), and the AI SDK gets a unified tool surface.

- **Unified access + per‑user auth:** One router for 500+ apps, with tools automatically scoped to each user’s connections.

- **Zero redeploys + AI‑SDK native:** New connections appear as tools immediately; tools already come with parameters/schemas for direct calls.

In Luno, that meant email/calendar/issue flows existed only if the user had connected those apps, with no special‑case code or per‑app SDKs.

Btw, this is what powers Rube MCP, at the backend.

### **Claude pulled the documentation and showed me the pattern:**

```typescript
// Initialize Tool Router MCP client
const mcpClient = await createToolRouterMCPClient(user.id)

if (mcpClient) {
  // Get tools from MCP client (AI SDK format)
  const mcpToolSet = await mcpClient.tools()

  // Combine with database tools
  const allTools = {
    ...dbTools,
    ...Object.fromEntries(
      Object.entries(mcpToolSet).map(([name, tool]) =>
        [`toolRouter_${name}`, tool]
      )
    )
  }

  // Use with streamText
  const result = streamText({
    model,
    messages: modelMessages,
    tools: allTools,
  })
}
```

Once I had the pattern, it clicked. Tool Router creates a session per user, exposes all connected apps as MCP tools, handles auth per user, and returns tools in AI SDK format. So if a user connects Gmail, Calendar, and Notion, the chatbot automatically gets those tools. No code changes. Dynamic tool access based on what the user has connected.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_6.png)

That's what makes **Tool Router** powerful. It's not just connecting apps, it's exposing those connections as tools your AI can use directly.

## The RLS Policies That Took Three Tries

The household invitation system was probably the most complex part. Invitations expire after 7 days, need email templates, and proper permission checks. Claude got the schema right on the first try. But the RLS policies? Three iterations.

First version: members could see invitations, but couldn't check ownership hierarchy properly. Second version: fixed the hierarchy but broke the permission logic for expired invitations. Third version: finally worked. The policy checked ownership, membership, and expiration all in the right order.

This is where having the plugin helped. I ran `/security-scan` and it caught issues I would've missed:

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_7.png)

- Trending updates weren't locked down properly

- Anonymous tracking needed server‑issued signed tokens with TTL

- The queue work needed proper batching

- Long queries should be precomputed on a schedule

- SQL indexes are missing on some aggregations

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_8.png)

Fixed all of these before deploying. The security‑reviewer agent checks for OWASP Top 10, suggests fixes, and validates implementations.

## Payment Integration and Cron Jobs

Lemon Squeezy integration was straightforward. The Skill had patterns for webhook handling and subscription management. Claude generated webhook handlers with proper signature verification on the first try.

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_9.png)

Cron jobs for bill reminders were more interesting. I set up **Supabase (edge functions) **that:

- Check upcoming bills

- Send email reminders via Resend

- Update notification preferences

- Handle timezone conversions

The timezone handling part needed manual fixes. Claude generated code that worked, but didn't account for daylight saving time properly. Had to correct that myself.

## What Actually Shipped

After 2-3 days, I had a production‑ready finance platform: transactions with categories, budgets with alerts, household sharing (7‑day invitations), subscription tracking, analytics, an agentic chatbot (Tool Router), automated bill reminders, and Lemon Squeezy payments.

The analytics dashboard was the last piece. Claude generated working code, but it split queries that should have been combined and missed opportunities to memoise. I optimised those manually.

## Dev-Toolkit Plugin (Quick Context)

Since I keep mentioning it, this plugin (built when Anthropic launched plugins) bundles the day‑to‑day work, security reviews, testing, and system design into slash commands and specialised agents.

- Core agents: security reviewer (OWASP), performance/load tester, compliance/testing/architecture specialists.

- Core commands: `/test`, `/code-review`, `/security-scan`, `/deploy`, `/monitor`.

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_10.png)

You can install it: [rohittcodes/claude-plugin-suite](http://github.com/rohittcodes/claude-plugin-suite)

The plugin meant I could run security reviews and code standardization continuously instead of at the end. Caught a lot of issues early.

## The Real Numbers

Let's talk money and time, because that's what actually matters.

**Setup (day 1): **~1 day total (Skills ≈4h, Rube MCP ≈30m, Supabase ≈2h).

![Image 11](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_11.png)

**Development (2-3 days):**

- Claude Code: $12.67 (architecture, complex integrations, Tool Router)

- Cursor: Pro account (routine CRUD, UI polish)

- Rube MCP: Free tier

- Total: $12.67 + Cursor Pro

**Time saved:** **200+ hours**. It typically takes **2-3 months** of solid work.

But here's the context: that $12.67 is *after* I switched to Cursor Pro for routine work. If I'd used Claude Code for everything, it would've been closer to $50-60. The cost management part is real; you need to be strategic about when you use the expensive model.

## Would I Do It Again?

Absolutely. But if I were starting over, I'd approach a few things differently. I'd create Skills on day zero, before writing a single line of code. The consistency they bring matters more than moving fast in the beginning, and every feature afterwards benefits from having those patterns established. The plugin would be there from the start, too; catching security issues and enforcing code standards continuously is far better than fixing problems at the end.

![Image 12](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_12.png)

I'd also budget more realistically. If you're building something serious, plan for $50-100 a month in AI costs. It's still cheaper than hiring someone or spending months of your own time, but it's not free. And Context7 MCP would be non-negotiable from the start, having documentation accessible in-session instead of constantly context-switching to docs is a massive productivity unlock.

The thing is, you still review code. You still make architecture decisions. You still fix edge cases. Claude Code handles the boring stuff, boilerplate, migrations, and configuration so that you can focus on the hard problems: architecture, security, performance, and user experience. That upfront day spent on Skills and setup? It paid for itself ten times over in consistency and speed.

## Final Thoughts

Claude Code's infrastructure is real. It's not hype. But it requires upfront investment in Skills, plugins, and MCP configuration. Once that's done, development becomes more conversational. "Build a subscription tracking feature with email reminders" actually works.

The value isn't replacing developers, it's handling the boring stuff so you can focus on what matters: the architecture, the security, the performance, the user experience. Luno took me 2-3 days, not 3 months. It's production‑ready with proper protection, error handling, and testing. That's the difference the full stack makes.

If you're building with Claude Code, invest in the infrastructure first. Create your Skills. Set up your MCPs properly. Build or install plugins that match your workflow. The upfront cost is worth it.

- Usage: ~15.5M input tokens, ~173k output tokens
