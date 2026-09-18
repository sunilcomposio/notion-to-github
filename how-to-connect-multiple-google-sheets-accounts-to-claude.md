# How to Connect Multiple Google Sheets Accounts to Claude in 2 Minutes

Claude can already read a spreadsheet you paste in and tell you what's going on. But real spreadsheet work means updating live sheets, adding rows, building charts, and cleaning up data, and that only works if Claude is pointed at the right file.

That gets old fast if you're juggling a work Google account, a personal one, and maybe a shared account for a client or a side project. You end up re-authenticating or explaining context every time you switch.

Composio fixes this by letting you keep each Google account as a separate connection and give it a simple label as `work`, `personal`, `client-a`. Once labeled, Claude uses the right one just because you named it.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_1.png)

> You do not need to write code, set up a Google Cloud project, or configure MCP yourself. You connect Composio once, sign in to Google for each account, and tell Claude which one to use.

### What you'll end up with

- One Composio connector in Claude
- Two or more Google Sheets accounts connected behind it
- A simple label for each account
- The ability to read, edit, create, and chart from the correct spreadsheet account in the same chat

For example:

> Using work, pull the latest numbers from the Q3 forecast sheet and tell me what changed since last week.

Or:

> Using personal, add today's expenses as a new row in my budget tracker.

Or:

> Using client-a, check whether the shared project sheet has been updated, and summarize anything new.

Composio's Google Sheets toolkit covers creating spreadsheets and new tabs, reading and writing ranges, batch-updating rows, clearing or filtering data, and generating charts from existing data.

### Before you start

You need:

- A Claude account
- A free [Composio account](https://dashboard.composio.dev/?utm_source=composio.dev&utm_medium=content&utm_campaign=how-to-connect-multiple-gmail-accounts-to-claude-in-2-minutes)
- Access to each Google account whose sheets you want Claude to work with
- About 5 to 10 minutes

Custom connectors using remote MCP are available on Claude, Cowork, and Claude Desktop across Free, Pro, Max, Team, and Enterprise plans. Free accounts are limited to one custom connector, which is enough for this setup since Composio is the only connector you're adding because the individual Google accounts sit behind it.

If you're on a Team or Enterprise plan, an Owner or Primary Owner may need to enable the connector for the organization before you can use it.

### **Step 1: Add Composio to Claude (One-time process)**

1. Open the [Composio \<\> Claude ](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Composio&connectorUrl=https%3A%2F%2Fconnect.composio.dev%2Fmcp).
2. This opens Claude's custom connector dialog.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_2.png)

1. Click **Continue**.
2. On the next screen, click **Add**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_3.png)

1. You'll be redirected to Composio's sign-in page. Log in (or create a free account) and approve the connection.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_4.png)

Then:

1. Return to Claude and confirm Composio shows as enabled under Connectors.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_5.png)

You only need to do this once, no matter how many Google accounts you connect afterward.

#### Quick check

Start a new chat and ask:

> What Composio tools do you have available?

If Claude lists Composio's tools, you're ready to connect Google Sheets.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_6.png)

### Step 2: Connect your first Google Sheets account

In Claude, type:

> Connect my Google Sheets account to Composio. Call it work.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_7.png)

Claude should hand you back a Google sign-in link.

1. Open the link.
2. Sign in with the Google account you want to use.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_8.png)

1. Review the Sheets permissions Google is asking for.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_9.png)

1. Approve the connection.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_10.png)

1. Return to Claude.

Claude should confirm the connection is active.

The label `work` is what you'll use later to steer Claude toward this specific account.

Keep labels short and obvious:

- `work`
- `personal`
- `client-a`
- `side-project`

Composio supports multiple connected accounts for the same toolkit, each with its own alias, so this first connection won't block you from adding more.

### Step 3: Connect another Google Sheets account

Now type:

> Connect another Google Sheets account. Call it personal.

Claude will generate a fresh authorization link.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_11.png)

Open it and sign in with the second Google account.

> Important: Check which Google account is active before you approve access. If your browser automatically signs you back into the first account, sign out first or open the link in a private/incognito window, then pick the correct account from Google's account picker.

Once approved, return to Claude. You should now have two separate connections behind the same Composio connector.

Repeat this for any other Google accounts you want Claude to access.

### Step 4: Check that the right accounts are connected

Ask Claude:

> List my connected Google Sheets accounts in Composio. Show the name of each connection and whether it's active.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_1.png)

Then ask:

> Identify which Google account is behind each connection name.

Worth doing before you let Claude write to anything. If `work` and `personal` turn out to point at the same Google account, reconnect the wrong one in a private window this time, and pick the correct identity during sign-in. Renaming a connection only changes its label; it doesn't move the underlying Google account.

### Step 5: Use the right Google Sheets account

Once your accounts are labeled, mention the label whenever it matters.

#### Read data from a specific account

> Using work, get the values from the "Revenue" tab in our forecasting sheet and summarize the trend.

#### Switch to another account

