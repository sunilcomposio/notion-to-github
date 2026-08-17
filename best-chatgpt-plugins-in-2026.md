# Best ChatGPT Plugins I'm Using in 2026

ChatGPT can only use information that it can see. If your work is in Gmail, Drive, Slack, or SharePoint, ChatGPT cannot read it until you connect that app.

A plugin makes this connection. This is also why many people connect 11 plugins in one evening. A week later, they use one plugin, and the other 10 remain as permissions that they have forgotten.

This post covers 10 plugins that stay useful. It also explains what access each one requires.

## What changed in July 2026?

OpenAI changed these terms in July, but many online guides still use the old terms. Here is a simple explanation.

A **plugin** can contain skills and apps. Some plugins contain only skills, while others contain a skill and the app that it needs.

A **ChatGPT app** gives ChatGPT access to another service. It connects ChatGPT to Gmail, Drive, Slack, or SharePoint, so it can read your information.

A **skill** is a defined process. It tells ChatGPT how to complete a task in the same way each time.

On 9 July 2026, OpenAI moved the old app directory into the Plugin Directory. Plugins are now the main way to find capabilities in ChatGPT and Codex. Your existing connections continued to work after this change, but the menus moved.

## An important change on 14 August

OpenAI stopped individual-user sync. From 10 August 2026, you can no longer create a new sync connection for your own account. On 14 August 2026, OpenAI [disabled the existing individual-user sync connections](https://openai.com/products/release-notes/) and started to delete the synced data.

Administrator-managed sync still works. If your team needs synced search across files, an admin must set it up for the whole workspace.

Read this before you follow any older guide. Many of them still tell you to turn on Sync yourself, and that step no longer works.

## How to connect a plugin

1. Open the directory at `chatgpt.com/plugins`. You can also go to **Settings → Plugins**, or select **Plugins** in the sidebar. This works in the web app and the desktop app.
2. Select the app that you want.
3. Select **Connect**.
4. Complete the OAuth sign-in for that service.
5. Use the app in a chat with an @ mention. You can also select **+ → More** and choose the app.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/best-chatgpt-plugins-in-2026/image_1.png)

If **Connect** is not available, the cause is usually a geographic restriction, your workspace settings, or your plan. If the message says "Disabled by admin," an administrator has turned the plugin off.

**A note on regions.** Several of these plugins do not work in the EEA, Switzerland, or the UK. The rules differ for each app, so check the plugin listing before you plan around it.

## If you are the admin

