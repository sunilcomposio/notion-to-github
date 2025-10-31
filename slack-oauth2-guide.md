In this guide, I will share the process for customizing the auth config for **Slack**. So, let’s begin.

---

## Setting up Slack

In this section, we’ll go through the process of setting up Slack and creating an OAuth2 application.

> NOTE: If you already have a Slack app and access to the Client ID and Client Secret, you can skip this section.

---

### Step 1: Create a Slack App

1. Go to the [Slack API Dashboard](https://api.slack.com/apps) and click **Create New App**.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/slack/image_1.png)

1. Choose **From scratch**.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/slack/image_2.png)

1. Enter your **App Name** (e.g., `Composio Integration`) and select your Slack **workspace**.

1. Click **Create App**.

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/slack/image_3.png)

---

### Step 2: Configure OAuth & Permissions

Once the app is created, open the **OAuth & Permissions** section from the left sidebar.

Scroll down to the **Redirect URLs** section and click **Add New Redirect URL**.

Paste the following Composio callback URL:

```plain text
https://backend.composio.dev/api/v3/toolkits/auth/callback
```

Click **Add** and then **Save URLs**.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/slack/image_4.png)

---

### Step 3: Add OAuth Scopes

Under the same **OAuth & Permissions** section, scroll to **Scopes**.

Slack scopes define what your app can do or access on behalf of a user.

You’ll need to select the scopes that match your integration’s functionality. For example:

- To read channel messages → `channels:history`

- To send messages → `chat:write`

- To read user info → `users:read`

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/slack/image_5.png)

---

## Scopes Supported by Composio

Instead of manually guessing the scopes in Slack, use the scopes that **Composio supports** for Slack.

**Scopes supported by Composio:**

Below are all the scopes that Composio supports for Slack. You can select these based on your integration needs:

```plain text
calls:read,calls:write,channels:read,channels:history,channels:write,chat:write,chat:write.public,emoji:read,files:read,files:write,groups:read,groups:history,groups:write,im:read,im:write,im:history,mpim:read,mpim:write,mpim:history,pins:read,pins:write,reactions:read,reactions:write,reminders:read,reminders:write,remote_files:read,remote_files:share,search:read,stars:read,stars:write,team:read,usergroups:read,usergroups:write,users:read,users:read.email,users:write,users.profile:read,users.profile:write,bookmarks:read,bookmarks:write,links:read,links:write
```

These define what actions your Slack integration can take.

Always include only the ones necessary for your workflow, for example, if your agent only needs to send messages, `chat:write` alone might be enough.

**Note:** Slack separates **Bot Token Scopes** (for app-level access) and **User Token Scopes** (for user-level actions). Composio typically uses **Bot Token Scopes**.

---

### Step 4: Get Your OAuth Credentials

Once you've created your Slack app, you can find your OAuth credentials in the **Basic Information** section:

1. In the left sidebar, click **Basic Information**

1. Scroll down to the **App Credentials** section

1. You'll see:

Copy the **Client ID** and **Client Secret**, as you'll need them for Composio.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/slack/image_6.png)

---

## Creating the Auth Config in Composio

With your Slack credentials ready, navigate to the [Composio Dashboard](https://platform.composio.dev/).

1. Click on **Create Auth Config**.

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/slack/image_7.png)

1. From the toolkit list, select **Slack**.

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/slack/image_8.png)

1. Ensure the authentication type is set to **OAuth2**.

1. Check **Use your own developer authentication** if using your custom Slack app.

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/slack/image_9.png)

1. Click **Create Slack Auth Config**.

---

### Step 5: Fill in OAuth Details

In the **Manage Auth Config** section, fill in the fields:

- **Client ID** → from Slack app

- **Client Secret** → from Slack app

- **Redirect URI** →

- **Scopes** → use the Composio-supported scopes listed above

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/slack/image_10.png)

![Image 11](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/slack/image_11.png)

Click **Create Slack Auth Config**.

---

## Base URL for Slack API

All Slack API calls are made via:

```plain text
https://slack.com/api/
```

---

That’s it! Your **Slack + Composio OAuth integration** is ready.

You can now copy the Auth Config ID (`ac_...`) and use it inside your code or secret manager.



- **Client ID**

- **Client Secret** (click "Show" to reveal it)

- **Signing Secret** (click "Show" to reveal it)

```plain text
https://backend.composio.dev/api/v3/toolkits/auth/callback
```
