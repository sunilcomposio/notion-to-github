In this guide, I will share the process for customizing the auth config for Supabase. So, let's begin.

## Setting Up Supabase

In this section, we’ll go through the process of setting up Supabase and adding a new application.

> 💁 **NOTE:** If you already have an application and access to the Client ID and Client Secret, you can skip this section.

### Step 1: Add a Supabase Application

Now, head over to your [Supabase Organization](https://supabase.com/dashboard/organizations) and go to the Organization Settings.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/supabase/image_1.png)

Now, click the **Add Application** button, name it appropriately, and enter your Website URL. For the **Authorization Callback URL**, use the following Composio’s callback URL.

```plain text
https://backend.composio.dev/api/v1/auth-apps/add
```

And with all the permissions that you require, click **Create**.

> **NOTE:** Granting Read and Write access to everything is not recommended for security reasons. However, for this demo, I will be using it. Ensure that you grant only the required scopes in your actual use case.

### Step 2: Copy the OAuth2 Credentials

You'll find the Client ID and Client Secret on the next page you are redirected to, in the **Organization Settings**.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/supabase/image_2.png)

Copy these as we will need them when setting up Composio. That's all the configuration you need to do on the Supabase side. It’s the simplest!

---

## Create the Auth Config in Composio

With your OAuth2 credentials ready, navigate to the Composio dashboard for your project to configure the authentication settings for Supabase.

1. Click on the **Create Auth Config** button to get a list of all the toolkits available.

In the sidebar that opens, choose Supabase for the toolkit. Ensure the authentication is set to **OAuth2**. There is also support for **API Key** with Supabase if you ever need it in the future. All the steps are the same. Just instead of Client ID and Secret, Composio will ask for the API key.

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/supabase/image_3.png)

1. Here we are interested in setting up our own custom developer credentials, so to do that, enable **Use your own developer credentials**.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/supabase/image_4.png)

Now, in the respective fields, paste the credentials you just copied from the Supabase Organization dashboard.

1. Click **Create Supabase Auth Config**, and the auth config should be created successfully.

> **NOTE:** By default, Composio will have **all** access.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/supabase/image_5.png)

If you want to limit the scopes from the Composio end, it's not possible. But, you can limit just the tools Composio has permission to run.

To get the tool names, head over to the **Tools & Trigger Types** and copy all the tools you want to limit.

For example, if I want to only let Composio create new project API keys, I'll use the “Copy the project API key**”** tool name and paste it in the **Manage Auth Config** tab.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/supabase/image_6.png)

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/supabase/image_7.png)

Unless you specifically need to do it, don't touch it, as it could lead to unnecessary complexity.

1. Once that's done, click **Connect Account** and Composio will ask for the Supabase Base API URL. Leave it at the default:

```plain text
https://api.supabase.com
```

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/supabase/image_8.png)

Click **Connect Account,** and you'll be presented with another screen where it will ask for your consent to connect Supabase with Composio.

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/supabase/image_9.png)

> **NOTE:** Make sure only the scopes you've given permission to are shown there. Since I've given it entire permission, it's asking for everything.

If there's a mismatch in scope permission, you might see a weird **Something went wrong** page.

Click **Authorize Composio with Supabase** and you should see a Connection Successful screen.

Once done, copy the auth config ID within Composio (which starts with `ac_`) and use it in your application code via a secret manager.

Your custom Supabase auth config is now ready to go! 🚀

---

## Test Supabase Connection (Optional)

Composio recently added support for Playground, where you can test the connection and how the tools would work with an agent.

1. Navigate to your Supabase Auth Config.

1. Click on the **Playground**.

1. Once you're there, try asking some related prompts, and if you did everything correctly, Composio should be able to access the details.

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/supabase/image_10.png)

And that's it! You're all set to go with Supabase! 🎉