Go to **Workspace settings → Plugins**. Select the plugin, review its skills and the apps that it needs, then set the Installation policy to **Available** or **Installed** for each role. OpenAI explains the full set of controls in its [admin documentation](https://help.openai.com/en/articles/11509118-admin-controls-security-and-compliance-for-plugins-and-apps).

Two points cause confusion here.

**Available** and **Installed** control installation only. They do not give access to an app or to its data.

Approval in ChatGPT does not change permissions in the source system. If a user cannot see a file in SharePoint, the plugin must not show that file to them in ChatGPT.

Keep your first version read-only. You can add write actions later.

## Best ChatGPT plugins

### 1. Google Workspace

For most people, Gmail, Calendar, and Drive hold about 80% of what any task needs. When you connect them, you remove the slowest step in almost every workflow, and that step is to find the thing before you can use it.

Note two changes before you start.

The separate Docs, Sheets, and Slides apps are gone, and [everything runs through the single Google Drive app now](https://help.openai.com/en/articles/10948259-google-drive-app-with-sync-self-service-setup). On Enterprise and Edu, those Drive actions stay off until an admin turns them on. On Business, they are on by default. New users now connect Google Drive once for Drive, Docs, Sheets and Slides, but previously connected standalone apps **continue to work**

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/best-chatgpt-plugins-in-2026/image_2.png)

From 13 August 2026, you can also keep a Doc, Sheet, or Slide open beside the conversation on the web. Your Drive files and folders appear in the ChatGPT Library, and you can pull a file into any chat with an @ mention. You do not upload it again.

**To connect:**

1. Go to **Settings → Plugins**. Some accounts show this as **Apps & connectors**.
2. Find **Google Drive**, **Gmail**, or **Google Calendar**.
3. Select **Connect**.
4. If it asks for a connection type, choose **Chat** where you can, or choose **Deep research and agent mode**.
5. Complete the Google OAuth flow.

Google added new OAuth scopes in June 2026. If you connected before then, you may see an authorization error, and you must approve the new scopes. OpenAI lists the details in its [Google data controls FAQ](https://help.openai.com/en/articles/10408842-google-app-for-chatgpt-data-controls-faq).

Use it to find the client emails that matter today and to draft the replies. Use it to check your real availability before you propose times. Use it to find the current version of a document, or to turn a brief into a Doc.

### 2. Slack

Slack records the reason behind a decision. To find that reason, you scroll through a thread for twenty minutes, and you are not sure you were even in it.

There are two separate products here, and people mix them up.

**The Slack connector** brings Slack content into ChatGPT. It works on Plus, Pro, Business, and Enterprise or Edu, and it works in chat, Deep Research, and Agent Mode.

**To connect:**

1. In ChatGPT, open **Settings → Plugins**.
2. Find **Slack**, then select **Connect**.
3. Complete the OAuth sign-in.
4. If prompted, ask your Slack admin to approve the connector.

**The ChatGPT app for Slack** puts ChatGPT inside Slack. Turn on the connector above first, because the app needs it.

**To install it:**

1. Open the Slack Marketplace and search for **ChatGPT**.
2. Install the app.
3. In Slack, select **Connect ChatGPT** to open the ChatGPT settings for Slack.
4. If Slack already appears connected, select **Disconnect**, then **Connect** again. This step is in the documentation, and it is not a fault.
5. Authenticate and choose the workspace where your admin enabled the app.

It needs a paid Slack plan, and it connects to one workspace at a time.

The best use is "what happened while I was out." After that, use it to build a meeting brief from the week's channel activity, or to find the conversation that led to a technical choice.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/best-chatgpt-plugins-in-2026/image_3.png)

### 3. SharePoint and OneDrive

If your knowledge sits across Word files, Excel workbooks, and PowerPoints, this one saves you real time.

Pick the right connector first. OneDrive covers personal files, and SharePoint or OneDrive for work and school covers your organisation.

**To connect:**

1. In ChatGPT, open **Settings → Plugins**.
2. Select the Microsoft app that matches your account.
3. Sign in with your Microsoft account.
4. Review and approve access.
5. On a work tenant, ask your Microsoft 365 admin to grant consent if prompted.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/best-chatgpt-plugins-in-2026/image_4.png)

Set your expectations here. In practice this is read-only. ChatGPT can find, read, and analyse your files, and it cannot upload, edit, move, or rename them.

For an organisation-wide rollout, use the admin-managed sync connector. One admin authenticates it and deploys it to everyone. That admin must be a SharePoint or tenant administrator and also a ChatGPT administrator, because one role alone is not enough. You can limit it to specific sites and folders, and updates reach ChatGPT within about an hour.

**One point to watch:** the newer scopes need Entra admin approval, and admins often disconnect and reconnect to get them. That forces a full re-index. Use the **Reconnect SharePoint for better syncing** prompt in **Workspace Settings → Apps** instead.

### 4. Canva

Check availability first, because this one has a hard limit. The Canva app does not work in the EU or in mainland China. Everywhere else it works on Free, Plus, and Pro with a free Canva account. Anything that needed Canva Pro in the normal editor still needs Pro when you come through ChatGPT.

**To connect:**

1. In ChatGPT, open **Settings → Apps & connectors** or **Settings → Plugins**.
2. Find **Canva**, then select **Connect**.
3. Sign in to Canva and authorise access.
4. In a chat, type `@Canva` with your prompt, or describe what you want.

Designs appear in the conversation and stay editable, and you can open them in Canva for finer control.

Use it to turn a report into a deck without two hours of reformatting, or to turn campaign copy into posts sized for each platform. It also does a readability pass before something goes out.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/best-chatgpt-plugins-in-2026/image_5.png)

### 5. Composio

Composio works differently from the others. You do not install it from the directory, because it is a hosted MCP integration that you add as a custom app.

Its strength is breadth. One connection reaches hundreds of apps, and that includes the smaller tools that will never get their own first-party listing. It handles app discovery, OAuth, connected accounts, and tool execution, and you would otherwise build and maintain all of that yourself.

It also loads tools selectively. Many MCP servers push every available tool into the model's context. Composio searches first, and it loads only the tools that your task needs.

**Before you start, you need three things:**

