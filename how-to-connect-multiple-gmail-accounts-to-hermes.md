# How to Connect Multiple Gmail Accounts to Hermes in 2 Minutes

Hermes is an agent that lives on your machine or server, remembers what it learns, and gets more useful the longer you use it. But an inbox-aware agent only helps if it's pointed at the right inbox.

That gets old fast if you're juggling a work Gmail, a personal one, and maybe a shared account for a client or a side project. You end up re-authenticating or re-explaining which inbox you mean every time you switch.

Composio fixes this by letting you keep each Gmail account as a separate connection and give it a simple label like `work`, `personal`, or `client-comp`. Once labeled, Hermes uses the right one just because you named it.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_1.png)

> You do not need to write code, set up a Google Cloud project, or build an MCP server yourself. You add Composio to Hermes once, sign in to Google for each account, and tell Hermes which one to use.

### What you'll end up with

- One Composio connection inside Hermes
- Two or more Gmail accounts connected behind it
- A simple label for each account
- The ability to read, search, draft, and send from the correct inbox in the same chat

For example:

> Using work, find the last email from Priya about the Q3 budget and summarize it.

Or:

> Using personal, search for my flight confirmation from last week.

Or:

> Using client-comp, draft a reply to the latest thread asking for the project timeline. Show it to me before sending.

Composio's Gmail toolkit covers fetching and searching emails, reading threads, creating drafts, sending messages, replying, and managing labels.

### Before you start

You need:

- Hermes installed and running (`hermes` opens a chat session)
- A free Composio account
- Access to each Google account whose inbox you want Hermes to work with
- About 5 to 10 minutes

Composio connects to Hermes through MCP and signs you in with OAuth, so there are no API keys or config files to handle. Since the individual Gmail accounts sit behind Composio, it's the only connection you need to add.

### Step 1: Add Composio to Hermes (one-time process)

1. Open Hermes and start a new chat.
2. Paste this link into the chat and send it:

```plain text
https://composio.dev/hermes
```

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_2.png)

1. Hermes reads the page and replies asking whether you want it to set things up. It won't change any settings until you say so. Reply with:

> Yes, set it up.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_3.png)

1. Hermes will give you a Composio sign-in link. Open it, log in (or create a free account), and approve the connection.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_4.png)

1. Return to Hermes and confirm it says Composio is connected. If Hermes asks you to reload, type:

```plain text
/reload-mcp
```

You only need to do this once, no matter how many Gmail accounts you connect afterward.

#### Quick check

Start a new chat and ask:

> What Composio tools do you have available?

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_5.png)

If Hermes lists Composio's tools, you're ready to connect Gmail.

### Step 2: Connect your first Gmail account

In Hermes, type:

> Connect my Gmail account to Composio. Call it work.

Hermes should hand you back a Google sign-in link.

1. Open the link.
2. Sign in with the Google account you want to use.
3. Review the Gmail permissions Google is asking for.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_6.png)

1. Approve the connection.
2. Return to Hermes.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_7.png)

Hermes should confirm the connection is active.

The label `work` is what you'll use later to steer Hermes toward this specific inbox.

Keep labels short and obvious:

- `work`
- `personal`
- `client-comp`
- `side-project`

Composio supports multiple connected accounts for the same toolkit, each with its own alias, so this first connection won't block you from adding more.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_8.png)

### Step 3: Connect another Gmail account

Now type:

> Connect another Gmail account. Call it personal.

Hermes will generate a fresh authorization link.

Open it and sign in with the second Google account.

> Important: Check which Google account is active before you approve access. If your browser automatically signs you back into the first account, sign out first or open the link in a private/incognito window, then pick the correct account from Google's account picker.

Once approved, return to Hermes. You should now have two separate connections behind the same Composio server.

Repeat this for any other Gmail accounts you want Hermes to access.

### Step 4: Check that the right accounts are connected

Ask Hermes:

> List my connected Gmail accounts in Composio. Show the name of each connection and whether it's active.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_9.png)

Then ask:

> Identify which email address is behind each connection name.

Worth doing before you let Hermes send anything. If `work` and `personal` turn out to point at the same Google account, reconnect the wrong one in a private window and pick the correct identity during sign-in. Renaming a connection only changes its label; it doesn't move the underlying Google account.

### Step 5: Use the right Gmail account

Once your accounts are labeled, mention the label whenever it matters.

#### Read and summarize

> Using work, summarize the unread emails from the last 24 hours and flag anything that needs a reply today.

#### Switch to another account

> Using personal, find the receipt from the Amazon order last Tuesday.

#### Search

> Using client-comp, find every thread from the last two weeks that mentions "invoice".

