# How Gong OAuth Works: Why It Requires a Technical Admin and How to Share Access

[Gong OAuth](https://help.gong.io/v1/docs/create-an-app-for-gong) controls how external applications access data in a Gong account. An application can use it to read calls, transcripts, users, CRM records, and other company data.

Gong handles OAuth at the organization level. One authorization can give an application access to data from many users and teams. Because this access can include sensitive company data, Gong only lets a [Technical Admin](https://help.gong.io/docs/find-your-technical-or-business-admin) approve it.

This requirement can surprise users. You can sign in to Gong and use it every day, but you can still see an **Insufficient permissions** message during OAuth. You can also see this message if you are a Business Admin. Gong requires the Technical Admin role for API and integration access.

After a Technical Admin approves the connection, Gong provides access and refresh tokens. The application uses these tokens to call the [Gong API](https://help.gong.io/apidocs/introduction-2). The approved OAuth scopes control which data and actions the application can access.

[Composio connected accounts](https://docs.composio.dev/docs/auth-configuration/connected-accounts) can manage this connection after the admin approves it. Composio stores and refreshes the tokens and connects them to a user ID in your application. You can keep the connection private or make it available to selected users and agents through [shared connections](https://docs.composio.dev/docs/shared-connections).

In this article, we will explain how Gong OAuth works, why it requires a Technical Admin, and how you can share the approved connection safely with Composio.

This guide is primarily for developers building internal applications or AI agents that need to provide several teammates with controlled access to one organization’s Gong connection. Multi-tenant SaaS applications can use the same pattern, but they must create and isolate a separate connected account for each customer.

## How the Gong OAuth Flow Works

Gong uses the OAuth 2.0 authorization code flow:

1. **Your application requests access.** It redirects a Gong Technical Admin to Gong with the required OAuth scopes.
2. **The Technical Admin approves the connection.** Gong displays the requested permissions and returns a temporary authorization code after approval.
3. **The application exchanges the code for credentials.** Gong returns an access token, refresh token, and customer-specific API base URL.
4. **The application calls the Gong API.** The access token authorizes API requests, while the refresh token keeps the connection active after the access token expires.

`Application → Gong approval → Authorization code → Access and refresh tokens → Gong API`

> 💡 When Composio manages the connected account, it handles the token exchange, secure credential storage, and token refresh. Your application and AI model do not need to handle Gong tokens directly.

## Why Gong OAuth Requires a Technical Admin

Gong does not support user-level OAuth. Instead, [authentication happens once at a global level](https://help.gong.io/docs/create-an-app-for-gong) for the customer’s Gong instance. This means an OAuth connection is not limited to the account of the person who approves it in the same way as a typical per-user integration.

The application requests a set of OAuth scopes during authorization. These scopes can allow it to retrieve calls, transcripts, participants, team-member information, activity statistics, CRM data, and other Gong resources. The scopes restrict which APIs the application can use, but approving them still creates a connection to company-level data.

Gong reserves this authorization for Technical Admins because they are responsible for system integrations, permission profiles, and compliance with the organization’s security requirements. The OAuth flow therefore asks the person approving the connection to sign in with Technical Admin credentials.

### Technical Admin vs. Business Admin

Gong treats technical and business administration as separate responsibilities:

| Role | Primary responsibilities | Can authorize Gong OAuth? |
| --- | --- | --- |
| Regular user | Uses Gong features allowed by their seat and permissions | No |
| Business Admin | Configures business features such as trackers, deal boards, and revenue analytics | No, unless also a Technical Admin |
| Technical Admin | Manages system integrations, permissions, seats, and security configuration | Yes |

Being able to use Gong—or even administer its business features—does not grant permission to approve an API integration. If the person starting the OAuth flow is not a Technical Admin, an existing Technical Admin must complete the authorization.

> 🔐 Composio does not bypass Gong’s admin requirement. A Gong Technical Admin must approve the initial OAuth connection before Composio can manage or share it.

## How to Share the Gong Connection With Composio

Gong requires a Technical Admin to authorize OAuth, but that does not mean every person using your application must repeat the admin-only flow. You can authorize one Gong connected account through Composio and make it available to explicitly approved application users and agent sessions.

Composio connections are **private by default**. Only the `userID` that owns a private connection can use it. A **shared connection** adds a per-connection access-control list, or ACL, that determines which other user IDs can access it.

> 🧪 Shared connections and their ACL fields are currently marked experimental in Composio’s documentation. Pin an SDK version and review the latest documentation before relying on the current API shape in production.

### 1. Create a Gong auth config in the Composio Dashboard

A Composio auth config is the reusable blueprint for the Gong OAuth flow. It defines the authentication method, credentials, and scopes, but it does not authorize a Gong account by itself. The Technical Admin provides that approval later through the Connect Link.

1. Open the [Composio Dashboard](https://dashboard.composio.dev) and go to Toolkits.
2. Search for **Gong**, and click **Add to Project** in the upper-right corner.
3. Give the auth config a recognizable name, such as **Gong – Internal Agents**, and click **Create**
4. Choose how the OAuth app will be managed:
	- Use **Composio Managed Auth** if its default scopes meet your needs.
	- Use **your own developer credentials** when you need your own Gong app, specific scopes, branding, or a dedicated production configuration.
5. If you use your own credentials, follow the [complete Gong OAuth setup guide](https://composio.dev/auth/gong) to register an OAuth integration in Gong. Copy the redirect URI displayed in your Composio Dashboard and register that exact value in Gong; do not construct the callback URL manually.
6. In Gong, select only the scopes your application needs. Enter the resulting client ID and client secret in the Composio auth-config form, and make sure the scopes in both systems match.

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-gong-oauth-works/image_1.png)

7. Copy the generated auth config ID, such as `ac_1234abcd`. You will pass this ID when creating the shared connection in the next step.

> 🔐 Creating the auth config does not grant access to Gong data. Access begins only after a Gong Technical Admin opens the Connect Link and approves the requested scopes.

For this shared-connection workflow, stop after copying the auth config ID. Do not create a connected account from the Dashboard—the SDK request below must create it as `SHARED`.

### 2. Create a shared Gong connection request

Create the connection as `SHARED` and define the application user IDs that should be allowed to use it. Shared connections are deny-by-default, so users who are not explicitly allowed cannot access the credentials.

```typescript
import { Composio } from "@composio/core";

const composio = new Composio({
  apiKey: process.env.COMPOSIO_API_KEY,
});

const gongAuthConfigId = "ac_your_gong_auth_config";

const connectionRequest = await composio.connectedAccounts.link(
  "gong_admin",
  gongAuthConfigId,
  {
    experimental: {
      accountType: "SHARED",
      aclConfigForShared: {
        allowedUserIds: ["user_alice", "user_bob"],
      },
    },
  },
);

console.log(connectionRequest.redirectUrl);
```

Here, `gong_admin` is the stable user ID that owns the connected account in your application. Replace `ac_your_gong_auth_config` with the auth config ID copied from the Composio Dashboard.

### 3. Have a Gong Technical Admin authorize the connection

Send `connectionRequest.redirectUrl` to a Gong Technical Admin. This is the Connect Link for the shared connection request created in the previous step.

1. The Gong Technical Admin opens the Connect Link and starts the account connection.
2. If prompted for an API base URL, enter the Gong API endpoint. For most Gong accounts, this is `https://api.gong.io`. Do not enter the Gong web-app URL shown in the browser. If your organization uses a custom API endpoint, confirm it with the Gong Technical Admin.

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-gong-oauth-works/image_2.png)

3. The admin reviews the requested scopes and approves the Gong consent screen.
4. Composio stores the OAuth credentials and marks the connected account as active.

Wait for the connection to complete and capture its connected-account ID:

```typescript
const connectedAccount =
  await connectionRequest.waitForConnection();

console.log(connectedAccount.id);
```

Save the returned connected-account ID. You will use it to make this specific Gong connection available in approved sessions.

### 4. Pin the shared connection into an authorized session

A shared connection is never selected implicitly. Your application must explicitly pin its ID into the session of an allowed user.

```typescript
const session = await composio.create("user_alice", {
  connectedAccounts: {
    gong: [connectedAccount.id],
  },
});

const tools = await session.tools();
```

The session can now use Gong tools through the Technical Admin-approved connection. The Gong access and refresh tokens do not need to pass through your application or the AI model.

### 5. Update or revoke access when needed

You can update the ACL as your team changes. For example, add only the users who require Gong access:

```typescript
await composio.connectedAccounts.updateAcl(
  connectedAccount.id,
  {
    allowedUserIds: ["user_alice", "user_charlie"],
  },
);
```

Removing a user ID from the allow list prevents that user’s future sessions from accessing the shared connection. You can also disable, revoke, or delete the connected account if the integration should no longer have access to Gong.

### Shared connection access rules

- The connection creator can always use the connection.
- Other users are denied unless the ACL allows them.
- A deny-list entry overrides an allow-list entry.
- The shared connection must be explicitly pinned into each authorized session.
- A session can pin at most one shared connection for the Gong toolkit.
- Prefer an explicit allow list over granting access to every user in the project.

This creates a clean separation of responsibilities: the Gong Technical Admin approves organization-level access once, Composio manages the credentials, and your application determines which users and agent sessions may use the connection.

## Why You May See an “Insufficient Permissions” Error

If Gong rejects the authorization because of insufficient permissions, first check the role of the person completing the OAuth flow.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-gong-oauth-works/image_3.png)

The person must be a Technical Admin for the relevant Gong instance. A regular user cannot approve the connection, and the Business Admin role by itself is not sufficient.

### How to resolve it

1. **Confirm which Gong account is signed in.**

Make sure the browser session belongs to the intended customer and not another Gong account.

2. **Check for the Technical Admin role.**

In Gong, open **My settings** and find the **Technical administrators** section. Gong lists the people who can help with integration access.

3. **Send the authorization link to a Technical Admin.**

The existing admin can open the same OAuth link, review the requested scopes, and approve the connection. Every application user does not need to become a Technical Admin.

4. **Restart the flow if the wrong person opened it.**

Sign out of Gong or use a separate browser session, then open a new authorization link with the Technical Admin account.

5. **Verify the integration configuration if the admin still cannot connect.**

Confirm that the redirect URI exactly matches the URI registered with Gong and that the authorization request contains only scopes configured for the OAuth app.

> ⚠️ Do not grant the Technical Admin role broadly just to complete OAuth. Ask an existing Technical Admin to approve the organization-level connection, then control who can use it through your application and Composio.
