In this guide, I’ll walk through the process of setting up OAuth for Microsoft Outlook using the Azure App Registrations portal. 

This lets your app connect securely to Microsoft Graph APIs (which power Outlook, OneDrive, Teams, etc.).

> Note: You do not need a Microsoft 365 Developer sandbox for this. A free Microsoft personal account is enough to register an OAuth app.

---

## Step 1: Create an Azure App Registration

1. Go to the [Azure Portal](https://portal.azure.com/#home).

1. In the left-hand menu, search for **App registrations** and click **+ New registration**.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_1.png)

1. Fill in your app details:

  - **Name**: Example → `Outlook Integration`

  - **Supported account types**:

    - Choose **Accounts in any organizational directory and personal Microsoft accounts** (so both work/school and personal Outlook accounts can log in).

  - **Redirect URI (optional)**: Choose **Web** and paste:

```plain text
https://backend.composio.dev/api/v3/toolkits/auth/callback
```

1. Click **Register**.

---

## Step 2: Generate Client Credentials

Once your app is created, you’ll be redirected to its Overview page.

1. Copy the **Application (client) ID** — this is your **Client ID**.

1. From the sidebar, go to **Certificates & secrets** → **+ New client secret**.

  - Add a description and set expiry (6 or 12 months recommended).

  - Copy the generated **Client Secret**  and save it securely.

> **Note** : The client secret is **Value** not the secret ID.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_2.png)

> ⚠️ Important: You won’t be able to see the secret again once you leave the page.

---

## Step 3: Configure API Permissions

Now, we’ll give the app permissions to access Outlook data.

1. In the sidebar, click **API Permissions** → **+ Add a permission**.

1. Select **Microsoft Graph**.

1. Choose **Delegated permissions**.

1. Add the required common Outlook-related scopes, such as:

  - `Mail.Read` → Read user’s emails

  - `Mail.Send` → Send emails on behalf of the user

  - `offline_access` → Enable refresh tokens

  - `openid profile email` → Basic login profile

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_3.png)

Click **Add permissions**.

---

## Step 4: Authentication

1. From the sidebar, open **Authentication**.

1. Under **Redirect URIs**, make sure this URL is added:

```plain text
https://backend.composio.dev/api/v3/toolkits/auth/callback
```

1. Under **settings**, enable **Allow public client flows** (this makes it easier to test).

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_4.png)

**Save changes.**

---

## Step 5: Create the Auth Config in Composio

With your Client ID and Client Secret ready, head over to the [Composio Dashboard](https://platform.composio.dev/?utm_source=chatgpt.com).

1. Click **Create Auth Config**.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_5.png)

1. Select **Microsoft Outlook**.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_6.png)

1. Choose **OAuth2** as the authentication type.

1. Check **Use your own developer authentication**.

1. Paste in your:

  - **Client ID** → from Azure App Registration

  - **Client Secret** → from Certificates & secrets

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_7.png)

  - **Redirect URI** →

```plain text
https://backend.composio.dev/api/v3/toolkits/auth/callback
```

1. click on **create Outlook config**.

---

## Step 6: Authorize and Connect

1. In Composio, click **Connect Account** for the Outlook config.

1. You’ll be redirected to Microsoft’s login screen.

1. Approve the requested permissions (Mail.Read, Mail.Send, etc.).

1. Composio stores the tokens once authorization succeeds.

1. now you can try using tool to check the connection.

---

## API Base URL

For Microsoft Graph (which powers Outlook), the base URL is:

```plain text
https://graph.microsoft.com/v1.0
```

Example endpoints:

- List user emails → `/me/messages`

- Send email → `/me/sendMail`

Once done, copy the auth config ID (which starts with `ac_`) and use it in your application code via a secret manager.

---

## Scopes for Other Microsoft Apps

If you want to integrate with other Microsoft services, you can reuse the same Azure app. Just add the required **scopes** in both Azure and Composio:

- **OneDrive** → `Files.ReadWrite`, `Files.read.all`

- **Teams** → `Channel.Create, Channel.ReadBasic.All, ChannelMessage.Read.All, ChannelMessage.ReadWrite, ChannelMessage.Send, ChannelSettings.ReadWrite.All, Chat.Create, Chat.Read, Chat.ReadBasic, Chat.ReadWrite, Chat.ReadWrite.All, ChatMessage.Read, ChatMessage.Send, Directory.ReadWrite.All, Group.ReadWrite.All, offline_access, People.Read.All, Presence.ReadWrite, Team.Create, Team.ReadBasic.All, TeamMember.ReadWrite.All, TeamsActivity.Read, TeamsActivity.Send, User.Read, OnlineMeetings.ReadWrite`

- **Sharepoint **→ `List.Read`

- **Excel **→` Files.ReadWrite Sites.ReadWrite.All offline_access User.Read`

Once scopes are added, you can configure additional auth configs in Composio for each service.



# Additional: Restricting Access to Specific Tenants

> *If you're using your own custom OAuth app and want to limit access to only your organization's tenant or specific customer tenants, follow the steps below.*

When the OAuth app is set to **Multiple Entra ID tenants**, it uses the `/common` endpoint, which means any Microsoft organisation could potentially authenticate. To restrict this, you can use the **Allowed tenants** setting to whitelist only specific organisations.

# Step 1: Set Supported Account Type

1. Go to [portal.azure.com](https://portal.azure.com/) and navigate to your app registration.

1. In the search bar, search for **App registrations** and click **+ New registration**.

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_8.png)

1. In the following page, you will be asked add your App name and Authentication type.

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_9.png)

1. Under **Authentication**, click the **Supported account types** tab.

1. Select **Multiple Entra ID tenants**.

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_10.png)

# Step 2: Restrict to Specific Tenants

1. Under the **Supported accounts** tab, select **"Allow only certain tenants (Preview)"**.

1. Click **Manage allowed tenants**.

![Image 11](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_11.png)

1. Add the **Tenant ID** of each organisation you want to allow.

![Image 12](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_12.png)

1. Click Apply.

1. (Optional) Add call back URL. Select Web and paste your callback URL.

1. Then click Register.

![Image 13](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_13.png)

# How to Find Your Tenant ID

1. Go to [portal.azure.com](https://portal.azure.com/).

1. Search for **Microsoft Entra ID**.

1. Your **Tenant ID** is listed on the Overview page.

> *⚠️ ****Important:**** If you select ****"Allow all tenants"**** instead, any Microsoft organization will be able to authenticate with your app. Only use this if you intend for your app to be publicly accessible.*
