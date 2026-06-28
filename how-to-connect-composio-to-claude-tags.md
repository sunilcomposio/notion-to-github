# How to Connect Composio to Claude (Tags)

A yet another new product from Anthropic. This time it aims to replace your Slack Coworkers. Yes, you don’t anymore have to deal with their annoying requests.

The Claude Tag is essentially a Slack Bot wrapper for Claude Code CLI. Whatever you could do with Claude Code or Cowork now you can do it with Claude Tag. The core idea is to integrate Claude where your  work actually happens.

Claude Tags can be added to your Slack channels as a coworker and it can access any tools your org uses and even your codebases. Anyone in the channel can invoke it by tagging @Claude in and it works in the background, finishes the tasks, and outputs the responses. Claude builds context by remembering relevant information from the channels it’s in, and can plan out tasks to complete in the future.

## Pricing and availability

The Claude Tags is available to only the Claude Teams and Enterprise plans, which means you cannot use it with your Pro and Max subscriptions.

And the uses are billed separately. You have to top-up Claude Tags with credits to use it. The credits follow API pricing and it can be expensive as you scale uses. Something to keep in mind before inducting Claude Tag to your Slack workspace.

## How does Claude Tags work?

Claude Tags is built to keep enterprises in mind. The Admin can limit which channels can access it, and what tools, plugins, skills Claude is allowed to use in that channel. The admin can provision repositories, tools, and skills for Claude.

For example, the Claude tags configured in #growth channel can only access specific repositories and tools such as GSC, Posthog, Google Ads, etc. While the #tech channel can access their own set of repositories and tools. Each channel essentially will have Claude with it’s own context and persona.

No individual can connect their tools with Claude in a shared channel.

Anyone in the channel can tag Claude, which means Claude will overwrite your work if someone else asks it to redo or "clean up" something you set up — it acts on whoever tagged it last, with no notion of ownership. For anything under version control this isn't really an issue; you can always revert. But for irreversible actions — sending a message, updating records in an app, kicking off a workflow — conflicting requests can collide with no undo.

It has an **ambient mode** (can be enabled by admin) which let’s Claude operate in the background, it fills in missing context in the conversation, follows up on threads that have gone quiet. Best kept to signal channels like incidents or CI; off where people think out loud, since proactive becomes noise fast.

**It has Routines** that run on the clock or on events.

