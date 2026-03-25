# Slack with Claude

I spend most of my day in Slack. Team updates, meeting notes, quick decisions, and follow-ups. It all lives there. Threads move fast, and channels fill up even faster. When I want to use that context with an AI, I end up copying pieces into a prompt, refining them, and explaining what the thread was about.

I have done that more times than I can count. It always feels clunky.

Slack MCP changes that. It connects directly to your workspace and lets tools work with real conversations in real time using the actual content from your channels and threads.

Once connected, you can use the Slack data you already have and respond with the context that matters.

In this post, I will show you how to set up Slack MCP and use it to make Claude Cowork more helpful with your day-to-day conversations.

## What is Slack MCP

Slack MCP is a secure integration that connects your Slack workspace to tools like Claude and Cursor. It gives large language models direct access to your real Slack data through a fixed set of supported actions.

Once connected, Slack MCP allows these tools to:

- Read messages from channels or threads**: **Pull recent messages from public or private Slack channels for quick reference or summaries.
- Post messages**: **Send new messages into any Slack channel or thread based on prompts or actions.
- Reply in threads**: **Add follow-up responses to existing conversations in a thread format.
- Search messages:** **Find messages by keyword, timestamp, or sender, directly through the model.
- List users or channels**: **Retrieve a list of members or available channels in your Slack workspace.
- Fetch** user info: **Get details about any team member to personalize messages or automate updates.
- and more ….
This setup turns Slack into a live data source. Instead of copying and explaining messages to your tools, you can let them interact directly with your workspace using actual, up-to-date information.

## Setting Up Slack MCP with Claude Cowork

Claude Cowork is Anthropic’s agentic AI system built for knowledge work. It runs on desktops, connects to local files and apps, and handles multi-step tasks from start to finish. It removes the need to connect to Slack through config files or terminal commands, offering a simpler, no-code way to achieve the same outcome.

**What you need before starting**

- **Claude Desktop:** Download it from claude.ai/download. Cowork is available inside the desktop app along with Chat and Code.
- **A Claude Pro or Max plan:** Cowork is included in the Pro plan for quick tasks, and in the Max plan for more complex workloads.
- **A Composio account:** Sign up for free at dashboard.composio.dev
### Step 1: Connect Slack in Composio

- Visit [dashboard.composio.dev](http://dashboard.composio.dev/) and log in
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/slack-with-claude/images/1.png)

- On the left sidebar, click **Connect Apps**
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/slack-with-claude/images/2.png)

- Search for **Slack** and select it
- Follow the authentication flow to authorize access to your Slack workspace
Once complete, Slack will appear as a connected app in your Composio dashboard.

### Step 2: Open Connectors in Claude Desktop

- Launch **Claude Desktop**
- Go to **Settings**, then click **Connectors**
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/slack-with-claude/images/3.png)

### Step 3: Add the Composio MCP server

- Click **"Add custom connector"**
- Paste: `https://connect.composio.dev/mcp`
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/slack-with-claude/images/4.png)

This single URL connects Claude Desktop to all apps linked in Composio.

### Step 4: Authorize in your browser

A browser window will open automatically. Sign in to your Composio account and grant access. Once confirmed, the connection becomes active.

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/slack-with-claude/images/5.png)

### Step 5: Start using Slack in Claude Cowork

Composio tools, including Slack, are now available inside Claude Desktop. You can ask Claude to read conversations, summarise threads, post replies, and manage follow-ups using live workspace data.

## Use Case: Summarize Long Messages and Follow Up in Slack Threads

In busy Slack channels, long-form messages and updates often contain important information, decisions, announcements, event details, or kudos. With Slack MCP connected, you can use Claude Cowork to automatically summarize those messages and post follow-ups directly in the same threads.

Prompt Example

```plain text
1. Read the most recent long messages or threads in #general and #social.
2. Summarize key points: announcements, actions, and acknowledgments.
3. Reply to each thread with a summary and any next steps.
```

### 🔧 What Happens Behind the Scenes (via Slack MCP)


### Why Use This?

- **Reduce cognitive load:** Get quick insights without reading every message
- **Stay aligned:** Team members can catch up even if they missed the original post
- **Move faster:** Follow-ups are generated and posted automatically
### **Watch it in action**

This video shows the full flow from reading real Slack messages to summarizing them and posting a follow-up directly in the thread using Slack MCP.

[Video](https://youtu.be/kCaVMjQhqC4)

## Summary

Slack MCP lets Claude Cowork access and interact with your Slack workspace in real time. Instead of copying and pasting threads into prompts, you can let Claude read messages, summarize conversations, post replies, and keep you updated directly in your Slack.

By following a few simple steps using Composio, you can securely connect Slack to Claude. Once set up, Claude becomes a helpful assistant, working right alongside you in your Slack channels.

### FAQs

**1. How is Slack MCP different from using Slack bots or integrations?**

Slack MCP provides direct context access through MCP, allowing Claude to understand full conversations and act on them, not just respond to predefined commands like typical bots.

**2. Does Claude read all Slack messages automatically?**

Claude only accesses data that is permitted through the Composio connection and authentication scope. Access remains controlled and tied to your permissions.

**3. Can Claude take actions in Slack or only analyse messages?**

It can do both. Claude can read messages, summarise discussions, and also post replies or follow-ups directly in channels or threads.

**4. How does Composio manage multiple tool connections?**

Composio acts as a unified layer. Once Slack and other tools are connected, they are exposed through a single MCP endpoint, avoiding repeated setup steps.

**5. Is this setup suitable for team-wide workflows or individual use?**

It works for both. Individuals can manage their own conversations, and teams can use it to keep discussions organised and ensure follow-ups are clearly documented.
