# How to Connect Multiple Gmail Accounts to Claude

You connect Gmail to Claude. It operates correctly. Then you try to add a second address. The second address replaces the first address without a clear warning.

This is not caused by an incorrect setting. Claude's Google Workspace connector authorizes **one Google identity**. This identity includes Gmail, Calendar, and Drive. If you sign in with a second account, it replaces the first account. Anthropic has an open request for multi-account support. Until that support is available, the built-in connector can use only one inbox.

This can be a problem if you use more than one email address. For example, you can have a personal Gmail account and a company domain. You can also be a freelancer with a client address, or you can use `you@company.com` and `billing@company.com`.

The solution is to not let Claude store the Gmail credentials directly. Use an intermediate layer that can store multiple Gmail connections. Composio does this. It stores multiple Gmail connections, gives each connection a label, and makes all of them available to Claude through a **single** custom connector.

This guide gives the complete procedure. It shows the Claude setup, the Composio chat flow, and the Composio dashboard flow for account management.

---

## What you'll end up with

One connector in Claude. Three (or five, or two) Gmail accounts behind it, each with a name you chose. Prompts like:

> Search my work Gmail and my personal Gmail for anything with "invoice" from the last 14 days, and tell me which account each result came from.
> Draft a reply from billing@ to the last message in that thread, but don't send it.

No disconnecting. No reconnecting. No "which account is Claude looking at right now?"

---

## Before you start

**On the Claude side:**

- Any Claude plan works: Free, Pro, Max, Team or Enterprise all support custom connectors. **Free accounts get exactly one custom connector**, so if you're on Free, this has to be it.
- On **Team and Enterprise**, only an Owner can add a custom connector at the organisation level. Members then connect their own account to it individually. If you're a member and the option is greyed out, that's why.
- Works in Claude on web, desktop, and in Cowork. The setup is shared across them.

**On the Composio side:**

- A free [Composio account](https://dashboard.composio.dev). The free tier is capped at 100K tool calls a month with unlimited connections and no card required, which is far more than a personal multi-inbox setup will ever touch.

**On the Google side:**

- Access to each Gmail account you want to connect.
- If any of them is a Google Workspace account with restricted third-party app access, your Workspace admin may need to approve the connection. Worth knowing before you're mid-flow.

## Step 1 — Add Composio as a custom connector in Claude

Composio publishes one shared MCP endpoint. You're not generating a personal URL, building a server, or pasting an API key, the URL below is the same for everyone, and authentication happens over OAuth when you click Connect.

1. Open Claude and go to **Customize → Connectors**. Direct link: `https://claude.ai/customize/connectors`
2. Click the **+** button next to Connectors, then choose **Add custom connector**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_1.png)

1. Fill in the dialog:
	- **Name:** `Composio`
	- **URL:** `https://connect.composio.dev/mcp`
	- **Advanced settings:** leave empty. Do not add OAuth client IDs, secrets, or custom headers, Composio handles auth automatically.
2. Click **Add**.
3. Click **Connect** on the new Composio entry. A browser window opens for Composio's authorisation screen. Sign in (or create the free account) and approve.
4. Back in Claude, confirm Composio appears in your connector list with a **CUSTOM** badge, and that it's toggled on.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_2.png)

**Quick sanity check.** Start a new chat and ask:

> What Composio tools do you have available?

Claude should list a handful of Composio meta-tools; search, manage connections, execute, and so on. Composio doesn't dump a thousand app tools into your context; it exposes seven meta-tools that discover and run the right tool on demand. That's why one connector can cover 1,000+ apps without wrecking your context window.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_3.png)

If nothing shows up, jump to [Troubleshooting](https://claude.ai/new?incognito=#troubleshooting).

---

## Step 2 — Connect your first Gmail account

Everything from here happens in chat. In a new conversation:

> Connect my work Gmail account to Composio. Label it **work**.

Claude will call Composio's connection tool and return a **markdown authorisation link**. Click it.

You'll get Google's standard consent screen. Pick the account, review the scopes, approve.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_4.png)

Come back to Claude. It polls until the connection reports as Active, then confirms.

That label  `work`  is the whole trick. Composio calls it an **alias**, and it's how you'll route every future request to the right inbox. Aliases must be unique per toolkit, so `work` and `personal` are fine; two accounts both called `gmail` are not.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_5.png)

---

## Step 3 — Connect the second Gmail account

This is the step the built-in connector can't do. Same conversation:

> Now connect a second Gmail account. Label it **personal**.

Claude calls the same tool again with `action: add`. Composio issues a **fresh** auth link, it does not reuse or overwrite the first connection. Each `add` creates a separate connected account with its own tokens.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_6.png)

### The one gotcha that trips everyone up

When you click the second link, your browser is probably still signed into the first Google account. **Google will often skip the account chooser and silently re-authorise the same inbox.** You'll end up with two connections pointing at the same mailbox and no idea why routing is broken.

Three ways to avoid it, pick one:

