In this guide, I’ll share the process for customizing the auth config for **Dropbox**. So, let’s begin.

---

## Setting up Dropbox

In this section, we’ll go through the process of setting up a Dropbox developer app and obtaining OAuth credentials.

> NOTE: If you already have a Dropbox developer app and access to its Client ID and Client Secret, you can skip this section.

---

### Step 1: Create a Dropbox App

1. Log in to your Dropbox account.

1. Visit the **Dropbox App Console**:

  [https://www.dropbox.com/developers/apps](https://www.dropbox.com/developers/apps)

1. Click **Create app**.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_1.png)

1. For **Choose an API**, select **Scoped access**.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_2.png)

1. For **Choose the type of access you need**, select:

  - **App folder** (access only to a specific folder), or

  - **Full Dropbox** (access to all files)

    depending on your use case.

1. Enter an app name (e.g., `Composio‑Dropbox`), and click **Create app**.

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_3.png)

---

### Step 2: Client ID and Client Secret

Once the app is created:

1. You’ll be taken to the app’s settings page.

1. Under the **App key** and **App secret** fields, you’ll see your **Client ID** and **Client Secret**.

  - **App key** → **Client ID**

  - **App secret** → **Client Secret**

1. Copy both values and save them securely — you’ll need them in Composio.

---

### Step 3: Configure Redirect URI

In the Dropbox developer app settings:

1. Scroll to the **OAuth 2** section.

1. Under **Redirect URIs**, click **Add** and enter:

```plain text
https://backend.composio.dev/api/v3/toolkits/auth/callback
```

1. Make sure the redirect URI matches exactly , otherwise Dropbox will reject the OAuth flow.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_4.png)

---

### Step 4: Configure OAuth Scopes (Permissions)

Dropbox lets you choose **scopes** that control what your app can access.

1. In your app settings, go to the **Permissions** tab.

1. Check the permissions your integration needs, for example:

  - `files.metadata.read` → Read file metadata

  - `files.content.read` → Read file contents

  - `files.content.write` → Modify uploaded files

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_5.png)

You should only request **permissions required** by your integration.

---

## Creating the Auth Config in Composio

With your Dropbox credentials ready, navigate to the **Composio dashboard** at [https://platform.composio.dev/](https://platform.composio.dev/) to configure the authentication settings for Dropbox.

1. Click on the **Create Auth Config** button to get a list of all the toolkits available.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_6.png)

1. In the sidebar, choose **Dropbox** for the toolkit.

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_7.png)

1. Ensure authentication is set to **OAuth2** (not API Key).

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_8.png)

1. Enable **Use your own developer authentication**.

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_9.png)

1. Click **Create Dropbox Auth Config**.

1. Once created, go to the **Manage Auth Config** tab and fill in the fields:

  - **Client ID** → Paste the App key from Dropbox

  - **Client Secret** → Paste the App secret

---

### Scopes Supported by Composio

Below are resource‑level scopes Composio supports for Dropbox. Add only the ones your integration requires:

```plain text
email
profile
account_info.write
account_info.read
files.metadata.write
files.metadata.read
files.content.write
files.content.read
openid
file_requests.write
file_requests.read
sharing.write
sharing.read
contacts.write
contacts.read
```

> Note: Composio will map these to the Dropbox scopes you enabled in the Dropbox app settings.

---

## Base URL for Dropbox

All Dropbox API requests go through:

```plain text
https://api.dropboxapi.com/2/
```

For file content operations, use:

```plain text
https://content.dropboxapi.com/2/
```

These are the endpoints for Dropbox API calls.

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_10.png)

---

## Final Step

Once done:

1. Copy the **Auth Config ID** (which starts with `ac_`)

![Image 11](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/customizing/image_11.png)

1. Store it securely using your secret manager

1. Use it in your application code to authenticate Dropbox via Composio
