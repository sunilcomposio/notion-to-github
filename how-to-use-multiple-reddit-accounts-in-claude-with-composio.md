# How to Use Multiple Reddit Accounts in Claude with Composio

Claude can help you write Reddit posts and comments, but it usually stops before it can act on your account.

If you run more than one Reddit account, you must log out and log in each time you want to post, reply, or read a thread. This takes time, and you can easily use the wrong account.

Composio keeps each Reddit connection separate, and you can give each one a short name, such as `personal`, `brand`, or `support`. When you use that name in a chat, Claude uses that account.

> You do not need to write code, make a Reddit developer app, or set up MCP yourself. You connect Composio one time, sign in to Reddit, and tell Claude which account to use.

### Read this first: Reddit rules for multiple accounts

Reddit lets you have more than one account, but you must use each account for its own purpose. Reddit does not let you use your accounts to support each other. This means no upvotes, replies, or comments from one of your accounts on posts from another of your accounts. Reddit also does not let you use a second account to get around a ban.

Each subreddit also has its own rules about self-promotion, account age, and karma. If you break these rules, Reddit or the moderators can remove your posts or ban your accounts. Read the rules of each subreddit before you post.

### What you get

- One Composio connector in Claude
- Two or more Reddit accounts behind it
- A short name for each account
- The ability to pick the correct Reddit account from the same Claude chat

For example:

> Using my brand Reddit account, write a post for r/SaaS about our new feature. Show me the final text before you post anything.

Or:

> Using support, reply to this comment in our subreddit. Show me the reply first and wait for my approval.

With Composio’s Reddit toolkit, Claude can make text or link posts in an existing subreddit and add a flair if you want one. It can also delete posts and comments that your account wrote, and it can search Reddit for content. [Composio ](https://composio.dev/toolkits/reddit/framework/vscode)also has a tool that edits your Reddit posts and comments.

### Before you start

You need:

- A Claude account
- A free Composio account
- Access to each Reddit account you want to connect
- Permission to use those Reddit accounts
- About 5 to 10 minutes

Claude supports custom remote MCP connectors on Free, Pro, Max, Team, and Enterprise plans. Claude Free lets you add one custom connector.

If you use Claude Team or Enterprise, an Owner or Primary Owner may need to turn on the connector for your organization first.

## **Step 1: Add Composio to Claude (One-time process)**

1. Click here: [Composio x Claude](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Composio&connectorUrl=https%3A%2F%2Fconnect.composio.dev%2Fmcp)
2. This will take you to the Claude Custom Connector modal

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_1.png)

3. Click Continue
4. And in the next modal, click Add

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_2.png)

5. It will take you to the authentication page. So, sign in to Composio and approve the connection.

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_3.png)

Then:

1. Return to Claude and make sure Composio is enabled.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_4.png)

You only need to do this once.

### Quick check

Start a new chat and ask:

> What Composio tools do you have available?

If Claude can see Composio, you are ready to connect LinkedIn.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_5.png)

## Step 2: Connect your first Reddit account

In Claude, type:

> Connect my Reddit account to Composio. Call it personal.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_6.png)

Claude gives you a Reddit authorization link.

1. Open the link.
2. Sign in to the Reddit account you want to use.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_7.png)

1. Read the permissions that Reddit shows you.
2. Click **Allow**.
3. Go back to Claude.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_8.png)

Claude now tells you that it connected your Reddit account.

Later, you use the name `personal` when you want Claude to pick this account.

Keep your account names short and clear. For example:

- `personal`
- `brand`
- `support`
- `client-acme`

You do not need your own Reddit developer credentials, because Composio gives you its own built-in Reddit app.

### Step 3: Connect another Reddit account

Now type:

> Connect another Reddit account. Call it brand.

Claude gives you a new authorization link.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_9.png)

Open it and sign in with your second Reddit account.

> **Important:** Reddit keeps you signed in to the last account you used. Check the username on the permission page before you click **Allow**. If Reddit shows your first account, sign out first, or open the link in a private browser window.

When you approve the connection, go back to Claude.

You now have two separate Reddit connections behind the same Composio connector.

Do the same steps for each other account you have permission to use.

### Step 4: Check your connected accounts

Ask Claude:

> List my connected Reddit accounts in Composio. Show the name of each connection and tell me if it is active.

You see each account that you added.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_10.png)

Then ask:

> Tell me which Reddit username each connection name uses.

Do this check before you let Claude post anything.

If `personal` and `brand` show the same Reddit username, remove the wrong connection and connect it again.

### Step 5: Use the correct Reddit account

When you have names for your accounts, use the name each time the account matters.

#### Write a post for one account

> Using brand, write a Reddit post for r/startups about our launch. Show me the title and text first, and do not post until I approve it.

#### Publish from a specific account

> Using brand, publish the Reddit post I just approved.

#### Reply from another account

> Using support, write a helpful reply to this comment. Show me the exact reply before you post it.

#### Read a thread

> Using personal, get the comments on this Reddit post and summarize the main questions people ask.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_11.png)

#### Search Reddit

> Using personal, search Reddit for recent posts about project management tools and list the five most active threads.

