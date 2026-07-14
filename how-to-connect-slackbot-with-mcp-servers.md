# How to Connect Slackbot with MCP Servers in 3 Steps

Slackbot recently added support for MCP, which means you can now connect it with external apps and let it take actions across your work tools directly from Slack

But the native app list is still limited. By the time of writing this post, there's just about **20 apps** that you can connect from the [Slack marketplace](https://slack.com/marketplace).

And for most teams, that's not enough.

But luckily, Slack allows you to set up or use your custom MCP servers and not have to be limited by the number of apps available in marketplace.

That's where [Composio](https://composio.dev/) helps you. It can connect your slack bots to **1000+ apps** that you can use.

In this guide, we’ll go through how to connect Slackbot with Composio’s MCP server in 3 steps.

> ℹ️ The steps will be pretty much the same with other MCP servers as well.

---

## What We're Building

Once this is set up, you can ask Slackbot things like:

```plain text
Find my latest unread Gmail emails.
Search my Notion workspace for launch notes.
Check my Google Calendar for meetings tomorrow.
```

And a bunch more. Imagine all the stuff you can do with 1000+ apps. 😵‍💫

I'll leave the rest to your imagination...

Slackbot sends the request to Composio Connect, Composio finds the right tool, asks you to connect the app if needed (one time), and then executes the action.

---

## Prerequisites

Before we begin, make sure you have:

- A Slack workspace with Slackbot MCP client access (comes with Business+ and Enterprise plan)
- Permission to create or configure a Slack app.
- A Composio account.

That's it.

---

## Step 1: Get the Composio Connect MCP URL

First, you need the MCP server URL from Composio.

For this setup, use Composio Connect:

`https://connect.composio.dev/mcp`

This is Composio’s hosted MCP server that gives your AI agent access to 1,000+ apps with just **7 meta-tools** that let the slackbot discover what's available, authorize apps on demand, and execute tools across apps in parallel through a single connection.

You don’t need to create a custom MCP server for this guide.

Composio also supports custom MCP servers for more scoped project-specific use cases, but those can require API-key-based auth. For Slackbot, Composio Connect is the simpler path because it works with OAuth-based MCP client flows.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-slackbot-with-mcp-servers/image_1.png)

---

## Step 2: Add Composio Connect to a Slack App

Now, we need to register the Composio MCP server inside a Slack app.

Go to the Slack developer dashboard and create a new app.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-slackbot-with-mcp-servers/image_2.png).png%22%2C%22permissionRecord%22%3A%7B%22table%22%3A%22block%22%2C%22id%22%3A%22398f261a-6dfe-80e9-af27-cc939f515244%22%2C%22spaceId%22%3A%2235adec70-dbef-4f42-9214-62ac8cdc4d75%22%7D%7D)

Once the app is created:

1. Open your Slack app.
2. In the left sidebar, go to **Features**.
3. Click **MCP Servers**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-slackbot-with-mcp-servers/image_3.png).png%22%2C%22permissionRecord%22%3A%7B%22table%22%3A%22block%22%2C%22id%22%3A%22398f261a-6dfe-806d-9786-f4403370289c%22%2C%22spaceId%22%3A%2235adec70-dbef-4f42-9214-62ac8cdc4d75%22%7D%7D)

1. Click **Get Started**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-slackbot-with-mcp-servers/image_4.png).png%22%2C%22permissionRecord%22%3A%7B%22table%22%3A%22block%22%2C%22id%22%3A%22398f261a-6dfe-802a-a94d-dc310c0a4acc%22%2C%22spaceId%22%3A%2235adec70-dbef-4f42-9214-62ac8cdc4d75%22%7D%7D)

Now fill in the MCP server details.

Use:

- **Name**: Composio (or anything you wish)
- **URL**: [https://connect.composio.dev/mcp](https://connect.composio.dev/mcp)
- **Auth Type**: Dynamic Client Registration

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-slackbot-with-mcp-servers/image_5.png).png%22%2C%22permissionRecord%22%3A%7B%22table%22%3A%22block%22%2C%22id%22%3A%22398f261a-6dfe-80a5-8cd8-ee698f06551e%22%2C%22spaceId%22%3A%2235adec70-dbef-4f42-9214-62ac8cdc4d75%22%7D%7D)

