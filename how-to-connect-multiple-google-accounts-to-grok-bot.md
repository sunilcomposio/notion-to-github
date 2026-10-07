# How to Connect Multiple Google Accounts to Grok Bot

Connecting one Google account to an Grok bot is pretty easy. Native plugins usually do the job fine. Things get more annoying when you have multiple Google accounts you want to use. Say, a personal Gmail account, a work account, and maybe another one for idk.

You want Grok Bot to check Gmail from one account, search Drive in another, I think you get the idea.

And of course, you probably don't want to disconnect and reconnect Google every time.

> 💁 **Quick note:** Grok Bot is limited when you want to handle multiple Google accounts in the same workflow.

With Composio, you can keep each Google account as a separate connection, and use them without logging in and out or setting up Google OAuth yourself

This guide explains how to connect multiple Google accounts to Grok Bot and use them in the same workflow.

The nice part is that Google is much easier to set up than some other integrations.

Composio provides managed Google OAuth, so you don't need to create a Google Cloud app, configure redirect URLs, or copy OAuth credentials before getting started.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_1.png)

> Related: [Connect multiple Tiktok accounts to Claude](https://composio.dev/content/connect-multiple-tiktok-accounts-to-claude)

## How Composio solves this

Composio sits between Grok Bot and your Google accounts.

The setup looks roughly like this:

```plain text
Grok Bot
   ↓
Composio
   ↓
Google
   ├── work-google
   ├── personal-google
   └── client-google
```

Each Google login stays as its own connected account.

We then give each connection an alias such as `work-google` or `personal-google`.

So I can ask:

> Summarize my unread emails from work-google.

And then:

> Find the latest invoice in personal-google.

Or I can use both accounts in the same task:

> Check my work and personal calendars for conflicts tomorrow.

Grok Bot can use Composio to select the correct Google connection for each action. I think you can see the amount of pain it solves. This becomes especially useful once Gmail, Drive, Docs, and so on start appearing in the same workflow.

Let's set it up.

## Before you start

You will need:

- Access to Grok Bot.
- A Composio account (free with over 100K tool calls a month 🔥).
- Access to each Google account you want to connect.
- Permission from your Google Workspace administrator if your organization restricts third-party applications.

That's basically it.

You don't need to create a Google developer app for the normal Composio flow.

Composio provides managed OAuth for Google, so each account can authorize access directly.

> 💁 If your organization requires its own Google OAuth application, you can configure custom OAuth separately. Most people following this guide do not need it.

One more thing before we start.

ℹ️ If you already use Grok Bot's own Google or Gmail plugin, I would disable it for this walkthrough.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_2.png)

Otherwise, Grok Bot may choose the native Google integration when we specifically want it to use the accounts connected through Composio.

Lovely, now that you have everything, let’s get it done.

## Step 1: Add Composio to Grok Bot

Open Grok Bot and go to **Plugins**.

Search for **Composio**. Select the Composio plugin and click **Add**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_3.png)

Once it is added, go to your installed plugins.

Open Composio and click **Authenticate**. Grok Bot should open the Composio authorization flow.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_4.png)

Sign in to the Composio account you want to use and approve the connection. Once authentication finishes, return to Grok Bot.

Composio should now appear as an installed and connected plugin. ✅

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_5.png)

### Quickly check the connection

I like to test this before connecting any accounts.

Ask Grok Bot:

> What Composio tools do you have available?

You should see the Composio tools available to the bot.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_6.png)

If you see them, we're good to go.

> 💁 I already have a few apps connected here, so they’re showing up for me. For you, though, this section will be empty.

## Step 2: Connect your first Google account

Let's start with your work Google account.

Ask Grok Bot:

> Connect my Google account using Composio. Label it work-google.

If you specifically want Gmail, you can make the request more explicit:

> Connect my Gmail account using Composio. Label it work-gmail.

Grok Bot should use Composio to create a new connection request and give you an authentication link.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_7.png)

Open the link.

Google will ask you to choose an account and review the requested permissions. Make sure that you only select the scopes that you require.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_8.png)

Check the email address shown by Google before approving access.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_9.png)

Select your work account and approve the permissions you want Composio to use.

Once authentication finishes, return to Grok Bot.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_10.png)

