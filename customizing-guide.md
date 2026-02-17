In this guide, I’ll walk you through the process of customizing the auth config for **Calendly**. Let’s get started.

## Setting up Calendly

In this section, we’ll go through the process of creating a Calendly OAuth application.

> NOTE:

  If you already have a Calendly OAuth app and access to the **Client ID** and **Client Secret**, you can skip this section.

## Step 1: Create a Calendly Developer App

1. Log in to your Calendly account.

1. Navigate to the Calendly Developer Portal:

```plain text
https://developer.calendly.com/
```

1. Click **My Apps**.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_1.png)

1. Click **Create New App**.

---

## Step 2: Register Your Calendly OAuth App

After clicking **Create New App**, you’ll be asked to fill in the following fields:

### App Details

1. **Name of app**

```plain text
Composio-Calendly
```

1. **Kind of app**

  Select:

```plain text
Web
```

1. **Environment type**

  Choose based on your use case:

```plain text
Sandbox
```

> Use Sandbox for testing and development.

    Switch to **Production** only when going live.

1. **Redirect URI**

  Enter the following redirect URL:

```plain text
https://backend.composio.dev/api/v3/toolkits/auth/callback
```

  **Important requirements:**

  - Must use `https`

  - No trailing slash

  - Must exactly match the value used in Composio

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_2.png)

Once created, Calendly will generate:

- **Client ID**

- **Client Secret**

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_3.png)

Copy both values and store them securely, you’ll need them shortly.

---

## Step 3: Configure Redirect URI

Ensure the following redirect URI is present in your Calendly app configuration:

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_4.png)

```plain text
https://backend.composio.dev/api/v3/toolkits/auth/callback
```

---

## OAuth Scopes in Calendly

Calendly uses **OAuth2 with explicit scopes** to control API access.

Scopes define what resources your integration can access during the OAuth authorization flow.

### Common Calendly OAuth Scopes

Calendly supports the following scopes:

- `default` → Access to the authenticated user’s Calendly data

- `users:read` → Read user profile information

- `events:read` → Read scheduled events

- `event_types:read` → Read event types

- `webhook_subscriptions:write` → Create and manage webhooks

> The actual scopes you should request depend on your integration’s requirements.

---

## Creating the Auth Config in Composio

With your OAuth credentials ready, navigate to the Composio dashboard:

```plain text
https://platform.composio.dev/
```

---

### Step 1: Create Auth Config

1. Click **Create Auth Config**

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_5.png)

1. Select **Calendly** from the list of available toolkits

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_6.png)

---

### Step 2: Configure Authentication Type

1. Ensure authentication is set to **OAuth2**

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_7.png)

1. Enable **Use your own developer authentication**

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_8.png)

---

### Step 3: Fill in Credentials

1. Open the **Manage Auth Config** tab

1. Paste:

  - **Client ID**

  - **Client Secret**

1. Click **Create Calendly Auth Config**

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_9.png)

---

### Scopes Supported by Composio

Below are the Calendly scopes supported by Composio. Add only the scopes required for your integration:

```plain text
default
users:read
events:read
event_types:read
webhook_subscriptions:write
```

---

## Base URL for Calendly

All Calendly API requests go through:

```plain text
https://api.calendly.com/
```

---

## Final Step

Once everything is set up:

1. Copy the **Auth Config ID** (starts with `ac_`)

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_10.png)

1. Store it securely using your secret manager

1. Use it in your application code to authenticate Calendly via Composio
