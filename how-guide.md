In this guide, I’ll share the process for creating OAuth2 credentials in **Snowflake** and connecting them to **Composio**. Let’s get started 

---

## Setting up Snowflake

Snowflake doesn’t give you a web UI for creating OAuth apps. Instead, you create an **OAuth security integration** using SQL commands.

> NOTE: You need access to a Snowflake account with the ACCOUNTADMIN role (or a role that has the privilege to create integrations).

---

### Step 1: Log in to Snowflake

1. Go to Snowflake Web UI.

1. Log in with your Snowflake account credentials.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_1.png)

1. Once logged in, click on the **Worksheet** tab (this is where you can run SQL commands).

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_2.png)

---

### Step 2: Create a New OAuth Integration

Copy and paste the following SQL command into your worksheet:

```sql
CREATE SECURITY INTEGRATION oauth_custom_all_roles
  TYPE = oauth
  ENABLED = true
  OAUTH_CLIENT = custom
  OAUTH_CLIENT_TYPE = 'CONFIDENTIAL'
  OAUTH_REDIRECT_URI = 'https://backend.composio.dev/api/v1/auth-apps/add'
  OAUTH_ISSUE_REFRESH_TOKENS = TRUE
  OAUTH_REFRESH_TOKEN_VALIDITY = 7776000;

```

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_3.png)

Then, click **Run** (▶️ button).

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_4.png)

This creates a new OAuth client inside your Snowflake account.

---

### Step 3: Retrieve Your Credentials

Now, run this command to get your **Client ID** and **Client Secret**:

```sql
DESC SECURITY INTEGRATION oauth_custom_all_roles;
```

1. This will display a table of settings.

1. Look for these two fields in the output:

```sql
SELECT SYSTEM$SHOW_OAUTH_CLIENT_SECRETS('OAUTH_CUSTOM_ALL_ROLES') AS SECRETS;
```

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_5.png)

Copy them somewhere safe. You’ll need them in Composio.

---

## Creating the Auth Config in Composio

Now let’s move over to Composio.

1. Open the [Composio Dashboard](https://platform.composio.dev/?utm_source=chatgpt.com).

1. Click on **Create Auth Config**.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_6.png)

1. From the list of toolkits, select **Snowflake**.

1. Make sure the authentication type is set to **OAuth2**.

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_7.png)

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_8.png)

---

### Step 4: Fill in Your OAuth Details

In the **Manage Auth Config** screen:

- **Client ID** → Paste the `OAUTH_CLIENT_ID` from Snowflake.

- **Client Secret** → Paste the `OAUTH_CLIENT_SECRET` from Snowflake.

- **Redirect URI** → Must exactly match:

```plain text
https://backend.composio.dev/api/v3/auth-apps/add
```

- **Scopes** → Preferred to Leave default (Snowflake handles permissions based on your integration).

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_9.png)

Click Create Auth Config.

---

Snowflake requires you to specify scopes for API access. Common scopes define what actions your integration can perform on behalf of a user.

**Scopes supported by Composio:**

Below are all the scopes that **Composio** supports for Snowflake. You should add these scopes based on your integration requirements:

```plain text
data.read,data.write,schemas.read,schemas.write,tables.read,tables.write,views.read,views.write,roles.read,roles.write,users.read,users.write,warehouses.read,warehouses.write,databases.read,databases.write,shares.read,shares.write,streams.read,streams.write,pipes.read,pipes.write,functions.read,functions.write,procedures.read,procedures.write,resources.read,resources.write,monitoring.read,monitoring.write,security.read,security.write,session.read,session.write,account.read,account.write
```

These scopes define the permissions your app can request during the OAuth authorization process. It’s essential to select only the scopes necessary for your application's functionality, adhering to the principle of least privilege.

**Note:** The actual scopes you should request depend on your specific integration requirements.

For example, if your app needs to read and write data, you would include `data.read` and `data.write` in your OAuth configuration.

You can customize these scopes according to your integration needs.

---

## Base URL for Snowflake

When using this auth config, API requests go through your Snowflake account URL:

```plain text
https://<your_account>.snowflakecomputing.com
```

Replace `<your_account>` with your actual Snowflake account name (you can find it in the login URL).

- If you don’t have one, you can sign up for a free Snowflake trial.

- `OAUTH_CLIENT_ID`

- `OAUTH_CLIENT_SECRET`

If you don’t find the desired client secret, run the following command:
