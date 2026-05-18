---
title: "How to Build Agents with the Cursor Agents SDK"
source: https://www.notion.so/composio/How-to-build-agents-with-the-Cursor-Agents-SDK-35df261a6dfe806f9d57e8f597e108b5
notion_page_id: 35df261a-6dfe-806f-9d57-e8f597e108b5
last_synced: 2026-05-18T11:50:29Z
---

Cursor recently released their [cursor agents SDK](https://cursor.com/blog/typescript-sdk) to the public, and it's quietly powering many teams to:

- invoke agents directly from CI/CD pipelines,
- create automations for end-to-end workflows,
- and embedding agents into core products.

Basically, the SDK lets developers deploy agents without overthinking building and maintaining the entire agent stack

This blog gives a glimpse of what is possible with the cursor agents SDK and how to overcome the restrictions of limited tool access.

Let's start with a brief overview of cursor agents.

---

## A Primer On Cursor Agents SDK

Building an agent from scratch is a massive headache. Cursor SDK skips the<br>plumbing.

Before the SDK, [Cursor](https://cursor.com) was strictly an interactive IDE, but its newly released SDK turns that<br>agentic power into headless infrastructure. 

It uses the same **harness **that powers the desktop app, meaning you get IDE-grade code generation programmatically.

>

	Harness: context engine, workspace management, and routing

In fact, the cursor harness matters more than the model. [Endor Labs](https://www.endorlabs.com/learn/gpt-5-5-sets-a-new-code-security-record-with-cursor-not-codex-in-agent-security-league) benched<br>GPT-5.5 natively at 61.5% functional correctness. Then, dropping the same model into Cursor's harness, it scored 87.2%. 

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/cursor-agents-sdk/6cd14c993b.png)

Cursor spent years tuning the context management and tool dispatch; the SDK hands you that refined engine.

Here is the spec list for those who care:

<table header-row="true">
<colgroup>
<col width="135.00000762939453">
<col width="533.0000152587891">
</colgroup>
<tr>
<td>Spec / Feature</td>
<td>Technical Detail</td>
</tr>
<tr>
<td>**The Harness**</td>
<td>Built-in codebase indexing, semantic search, & instant grep.</td>
</tr>
<tr>
<td>**Execution**</td>
<td>Cloud (sandboxed VMs with durable state), Self-Hosted, Local.</td>
</tr>
<tr>
<td>**Models**</td>
<td>Agnostic. Uses `composer-2/composer-3` (default), Claude, or OpenAI.</td>
</tr>
<tr>
<td>**Integrations**</td>
<td>Deep MCP support, auto-loaded `.cursor/skills/`, & hooks.</td>
</tr>
<tr>
<td>**Subagents**</td>
<td>Main agent can spawn subagents with distinct prompts/models.</td>
</tr>
</table>
This means we have a perfect stack to build our agent. Let’s get started with the setup.

## How to Set Up Cursor Agents SDK

You can install it using `npm` to get started, then use Cursor's native `cursor SDK skill` for guidance.

So open your terminal and type:

```plain text
npm install @cursor/sdk
```
**Output**

Once done, install the cursor skill for guiding the cursor / Claude Code. In the same terminal, run:

```plain text
npm add skills /cursor-sdk
```
But there is a catch: by default, the Cursor SDK agents ship with a few default tools + MCP support.

However, this means connecting to or configuring multiple MCP servers is such a hassle for a production-grade product.

So let's automate it with Composio as the orchestrator, connect and configure it once, and you get secure access to 1000+ tools with optimized tool calls and context.

---

## Building a Production Agent With Composio MCP + Cursor Agent SDK

### Why Composio?

[Composio](https://composio.dev/) offers a single MCP server you can plug into with the Cursor SDK to access [1000+ apps ](https://composio.dev/toolkits)instantly. 

When you’re building an agent that requires interacting with external applications, let’s say a Sales agent with [Gong](https://composio.dev/toolkits/gong), [HubSpot](https://composio.dev/toolkits/hubspot), [Salesforce connectors](https://composio.dev/toolkits/salesforce), etc., you’d spend weeks on partnerships and integrations. Composio completely removes this friction, so you build what matters

So, let’s explore one example of a GitHub agent that pulls the repository, analyzes it, creates a dev branch, performs automatic refactoring, pushes the code, and opens a PR for review - a simple but powerful use case.

Let's begin!

### Setup Workspace

Head to the terminal and run the following commands

```plain text
mkdir cursor-agent
cd cursor-agent
npm init -y
npm install typescript ts-node @types/node --save-dev
npx tsc --init
```
The command will create a new project named `cursor_agent` , switch to the folder, initialize a blank `npm` project (ensure `package.json` gets created), install typescript support for development, and initialize the typescript compiler and linter.

Since our repository is set up, let’s configure environment variables.

### Set up Environment Variables

Add a `.env` file to keep secrets secure and add the following values:

```plain text
CURSOR_API_KEY=your-cursor-api-key
COMPOSIO_API_KEY=your-composio-api-key
```
You can get the `CURSOR_API_KEY `by going to the [Cursor integration](https://cursor.com/dashboard/integrations) page and creating a new API key.

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/cursor-agents-sdk/4677eec1af.png)

For `COMPOSIO_API_KEY`  , visit the [Composio Dashboard (Platform)](https://dashboard.composio.dev/), log in/sign up to the account, and head to the default project (or create one) → profile→ API KEY.

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/cursor-agents-sdk/bf301312b3.png)

Click “Create One”, it creates one, copy and then paste it into `.env` . Make sure it starts with `ak_` .

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/cursor-agents-sdk/ebf83bbc68.png)

Time to write the code.

### The Code

Now, create a new `index.ts` file and paste the following code.

```typescript
// imports
import "dotenv/config";
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";
import type { SDKAgent } from "@cursor/sdk";
import {
  DEFAULT_USER_ID,
  disposeAllAgents,
  getComposioApiKey,
  getOrCreateRuntime,
  requireEnv,
} from "./runtime.ts";

// log tool calls to stdout
function logToolCall(
  name: string,
  args: unknown,
  status: "running" | "completed" | "error",
): void {
  if (status === "running") {
    console.log(`\n[tool: ${name}] ${JSON.stringify(args)}`);
  } else if (status === "completed") {
    console.log(`[tool: ${name}] done`);
  }
}

// stream assistant text and tool events - act as streaming handler
async function runAgentChatStreaming(agent: SDKAgent, message: string): Promise<void> {
  const run = await agent.send(message);

  process.stdout.write("\nagent > ");
  for await (const event of run.stream()) {
    if (event.type === "assistant") {
      for (const block of event.message.content) {
        if (block.type === "text") process.stdout.write(block.text);
      }
    } else if (event.type === "tool_call") {
      logToolCall(event.name, event.args, event.status);
    }
  }

  const result = await run.wait();
  if (result.status === "error") {
    throw new Error(`Agent run failed (${result.id})`);
  }
  process.stdout.write("\n\n");
}

// runs the chat loop, adds a readline interface and disposes agents on exit
async function main(): Promise<void> {
  requireEnv("CURSOR_API_KEY");
  getComposioApiKey();

  console.log("Setting up Composio session...");
  const runtime = await getOrCreateRuntime(DEFAULT_USER_ID);
  console.log(
    `Ready (user: ${runtime.userId}, session: ${runtime.sessionId}, tools: ${runtime.toolsCount}).`,
  );
  console.log("Type a message, or 'exit' to quit.\n");

  const rl = readline.createInterface({ input, output });

  try {
    while (true) {
      const userInput = (await rl.question("you > ")).trim();
      if (!userInput) continue;
      if (userInput === "exit" || userInput === "quit") break;

      await runAgentChatStreaming(runtime.agent, userInput);
    }
  } finally {
    rl.close();
    await disposeAllAgents();
  }
}

main().catch((err) => {
  console.error(err);
  process.exit(1);
});
```
Let's look at the code: 

- We start by loading dependencies from `.env`, `readline`, and shared helpers from [`runtime.ts`](https://github.com/DevloperHS/cursor_agents/blob/main/runtime.ts) (`getOrCreateRuntime`, API key checks, and cleanup)
- For a sanity check, we validate `CURSOR_API_KEY` and `COMPOSIO_API_KEY` before the chat loop starts (Composio is also checked when `runtime.ts` loads).
- Next, we create a Composio session for the default user and spin up a Cursor agent on `composer-2` with the Composio MCP server for tools (handled in `runtime.ts`).
- We keep the same agent in the loop for every message, so tools and sessions stay wired throughout the CLI session.
- We run a terminal chat loop with readline: read input → send to agent → stream assistant text and tool status → repeat until `exit` or `quit`.
- While streaming, we log tool calls on `running` and `completed`, and throw if the agent run ends in error.
- On exit, we close readline, dispose of agents via `runtime.ts`, and log any failure from `main()` before exiting with code 1.

If you encounter an issue, refer to the codebase in the project's [GitHub repo](https://github.com/DevloperHS/cursor_agents).  Feel free to clone it, tweak it to your liking, or use it as it is.

And that's it, all set, time to run the agent!

---

## Run the agent

To run the agent in the terminal and at the project root, type:

```plain text
npx ts-node index.ts
```
and an interactive chat opens up where you can prompt it step by step / direct it to do the entire task at once. Here is a demo of me using it in chat mode (step by step)

<video src="file://%7B%22source%22%3A%22attachment%3Ac3ecbe78-f8ba-4dc1-9397-2ed3ed2eb102%3Acursor_agents.mp4%22%2C%22permissionRecord%22%3A%7B%22table%22%3A%22block%22%2C%22id%22%3A%2235df261a-6dfe-8092-970b-d8735b849b7a%22%2C%22spaceId%22%3A%2235adec70-dbef-4f42-9214-62ac8cdc4d75%22%7D%7D"></video>

As you can see, the cursor agent acts as the brain, calling the [Composio GitHub tools](https://composio.dev/toolkits/github) to clone the repo, check for changes, create a dev branch, refactor the code, commit to the dev branch, and finally open a PR.

Usually, this takes many human hours, but with a cursor-agent, it takes only 5 minutes.

Also, with the new update, you need to add a GitHub token to use GitHub with the cursor/agent. Using Composio fixes this need as well.

> Note: For simplicity I have used terminal , but you can add a ui layer on top of the agent.

With this, we have reached the end of the article. Here is my closing note.

---

## Final Thoughts

In this article, we learned how to use the Cursor SDK and Composio MCP to build a GitHub chatbot while keeping it optimized for asynchronous operations and following best practices, such as masking sensitive variables.

But this is just the tip of the iceberg with what's possible with the cursor agent SDK. Feel free to experiment and build your own assistant/chatbot. You can explore more in the [cursor-cookbook](https://github.com/cursor/cookbook).

At the end of the day, building agents / multi-agent systems is not hard; planning and orchestrating them to work efficiently, collaboratively, and securely is. In fact, it's the next valuable skill in 2026.

So what are you waiting for? Get started with Cursor Agents while [Composio MCP](https://composio.dev/) handles the tooling layer.
