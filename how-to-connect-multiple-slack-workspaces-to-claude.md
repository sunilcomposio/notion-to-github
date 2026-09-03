# How to Connect Multiple Slack Workspaces to Claude

You connect Slack to Claude. It works. Then you go to add your client's workspace and discover there's nowhere to put it.

Claude's built-in Slack connector holds **one workspace per Claude account**, and there is no path - UI, config, or otherwise, to a second one.

Anyone consulting, contracting, advising, or running an agency is already aware of this issue. The productivity unlock from having multiple Slack workspaces in Claude is very much understated.

This blog post explains how to add multiple Slack Workspaces to Claude via Composio.

## The limit, precisely

There's an [open feature request on](https://github.com/anthropics/claude-code/issues/44243)<span discussion-urls="discussion://3cef261a-6dfe-80f3-8d8b-f91f0f9b26f7/3cef261a-6dfe-8086-a207-ea2470d9afa6/3cff261a-6dfe-80ee-8e94-001c08774763">[ Anthropic's tracker](https://github.com/anthropics/claude-code/issues/44243)</span> that states it directly: the built-in Slack MCP connector supports only one workspace per Claude account, with no way to add a second through the UI or configuration. The example the issue itself gives is a consultant with their own company workspace plus a client workspace, and a founder who advises other companies.

The same issue spells out what reaching workspace #2 costs today. Connecting the first is one click. Connecting the second means standing up a self-hosted MCP server from scratch, downloading a third-party binary, manually creating a Slack app at api.slack.com, configuring seven OAuth scopes, writing MCP JSON config with the correct CLI flags, and restarting your client. For one extra workspace.

The obvious workaround, adding the same connector twice under two names, one per workspace is blocked. Claude refuses a server URL that's already registered.

It's an open issue, not a fixed one. No ship date announced.

---

## What you'll build

One custom connector in Claude. Behind it, every Slack workspace you work in, each labelled with the client's name. Then prompts like:

> In **comp-slack(CLIENT)**, find the last three messages in #design-review mentioning the nav redesign. Don't post anything. Give me the channel and timestamp for each.
> I have a call with Northwind in 20 minutes. From **northwind-slack** only, tell me what's changed since Thursday and what they're waiting on from us.

The tool that makes this work is Composio, it holds many Slack connections at once, labels each, and exposes all of them to Claude through a single connector.

---

## Before you start

**Claude:**

- Custom connectors work on Free, Pro, Max, Team and Enterprise. **Free is limited to one custom connector**, so this would have to be it.
- On **Team/Enterprise**, only an Owner can add the connector at organisation level. Members then connect their own account to it individually.

**Composio:**

- A free [Composio account](https://dashboard.composio.dev). 100K tool calls a month, unlimited connections, no card.
- **Use an agency-owned account, not a personal one.** If the person who set this up leaves, the client connections shouldn't leave with them.

**Client side — the slow part:**

- You need permission to install apps in each client workspace, or an admin who'll approve the request. Plenty of client workspaces restrict this. Ask before you're mid-flow, not after.

**Time:** about 6 minutes for the connector, then 1–2 minutes per workspace, plus however long approvals take.

---

## Step 1 — Add Composio as a custom connector

Composio publishes one shared MCP endpoint. Same URL for everyone, no API key, OAuth on click.

1. In Claude, open **Customize → Connectors** (`https://claude.ai/customize/connectors`).
2. Click the **+** button, then **Add custom connector**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_1.png)

1. Fill in:
	- **Name:** `Composio`
	- **URL:** `https://connect.composio.dev/mcp`
	- **Advanced settings:** leave blank. No client ID, no secret, no headers; Composio handles auth automatically.
2. Click **Add**, then **Connect**. A browser window opens; sign in to Composio (or create the free account) and approve.
3. Back in Claude, confirm Composio appears with a **CUSTOM** badge and is toggled on.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_2.png)

**Sanity check.** In a new chat: *"What Composio tools do you have available?"* You should get a short list of meta-tools. Composio doesn't dump every app's tools into your context, it exposes a handful that discover and run the right tool on demand, which is how one connector fronts 1,000+ apps without wrecking your context window.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_3.png)

---

## Step 2 — Pick your Slack toolkit (do this before connecting)

Composio ships **two** Slack toolkits, and choosing wrong is the most common setup mistake here.

