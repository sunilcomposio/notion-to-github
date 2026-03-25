# Outlook with Claude

I use Outlook every day. Emails carry updates, questions, follow-ups, and decisions. Calendar events are packed with context. It is where most of my work lives, whether I plan for it or not.

Getting that information into LLMs like Claude takes too much effort. I copy pieces from a thread, clean them up, explain what they mean, and hope the response lands.

That routine gets old fast.

Outlook MCP changes how this works. It connects Claude to my inbox and calendar so it can pull what it needs directly. Everything stays up to date, and nothing important gets lost in translation.

In this blog, I will demonstrate how to set up Outlook MCP and utilize it with Claude to bring real context into your workflow without requiring additional steps.

## What is Outlook MCP

**Outlook MCP** is a secure integration that connects your Outlook account to large language models (LLMs) like Claude and tools like Cursor. It allows these AI systems to access your Outlook data in real time using a fixed set of supported actions.

With Outlook MCP, LLMs can read calendar events, list emails, send replies, create drafts, and more based on the actual content in your inbox and calendar. This eliminates the need to copy content or explain context in every prompt manually.

Once connected, Outlook becomes a core part of your AI workflow. It gives your assistant direct access to the information and tools you already use every day.

### What you can do with Outlook MCP

- **Read and manage your inbox**: List messages, reply to emails, send new messages, and download attachments from specific threads.
- **Create and update emails: **Draft emails before sending and update message content when needed.
- **View your calendar: **List upcoming or past events and get full details of any specific calendar entry.
- **Create events and schedule meetings: **Add new events to your Outlook calendar with defined start and end times.
- **Manage contacts: **Look up existing contacts or create new ones in your Outlook account.
- **Access your Outlook profile: **Retrieve basic profile details to support personalized responses and workflows.
## Connect Outlook to Claude Cowork using MCP

Claude Cowork is Anthropic’s desktop-based AI system designed for handling multi-step knowledge work. It connects to local files and external tools, allowing it to complete tasks end-to-end. Outlook can be linked without manual configuration via a guided setup in Composio.

**Prerequisites**

- **Claude Desktop:** Install it from claude.ai/download. Cowork is available within the app alongside Chat and Code.
- **Claude Pro or Max plan:** Required to access Cowork features, with higher limits available in Max.
- **Composio account: **Create an account at [dashboard.composio.dev](http://dashboard.composio.dev/) and use it to manage integrations 
### Step 1: Link Outlook in Composio

- Sign in to your Composio dashboard: [https://connect.composio.dev/](https://connect.composio.dev/)
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/outlook-with-claude/images/1.png)

- Navigate to **Connect Apps** from the sidebar
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/outlook-with-claude/images/2.png)

- Search for **Outlook** and choose it from the list
- Complete the sign-in and permission flow to grant access
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/outlook-with-claude/images/3.png)

Outlook will then be listed among your connected applications.

### Step 2: Access Connectors in Claude Desktop

- Open Claude Desktop
- Go to **Settings**
- Select **Connectors**
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/outlook-with-claude/images/4.png)

### Step 3: Register the MCP endpoint

- Click **Add custom connector**
- Enter the MCP server URL: `https://connect.composio.dev/mcp`
![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/outlook-with-claude/images/5.png)

This endpoint links Claude to all tools configured in your Composio account.

### Step 4: Complete authorization

A browser prompt will appear. Log in to Composio and approve access. Once confirmed, the integration becomes active.

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/outlook-with-claude/images/6.png)

### Step 5: Use Outlook within Claude

Outlook is now accessible inside Claude Cowork. You can retrieve emails, review threads, draft responses, and manage follow-ups directly using current mailbox data.

## **Use Case: Generate and Send Availability Based on Outlook Calendar**

Consider you have a few meetings tomorrow, and you want to let someone know when you will be available. Instead of checking your calendar manually and drafting an email, you can ask Claude to handle it using the Outlook MCP connection.

### Prompt

```plain text
1. Check my Outlook calendar for tomorrow.
2. List my busy time slots and identify when I am free.
3. Send an email to example@gmail.com with my availability.
```

### What happens behind the scenes

1. **List events:** MCP fetches all events from your Outlook calendar for the day
1. **Parse busy times:** Free time is inferred from gaps between scheduled events
1. **Send email:** A message is composed and sent via the **Send email** action
### Watch it in action

