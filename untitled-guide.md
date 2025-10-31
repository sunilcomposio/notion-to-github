In this guide, I will share the process for customizing the auth config for Monday.com. So, let's begin.

## Setting up Monday.com

In this section, we'll go through the process of setting up Monday.com and creating an OAuth application.

> NOTE: If you already have a Monday.com OAuth app and access to the Client ID and Client Secret, you can skip this section.

### Step 1: Create a Monday.com Developer App

Head over to the [Monday.com Developers](https://monday.com/developers/apps) page and create a new app.

1. Log in with your Monday.com account.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/untitled/image_1.png)

1. Click **Create App** in the developer console.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/untitled/image_2.png)

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/untitled/image_3.png)

### Step 2: Register Your OAuth App and Generate Credentials

Once you've created the app, you'll need to configure OAuth settings and generate credentials.

1. Give your app a name (e.g., `Composio-Monday`).

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/untitled/image_4.png)

1. After creating the app, Monday.com will automatically generate the Client ID, Client Secret, and Signing Secret at the bottom of the page.

1. Copy the Client ID and Client Secret somewhere safe, as you'll need them shortly.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/untitled/image_5.png)

Now, you'll need the OAuth credentials. After saving, Monday.com will show the **Client ID** and **Client Secret**. Copy them somewhere safe.

### Step 3: Set the Redirect URI

You'll also need to configure the Redirect URI to point to Composio's callback URL.

In the Redirect URLs section of your Monday.com OAuth app, add the following URL:

```plain text
https://backend.composio.dev/api/v3/toolkits/auth/callback
```

Make sure there's no trailing slash, and the protocol is `https`.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/untitled/image_6.png)

### Step 4: Configure OAuth Scopes

Monday.com requires you to specify scopes for API access. Common scopes include:

- `boards:read` → Read board data

- `boards:write` → Create and modify boards

- `workspaces:read` → Read workspace information

- `users:read` → Read user data

- `me:read` → Read current user information

**Scopes supported by Composio:**

Below are all the scopes that Composio supports for Monday.com. You should add these scopes based on your integration requirements:

```plain text
account:read,assets:read,boards:read,boards:write,docs:read,docs:write,me:read,notifications:write,tags:read,teams:read,teams:write,updates:read,updates:write,users:read,users:write,webhooks:read,webhooks:write,workspaces:read,workspaces:write
```

These scopes define the permissions your app can request during the OAuth authorization process. It's essential to select only the scopes necessary for your application's functionality to adhere to the principle of least privilege.

**Note:** The actual scopes you should request depend on your specific integration requirements. For example, if your app needs to read and write board data, you would include `boards:read` and `boards:write` in your OAuth configuration.

You can customize these based on your integration needs.

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/untitled/image_7.png)

That's all you need to set up on the Monday.com side.

---

## Creating the Auth Config in Composio

With your OAuth credentials ready, navigate to the [Composio dashboard](https://platform.composio.dev/) to configure the authentication settings for Monday.com.

1. Click on the **Create Auth Config** button to get a list of all the toolkits available.

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/untitled/image_8.png)

1. In the sidebar that opens, choose **Monday.com** for the toolkit. Stick with all the default settings for now, as we'll configure it shortly.

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/untitled/image_9.png)

1. Ensure the authentication is set to **OAuth2** and not **Bearer Token**.

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/untitled/image_10.png)

1. Also, make sure to check "**Use your own developer authentication**"

1. Then, click **Create Monday Auth Config**.

1. Once you have the auth config for Monday.com set up, go to the **Manage Auth Config** tab, where you can fill in the auth config fields.

1. Paste the Client ID and Client Secret you just copied from Monday.com into their respective fields.

![Image 11](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/untitled/image_11.png)

Instead of manually selecting scopes from Monday.com, use the scopes that **Composio supports** for Monday.com.

![Image 12](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/untitled/image_12.png)

## Base URL for Monday

All Bitbucket API requests go through:

```plain text
https://api.monday.com/v2
```

---

This is the endpoint for all Monday.com API calls.

Once done, copy the auth config ID (which starts with `ac_`) and use it in your application code via a secret manager.

Your custom Monday.com auth config is now ready to go!