#### Draft a reply

> Using work, draft a reply to the latest email from my manager. Keep it short and don't send it yet.

#### Send an email

> Using personal, send a note to mom saying I'll call her on Sunday. Show me the draft first.

For anything that sends or deletes, it's worth telling Hermes to show you the exact message before committing.

### Make account selection more reliable (Hermes remembers)

Hermes keeps persistent memory, so you can teach it your account map once instead of repeating it. Tell it something like:

> Remember this: my Gmail connections in Composio are work = my company Google account, personal = my personal Gmail, client-comp = the shared client inbox. Always use the named connection when I specify one. If I ask you to send, reply, or delete and haven't named an account, ask me which one to use first.

This gets more useful the more accounts you add, since it stops Hermes from guessing which inbox you meant.

### A note about Gmail permissions

Each connection only has the access you approved on Google's consent screen. If you only want Hermes to read and draft, don't grant send access. Review the permissions on each account before approving, and keep a stricter setup for accounts where a stray email would be costly (work and client accounts, for example).

If a send action fails with a permissions error, the connection was probably authorized without send access. Reconnect it and approve the full set of permissions you need.

### Optional: Manage Gmail accounts from Composio

The chat method above is the fastest way to add accounts. Use the dashboard when you want to rename, reconnect, or remove one.

1. Go to [dashboard.composio.dev](https://dashboard.composio.dev/).
2. Sign in with the same Composio account you used for Hermes.
3. Find **Gmail** in your connections.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_10.png)

1. Click **Connect** and complete the Google OAuth flow, picking the account for this connection.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_11.png)

1. Click **Connect again** to add the next account. Composio keeps it as a separate connection rather than replacing the first.
2. Give each connection a clear label (`work`, `personal`, `client-comp`).

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_12.png)

From this screen you can also:

- **Rename** a connection when a default name like `gmail-2` stops being useful.
- **Disconnect/delete** a connection you no longer want Hermes touching.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-gmail-accounts-to-hermes/image_13.png)

- **Re-authorize** a connection that's expired. Tokens usually refresh automatically, but a revoked refresh token marks the connection as `EXPIRED` and requires a manual reconnect.

### Troubleshooting

#### Hermes doesn't show Composio's tools

Run `/reload-mcp` in your chat, or restart Hermes. If it still doesn't show up, paste `https://composio.dev/hermes` into the chat again and reply "Yes, set it up" to repeat Step 1.

The desktop app doesn't list `/reload-mcp` in its slash menu, so if you see that message, just open a new chat.

#### The Google sign-in link expired

Ask Hermes:

> Create a new Gmail connection link for this account.

Use the fresh link rather than reopening an old one, because expired links won't complete.

#### Both account labels point to the same Google account

You likely approved the same Google identity twice. Remove the incorrect connection and reconnect it while signed into the right account. A private browser window makes this easier to control.

#### Hermes used the wrong Gmail account

Repeat the request and name the account explicitly:

> Use work for this. Do not use personal or client-comp.

Saving the account map to memory (see above) also helps prevent this going forward.

#### Hermes can't see a tool you expect

If you've set tool filters in your Hermes MCP config, check that they aren't excluding the Gmail tools.

### What this costs

Hermes is open source. Composio's free plan currently includes a monthly allowance of tool calls with no credit card required, which is enough to test this workflow with a couple of accounts before deciding whether to pay. Exact limits change over time, so check Composio's current pricing page for the latest numbers before relying on them.

### Is this safe?

Gmail authentication goes through Google's own OAuth screen, so you're not handing your password to Hermes or Composio. Each account you connect becomes its own separate authorization, so disconnecting one doesn't affect the others.

For anything that sends email, keep an approval step in your prompt: ask Hermes to prepare the message and show it to you, then send only after you confirm.

### FAQ

#### Can Hermes use more than one Gmail account?

Yes. Composio supports multiple connected accounts for the same toolkit, each identified by its own alias.

#### Do I need to create a Google Cloud project or OAuth app?

Not for this setup. Composio provides managed OAuth for its Gmail toolkit. You'd only need your own Google Cloud credentials if you wanted to manage your own OAuth scopes directly.

#### Can Hermes send emails, or just read them?

Both, depending on the permissions you approve during Google's consent screen.

#### Does this work with Google Workspace accounts?

Yes, though some Workspace admins restrict third-party app access. If sign-in is blocked, ask your admin to allow the app.

#### Can I connect Calendar, Drive, Slack, or Notion through the same Composio server?

Yes. Composio is the connector layer, so once it's added to Hermes you can authorize additional Google apps, or entirely different services, without adding another MCP server to your config.