#### Edit or delete your own content

> Using brand, change the text of my last post to fix the typo in the second line. Show me the new text first.

For anything public, tell Claude to show you the final text before it posts, edits, or deletes.

### Make account selection more reliable

Add text like this to your Claude preferences or project instructions:

> My Reddit connections in Composio are: personal = my own Reddit account, brand = our company account, and support = the account our support team uses. Always use the named connection when I give one. If I ask you to post, comment, edit, or delete and I do not give an account name, ask me which Reddit account to use first.

This helps more as you add more accounts.

Composio can send each action through a named account, so Claude does not need to guess and use the last account it used.

### A note about subreddit limits

A post can fail even when your connection works. Subreddits often set limits like these:

- A minimum account age or karma
- A required post flair
- Text posts only, or link posts only
- Posts from approved users only

If a post fails, ask Claude to show you the full error. Then read the rules of that subreddit. Sometimes Reddit or AutoModerator removes a post right after you publish it. Claude cannot fix this, so contact the moderators if you think the removal is a mistake.

### Optional: Manage Reddit accounts from Composio

The chat method above is the easiest way to add accounts. Use the dashboard when you want to rename, reconnect, or remove an account.

1. Go to [dashboard.composio.dev](https://dashboard.composio.dev/).
2. Sign in with the same Composio account that you connected to Claude.
3. Open **For You** from the product switcher.
4. Find **Reddit in the Apps**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_12.png)

Click **Connect** and complete the Reddit sign-in.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_13.png)

1. Click **Connect on Reddit** again for your next account. Composio adds it as a separate connection and keeps the first one.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_14.png)

1. Give each connection a clear name. This way, you can use the same app with many accounts, and you can call each one by its name.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_15.png)

From this screen you can also:

- **Rename** a connection when its default name does not help you.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_16.png)

- **Disconnect or delete** a connection when you do not want Claude to use it. Composio deletes it immediately.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-use-multiple-reddit-accounts-in-claude-with-composio/image_17.png)

- **Re-authorize** a connection that expired. Tokens usually refresh on their own, but if a token expires or you revoke it, Composio marks the connection EXPIRED and you must connect it again.

### Troubleshooting

#### Composio does not show in Claude

Go to **Customize → Connectors** and make sure Composio is on.

If you still cannot see it, open the Composio connector menu and clear its cache. If that does not help, remove the connector and add it again.

#### The Reddit sign-in link expired

Ask Claude:

> Create a new Reddit connection link for this account.

Then use the new link and do not open the old one again.

#### Two account names use the same Reddit username

You probably approved the same Reddit account two times.

Remove the wrong connection. Then sign in to the correct Reddit account and connect it again. A private browser window makes this easier.

#### Reddit returns a 403 error

Your account may not have permission for that action. For example, the subreddit may be private or restricted, or it may have banned your account. A 403 can also mean the connection does not have the correct Reddit scopes. Check the subreddit first, and then connect the account again if you need to.

#### Reddit returns 429 errors

Reddit limits how many API requests you can send. Slow down and try again later. Do not ask Claude to run the same request again and again.

#### Claude used the wrong Reddit account

Make the request again and give the account name clearly.

For example:

> Use brand for this action. Do not use any other Reddit connection.

The account list from the section above also helps.

### What this costs

At the time of writing, the Composio Free plan gives you 100,000 tool calls each month, and you do not need a credit card.

Composio-managed apps have their own limit of up to 20,000 tool calls inside that free usage. This limit matters more for this no-code setup.

For a normal personal setup with a few Reddit accounts, this gives you plenty of room to test before you pay. Check the Composio pricing page for current numbers.

### Is this safe?

Composio becomes another service that can do the Reddit actions you allow. So read the permissions before you connect an account.

Reddit uses OAuth for sign-in. You give access through Reddit, and you never give your Reddit password to Claude or Composio. Composio saves each approval as a connected account and handles the credentials for you.

For public actions such as posts, comments, edits, or deletes, keep an approval step in your prompt. Ask Claude to prepare the action first, and let it run the action only after you approve the final result.

### FAQ

#### Can Claude use more than one Reddit account?

Yes. Composio supports many connected accounts for the same app, and you can give each one a name such as `personal` or `brand`.

#### Do I need to make a Reddit developer app?

No. Composio gives you a built-in Reddit app and handles the sign-in for you.

#### Can Claude post on Reddit?

Yes. Claude can make text and link posts, add comments, edit your posts and comments, and delete them.

#### Can I use my accounts to upvote or reply to each other?

No. Reddit does not allow this, and it can ban all the accounts involved. Use each account for its own separate purpose.

#### Does this work in Claude Web and Desktop?

Yes. Claude supports custom connectors in Claude, Cowork, and Claude Desktop.

#### Can I connect other apps through the same Composio connector?

Yes. You do not need a new MCP connector for each app. After you approve other supported apps, the same Composio connection gives Claude access to them too.

#### Connect your Reddit accounts to Claude

You get one Composio connector, separate Reddit accounts, and a clear name for each one.

After you connect them, you can tell Claude which Reddit account to use before it reads a thread or takes an action.
