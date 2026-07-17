# How to Send Emails in Gmail from Claude

You can’t send emails from Claude’s Gmail connector, and this guide is exactly for that.

My entire work lately has shifted to Claude. It has become my default workspace, and I have it connected with all the tools I need for my work. PostHog, Plausible, Search Console, Ahrefs, and a dozen others. Gmail was among them, but it was totally useless.

The default connector has just six tools: create_draft, get_thread, list_drafts, list_labels, and search_threads.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-send-emails-in-gmail-from-claude/image_1.png)

Can you imagine having a Gmail connector and being unable to send emails or triage existing ones? What's even the point of it?

So, [Composio Gmail](https://composio.dev/toolkits/gmail) is how you solve it.

It provides more than 61 tools, including 28 read-only actions for fetching emails, drafts, and threads; write actions for creating drafts, modifying labels, forwarding messages, and sending emails; and seven destructive actions for deleting emails, drafts, and threads.

And cherry on top, if you’re worried about Claude deleting or sending emails on its own, we’ve an Enhanced Control feature, which lets you control which tools you want to expose to Claude or any agent for that matter. Read more on it here: [Introducing Enhanced Control](https://composio.dev/content/introducing-enhanced-controls-beta)

## How to add Gmail connector to Claude in 4 steps

### Step 1: Sign up to Composio

Go to [dashboard.composio.dev](http://dashboard.composio.dev) and make sure you’re in the For you dashboard.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-send-emails-in-gmail-from-claude/image_2.png)

### Step 2: Create a new Gmail connection

Click on the Gmail icon and click Connect

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-send-emails-in-gmail-from-claude/image_3.png)

Authorise Composio to access your Gmail account. Here, you can select all the tools you want Claude to use. It defaults to all the tools.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-send-emails-in-gmail-from-claude/image_4.png)

Note: To get the enhanced control, go to settings and toggle enhanced control.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-send-emails-in-gmail-from-claude/image_5.png)

Finish the authentication process, and you have your Gmail connection ready.

Bonus: You can also connect multiple Gmail accounts through Composio, and honestly, no other provider is doing that.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-send-emails-in-gmail-from-claude/image_6.png)

### Step 3: Add Composio Gmail to Claude

Now copy this MCP URL

```markdown
https://connect.composio.dev/mcp
```

It’s there in the dashboard as well.

Now, open [Claude.ai](http://Claude.ai), go to [Claude Connectors](https://claude.ai/new#settings/customize-connectors) in the Settings, and click on **Add custom connector**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-send-emails-in-gmail-from-claude/image_7.png)

Fill in the boxes with the connector name and MCP server URL you just copied, and click **Add**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-send-emails-in-gmail-from-claude/image_8.png)

### Step 4: Connect Composio in Claude and start using Gmail

You’ll see a screen to authenticate Composio. Click on **Connect**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-send-emails-in-gmail-from-claude/image_9.png)

Click Allow access.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-send-emails-in-gmail-from-claude/image_10.png)

Annnd voilà! We’re done.

Now, ask Claude to draft an email and send it to whoever you want.

```markdown
Draft and send an email to [recipient@email.com] with the subject “Quick catch-up.” 
Tell them I’d like to schedule a 30-minute call next week to discuss [topic]. 
Keep it concise and friendly, and ask which day and time works best for them.
```

It doesn’t stop here. You can connect any number of apps to Claude from Composio’s massive [1047 toolkit catalogue.](https://composio.dev/toolkits)
