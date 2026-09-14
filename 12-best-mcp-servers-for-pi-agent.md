# 12 Best MCP Servers for Pi Agent Every Developer Should Use in 2026

I already covered the [best Pi Agent extensions](https://composio.dev/content/top-pi-extensions) and the [best Pi Agent skills](https://composio.dev/content/top-pi-agent-skills).

So MCP was the obvious next one. ✌️

Pi intentionally ships without built-in MCP support.

Mario Zechner has explained why. A large MCP server (not all) can put a lot of tool definitions into the context window before the agent even uses them. In his own tests, Playwright MCP used around 13.7K tokens for 21 tools, while Chrome DevTools MCP used around 18K tokens for 26 tools.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_1.png)

That is a pretty big thing for an agent built around keeping the core small.

But Pi can still use MCP. You just need the `pi-mcp-adapter`, which exposes your MCP servers through one proxy tool of roughly 200 tokens and loads the actual servers only when Pi needs them.

These are the 12 MCP setups I would actually consider for Pi in 2026.

---

## TL;DR

If you just want the list, here you go.

- **Composio MCP** connects Pi to 1,500+ apps with managed authentication and dynamic tool loading.

> 💁 Most “best MCP server” articles repeat GitHub, Notion, Slack, Supabase, Sentry, Stripe, Exa, and similar tools, but Composio gives you **all of them in one place**, so there is no need to install and manage separate MCP servers for each one.

- **pi-mcp-adapter** gives Pi token-efficient access to almost any standard MCP server.
- **Context7** gives Pi current, version-specific library documentation and code examples.
- **Playwright MCP** lets Pi automate and test real browsers through structured accessibility data.
- **Filesystem MCP** gives Pi controlled access to directories outside the current project.
- **Chrome DevTools MCP** adds browser debugging, console inspection, network data, and performance traces.
- **Figma Dev Mode MCP** gives Pi structured design context for design-to-code workflows.
- **Memory MCP** gives Pi persistent knowledge-graph memory across sessions.
- **Sequential Thinking MCP** adds structured, revisable reasoning for difficult engineering tasks.
- **Git MCP** lets Pi inspect and change your local Git working tree.
- **Kubernetes MCP** gives Pi controlled access to Kubernetes and Helm operations.
- **Docker MCP Gateway** runs MCP servers through isolated containers with secrets and lifecycle management.

You definitely do not need all of these.

Start with Composio and `pi-mcp-adapter`, then add the servers that solve a real problem in your workflow.

## How MCP Servers Work With Pi Agent

Pi's core gives the model four tools: `read`, `write`, `edit`, and `bash`.

MCP support comes through an extension. The one I would use is `pi-mcp-adapter`.

Install it with:

```bash
pi install npm:pi-mcp-adapter
```

Then restart Pi.

The adapter uses `.mcp.json` for project-level MCP configuration.

A basic config looks like this:

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

You can also keep shared servers in:

```plain text
~/.config/mcp/mcp.json
```

Then use:

```plain text
/mcp
```

inside Pi to inspect and manage your MCP servers. The adapter keeps one lightweight `mcp` tool in context and discovers individual tools when Pi actually needs them.

Adding ten MCP servers **does not** mean Pi needs to dump every tool schema from every server into the context window at startup. Which is pretty nice way to handle MCP than most other coding agents.

Now let’s get into the ones worth adding.

---

## 1. Composio MCP

> ℹ️ Connects Pi to 1,500+ apps with managed authentication, tool discovery, and multi-app execution.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_2.png)

If I were setting up Pi from scratch, I would handle app integrations first.

Most useful developer workflows eventually leave the repository. You need GitHub issues, Linear tickets, Slack messages, Gmail, Notion docs, Sentry errors, Supabase, Stripe, or some other external system.

Composio gives Pi one managed way to work with those services instead of maintaining a separate MCP server and authentication flow for each app.

We recently added support for over 1,500 integrations, including GitHub, Notion, Slack, Linear, Supabase, Stripe, and many others.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_3.png)

### What it does

- Connects Pi to 1,500+ external apps
- Handles OAuth, API keys, refresh tokens, and scopes
- Searches for relevant tools when Pi needs them
- Supports multi-tool and multi-app workflows
- Keeps large tool catalogs out of the main model context
- Supports both CLI and MCP-based Pi setups