Your first Google connection should now be active.

### What does `work-google` do?

`work-google` is simply a readable name for that connected account.

Behind the scenes, Composio keeps each authorization as a separate connected account.

Instead of making Grok Bot deal with something like:

```plain text
ca_xxxxxxxxx
```

we can tell it to use:

```plain text
work-google
```

This gets much more useful once we add the second account.

## Step 3: Connect another Google account

Now let's add your personal Google account.

Ask Grok Bot:

> Connect another Google account using Composio. Keep my existing connection and label the new one personal-google.

If you're connecting Gmail specifically, use:

> Connect another Gmail account using Composio. Keep my existing connection and label it personal-gmail.

Grok Bot should create another authentication request.

Google may automatically select whichever account is already active. I would open the authentication link in an incognito window or another browser profile.

Sign in using your personal Google account.

```plain text
work-google
personal-google
```

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_11.png)

Want to add some more?

Repeat the same process. Each authorization creates a separate connected account.

Composio's multi-account configuration defaults to five accounts per toolkit and can be configured between two and ten. ✌️

## Step 4: Verify the accounts before using them

Before letting Grok Bot send emails, modify files, or change anything important, verify the connections.

Ask:

> List all my connected Google accounts in Composio with their aliases and connection status.

You should get something similar to:

```plain text
work-google       ACTIVE
personal-google   ACTIVE
... you could add many more
```

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_12.png)

## Step 5: Use multiple Google accounts together

Now we can actually do something useful.

For example:

> Summarize today's unread emails from work-gmail and personal-gmail. Keep the results separated by account.

Grok Bot can use one connection for the first Gmail request and another connection for the second.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_13.png)

This is where the setup becomes much more useful. You are not just switching between Google accounts.

Grok Bot can work across the correct account and the correct Google app as required.

### Gmail, Drive, Calendar, Docs, and Sheets

Composio exposes Gmail, Google Drive, Google Calendar, Google Docs, and Google Sheets as Google toolkits.

So if you only need Gmail, you can keep aliases such as:

```plain text
work-gmail
personal-gmail
```

If you're also connecting other Google services, keep the naming consistent:

```plain text
work-gmail
work-calendar
work-drive

personal-gmail
personal-calendar
personal-drive
```

That makes it much harder for the bot to choose the wrong account.

Composio also has a [Google Super toolkit](https://docs.composio.dev/toolkits/googlesuper) that combines several Google services behind one Google connection.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_14.png)

That can be useful if you want one Google identity to work across Gmail, Calendar, Drive, Sheets, Meet, and other supported Google services.

For example:

```plain text
work-google
personal-google
```

instead of maintaining separate aliases for every Google toolkit.

Which approach you use depends on how much Google access you want to give the bot.

## A note for Google Workspace accounts

Personal Google accounts usually go through the normal OAuth flow.

Work accounts can be different.

Google Workspace administrators can restrict which third-party apps employees are allowed to authorize.

So if Google tells you that the application is blocked when connecting your work account, the problem may be your Workspace policy rather than Grok Bot or Composio.

Your Workspace administrator may need to allow the application under Google's third-party app access controls.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_15.png)

I would also start with read actions when connecting a company account.

Try things such as:

> List my five latest emails from work-gmail.

or:

> Show tomorrow's events from work-calendar.

Once you know the correct account is being selected, move on to send, edit, delete, or share actions.

## Managing the accounts later

You don't need to rebuild everything if one of your Google accounts changes.

You may want to:

- Add another Google account.
- Rename an account alias.
- Disconnect an old client account.
- Reauthorize an expired connection.
- Check which Google connections are still active.

You can ask Grok Bot to manage these through Composio.

For example:

> List all my Gmail connections in Composio.

Or:

> Connect another Gmail account and call it client-gmail.

Or, even removing some connections that you don’t want anymore.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_16.png)

Keep one thing in mind.

Connections made through Composio's developer Platform are separate from connections created through the **For You / Connect** flow used by the Grok Bot plugin.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-google-accounts-to-grok-bot/image_17.png)

So if you already connected Gmail somewhere else in the Composio developer dashboard and it doesn't appear here, you may need to authenticate it again through the Grok Bot flow.