| Toolkit | What it gives you | Use it for |
| --- | --- | --- |
| **`slack`** | The standard integration. `scopes` refers to **bot-user scopes**; acting as the actual Slack user requires user-token permissions configured separately via `user_scopes`. | Posting, replying, channel operations, most write actions |
| **`slackbot`** | Workspace-wide **read** access on behalf of the installing user — search messages and files, browse channel and thread history, look up users and resolve them by email. **Includes direct messages.** Still posts and reacts as the bot. | Broad search and context-gathering across a whole workspace |

For agency reporting: *"what did the client actually say about X"* , `slackbot` is usually what you want, because standard bot scopes only see channels the app was explicitly invited to.

**But read that DM line again.** `slackbot` reaching direct messages in a *client's* workspace is a materially different thing from reaching them in your own. Either use the narrower `slack` toolkit for client workspaces, or get explicit sign-off from the client. Don't let this one slide because it was convenient, it's exactly the detail that turns up in a security review later.

---

## Step 3 — Connect your first workspace

Everything from here happens in chat. New conversation:

> Connect my agency's Slack workspace to Composio. Label it **internal-slack**.

Claude returns an authorisation link as a clickable markdown link. Click it. Slack's OAuth screen appears, pick the workspace, review the requested permissions, approve.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_4.png)

Come back to Claude. It polls until the connection reports Active, then confirms.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_5.png)

That label is the whole mechanism. Composio calls it an **alias**, and it's how every future request gets routed to the right workspace. Aliases must be unique per toolkit.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_6.png)

---

## Step 4 — Connect the second, third, fourth workspace

Same conversation:

> Now connect another Slack workspace. Label it **comp-slack**.

Composio issues a **fresh** authorisation link and creates a separate connection. It does not overwrite the first, that's the entire point of the setup.

To verify it, enter the same query in the chat, it shows the connections are established.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_7.png)

### The gotcha that will get you

Slack's authorisation screen has a **workspace dropdown in the top right**. If you're signed into several Slack workspaces in that browser, it defaults to one of them and it is genuinely easy to approve the wrong workspace and not notice for a week.

Check the dropdown every single time. Three ways to make this reliable:

- Click the dropdown and explicitly select the intended workspace before approving.
- Use separate browser profiles per client and open each auth link in the matching profile.
- Open the link in an **incognito window** and sign in deliberately.

Repeat per client. Composio enforces a cap on accounts per toolkit, the SDK default is 5, configurable between 2 and 10, so an agency running more than five active Slack clients should raise it or split across Composio projects.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_8.png)

---

## Step 5 — Verify before you trust it

Don't assume. Ask:

> List all my connected Slack accounts in Composio with their aliases and status.

Only connections marked **ACTIVE** can execute tools.

Then confirm each alias actually points where you think:

> For each connected Slack account, tell me the workspace name it's authorised against, next to its alias.

Do this properly. Two aliases silently pointing at the same workspace is the failure mode that produces a confident, detailed, completely wrong answer about the wrong client.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_9.png)

---

## Step 6 — The Composio dashboard flow

Chat is faster for connecting. The dashboard is where you audit, rename, revoke and debug, and for agency work, the audit trail is the point.

Go to **`dashboard.composio.dev`** and sign in with the account you authorised in Step 1.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_10.png)

### First: know which surface you're on

Composio's dashboard splits in two, and this confuses everyone exactly once:

| Surface | What lives there |
| --- | --- |
| **For You** | Personal app connections for agents and MCP clients — the setup you just built |
| **Platform** | Developer projects: Playground, API Keys, Toolkits, Skills, Users, Sessions, Auth Configs, Triggers, Logs |

**Resources in one surface do not appear in the other.** If your Slack connections aren't where you expect, you're on the wrong surface. Use the **product switcher**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_11.png)

### For You — the click-through path

The point-and-click equivalent of Steps 3 and 4, and the better route when you're onboarding several clients in one sitting:

1. Open **For You** from the switcher.
2. Find **Slack** in the app list.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_12.png)

1. Click **Connect** and complete Slack's OAuth flow.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_13.png)

1. Click **Connect on Slack again(Connect New)** for the next workspace, it adds a separate connection rather than replacing the first.
2. Label each connection with the client name.

The same workspace-dropdown caveat from Step 4 applies here.