I would actually use the [Composio CLI](https://docs.composio.dev/docs/cli) first with Pi.

Pi already treats shell commands as first-class tools, so the CLI is much simpler and avoids MCP server configuration. The MCP way is still useful if you want everything behind the same MCP interface.

### Install

Install the Composio CLI:

```bash
curl -fsSL <https://composio.dev/install> | bash
```

Then log in:

```bash
composio login
```

You can ask Pi to connect an app, or use commands such as:

```bash
composio search "what can I do with Datadog?"
composio link datadog
```

For the MCP route, first install the Pi MCP adapter:

```bash
pi install npm:pi-mcp-adapter
```

Then add this to `.mcp.json`:

```json
{
  "mcpServers": {
    "composio": {
      "url": "<https://connect.composio.dev/mcp>"
    }
  }
}
```

Then authenticate from Pi using the MCP authentication flow.

> 💡 I would check Composio before installing an app-specific MCP server. If the same app is already available as a managed toolkit, why not use it than manage another server on your own?

**Resources**

- Composio toolkits: [Browse Composio toolkits](https://composio.dev/toolkits)
- Documentation: [Composio docs](https://docs.composio.dev/)
- Pi integration guide: [Composio with Pi](https://composio.dev/toolkits/vercel/framework/pi-agent)

---

## 2. pi-mcp-adapter

> ℹ️ Adds token-efficient MCP support to Pi through one lightweight proxy tool.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_4.png)

This is technically a Pi extension, but it belongs near the top of this list because almost every MCP server below depends on it.

The adapter was built specifically around Pi's approach to context.

Instead of registering hundreds of MCP tools directly with the model, it gives Pi one `mcp` tool that uses roughly 200 tokens. Pi can then search for the server or tool it needs and call it on demand.

Servers also start lazily, so they do not all launch when Pi starts.

### What it does

- Adds MCP support to Pi
- Keeps one small proxy tool in context
- Starts MCP servers only when needed
- Supports local and remote MCP servers
- Reads normal `.mcp.json` configuration
- Lets you promote important MCP tools into direct Pi tools

The project has already grown to around **1.5K GitHub stars**, which is pretty good for something this Pi-specific.

### Install

```bash
pi install npm:pi-mcp-adapter
```

Then restart Pi:

```plain text
/restart
```

Open the MCP panel:

```plain text
/mcp
```

Or run:

```plain text
/mcp setup
```

to create or import server configuration.

> 💡 This is one of the few things on this list I would install even before deciding which MCP servers I need.

**Resources**

- GitHub: [pi-mcp-adapter repository](https://github.com/nicobailon/pi-mcp-adapter)
- Package: [pi.dev package](https://pi.dev/packages/pi-mcp-adapter)
- Architecture: [DeepWiki guide](https://deepwiki.com/nicobailon/pi-mcp-adapter)

---

## 3. Context7

> ℹ️ Gives Pi current, version-specific documentation instead of relying only on model training data.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_5.png)

This is probably one of the easiest MCP servers to recommend for coding. In almost all my listicle for "top MCP Servers", this is almost always there, no matter if it's Claude Code, Codex, OpenCode or anything else.

Models know a lot about React, Next.js, MongoDB, Cloudflare, Supabase, and thousands of other libraries. The problem is that APIs change quickly.

Context7 fetches current documentation and examples and puts the relevant parts into the agent's context.

So instead of asking Pi to remember how a particular library worked during training, you can tell it to check the actual documentation for the version you are using.

### What it does

- Retrieves current library documentation
- Supports version-specific queries
- Returns code examples
- Resolves libraries into Context7 IDs
- Helps reduce outdated API suggestions
- Works through MCP or its own CLI and skills

Context7 is currently sitting at around **62K GitHub stars**, making it one of the largest MCP-related projects around.

### Install

This uses normal MCP compatibility through `pi-mcp-adapter`.

For the remote server:

```json
{
  "mcpServers": {
    "context7": {
      "url": "<https://mcp.context7.com/mcp>",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

You can also run it locally:

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp", "--api-key", "YOUR_API_KEY"]
    }
  }
}
```

Context7 now also ships a dedicated Pi extension, but for this article I am keeping the MCP route so the setup stays consistent with the rest of the list.

> ⚠️ Context7 documentation can include community-contributed content. The project itself says it cannot guarantee the accuracy or security of every library entry, so I would still verify anything security-sensitive.

**Resources**

- GitHub: [Context7 repository](https://github.com/upstash/context7)
- Website: [Context7](https://context7.com/)
- Releases: [Context7 releases](https://github.com/upstash/context7/releases)

---

## 4. Playwright MCP

> ℹ️ Gives Pi deterministic browser automation through structured page data.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_6.png)

If you use Pi for frontend work, Playwright MCP is one of the more useful additions here.

It drives a real browser and exposes the page through accessibility snapshots. The agent can understand elements, forms, buttons, text, and page structure without relying entirely on screenshots.

That makes it useful for testing workflows where Pi needs to keep interacting with the browser and checking what happened.

### What it does

- Automates Chromium, Firefox, and WebKit workflows
- Reads structured accessibility snapshots
- Clicks, types, navigates, and fills forms
- Captures screenshots
- Supports end-to-end testing workflows
- Helps agents generate or repair browser tests

The repository currently has around **37K GitHub stars**.

### Install

Playwright does not have a Pi-specific MCP setup.

Add the normal MCP configuration through `pi-mcp-adapter`:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

> 💡 I would use Playwright when Pi needs an ongoing browser session and structured page interaction. For simple coding-agent browser tasks, Microsoft itself now points developers toward its CLI and skills workflow because it can use fewer tokens.

There is also a security detail worth knowing.

A reported Playwright MCP issue showed password input content appearing inside accessibility snapshots. Browser agents can see sensitive page state, so I would keep test accounts separate from important personal or production sessions.

**Resources**

- GitHub: [Microsoft Playwright MCP](https://github.com/microsoft/playwright-mcp)
- Playwright: [Playwright documentation](https://playwright.dev/)

## 5. Filesystem MCP

> ℹ️ Gives Pi controlled access to local directories that you explicitly allow.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_7.png)

Pi already has `read`, `write`, and `edit`.

So why add another filesystem tool?

For me, the useful part is directory scoping.

You can expose another folder such as documentation, exported designs, notes, generated files, or a shared workspace while keeping the MCP server restricted to the directories you explicitly provide.

### What it does

- Reads and writes files
- Creates and lists directories
- Moves files and directories
- Searches local files
- Reads file metadata
- Restricts access to allowed directories

It lives inside the official `modelcontextprotocol/servers` reference repository, which currently has around **90K GitHub stars**.

### Install

This uses normal MCP compatibility through the adapter.

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/path/to/allowed/directory"
      ]
    }
  }
}
```

You can add more directories as additional arguments.

> 💡 I would not point this at your entire home directory. Give Pi the smallest directory scope that actually solves the task.

The server also supports MCP Roots for clients that support dynamic directory access control.

**Resources**

- GitHub: [Filesystem MCP source](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)
- MCP examples: [Model Context Protocol examples](https://modelcontextprotocol.io/examples)

## 6. Chrome DevTools MCP

> ℹ️ Lets Pi inspect, debug, and profile a real Chrome browser.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_8.png)

Playwright and Chrome DevTools look similar at first, but I would use them for different jobs.

Playwright is stronger when Pi needs deterministic browser interaction and cross-browser testing.

Chrome DevTools MCP becomes more useful when the question is why the application is slow, why a request failed, what appeared in the console, or what Chrome actually did while rendering the page.

### What it does

- Reads browser console output
- Inspects network requests
- Captures screenshots and page state
- Records performance traces
- Helps investigate metrics such as LCP and CLS
- Controls a live Chrome browser

The official Google repository now has around **51.7K GitHub stars**.

### Install

Add it through `pi-mcp-adapter`:

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

For cleaner temporary browser sessions, you can also use:

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest", "--isolated"]
    }
  }
}
```

> ⚠️ Chrome DevTools MCP can inspect whatever exists inside the browser session it connects to, including sensitive page data. I would avoid connecting it to your normal logged-in Chrome profile unless the task actually needs that access.

**Resources**

- GitHub: [Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp)
- Chrome documentation: [Chrome DevTools for agents](https://developer.chrome.com/docs/devtools/agents)

## 7. Figma Dev Mode MCP

> ℹ️ Gives Pi structured Figma design context for design-to-code work.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_9.png)

Composio already has a Figma toolkit, but I kept Figma Dev Mode MCP in this list for one reason.

The two solve different jobs.

The Composio integration covers normal Figma API actions. Figma's own MCP server is built specifically around giving coding agents design context such as components, variables, layout information, screenshots, and Code Connect mappings.

That makes it much more useful when Pi is implementing an interface from an actual design.

### What it does

- Extracts structured design context
- Reads components and variables
- Gets screenshots for visual reference
- Works with Code Connect mappings
- Helps generate code from selected frames
- Supports write-to-canvas workflows where available

### Install

Figma does not currently document a dedicated Pi setup.

Use its standard remote MCP server through `pi-mcp-adapter`:

```json
{
  "mcpServers": {
    "figma": {
      "url": "<https://mcp.figma.com/mcp>"
    }
  }
}
```

You can then authenticate through the supported MCP OAuth flow.

> 💡 If you use a proper design system, connect Figma components to the codebase with Code Connect. Figma itself recommends this because the agent can reuse real components instead of guessing how the design maps to code.
> ⚠️ Check your Figma plan before depending on this heavily. MCP rate limits vary by plan.

**Resources**

- Figma MCP guide: [Figma MCP Server Guide](https://github.com/figma/mcp-server-guide)
- Official documentation: [Figma MCP documentation](https://developers.figma.com/docs/figma-mcp-server/)
- Rate limits: [Figma MCP rate limits](https://developers.figma.com/docs/figma-mcp-server/rate-limits-access/)

## 8. Memory MCP

> ℹ️ Gives Pi persistent knowledge-graph memory across sessions.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_10.png)

Pi sessions eventually end, and useful project context disappears unless something stores it.

Memory MCP gives the agent a simple persistent knowledge graph made from entities, relations, and observations.

So Pi can store things such as architecture decisions, ownership information, naming conventions, project rules, or facts it should reuse later.

### What it does

- Stores persistent entities
- Records relationships between entities
- Adds observations to known entities
- Searches stored knowledge
- Keeps information in a local JSONL file
- Preserves useful context across sessions

Like Filesystem, it is part of the official `modelcontextprotocol/servers` repository, which currently has around **90K stars**.

### Install

Add the standard MCP server through the Pi adapter:

```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

You can also choose where the memory file lives:

```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"],
      "env": {
        "MEMORY_FILE_PATH": "/path/to/memory.jsonl"
      }
    }
  }
}
```

> ⚠️ This is intentionally simple memory. It does not give you user identity separation, or advanced temporal reasoning. You still need to decide what Pi should remember.

**Resources**

- GitHub: [Memory MCP source](https://github.com/modelcontextprotocol/servers/tree/main/src/memory)
- MCP examples: [Model Context Protocol examples](https://modelcontextprotocol.io/examples)

## 9. Sequential Thinking MCP

> ℹ️ Gives Pi a structured way to break down and revise difficult multi-step reasoning.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_11.png)

I would not keep this active for every simple coding task.

It becomes useful when the task has enough moving parts that Pi should explicitly work through them, revise earlier assumptions, branch into alternatives, and then continue.

Think architecture changes, migrations, difficult debugging, or work where one wrong assumption can affect several later steps.

### What it does

- Breaks problems into ordered reasoning steps
- Lets the agent revise previous steps
- Supports alternate reasoning branches
- Adjusts the expected number of steps while working
- Helps with planning and debugging
- Works without an external API

It is another official MCP reference server inside the roughly **90K-star** `modelcontextprotocol/servers` repository.

### Install

```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    }
  }
}
```

You can also run it through Docker:

```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "docker",
      "args": ["run", "--rm", "-i", "mcp/sequentialthinking"]
    }
  }
}
```

> 💡 I would use this only when the problem actually needs it. Structured reasoning adds more work and more tokens, so using it for a tiny edit does not make much sense.

**Resources**

- GitHub: [Sequential Thinking MCP source](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking)

## 10. Git MCP

> ℹ️ Lets Pi work directly with the local Git repository it is editing.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_12.png)

Composio gives Pi access to GitHub, but GitHub and Git are different parts of the workflow.

The GitHub integration works with the remote service for things such as issues, pull requests, workflows, and repository APIs.

Git MCP works with the local checkout.

That makes it useful when Pi needs to inspect changes, review history, stage files, or work with commits before anything reaches GitHub.

### What it does

- Reads repository status
- Inspects diffs
- Searches commit history
- Stages changes
- Creates commits
- Works directly on the local repository

It is maintained inside the official MCP reference server repository.

### Install

This server uses Python and works well through `uvx`.

```json
{
  "mcpServers": {
    "git": {
      "command": "uvx",
      "args": ["mcp-server-git", "--repository", "/path/to/repository"]
    }
  }
}
```

You can also install it with `pip` if you prefer.

> 💡 Pi already has `bash`, so it can technically run normal Git commands itself. I would add Git MCP when I want explicit Git tools and tighter control around the operations the agent performs.

**Resources**

- GitHub: [Git MCP source](https://github.com/modelcontextprotocol/servers/tree/main/src/git)

## 11. Kubernetes MCP

> ℹ️ Lets Pi inspect and manage Kubernetes clusters through MCP tools.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_13.png)

This is where the list starts becoming much more stack-specific.

If you do not run Kubernetes, skip it.

If you do, this gives Pi structured tools for cluster operations instead of making it construct every `kubectl` or Helm command manually.

### What it does

- Lists and inspects Kubernetes resources
- Manages pods and deployments
- Applies manifests
- Supports Helm operations
- Handles port forwarding
- Uses your configured kubeconfig

The project currently has around **1.6K GitHub stars**.

### Install

Make sure `kubectl` works first:

```bash
kubectl get pods
```

Then add the server:

```json
{
  "mcpServers": {
    "kubernetes": {
      "command": "npx",
      "args": ["mcp-server-kubernetes"]
    }
  }
}
```

For most setups, I would start with its non-destructive mode:

```json
{
  "mcpServers": {
    "kubernetes": {
      "command": "npx",
      "args": ["mcp-server-kubernetes"],
      "env": {
        "ALLOW_ONLY_NON_DESTRUCTIVE_TOOLS": "true"
      }
    }
  }
}
```

That disables destructive operations such as deleting pods, deployments, and namespaces.

> ⚠️ This server operates against your real Kubernetes permissions. I would use a restricted kubeconfig and non-destructive mode before allowing an agent anywhere near an important cluster.

**Resources**

- GitHub: [Kubernetes MCP server](https://github.com/Flux159/mcp-server-kubernetes)

## 12. Docker MCP Gateway

> ℹ️ Runs and manages MCP servers through isolated Docker containers.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_14.png)

Docker MCP Gateway is a slightly different entry.

Instead of adding one specific capability such as memory or browser debugging, it gives you infrastructure for running MCP servers through Docker.

The Gateway manages server lifecycle, credentials, routing, OAuth, and isolated container execution. That gets useful once you start running several local MCP servers and do not want every package executing directly on the host.

### What it does

- Runs MCP servers in isolated containers
- Manages MCP server lifecycle
- Handles secrets and credentials
- Supports OAuth flows
- Provides dynamic tool discovery
- Adds centralized logging and call tracing

The open-source `docker/mcp-gateway` repository currently has around **1.4K stars**.

### Install

Recent Docker Desktop versions include the MCP CLI when MCP Toolkit is enabled.

You need Docker Desktop 4.59+.

Then confirm the MCP CLI is available:

```bash
docker mcp --help
```

If you install the CLI plugin manually, Docker documents the plugin path as:

```plain text
~/.docker/cli-plugins/docker-mcp
```

Pi can then connect to the Gateway as an MCP client through `pi-mcp-adapter`.

> 💡 I would look at Docker MCP Gateway once your MCP setup grows beyond a few simple local servers. Container isolation also gives you a cleaner boundary than running random `npx` and `uvx` MCP packages directly on your machine.

Docker's current Gateway also includes logging and call tracing, and Docker Desktop handles the Gateway automatically when MCP Toolkit is enabled.

**Resources**

- GitHub: [Docker MCP Gateway](https://github.com/docker/mcp-gateway)
- Documentation: [Docker MCP Gateway docs](https://docs.docker.com/ai/mcp-catalog-and-toolkit/mcp-gateway/)

---

## Wrap Up!

I would not install all 12 on day one. 💁

For a normal Pi setup, I would start with **Composio** for external apps and **pi-mcp-adapter** for the rest of the MCP ecosystem.

Then add **Context7** if you code against changing libraries.

Add **Playwright** and **Chrome DevTools** if you do frontend work.

Add **Filesystem**, **Memory**, and **Git** when you want more local context.

GitHub, Notion, Stripe, Sentry, Firecrawl, Exa, Slack, Jira, and similar app integrations are already available through Composio. We have over **1500 apps support**, so imagine all the options you have. Installing a separate server for each one would just give you more authentication, configuration, and servers to maintain.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/12-best-mcp-servers-for-pi-agent/image_15.png)

And keep the security side in mind.

An MCP server runs with whatever access you give it. Filesystem can read your files. Chrome can see browser state. Kubernetes can change clusters. Docker can run containers.

Give each server the smallest permissions it needs. Pi pretty much has zero guard-rails, so you have to be a bit more careful.

That is pretty much it.

If I missed an MCP server that genuinely adds something new to Pi, let me know ✌️