For accounts you no longer use, disconnect them instead of leaving their authorization active.

Pretty basic, but worth doing.

## Conclusion

That's basically it.

Google is much easier to set up than integrations where you first need to create your own OAuth app like [Tiktok](https://composio.dev/content/connect-multiple-tiktok-accounts-to-claude) we did earlier.

Add the Composio plugin to Grok Bot, authenticate Composio, and then connect each Google account separately. That simple.

Once you have this part working, you can also run workflows across multiple accounts and Google apps in the same task.

Just verify each alias before you start sending emails, editing files, or changing calendar events.

And whenever you authenticate another Google account, check the email shown on Google's consent screen before approving it.

That's literally all the work needed. 🔥

## FAQ

### Can Grok Bot connect to Google?

Yes.

Grok Bot supports plugins, and Composio can give it access to Google services such as Gmail, Drive, Calendar, Docs, and Sheets.

This guide uses Composio so you can keep separate Google connections and reference them with aliases.

### Can I connect multiple Google accounts to Grok Bot?

Yes, through Composio.

Each Google authorization becomes its own connected account.

You can then give the accounts aliases such as:

```plain text
work-google
personal-google
client-google
```

and tell Grok Bot which one it should use.

### Do I need a Google Cloud developer app?

Not for the standard setup in this guide.

Composio provides managed Google OAuth, so you can connect a Google account without first creating your own OAuth application.

You only need a custom Google OAuth configuration if you want control over things such as your Google Cloud project, consent-screen branding, or requested scopes.

### Do all my Google accounts get merged?

No.

Each Google login remains its own authenticated connection.

Aliases just give Grok Bot and Composio an easier way to identify the connection you want to use.

### How many Google accounts can I connect?

Composio's multi-account mode defaults to five connected accounts per toolkit.

The configured limit can range from two to ten.

### Should I use `work-google` or `work-gmail`?

Either naming approach can work, but keep it consistent.

If you're only connecting Gmail, something like:

```plain text
work-gmail
personal-gmail
```

is clearer.

If you're using a unified Google connection across several services, aliases such as:

```plain text
work-google
personal-google
```

are easier to remember.

### Can I use Gmail, Drive, and Calendar from different Google accounts?

Yes.

For example, you could ask Grok Bot to search Gmail from your work account and then compare the dates it finds with your personal Google Calendar.

Keep clear aliases so Grok Bot knows which connection belongs to each Google identity.

### Can Grok Bot access both accounts in the same prompt?

Yes.

For example:

> Summarize unread emails from work-gmail and personal-gmail separately.

Or:

> Compare work-calendar and personal-calendar and find any conflicts tomorrow.

Composio can route each tool execution to the selected account.

### What if Google keeps connecting the wrong account?

This usually happens because Google reuses an account that is already signed into your browser.

Open the new authentication link in an incognito window or a separate browser profile.

Then check the email address displayed on Google's consent screen before approving access.

### Why does my work Google account say the app is blocked?

Your Google Workspace administrator may restrict third-party OAuth applications.

If that happens, your administrator may need to approve or trust the application through the Google Workspace Admin console before you can connect it.

### Can I send emails from different Gmail accounts?

Yes, as long as the connected account has the required Gmail permissions.

Be explicit about which account should send the message.

For example:

> Draft the reply using work-gmail. Show it to me before sending.

I would verify account routing with read actions first before allowing send, delete, move, or other write operations.

### Can I copy a Drive file from one Google account to another?

You can build a workflow that reads or downloads the file from one Google connection and then uploads it through another connection.

There isn't a single Google API action that magically transfers ownership across unrelated accounts in every case.

So think of it as two steps:

```plain text
work-drive
   ↓
Get/download file
   ↓
Grok Bot + Composio
   ↓
personal-drive
   ↓
Upload file
```

The exact result still depends on the file type and the permissions of both accounts.

### Can I rename an account later?

Yes.

Composio connected accounts support aliases, so you can rename a connection if something such as `google-2` eventually needs to become `client-google`.

Clear names become increasingly important once you have several accounts connected.

### What happens if I remove one account?

The other connected Google accounts remain separate.

Removing `client-google`, for example, does not remove `work-google` or `personal-google`.

You can reconnect the removed account later if you need it again.