- A ChatGPT account with access to ChatGPT Work. It is rolling out across<br>Pro, Enterprise, Edu, Plus, and Business plans.
- Access to the Composio workspace that you want to connect.
- A Composio MCP account.

**The step almost everyone misses:** when you authenticate the server, you connect ChatGPT to Composio. You do not authorise every app in the catalogue. You still connect each app inside Composio.

Composio's own documentation makes the fair counterpoint. If you need one read-only app, a built-in connector is enough, and you do not need an MCP server to summarise a few documents. Composio helps when you need write actions, or when your workflow crosses several tools.

### 6. GitHub

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/best-chatgpt-plugins-in-2026/image_6.png)

If you have ever answered "how does this part of the system work" by cloning a repository and reading for an hour, this one removes that hour.

The connector reads code, README files, and documentation inside the repositories you authorise. It then answers questions and cites the file paths in the reply. You can follow a flow across files without opening any of them.

**To connect:**

1. In ChatGPT, open **Settings → Plugins**.
2. Find **GitHub**, then select **Connect**.
3. Complete the GitHub OAuth sign-in.
4. Select the repositories you want ChatGPT to see.

Private repositories are included if you have access to them. A new repository takes a few minutes to index, so an empty first result usually means you asked too soon.

Use it to understand an unfamiliar codebase before you join a project. Use it to write an architecture summary for people who do not read code. Use it to find every TODO that mentions something you plan to deprecate.

### 7. Linear

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/best-chatgpt-plugins-in-2026/image_7.png)

Linear records the work, and the comments on each issue record the reason for the work. The second part is the part you cannot find later.

This connector changed recently, and older guides get it wrong. Linear used to work in deep research mode only. It now works in chat as well, so you can ask about your tracker in a normal conversation. It also reads and writes, because it runs on [Linear's own MCP server](https://linear.app/integrations/chatgpt). ChatGPT can file an issue or update one when you ask it to.

It keeps your existing user-level permissions, so you see what you could already see in Linear.

**To connect:**

1. In ChatGPT, open **Settings → Plugins**.
2. Find **Linear**, then select **Connect**.
3. Sign in to your Linear workspace.
4. Review and approve access.

Use it to build a status picture across several projects at once. Use it to find the issue thread behind a decision that now looks strange. Use it to write a release summary from the work that shipped.

### 8. Figma

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/best-chatgpt-plugins-in-2026/image_8.png)

Most connectors on this list read. Figma writes, and that is why it is here.

You can ask for a FigJam flowchart or a Figma Slides deck in a normal conversation, and ChatGPT creates the file in Figma. Start the prompt with the word Figma, or select the **+** icon and choose the Figma app. Explicit triggering gives better results.

**To connect:**

1. In ChatGPT, open the left sidebar.
2. Select your profile or username.
3. Select **Apps & connectors**.
4. Find **Figma**, then select **Connect**.
5. Sign in to Figma and allow access.

Figma and Canva overlap, so choose on the output. Canva produces something close to finished, and it suits social posts, one-pagers, and decks that go out as they are. Figma produces something a designer will continue to work on, and it suits flowcharts, wireframes, and structure.

Use it to turn a written plan into a diagram that people will look at. Use it to give a designer a starting file instead of a paragraph that describes what you want.

### 9. HubSpot

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/best-chatgpt-plugins-in-2026/image_9.png)

A CRM answers "what is the value of this deal" easily. It answers "what do our last twelve losses have in common" slowly, if at all. That second question is the one worth connecting for.

Check two things before you start. The connector works in the web version of ChatGPT only. It also needs a paid OpenAI plan, which means Pro, Plus, Team, Enterprise, or Edu, although any HubSpot tier works, including free. Chat on the Pro tier is not available in the EEA, Switzerland, or the UK.

**To connect:**

1. In ChatGPT, open **Settings → Connectors**.
2. Select **HubSpot**, then click **Connect**.
3. Click **Continue to HubSpot**.
4. Sign in with your HubSpot credentials.
5. Select the HubSpot account you want to connect.
6. Select the permissions you want to allow, then click **Connect App**.

A Super Admin, or a user with App Marketplace permissions, must complete the first setup. Other users then repeat the same steps for themselves.