From this screen you can also **rename** a connection when `slack-2` stops being a useful name, **disconnect or delete** one (immediate), and **re-authorise** an expired one. Tokens refresh automatically, but a revoked or expired refresh token moves a connection to EXPIRED and needs a manual reconnect.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_14.png)

### Platform — for the audit trail

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_15.png)

On the developer surface, **Logs** gives you the request, response, error, version and Log ID for every tool call, retained 7 days on Free, 30 on Pro. **Auth Configs** holds the OAuth blueprint per toolkit, including custom scopes if you bring your own Slack app.

For an agency, that log view is your answer to *"did your AI touch our Slack last Tuesday?"* Know where it is before someone asks.

---

## Step 7 — Naming, and how not to leak client data

This matters more than the setup, and it's the part most guides skip entirely.

Once Claude can reach five client workspaces at once, the failure mode changes. It stops being *"Claude can't see the thing"* and becomes *"Claude saw the wrong client's thing, and posted about it."*

### Use a rigid alias convention

Pick one and never deviate:

```plain text
<client>-slack
```

`comp-slack`, `northwind-slack`, `internal-slack`.

Rigid matters because ambiguous aliases (`slack2`, `client-slack`) are how Claude ends up guessing. Guessing is precisely what you're eliminating.

### Give Claude the map and the rules

Put this in your Claude personal preferences, or better, in a **project** per client:

> **Connected Slack workspaces:**`internal-slack` = our agency<br>`comp-slack` = Comp Corp (client)<br>`northwind-slack` = Northwind (client)

**Rules:**

	1. Never post, reply, react, or send in any workspace whose alias starts with a client name. Read only, draft for me instead.
	2. Always state the source workspace and channel for every fact you report.
	3. Never move information between two different client workspaces. Client → internal is fine. Client → other client is never fine.
	4. If I don't name a workspace and the request could touch more than one, ask before acting.

Rule 3 is the one that protects you commercially. Rule 4 protects you from the default behaviour: when several accounts are connected and none is named, the most recently connected one gets used, rarely the one you meant.

### One Claude project per client

Aliases are the mechanism; projects are the habit. A project per client, with that client's rules in the instructions, means every chat already points at the right workspace by default and whoever joins the account team inherits the guardrails instead of learning them the hard way.

### Turn off Claude's built-in Slack connector

If Anthropic's Slack connector and Composio's Slack are both live, Claude has two routes to Slack and will sometimes take the one that only sees a single workspace, quietly answering about the wrong client. Go to **Customize → Connectors** and disconnect the built-in Slack connector. Let Composio own it.

---

## Prompts that earn the setup

**Monday sweep across clients:**

> For each of `comp-slack` and `northwind-slack`, list unresolved questions directed at our team in the last 7 days. Group by client, include channel and timestamp. Read only.

**Finding the decision nobody wrote down:**

> Search `comp-slack` for the thread where we agreed the launch date. Give me the message, who said it, and the timestamp. Don't reply.

**Escalation check:**

> Across all connected client workspaces, find anything from the last 48 hours that reads like it needs a same-day response. Name the workspace on every item.

**Drafting without posting:**

> Draft a reply to the last message in `#design-review` in `comp-slack`. Show it here. Do not post it.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_16.png)

Claude first checks whether the channel exists, then posts the message with the mention. You can confirm it landed by opening the channel in Slack.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_17.png)

---

## Troubleshooting

**Composio's tools don't show up in Claude.**

1. Check **Customize → Connectors,** Composio should show a **CUSTOM** badge and be toggled on.
2. Click the **⋮** next to Composio and choose **Clear cache**.
3. Still nothing: **⋮ → Disconnect**, then **Remove**, then re-add.

**The auth link expired or didn't open.**<br>Composio's OAuth links are deliberately short-lived. Ask Claude to retry the connection and it generates a fresh one. Don't reuse an old link from earlier in the thread.

**Two aliases point at the same workspace.**<br>You missed the workspace dropdown on Slack's OAuth screen. Delete the duplicate in the dashboard and redo it in an incognito window.

**Search returns nothing from channels you can clearly read.**<br>Standard bot scopes only cover channels the app was added to. Either invite the app to the channel, or use the `slackbot` toolkit for workspace-wide read.

**Slack's AI-powered search fails.**<br>Slack's `assistant.search.context` requires the OAuth app to have the **Agents & AI Apps** feature enabled *and* the workspace to be on **Business+ or higher**. A client on a lower Slack plan can't use it, and nothing you configure changes that.