- On the consent screen, click your avatar / **Use another account** and sign in with the second address.
- Open the auth link in an **incognito / private window**, then sign in fresh.
- Sign out of Google entirely first, then click the link.

After approving, Claude confirms the second connection is Active.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_7.png)

**Repeat for as many inboxes as you need.** Third account? `Connect another Gmail and label it client-acme.` Composio enforces a maximum number of accounts per toolkit; the SDK default is 5, configurable between 2 and 10, which is comfortably above what most people need. If you hit the ceiling, you'll get a clear error rather than a silent overwrite.

---

## Step 4 — Verify what's connected

Don't assume. Ask:

> List all my connected Gmail accounts in Composio, with their aliases and status.

You should get back each account's ID, its alias, and its status. Only accounts marked **ACTIVE** can execute tools.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_8.png)

If you want belt-and-braces confirmation that the aliases map to the addresses you think they do, ask Claude to fetch the profile for each one:

> For each connected Gmail account, get the profile and show me the email address next to its alias.

This catches the "both connections are actually the same inbox" mistake immediately.

---

## Step 5 — The dashboard flow (managing accounts in Composio)

The chat flow is faster for adding accounts. The dashboard is better for auditing, renaming, revoking and debugging, and it's where you'll go when something looks wrong.

Head to **`dashboard.composio.dev`** and sign in with the same account you authorised in Step 1.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_9.png)

### First: know which surface you're on

Composio's dashboard has **two surfaces**, and this confuses people constantly:

| Surface | What lives there |
| --- | --- |
| **For You** | Personal app connections for agents and MCP clients — the Claude setup you just built |
| **Platform** | Developer projects: Playground, API Keys, Toolkits, Skills, Users, Sessions, Auth Configs, Triggers, Logs |

**Resources in one surface do not appear in the other.** If you connected Gmail through Claude and can't find it in Platform, you're simply on the wrong surface. Use the **product switcher** to flip between them.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_10.png)

### The For You flow — add, rename, remove

This is the point-and-click equivalent of Steps 2 and 3, and it's the better path if you'd rather not do it conversationally.

1. Open **For You** from the switcher.
2. Find **Gmail** in the app list.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_11.png)

1. Click **Connect** and complete the Google OAuth flow.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_12.png)

1. Click **Connect on Gmail again** for the next account. Composio adds it as a separate connection rather than replacing the first, this is the behaviour the whole guide hinges on.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_13.png)

1. Give each connection a clear label. Composio's own marketing shows this pattern as `WORK` / `PERSONAL` / `TEAM` badges, and that's exactly the mental model: same app, several identities, each addressable by name.

The same incognito caveat from Step 3 applies here, check which Google account the consent screen is actually offering.

From this screen you can also:

- **Rename** a connection when `gmail-2` stops being a useful name.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_14.png)

- **Disconnect / delete** a connection you no longer want Claude touching. Deletion is immediate.
- **Re-authorise** a connection that's expired. Tokens refresh automatically, but a revoked or expired refresh token moves a connection to EXPIRED and needs a manual reconnect.

### The Platform flow — for developers

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_15.png)

If you're building on the SDK rather than just wiring up your own Claude, the Platform sidebar gives you: **Auth Configs** (the OAuth blueprint per toolkit; scopes, credentials, managed vs. your own app), **Users** and **Sessions** (correlating a user with their connections and executions), and **Logs** (request, response, error, version and Log ID for every tool call, retained 7 days on Free).

That log view is the single most useful debugging surface when a Gmail action fails and you can't tell whether it's auth, scopes, or a bad argument.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_16.png)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-claude/image_17.png)

---

## Step 6 — Actually using it

Now the payoff. Back in Claude, be explicit about which inbox you mean. Composio's executor takes an account parameter, and Claude passes the alias or account ID through with each call, so naming the account in plain English is enough.

**Cross-account search:**

> Search both my work and personal Gmail for messages from stripe.com in the last 30 days. Show the source account on every row.

**Targeted drafting:**

> Using the **billing** account, draft a reply to the most recent unread thread. Don't send it, save it as a draft.

**Morning triage:**

> Check unread mail in **work** and **client-acme** from the last 24 hours. Group by sender and flag anything that reads like it needs a reply today. Ignore my personal account.

**Cross-account reasoning:** the thing that's genuinely impossible with the built-in connector:

> Find every email about the Q3 renewal across all three connected accounts, put them in date order, and tell me who's waiting on a reply from me.

### Two tips that make routing reliable

**1. Give Claude a mapping.** Add a line to your Claude personal preferences or project instructions:

> My connected Gmail accounts are: **work** = me@company.com, **personal** = me@gmail.com, **billing** = billing@company.com. When I mention an account by name, use that one. If I don't specify and the request could touch email, ask which account before acting.

That last clause matters. When several accounts are active and you haven't named one, the default is the most recently connected account, which is rarely the one you meant.

