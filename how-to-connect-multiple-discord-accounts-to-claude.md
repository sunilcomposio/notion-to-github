# How to Connect Multiple Discord Accounts to Claude

You open Claude, connect your Discord account, fetch a few messages, and everything works fine. The problem starts when the data sits in another Discord account.

Claude doesn’t let you connect to and switch between multiple Discord accounts, so you have to disconnect, reauthenticate, and repeat the process.

That gets painful fast when you need data from several Discord accounts, which is common in enterprise workflows.

So if you have been in a similar situation, here is a cleaner way to handle it. Composio lets you connect as many as 10 of your Discord accounts to Claude, and I’ll explain how to do it in this guide.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_1.png)

---

## How to Connect to Multiple Discord Accounts

Create an account at[ Composio](https://dashboard.composio.dev/) using Google login, set up 2FA, and then ensure you are in the default view

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_2.png)

Head to the mode selector and switch to the **FOR YOU** view, then select I**nstall **next to Claude + Claude Cowork.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_3.png)

A new page will open.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_4.png)

Click on **Add Composio connector in Claude, **and it opens a Claude pop-up

Or just click here: [Composio \<\> Claude](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Composio&connectorUrl=https%3A%2F%2Fconnect.composio.dev%2Fmcp)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_5.png)

Click Continue, and it will ask for your preferred authentication method and OAuth registration for the tools you will use.

> I keep it at the default, and it's preferred as well.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_6.png)

Click **Add, then** grant permissions by pressing Allow access, **the**n sign in to your composio account.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_7.png)

Once done, head to a chat window and ask Claude to configure multiple Discord accounts. For this, paste the given prompt:

```plain text
Use Composio to connect my work Discord account.
Call this account work-discord.
```

Naming/aliases are important, so don’t skip it. I called mine one work-discord.

When prompted, select the work email and authenticate.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_8.png)

Now paste the 2nd prompt

```plain text
Using Composio, connect another YouTube account.
Keep my existing YouTube connection.
Call this one personal-youtube.
```

</synced_block_reference>

Repeat the same process, but select a different account. I named this one personal-discord.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_9.png)

Bravo, you are now done; the only thing left is to test our connection; let’s go through it

Related: [Connect multiple Github accounts to Claude](https://composio.dev/content/how-to-connect-multiple-github-accounts-to-claude)

---

## Testing the Composio Multi Account Setup

To test our multi-account setup, ask it to retrieve the files; don’t forget to add aliases.

Here is me trying it!

**Prompt**

```plain text
Task 1 (work-discord):
1. Fetch the Discord profile (username, email, user ID)
2. List all servers and their member counts
3. Create a table: Server Name | Member Count | Join Status

Task 2 (personal-discord):
1. Get the membership info in server "The Sandbox" (roles, join date)
2. Compare your membership across all servers
3. Show which server you're most active in (highest member count)

Only give me 2 table with all the data in tables.
```

**Output**

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_10.png)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_11.png)

As you can see, it listed all the servers, their member count, and their status for both accounts, as requested.

### A small caveat

You can fetch data from Discord integrations, but for advanced functionality, you need to connect to Composio’s Discord bot.

These features include:

**Reading/Writing Messages:**

- Read messages from channels
- Send messages to channels or DMs
- Create/manage DMs
- Add reactions to messages
- List channels in servers
- Manage server resources

**Server Management:**

- Create/delete channels
- Manage roles
- Moderate members
- Post announcements
- Fetch message history

You can learn more at: [Compsio's Discord bot](https://composio.dev/toolkits/discordbot)

---

## **Make multiple account selection more reliable**

Now, using the same approach, you can connect it to multiple Discord accounts.

However, the agent can hallucinate sometimes. You can make it more foolproof by adding instructions in Claude preferences / as an instruction

```plain text
My Discord accounts are: work-discord= me@company.com, personal-discord= me@gmail.com. Use the named account when I specify one. If I do not specify an account, ask me which one to use before taking any email action.
```

> Note: If Claude’s built-in Discord connector is also enabled, Claude may sometimes use it instead of Composio. For the clearest setup, turn off the built-in Discord connector and use Composio for Discord integration.

---

## **Optional - Manage accounts from the Composio dashboard**

The chat method above is the easiest way to add accounts. Use the dashboard when you want to rename, reconnect, or remove one.

1. Go to [dashboard.composio.dev](https://dashboard.composio.dev/).
2. Sign in with the same Composio account you used to connect to Claude.
3. **F**rom the product switcher, open For You.
4. Find **Discord**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_12.png)

1. Click **Connect** and complete the Discord OAuth flow.

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_13.png)

1. Click **Connect on Discord again** for the next account. Composio adds it as a separate connection rather than replacing the first; this behaviour is what the whole guide hinges on.

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_14.png)

2. Give each connection a clear label. Composio's own marketing shows this pattern as `WORK` / `PERSONAL` / `TEAM` badges, and that's exactly the mental model: same app, several identities, each addressable by name.

From this screen you can also:

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-discord-accounts-to-claude/image_15.png)

- **Rename** a connection when personal-discord / work-discord stops being a useful name.
- **Disconnect/delete** a connection you no longer want Claude touching. Deletion is immediate.
- **Re-authorise** a connection that's expired. 

> Note: Tokens do refresh automatically, but a revoked or expired refresh token moves a connection to EXPIRED and needs a manual reconnect.

Since I mostly work through Claude, I rarely use this method, but I covered it in case someone needs a more visible approach.

---

## Conclusion

Automation is becoming the default way we work, and more of our daily workflows now depend on agents accessing the right data at the right time.

But those workflows break quickly when the data sits across multiple accounts. Switching connections, reauthenticating, and repeating the same setup slows everything down.

Composio’s multi-account support removes that friction. You can connect several accounts for the same app, name them clearly, and let Claude use the right one inside the same workflow.