**Channel admin actions fail.**`admin.conversations:write` is an enterprise-level Slack scope. Tools like deleting conversations need the workspace on an **Enterprise** plan. Check the client's Slack plan first.

**A Slack action fails with an auth error.**<br>Ask Claude to inspect that Slack connection. If it's unhealthy or expired, disconnect and reconnect that specific workspace, then retry. Check **Logs** in Platform for the actual error body.

**Claude used the wrong client's workspace.**<br>Name the alias explicitly, and get the rules block into your project instructions. This should be a one-time failure, not a recurring one.

**"Add custom connector" isn't available.**<br>Free plan with one custom connector already used, or you're a member on Team/Enterprise where only an Owner can add connectors.

---

## Offboarding a client

1. **Delete the connection in Composio.** Dashboard → For You → find `comp-slack` → disconnect. Deletion is immediate.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-slack-workspaces-to-claude/image_18.png)

1. **Revoke on the client's side too.** Their workspace admin removes the app under Slack's app management. Do both, deleting your side doesn't guarantee the grant is gone from theirs.
2. **Remove the client's block** from your Claude project instructions so a stale alias can't be referenced later.
3. **Confirm in writing.** *"Our AI assistant's access to your Slack was revoked on \[date\]"* is a five-line email that makes your next security review much shorter.

Being able to do this cleanly, per client, in two minutes is a real argument for this architecture over the disconnect-and-reconnect approach, where "revoked" just means "not currently selected."

---

## Costs and honest limits

**Composio Free:** 100K tool calls a month, unlimited connections, 50K trigger events, 3 team members. Hard-capped, no card, no surprise bill. A five-client agency doing daily sweeps won't come close. Pro is \$29/month with included usage credit if you outgrow it.

**What this doesn't fix:**

- **Nothing runs unattended.** Claude connectors work inside a conversation you start. There's no "alert us when the client posts in #urgent." That's a different class of tool.
- **Account caps.** Default 5 per toolkit, configurable 2–10. Past that, split across Composio projects.
- **Client plan limits are still client plan limits.** Business+ and Enterprise Slack features stay unavailable on a client's lower-tier workspace.
- **No privilege escalation.** Claude reaches what the connected identity reaches, no more.
- **You're adding a vendor.** One more party in the chain to your clients' systems. Composio is SOC 2 Type 2 and ISO 27001 compliant, tokens are OAuth and encrypted at rest and in transit, and nobody holds a client password. Still worth stating plainly to clients upfront rather than during their security review.

---

## FAQ

**Can Claude connect to two Slack workspaces natively?**<br>No. One workspace per Claude account, with no UI or configuration path to a second, per Anthropic's own open issue.

**Can I add the built-in connector twice under different names?**<br>No. Claude rejects a server URL that's already registered.

**Does this work in Cowork and Claude Code?**<br>Yes. Cowork uses the same Customize → Connectors flow. Claude Code takes the same endpoint as an HTTP MCP server, or Composio's native agent plugin via the CLI, which is the shorter path for coding agents.

**Do my workspaces get merged?**<br>No. Each connection keeps its own token and stays separately addressable. That's exactly why routing by alias works.

**How many workspaces can I connect?**<br>Default cap is 5 per toolkit, configurable from 2 to 10.

**Can each person on my team have their own connections?**<br>Yes. Composio scopes connections per user, so two people never touch each other's credentials. Shared connections also exist as a feature with per-user access control if you'd rather have one agency-held connection per client.

**Will clients see we've connected something?**<br>Yes, and they should. Installed apps are visible to Slack workspace admins. Tell them first.

**What about Notion, Linear, Google Drive?**<br>Same connector, same pattern, same multi-account behaviour. Most agency setups end up with three or four apps per client behind the one connector.

---

## The short version

Claude's Slack connector holds one workspace per account, and it's an open, unfixed limitation with consultants and agencies named in the request.

Add `https://connect.composio.dev/mcp` under **Customize → Connectors**. Connect each workspace with a `client-slack` alias. Watch the workspace dropdown on Slack's OAuth screen every time,  that's where setups quietly go wrong. Then write the guardrail block into a project per client before you run anything real.

Setup is fifteen minutes. The guardrails are the part worth getting right.