**Set your expectations here.** The connector can both read and write HubSpot data, but write access depends on the object. It can read, create, and update contacts, companies, deals, tickets, line items, and products. It can also read engagement history and create or update calls, emails, meetings, tasks, and notes. Other objects, including invoices, orders, carts, quotes, subscriptions, and segments, are read-only. It cannot delete records.

ChatGPT can create and update records and log activities. Bulk creates and updates are limited to 10 records per request. Custom objects are not supported, and HubSpot's custom validation rules are not applied when the connector creates or updates records.

If you connected the HubSpot connector before February 23, 2026, you need to upgrade it to get the newer edit permissions and engagement access. Workspace admins must approve the updated app before users can upgrade and reconnect. HubSpot records create and update actions in the Audit Log against both the user and the ChatGPT connector.

HubSpot recommends setting write tools to **Needs Approval**, so you can review changes before they run. If you instead set them to **Always allow**, edits can happen without a confirmation prompt.

If your HubSpot account has Sensitive Data turned on, the connector cannot access engagement data or custom Sensitive Data properties. Access otherwise follows each user's existing HubSpot permissions.

### 10. Notion

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/best-chatgpt-plugins-in-2026/image_10.png)

If your team keeps decisions, processes, and project history in Notion, this is the connector that lets ChatGPT answer from your own material instead of from general knowledge.

**To connect:**

1. In ChatGPT, open **Settings → Connectors**. Some accounts show this as **Apps**.
2. Find **Notion**, then click **Connect**.
3. Sign in and authorise your Notion workspace.
4. Grant access to the specific pages and databases you want ChatGPT to reach.

Connectors are not available on the free ChatGPT tier.

**One trap worth flagging:** sharing a parent page does not reliably cascade to every page beneath it. If a sub-page does not appear in results, share that sub-page directly rather than assuming the permission was inherited.

Use it to find a decision and the reasoning recorded alongside it. Use it to pull the current version of a process instead of the version someone remembers. Use it to ground a research brief in your internal sources before ChatGPT goes anywhere near the public web.

## Connecting every app you use though one plugin

If you work across GitHub, Figma, Linear, Slack, etc, you’d have to install and configure every one of them.

And this is inefficient. It adds unnecessary bloat to the model’s context. Besides in Codex or ChatGPT you can’t use Gmail and Outlook to send or delete emails. This extends to other plugins where you can barely do anything with limited skills.

Composio solves this and more…

**To connect Composio in ChatGPT Work:**

1. Go to **Settings** and open the **Plugins** section.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/best-chatgpt-plugins-in-2026/image_11.png)

1. Select the **MCP** tab.
2. Select **Add server**.
3. In the **Name** field, enter `Composio`.
4. For **Type**, select **Streamable HTTP**.
5. In the **URL** field, paste `https://connect.composio.dev/mcp`.
6. Select **Save**. The server now appears in your MCP plugins list.
7. Select **Authenticate**, log in through Composio, then select **Allow access**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/best-chatgpt-plugins-in-2026/image_12.png)

Composio connects through OAuth, so you sign in and approve specific permissions. Read the permission screen carefully if you use a work account. Grant only the permissions that you need, and keep manual confirmation on for write actions. Composio gives the full steps and its security guidance in its [ChatGPT Work setup guide](https://composio.dev/toolkits/composio/framework/chatgpt).

### How Composio keeps it secure and efficient

- **Tool routing:** Composio searches the available integrations and loads only the tools needed for the task. This keeps irrelevant tool definitions out of the model’s context and makes selection more reliable.
- **Auth management:** Composio manages OAuth connections and credentials centrally, so users authenticate through the provider instead of pasting tokens into prompts or configuration files. Each app is authorised separately with its own permissions.
- **Team-level access:** Teams can manage shared integrations and access policies from one workspace instead of asking every person to configure the same stack independently.
- **Role-based access control (RBAC):** Administrators can limit who can access particular integrations and actions based on their role. This helps keep sensitive or write-capable tools available only to the people who need them.

The practical result is a smaller tool context, fewer repeated setup steps, and clearer control over who can access and change data.

## A rollout that survives

If you connect everything on day one, it fails in a predictable way. You have too many things to evaluate, no baseline for a good answer, and no way to tell which connection produced the wrong one.

So pick one task you already do every week. Write down what a good result looks like, and actually write it. Connect one app, review its permissions properly, and keep human approval on anything that matters.

Add the next one when the first stops needing your attention.
