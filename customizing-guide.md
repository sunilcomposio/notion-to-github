In this guide, we’ll walk through how to configure authentication for **Notion** using **OAuth 2.0** and **API Key **with Composio.

Notion supports **public OAuth integrations**, making it straightforward to set up and test.

---

## Setting up Notion

In this section, we’ll create a Notion integration to obtain the **Client ID** and **Client Secret**.

> **NOTE:**

  If you already have a Notion integration with OAuth credentials, you can skip to **Creating the Auth Config in Composio**.

---

## Step 1: Create a Notion Account (if needed)

If you don’t already have one, sign up here:

```plain text
https://www.notion.so/
```

A free workspace is sufficient.

---

## Step 2: Create a Notion Integration

1. Go to the Notion integrations page:

```plain text
https://www.notion.so/profile/integrations
```

1. Click **New integration**

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_1.png)

---

## Step 3: Configure the Notion Integration

After creating a new integration, configure the basic details as follows:

### Basic Information

- **Integration Name**

```plain text
Composio Notion Integration
```

- **Associated Workspace**

  - Select the Notion workspace where this integration will be installed and tested.

- **Integration Type**

  - Select **Public**

> **Why Public?**

  Public integrations allow OAuth-based authentication and can be connected by multiple users or workspaces. This is required for Composio’s OAuth flow.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_2.png)

---

## Step 4: Configure OAuth Settings

1. Navigate to the **OAuth domains & URIs** section in the integration settings.

1. Add the following **Redirect URI**:

```plain text
https://backend.composio.dev/api/v3/toolkits/auth/callback
```

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_3.png)

### Redirect URI Requirements

- Must use **HTTPS**

- Must match the URI exactly

- No trailing slash

- No URL fragments, wildcards, or relative paths

This redirect URI is required so Notion can return the authorization code to Composio after the user successfully authenticates.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_4.png)

---

## Step 5: Select Capabilities (Permissions)

Notion permissions are configured as **Capabilities**, not traditional scopes.

Commonly enabled capabilities:

- Read content

- Update content

- Insert content

- Read user information

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_5.png)

> Select only the capabilities your integration requires.

---

## Step 6: Copy OAuth Credentials

Once saved, Notion will generate:

- **Client ID**

- **Client Secret**

- **Authorization URL**

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_6.png)

> **Important:**

  Store the Client Secret securely. Treat it like a password.

---

## Creating the Auth Config in Composio

Now that your Notion OAuth credentials are ready, configure them in Composio.

---

## Step 7: Create a Notion Auth Config

1. Go to the **Composio Dashboard**:

```plain text
https://platform.composio.dev/
```

1. Click **Create Auth Config**

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_7.png)

1. Select **Notion** from the toolkit list

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_8.png)

1. Ensure authentication type is **OAuth2**

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_9.png)

1. Enable **Use your own developer authentication**

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_10.png)

1. Click **Create Notion Auth Config**

---

## Step 8: Configure the Auth Config

Open the **Manage Auth Config** tab and fill in:

- **Client ID** → from Notion

- **Client Secret** → from Notion

- **Redirect URI**

```plain text
https://backend.composio.dev/api/v3/toolkits/auth/callback
```

![Image 11](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_11.png)

---

## METHOD 2:  API Key (Internal Integration Secret) Authentication

In addition to OAuth, Notion also supports **API Key–based authentication** using an **Internal Integration Token**. This method is useful for server-to-server use cases or when OAuth is not required.

![Image 12](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_12.png)

> **Important:**

  API Key authentication only works with **Internal integrations**. Public integrations **must use OAuth**.

---

### When to Use API Key Authentication

Use an **Internal Integration Token** if:

- You are integrating with **your own Notion workspace**

- You do **not** need multi-user OAuth access

- You want a **simpler, non-redirect** authentication flow

- The integration will be used by a **single workspace**

---

### Creating an Internal Integration Token

1. On the **Notion Integrations** page, click **New integration**

1. Enter a name such as:

```plain text
Composio-API-Test
```

1. Select the **Associated Workspace**

1. Set the **Integration Type** to **Internal**

1. Click **Submit**

Once created, Notion will generate an **Internal Integration Token**.

![Image 13](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_13.png)

> **Save the token immediately,** it will not be shown again.

---

### Using API Key Authentication in Composio

To configure API Key authentication in Composio:

1. Choose **API Key** as the authentication method

![Image 14](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_14.png)

1. Click **Create Auth Config**

---

## How OAuth Works with Composio

When a user connects their Notion account:

- Composio handles the OAuth authorization flow

- The user approves access to selected capabilities

- Access and refresh tokens are securely stored by Composio

- Tokens are refreshed automatically

![Image 15](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_15.png)

---

## How Authentication Works (API Key)

- The Internal Integration Token is stored securely in Composio

- Composio sends the token in the request headers

- No OAuth redirect or user consent flow is involved

- Access is limited to explicitly shared pages only

![Image 16](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_16.png)

---

## Base URL for Notion API

All Notion API requests are sent to:

```plain text
https://api.notion.com/v1
```

## Test Notion Connection (Optional)

You can verify the connection using **Composio Playground**.

1. Open your **Notion Auth Config**

1. Navigate to the **Playground**

![Image 17](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_17.png)

If configured correctly, Composio will successfully access your Notion workspace.

---

## Final Step

Once everything is set up:

1. Copy the **Auth Config ID** (starts with `ac_`)

![Image 18](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_18.png)

1. Store it securely in your secret manager

1. Use it in your application code to authenticate Notion via Composio

Your **Notion integration** is now ready to use!