**2. Turn off Claude's built-in Gmail connector.** If both Anthropic's Google Workspace connector and Composio's Gmail are enabled, Claude has two ways to reach email and will sometimes pick the one that only sees a single inbox. Go to **Customize → Connectors**, disconnect the Google Workspace connector, and let Composio own email. Keep it if you specifically want Drive or Calendar through Anthropic's connector — just be aware you'll get occasional mixed results on email.

---

## Troubleshooting

**Composio's tools don't show up in Claude.**

1. Check **Customize → Connectors,** Composio should be listed with a **CUSTOM** badge and toggled on.
2. Click the **⋮** next to Composio and choose **Clear cache**.
3. Still nothing: **⋮ → Disconnect**, then **Remove**, then re-add via Add custom connector.

**The auth link expired or didn't open.**<br>Composio's OAuth links are deliberately short-lived. Ask Claude to retry the connection and it generates a fresh one. Don't reuse an old link from earlier in the conversation.

**Both connections point at the same inbox.**<br>You approved the second one while still signed into the first Google account. Delete the duplicate from the dashboard, then redo Step 3 in an incognito window.

**A Gmail action fails with an auth error.**<br>Ask Claude to inspect the Gmail connection. If it reports as unhealthy or expired, disconnect and reconnect that specific account, then retry. Check the **Logs** view in Platform if you want the actual error body.

**Google Workspace says the app needs administrator approval.**<br>Your Workspace admin has restricted third-party app access. Send them the approval request, wait for it, then re-run the authorisation. Nothing on the Claude or Composio side will get around this, and nothing should.

**"Add custom connector" isn't available.**<br>You're on Free and already have one custom connector (the limit), or you're a member on a Team/Enterprise plan where only an Owner can add connectors.

**Claude used the wrong account.**<br>Name the account explicitly in the prompt, and add the account mapping to your preferences as described above.

---

## What this costs, honestly

Composio's free tier is 100K tool calls a month, unlimited connections, 50K trigger events and 3 team members, hard-capped, no card, no surprise bill. Connected accounts on Composio-managed apps are free up to 1,000. A personal setup with three inboxes will not come close to any of these limits. Pro is \$29/month if you outgrow it.

Note that the current pricing applies to signups from 15 August 2026; earlier accounts are on legacy pricing through the end of the year.

---

## Is this safe?

Reasonable question - you're putting a third party between Claude and your email.

- **No passwords.** Every connection is OAuth. Composio holds refresh tokens, not credentials, and an agent can't be prompt-injected into leaking a password that was never in the system.
- **Scoped.** You control which Gmail scopes and actions are permitted when the account connects. If you never want Claude sending mail, don't grant send.
- **Encrypted, and audited.** Tokens and configuration are encrypted at rest and in transit. Composio is SOC 2 Type 2 and ISO 27001 compliant.
- **Revocable from both ends.** Delete the connection in the Composio dashboard, and independently revoke access at `myaccount.google.com/connections`. Do both if you want it properly gone.

The honest trade: you're extending trust to one more vendor. That's a real decision, and it's the same decision you already made when you connected Gmail to anything. Make it deliberately.

---

## FAQ

**Can Claude connect to two Gmail accounts natively?**<br>No. The Google Workspace connector authorises one Google identity, and it bundles Gmail, Calendar and Drive under that single account. A second sign-in replaces the first.

**Does this work in Claude Cowork and Claude Code?**<br>Yes. Cowork uses the same Customize → Connectors flow. For Claude Code, add the same endpoint as an HTTP MCP server, or install Composio's native agent plugin via the CLI, which is the shorter path for coding agents.

**Do my inboxes get merged?**<br>No. Nothing is forwarded, imported or combined. Each Gmail account keeps its own credentials and stays separately addressable — that's precisely why account routing works.

**How many accounts can I add?**<br>More than you need. Composio enforces a per-toolkit cap that defaults to 5 and is configurable from 2 to 10.

**Does this work for shared team mailboxes?**<br>Yes, a `support@` or `billing@` address connects like any other Gmail account, provided you can complete OAuth for it.

**Can I add non-email accounts too?**<br>Yes, and this is the underrated part. The same single connector reaches 1,000+ apps; Calendar, Drive, Slack, Notion, Linear, GitHub, HubSpot. Multi-account applies to those as well. Two Slack workspaces, two GitHub accounts, same pattern.

**Will Anthropic ship native multi-account support?**<br>It's one of the most-requested connector features and there's an open issue tracking it. No ship date has been announced. If it lands, you can move back, but your Composio connections would come with you to any other agent anyway, which is arguably the better position to be in.

---

## The short version

Claude's Gmail connector holds one account. Composio holds as many as you want and hands all of them to Claude through one custom connector.

Add `https://connect.composio.dev/mcp` under **Customize → Connectors**. Ask Claude to connect each Gmail account with a label. Use an incognito window for the second one. Then name the account in your prompts.

6 minutes, and the disconnect-reconnect its done.