> Using personal, show me this month's entries in my expense tracker.

#### Update rows

> Using client-a, mark every row where status is "done" as complete. Don't touch anything else.

#### Create a new sheet

> Using work, create a new spreadsheet called "October Standup Notes" with columns for Date, Owner, and Update.

#### Build a chart

> Using personal, build a bar chart of monthly spending by category from my budget sheet.

For anything that changes shared data, it's worth telling Claude to show you the exact change before committing it.

### Make account selection more reliable

Add something like this to your Claude preferences or project instructions:

> My Google Sheets connections in Composio are: work = my company Google account, personal = my personal Gmail, client-a = the shared client workspace. Always use the named connection when I specify one. If I ask you to edit or delete data and I haven't named an account, ask me which one to use first.

This gets more useful the more accounts you add, since it stops Claude from guessing which one you meant.

### A note about shared spreadsheet permissions

A Google account that connects successfully to Composio can still fail on a specific sheet if that sheet was never shared with it.

Google Sheets permissions are set per file, not per Google account in general. If you want Claude to edit a spreadsheet someone else owns, that person needs to share it with your connected account with edit access, if writing is the goal, before Claude can touch it.

So if Claude can read and write to your own sheets fine but hits a permissions error on a colleague's sheet, the fix is usually a sharing setting in Google Sheets, not something wrong with Composio or Claude.

### Optional: Manage Google Sheets accounts from Composio

The chat method above is the fastest way to add accounts. Use the dashboard when you want to rename, reconnect, or remove one.

1. Go to [dashboard.composio.dev](https://dashboard.composio.dev/).
2. Sign in with the same Composio account you used to connect to Claude.
3. Open **For You** from the product switcher.
4. Find **Google Sheets**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_12.png)

1. Click **Connect** and complete the Google OAuth flow, picking the account for this connection.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_13.png)

1. Click **Connect again** to add the next account. Composio keeps it as a separate connection rather than replacing the first.
2. Give each connection a clear label (`WORK`, `PERSONAL`, `client-a`).

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_14.png)

From this screen you can also:

- **Rename** a connection when a default name like `googlesheets-2` stops being useful.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_15.png)

- **Disconnect/delete** a connection you no longer want Claude touching. Deletion is immediate.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-sheets-accounts-to-claude/image_16.png)

- **Re-authorize** a connection that's expired. Tokens usually refresh automatically, but a revoked refresh token moves a connection to `EXPIRED` and requires a manual reconnect.

### Troubleshooting

#### Composio doesn't appear in Claude

Go to **Customize → Connectors** and check that Composio is enabled. If it still doesn't show up, remove the connector and add it again.

#### The Google sign-in link expired

Ask Claude:

> Create a new Google Sheets connection link for this account.

Use the fresh link rather than reopening an old one because expired links won't complete.

#### Both account labels point to the same Google account

You likely approved the same Google identity twice. Remove the incorrect connection and reconnect it while signed into the right account. A private browser window makes this easier to control.

#### Claude edited the wrong sheet

Check that it used the correct label, and that the label is actually the account with access to that specific spreadsheet.

#### A write action fails with a permissions error

Confirm the connected Google account has edit access to that exact sheet. See the note above about sharing settings being per-file.

#### Claude used the wrong Google Sheets account

Repeat the request and name the account explicitly:

> Use work for this. Do not use personal or client-a.

Adding the account map from the section above also helps prevent this going forward.

### What this costs

Composio's free plan currently includes a monthly allowance of tool calls with no credit card required, which is enough to test this workflow with a couple of accounts before deciding whether to pay. Exact limits change over time, so check Composio's current pricing page for the latest numbers before relying on them.

### Is this safe?

Google Sheets authentication goes through Google's own OAuth screen, so you're not handing your password to Claude or Composio. Each account you connect becomes its own separate authorization, so disconnecting one doesn't affect the others.

For anything that writes to a shared sheet, keep an approval step in your prompt: ask Claude to prepare the change and show it to you, then execute only after you confirm.

### FAQ

#### Can Claude use more than one Google Sheets account?

Yes. Composio supports multiple connected accounts for the same toolkit, each identified by its own alias.

#### Do I need to create a Google Cloud project or OAuth app?

Not for this setup. Composio provides managed OAuth for its Google Sheets toolkit. You'd only need your own Google Cloud credentials if you wanted to manage your own OAuth scopes directly.

#### Can Claude write to sheets, or just read them?

Both, depending on the permissions you approve during Google's consent screen. You can also scope a connection to read-only.

#### Can it build charts?

Yes. The toolkit includes actions for generating charts from existing spreadsheet data.

#### Does this work on Claude Web, Desktop, and Cowork?

Yes. Custom connectors are supported across Claude, Cowork, and Claude Desktop.

#### Can I connect Gmail, Drive, or Docs through the same Composio connector?

Yes. Composio is the connector layer, so once it's added to Claude you can authorize additional Google apps, or entirely different services, without adding another connector to Claude itself.