For the auth type, select **Dynamic Client Registration**.

> ℹ️ This is the right option for MCP servers that support OAuth discovery and client registration. Slack handles the client registration automatically, so you don’t need to manually create OAuth credentials first.

Now, if you click the three dots and then **Tools**, you should see that it currently cannot fetch the tools because the MCP server uses a dynamic connection and must be installed in your workspace first.

So, now head over to the **Install App** tab, and install it to the workspace you selected when creating the app.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-slackbot-with-mcp-servers/image_6.png).png%22%2C%22permissionRecord%22%3A%7B%22table%22%3A%22block%22%2C%22id%22%3A%22398f261a-6dfe-80f3-8fb5-e06e2e0618f3%22%2C%22spaceId%22%3A%2235adec70-dbef-4f42-9214-62ac8cdc4d75%22%7D%7D)

If your workspace requires approval, send the app request to your admin.

---

## Step 3: Connect Composio inside Slackbot

Once your Slack app is installed and approved, open a DM with Slackbot.

Then, just type in a prompt that requires using the app, Slack will use the correct app automatically for you.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-slackbot-with-mcp-servers/image_7.png).png%22%2C%22permissionRecord%22%3A%7B%22table%22%3A%22block%22%2C%22id%22%3A%22398f261a-6dfe-80b5-9d0d-ff75954a323d%22%2C%22spaceId%22%3A%2235adec70-dbef-4f42-9214-62ac8cdc4d75%22%7D%7D)

Click **Connect, **and you’ll be taken to a confirmation page.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-slackbot-with-mcp-servers/image_8.png)

Click on Continue, and then confirm it on the Composio end.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-slackbot-with-mcp-servers/image_9.png)

If everything went well, you should see that your account is connected.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-slackbot-with-mcp-servers/image_10.png)

After that, Slackbot should be able to discover Composio’s MCP tools.

Start with a simpler test:

```plain text
What tools are available from Composio?
```

Once that goes through, now try an actual app action.

For example:

```plain text
Send a mail to x@y.com saying 'Hi, from Composio 👋 inside Slackbot'
```

If Gmail is not connected yet, Composio should generate an OAuth link for you to connect it. Once you approve it, the connection persists for future use. So, you don't have to repeat this step again and again.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-slackbot-with-mcp-servers/image_11.png).png%22%2C%22permissionRecord%22%3A%7B%22table%22%3A%22block%22%2C%22id%22%3A%22398f261a-6dfe-80e0-906f-f79c2b812612%22%2C%22spaceId%22%3A%2235adec70-dbef-4f42-9214-62ac8cdc4d75%22%7D%7D)

Slackbot may ask you to approve the action before it writes data to another app.

That's expected. Once connected, Slackbot can use that app through Composio.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-slackbot-with-mcp-servers/image_12.png).png%22%2C%22permissionRecord%22%3A%7B%22table%22%3A%22block%22%2C%22id%22%3A%22398f261a-6dfe-80e8-a7d1-fa15f580c3b1%22%2C%22spaceId%22%3A%2235adec70-dbef-4f42-9214-62ac8cdc4d75%22%7D%7D)

Voilà, you've successfully connected Slackbot to Composio MCP. 🎊

Here’s a quick workflow for initiating a connection and running an actual app action:

https://github.com/sunilcomposio/notion-assets/raw/main/assets/how-to-connect-slackbot-with-mcp-servers/demo.mp4

---

## Final Thoughts

Slack's own marketplace is good, and if it covers all the tools you require, you can completely stick to it.

But for some of you, that's simply not enough. I hope this helps overcome that problem.

So instead of jumping between different tools, you can ask Slackbot to find information, create tasks, update records, and run actions across your apps from inside Slack.

This is a much-needed quality-of-life improvement for teams that already live in Slack.

Slackbot gives you the interface. MCP gives you the protocol.

And Composio gives you the app layer. 👌
