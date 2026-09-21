# How to Connect Multiple YouTube Accounts to Claude

You open Claude, connect your YouTube account, fetch a few videos or channel details, and everything works fine. The problem starts when the content sits in another YouTube account.

Claude doesn’t let you connect to and switch between multiple YouTube accounts, so you have to disconnect, reauthenticate, and repeat the process.

That gets painful fast when you need videos, channel data, or other content from several YouTube accounts, which is common for teams managing multiple channels.

So if you have been in a similar situation, here is a cleaner way to handle it.

---

## How to Connect to Multiple YouTube Accounts

Create an account at [Composio Dashboard](https://dashboard.composio.dev/) using Google login, set up 2FA, and then ensure you are in the default view

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_1.png)

Head to the mode selector and switch to the **FOR YOU** view, then select I**nstall **next to Claude + Claude Cowork.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_2.png)

A new page will open.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_3.png)

Click on **Add Composio connector in Claude, **and it opens a Claude pop-up

Or just click here: [Composio x Claude](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Composio&connectorUrl=https%3A%2F%2Fconnect.composio.dev%2Fmcp)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_4.png)

Click Continue, and it will ask for your preferred authentication method and OAuth registration for the tools you will use.

> I keep it at the default, and it's preferred as well.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_5.png)

Click **Add, then** grant permissions by pressing Allow access, **the**n sign in to your composio account.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_6.png)

Once done, head to a chat window and ask Claude to configure multiple YouTube accounts. For this, paste the given prompt:

```plain text
Using Composio, connect my work YouTube account.
Call this account work-youtube.
```

Naming/aliases are important, so don’t skip it. I called mine one `work-youtube`.

When prompted, select the work email and authenticate.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_7.png)

Now paste the 2nd prompt

```plain text
Using Composio, connect another YouTube account.
Keep my existing YouTube connection.
Call this one personal-youtube.
	```

</synced_block_reference>

Repeat the same process, but select a different account. I named this one `personal-youtube` .

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_8.png)

Bravo, you are now done; the only thing left is to test our connection; let’s go through it

---

## Testing the Composio Multi Account Setup

To test our multi-account setup, ask it to retrieve the files; don’t forget to add aliases.

Here is me trying it!

**Prompt**

```plain text
From my work-youtube channel, fetch all videos and their statistics (views, likes, comments). From my personal-youtube channel, fetch all videos. 

Output in table format with columns: Video Title, YouTube Link, Views, Likes, Comments, Category, Privacy Status. Sort by most viewed first.
```

The catch is, for personal-youtube, the stats should show 0 videos, cause I haven’t created any channels yet. Let’s see how it performs.

**Output**

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_9.png)

As you can see, it listed videos from the `work-youtube` (2/37) with reasons (I didn’t stated it to perform batch operation), but as expected, it clearly stated there is no YouTube channel for personal-youtube

---

## **Make multiple account selection more reliable**

Now, using the same approach, you can connect it to multiple drives.

However, the agent can hallucinate sometimes. You can make it more foolproof by adding instructions in Claude preferences / as an instruction

```plain text
My YouTube accounts are: work-youtube = me@company.com, personal-youtube = me@gmail.com. Use the named account when I specify one. If I do not specify an account, ask me which one to use before taking any email action.
```

> Note: If Claude’s built-in GDrive connector is also enabled, Claude may sometimes use it instead of Composio. For the clearest setup, turn off the built-in GDrive connector and use Composio for drive integration.

---

## **Optional - Manage accounts from the Composio dashboard**

The chat method above is the easiest way to add accounts. Use the dashboard when you want to rename, reconnect, or remove one.

1. Go to [dashboard.composio.dev](https://dashboard.composio.dev/).
2. Sign in with the same Composio account you used to connect to Claude.
3. **F**rom the product switcher, open For You.
4. Find YouTube.

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_10.png)

5. Click **Connect** and complete the Google OAuth flow.

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_11.png)

6. Click **Connect on GDrive again** for the next account. Composio adds it as a separate connection rather than replacing the first; this behaviour is what the whole guide hinges on.

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_12.png)

7. Give each connection a clear label. Composio's own marketing shows this pattern as `WORK` / `PERSONAL` / `TEAM` badges, and that's exactly the mental model: same app, several identities, each addressable by name.

From this screen you can also:

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-youtube-accounts-to-claude/image_13.png)

- **Rename** a connection when `personal-youtube` stops being a useful name.
- **Disconnect/delete** a connection you no longer want Claude touching. Deletion is immediate.
- **Re-authorise** a connection that's expired. 

> Note: Tokens do refresh automatically, but a revoked or expired refresh token moves a connection to EXPIRED and needs a manual reconnect.

Since I mostly work through Claude, I rarely use this method, but I covered it in case someone needs a more visible approach.

---

## Conclusion

Automation is becoming the default way we work, and more of our daily workflows now depend on agents accessing the right data at the right time.

But those workflows break quickly when the data sits across multiple accounts. Switching connections, reauthenticating, and repeating the same setup slows everything down.

Composio’s multi-account support removes that friction. You can connect several accounts for the same app, name them clearly, and let Claude use the right one inside the same workflow.