For more on How Claude Tags work, check Anthropic’s [official documentations](https://claude.com/docs/claude-tag/concepts/how-it-works).

## How to give Claude Tag all your company contexts?

Claude Tag is as useful as the context you provide. It can do a lot of things, from getting records from [Salesforce](https://composio.dev/toolkits/salesforce), getting product analytics from [Posthog](https://composio.dev/toolkits/posthog) or [Mixpanel](https://composio.dev/toolkits/mixpanel), to resolving [Linear](https://composio.dev/toolkits/linear) tickets right from Slack.

But here’s the bummer. Anthropic has a limited set of connectors officially available in Claude Tag (Vercel, Datadog, Stripe, Sentry, Gitlab, Notion, Linear, etc) and on top of it you’ve to individually add all the services and the audit logs doesn’t preserve the API calls to the endpoints of these services. So, the entire execution is sort of a blackbox.

But, if you’re using Claude Tag for destructive actions, you need to keep a track of what it does.

This is where Composio comes into the picture.

- Composio MCP provides a gateway to over 1000 managed integrations, CRMs, ticketing services, analytics, and a lot more.
- You can track all the API calls made by Claude in the Composio dashboard.
- Composio manages OAuth handling so you can just plug the applications and get started in minutes.

So, let’s see how you can add Composio or any custom MCP for tha matter to Claude Tag.

## How to connect Composio or any custom plugin to Claude Tags?

To re-iterate:

- You need to be the Slack Admin
- And you need to have a Claude Teams or Enterprise plan and also admin access
- Composio API Key. You can create one for free in the [dashboard](https://dashboard.composio.dev/).

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_1.png)

### Step 1: Add Claude to your Slack Workspace

Go to the [Slack marketplace](https://sunilkumardas-kzx1836.slack.com/marketplace/A08SF47R6P4-claude) and install the Claude app.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_2.png)

### Step 2: Configure your Slack workspace inside Claude Tag Admin

1. Go to [Claude Tag admin panel](https://claude.ai/admin-settings/claude-tag) to configure your Slack workspace with Claude

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_3.png)

2. Click on Connect and and type `@claude connect `in any channel. And it’ll return a code paste that in the pop-up.

   ![It’s the wrong code btw](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_4.png)

3. If you want to keep Claude Tag to limited to few channels, clcik on the drop down shown in the picture above and add channel IDs.
4. Leave the rest of the steps as it it. We will be back here soon.

### Step 3: Adding Composio MCP (or any custom connectors)

1. In the next step, go to [Access bundles](https://claude.ai/admin-settings/claude-tag/access-bundles) and click on **Create**

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_5.png)

Claude has a default set of apps to choose from. You have connectors for apps such as Notion, Gmail, Gong, Hubspot, Linear, etc.

Each has their own authentication setup (OAuth, API key), that you have to individually.

> Note: You should create separate user accounts for Claude, instead of authenticating from your personal user accounts and provision access according to the requirements.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_6.png)

1. To add a custom connector like Composio MCP, click on **+ Connect Another app**.
2. Fill in the details, make sure the authentication methods are correct. 

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_7.png)

3. For Composio replace the Add the following in **Custom headers **section
	- Allowed websites: connect.composio.dev
	- Name: x-consumer-api-key
	- Prefix: empty
	- value: ck_\*\*\*\*\*\*\*\*\* (Composio API Key you just created)

### Step 4: Adding a custom Composio plugin to Claude

Now this step is about adding a custom plugin with MCP and you need to add a plugin repository to your GitHub org. If you haven’t then add your [GitHub org to Claude](https://claude.ai/admin-settings/claude-code/github) before proceeding

Now clone this [Composio plugin repo ](https://github.com/composio-community/composio-plugin)and make sure it’s in your GitHub org. If you already have a custom plugin just add the below MCP server in `.mcp.json` this will point Claude to Composio MCP.

```shell
{
  "mcpServers": {
    "composio": {
      "type": "http",
      "url": "https://connect.composio.dev/mcp"
    }
  }
}
```

#### Register the repo as a plugin marketplace

- Go to [https://claude.ai/admin-settings/plugins](https://claude.ai/admin-settings/plugins)
- Click on Add plugins

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_8.png)

- Click on **Sync from GitHub**. It will show all the plugin repositories that Claude can sync from.

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_9.png)

- Select the repository and set the Default Access to `Available to Install `and sync automatically.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_10.png)

#### Attach the plugin to the Access Bundle

Now, in the [Access Bundle section](https://claude.ai/admin-settings/claude-tag/access-bundles), toggle the plugin you just added.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_11.png)

### Step 5: Verify it works

Go back to you Slack channel and send Claude a message.

```shell
@Claude Can you access Composio tools?
```

And you should get an affirmative response.

### Step 6: Configuring your apps in Composio MCP

As the agent is headless, you need to configure application from the dashboard.

So, go to [Composio Dashboard](https://dashboard.composio.dev/), toggle to **For You** on the top left corner.

Configure which ever apps you want Claude to access from a catalogue of 1000 tools.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_12.png)

It’s important to keep in mind, you are giving Claude access to all the apps via Composio. So, it’s ideal to use a separate account for Claude, which has access to specific apps a channel needs.

For example, the Access Bundle for #growth should have a separate Composio ID then the #tech. Each will have their respective access to tools.

To connect Gmail, click on connect and authenticate the Gmail account Claude needs access to.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_13.png)

You can now test the integration with Claude tags.

This is an example of me giving Claude access to our search console

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-composio-to-claude-tags/image_14.png)

## End Note