This video shows the full flow from checking your Outlook calendar to sending an email with your availability.

[Video](https://youtu.be/jq9QtTbgNw8)

[Video](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/f14a9f62-5cf2-4418-b389-86a812213dbc/Outlook_-_Made_with_Clipchamp.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCTL3DDV%2F20260325%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260325T145829Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFGJpUYszmFiE%2BJAo3jiHpV%2FirLhBdZw%2FdxOofiyJB2WAiEAh5UHPTKQ4g3k7aJ%2Bu6j9AqetOtzE3fdoT9GaRnVvmfkqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBkI2arNXvJxQo%2FnCircAyu5RXKLlwOTlHK9nT%2BAoVcydvgp5x66wDcDKy%2FsBCAvSNpDrZM1f7IqyxjkgjDsytKbYzFrtbCKItLJ2U93ND%2FKMKrdtVBLY9uxEPaIYU%2B7Vr4ITcJXWI0VxfB85kLMWYLct0AsDflefJU%2BNFgEA0rz%2FDRJ2urfqJwguYXexthRWHdqNtyQUb80ECwhJ3GiA42o0L2qSJZe%2BUAp%2BWBq1zaLzC8Z244jyNomjabe8zZljoftDGz5GY9Mv%2BgSYpc1AqqTpoLgDqKz8FpQEBuoQp4V2NoXUUm7IIQA%2F5ZrzS%2Bm%2B1YCM%2BXmEBJLiVUsd4KSrtFmYZt6MDjeDhcF0n0sQp%2BMK8ygx2LZYQl8HvpV1AWBuDjkWNd0%2BziaNWZO4HE1vU0LDuPVC5rcJp3C0iFPpc70JZil%2FLGPJ%2B5IFDAPCicdv5fAW%2FIY7QsUewOiiciyFORsoq7iWk7qxLUcovuGuZRQlP7%2FuWmpGA%2FkLsnOp9uK5gNPkRQVyFjr8Uh%2B%2BUrd7ycq436bD0LlhM6AGdoK%2FHrGB6v5KLOl2THT6p9iUsG%2FdGAnm0KnpNG7VOAw1x0JbDmy63GkpgcsNsqkvGEWaUVCKdG%2FVWan5ELJl1EOjl7RbOMxgr%2FyMPrb3KVeMOXgj84GOqUBGiJb4KGXVW9SyLt8CnoTEDOSNB4JsTPgbgL2OGxI9E1WO3Z4iAJuSiDnxiJj7aZdWXYEpDEoqpvyEyR74CdOtdk3CP9Df7sD3MiEwLBtVhCWim5wH%2BWfdkYuX8kYrqkiazXN1MWwY72zIQbbbi%2B5Row%2FPV7poyZnwNQRt2FhVDuU6B32cAYDwZRpgViz7%2F0MBgiITrBNKFEg42PAWn%2Fpfn05H2hp&X-Amz-Signature=633abaad1829e1e18b76a2689549a4f7d6e3db34e19a300f8e40e473bc909753&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

## Summary

Outlook holds the context behind your work. Composio’s Outlook MCP brings that context into Claude in a way that fits naturally into your workflow.

Once the connection is set up, Claude Code can access real-time information from your inbox and calendar. You can stay focused and move through tasks with less effort.

## Frequently Asked Questions

### 1. Can I control what Outlook MCP can access?

Yes. When setting up the MCP through Composio, you choose which actions to enable. You can allow only what your workflow needs, like listing calendar events or sending emails.

### **2. Does Claude store any of my Outlook data?**

No. Claude accesses your data live through Composio’s secure MCP connection. Nothing is stored permanently or locally unless you explicitly save it yourself.

### **3. Can I use the same Outlook MCP server with other tools?**

Yes. The same MCP endpoint can be used across tools like Cursor or custom scripts, as long as they support MCP and the required actions.

### **4. What happens if I update or delete an Outlook event?**

Since Claude pulls data in real time, any updates to your Outlook calendar or inbox will be reflected immediately in future prompts.

### **5. Do I need to run the setup script every time?**

No. You only need to run the script once. After setup, the connection persists in your Claude config unless you remove it manually.
