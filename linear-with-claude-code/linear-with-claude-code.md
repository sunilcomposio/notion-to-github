Linear with CC

In my previous posts, I showed how I used Claude with Composio to skip dashboards and manage tools like Neon and Supabase directly from the terminal.

This time, let’s talk about Linear.

Linear is already fast and developer-friendly, but even then, opening the UI every time to create issues, assign tasks, or update statuses can break your flow.

So instead, I connected Linear with Claude Code using Composio.

Now, I can manage my entire Linear workspace using simple prompts, no UI, no context switching.

## What is MCP?

This time, let’s briefly explain MCPs with a use-case lens:

> *Think of MCPs as a way to turn APIs into something that Claude can “understand” and “use”, like plugging tools into some AI Agent’s brain.*

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/linear-with-claude-code/images/1.png)

For more background information, refer to my [Jira blog](https://composio.dev/blog/jira-mcp-server) or Anthropic’s [MCP overview](https://modelcontextprotocol.io/introduction). Also, check out [Airtable ](https://composio.dev/blog/how-to-setup-airtable-mcp-for-effective-work-manahement)and the [Asana MCP server](https://composio.dev/blog/how-to-use-asana-with-claude-and-cursor-for-effective-project-management).

## What can a Linear MCP Server do?

Let’s say you’re in a flow, ideating or writing code, and you suddenly think:

“I should create a bug ticket and assign it to someone in the frontend team.”

With Linear MCP and Claude, you type:

> *“Create a bug in the Payments project called “Fix refund edge case crash” and assign it to @alex.”*

… and it’s done.

No switching tabs. No forms, no remembering project IDs.

### Things you can do with Linear MCP Server:

- Create issues using `LINEAR_CREATE_LINEAR_ISSUE`
- **Update issue status, title, priority** with `LINEAR_UPDATE_ISSUE`
- **Delete issues** when no longer relevant using `LINEAR_DELETE_LINEAR_ISSUE`
- **Fetch issue details** on demand with `LINEAR_GET_LINEAR_ISSUE`
There are many tools available for use; follow this documentation page.

## Why use Composio for this?

If you connect directly to Linear’s API, you’d have to handle:

- OAuth flows or API tokens
- Session and token management
- API updates and tool definitions
Composio handles all of that for you.

It acts as an integration layer that manages authentication and tool access, so you can just connect Linear and start prompting.

---

## What we’ll cover

- What Linear MCP is and how it works
- How to connect Linear with Claude Code
- How to automate issue tracking using prompts
## Setup using Composio Connect (Recommended)

With the new setup, you no longer need to configure MCP servers manually or run CLI commands.

### Step 1: Open Composio Connect

Go to: [https://connect.composio.dev/](https://connect.composio.dev/)

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/linear-with-claude-code/images/2.png)

---

### Step 2: Enable Linear Integration

- Search for **Linear**
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/linear-with-claude-code/images/3.png)

- Click **Integrate**
- Authenticate your Linear account
Once connected, your integration is ready to use.

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/linear-with-claude-code/images/4.png)

### Open Connectors in Claude Desktop

- Launch Claude Desktop
- Go to **Settings**
- Select **Connectors**
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/linear-with-claude-code/images/5.png)
### Step 3: Add the MCP server

- Click **Add custom connector**
- Enter the URL: `https://connect.composio.dev/mcp`
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/linear-with-claude-code/images/6.png)
This connects Claude to all applications linked through Composio.

### Step 4: Authorise access

A browser window will open. Sign in to your Composio account and approve the connection. Once completed, the integration becomes active.

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/linear-with-claude-code/images/7.png)

## Using Linear with Claude Code

Once your Linear integration is connected via Composio Connect, you can start managing your issues directly using Claude Code.

For example, you can prompt:

- “Create a bug in the Billing project with priority High.”
- “Assign this issue to Emily and label it urgent.”
- “Create a new tasks due this week.”
- “Update the status of issue XYZ to Done.”
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/linear-with-claude-code/images/8.png)

Claude will automatically execute these actions and return structured results.

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/linear-with-claude-code/images/9.png)

---

## Conclusion

Linear already provides a great developer experience, but combining it with Claude Code and Composio makes it even more powerful.

With Composio Connect, you can skip manual setup entirely and start managing your issues using natural language.

This lets you stay focused, move faster, and automate repetitive project management tasks effortlessly.


