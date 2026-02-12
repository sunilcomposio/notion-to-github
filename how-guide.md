# How to create OAuth2 credentials for Workday

In this guide, I'll walk you through setting up OAuth2 credentials for Workday and configuring the authentication in Composio. So, let's begin.

## Setting up Workday

In this section, we'll go through the process of registering an API client in Workday and generating the Client ID, and Client Secret needed for OAuth2 authentication.

> **NOTE:** Workday recommends an Integration System User (ISU) with the right domain permissions to use API clients. If your Workday admin hasn't set this up yet, see the Additional: ISU & Security Group Setup section at the bottom of this guide.

### Step 1: Register an API Client

1. Search for **Register API Client** in Workday and select the task.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_1.png)

1. Enter a **Client Name** (e.g., `Composio-Workday`), Select the **Non-Expiring Refresh Tokens** option, Add the below redirect URI if you are using our cloud.

```javascript
https://backend.composio.dev/api/v1/auth-apps/add
```

  Also, add the required scopes. At minimum, include the `Integration` scope and add any additional scopes based on what your integration needs. Click **OK** to generate the **Client ID** and **Client Secret**.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_2.png)

1. You will be redirected to the below page where you can find the Client ID and Client Secret along with REST API, Token and Authorization endpoints.

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_3.png)

> ⚠️ **Important:** Save the Client Secret immediately — you won't be able to see it again after leaving this page.

That's all you need from the Workday portal.

## Creating the Auth Config in Composio

With your OAuth credentials ready, navigate to the [Composio dashboard](https://platform.composio.dev/) to configure the authentication settings for Workday.

1. Click on the **Create Auth Config** button to get a list of all the toolkits available.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_4.png)

1. In the sidebar that opens, choose **Workday** for the toolkit. Stick with all the default settings for now

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_5.png)

1. Add your Client ID and Secret.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_6.png)

1. Then, click **Create Workday Auth Config**.

1. You can also update the existing configuration in the Manage Auth Config tab

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_7.png)

Once done, copy the auth config ID (which starts with `ac_`) and use it in your application code via a secret manager. Your Workday auth config is now ready to go! 

## Additional: ISU & Security Group Setup

> Most Workday tenants will have this configured by an admin. If you're unsure, check with your Workday admin before proceeding.

### Create an Integration System User (ISU)

An ISU is a dedicated service account for integrations — it keeps API operations separate from regular user accounts.

1. Type **Create Integration System User** into Workday's search bar and select the task.

1. Enter a **username** and set a **password**.

1. Set **Session Timeout Minutes** to `0` to prevent the ISU from timing out.

1. Select the **Do Not Allow UI Sessions** checkbox to restrict UI logins.

1. Navigate to the **Maintain Password Rules** task and add the ISU to the **System Users exempt from password expiration** field.

### Create a Security Group

1. Search for **Create Security Group** in Workday and select the task.

1. Select a security group type:

  - **Integration System Security Group (Unconstrained)** — access to all data instances.

  - **Integration System Security Group (Constrained)** — access scoped by context.

1. Add your ISU as a member of the security group.

1. Click **Done**.

### Grant Domain Permissions

1. Search for **Maintain Permissions for Security Group** and select the task.

1. Choose your security group and go to the **Domain Security Policy Permissions** tab.

1. Ensure the security group has **GET permissions** for:

  - `Integration Build`

  - `Integration Process`

  - `Integration Debug`

  - `Integration Event`

  - `Worker Data: Current Staffing Information`

  - `Worker Data: Public Worker Reports`

1. Click **OK**, then **Done**.

1. Search for **Activate Pending Security Policy Changes**, enter a comment, and confirm.
