In this guide, I will share the process for customizing the auth config for Linear. So, let's begin.

## Setting Up Linear

In this section, we’ll go through the process of setting up Linear and adding a new application.

> 💁 **NOTE:** If you already have an application and access to the Client ID and Client Secret, you can skip this section.

### Step 1: Create a Linear Workspace

First and foremost, head over to Linear and [log in or sign up](https://linear.app/) if you don't have an account already.

Then Linear will ask you to create a workspace. Give it a nice name and details:

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/linear/image_1.png)

### Step 2: Create a New OAuth Application

Now, head over to your [Linear Developer Portal](https://linear.app/settings/api) and create a new OAuth application.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/linear/image_2.png)

Name it appropriately. For the Callback URL, configure it to point to Composio's callback URL, which is:

```plain text
https://backend.composio.dev/api/v1/auth-apps/add
```

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/linear/image_3.png)

And fill in the rest of the fields, such as providing your GitHub username and, optionally, enabling support for generating an OAuth access token if required.

### Step 3: Copy the OAuth2 Credentials

You'll find the Client ID and Client Secret on the next page you are redirected to, for the new application you just created:

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/linear/image_4.png)

Copy these as we will need them when setting up Composio. That's all the configuration you need to do on the Linear side.

---

## Create the auth config in Composio

With your OAuth2 credentials ready, navigate to the Composio dashboard for your project to configure the authentication settings for Linear.

1. Click on the **Create Auth Config** button to get a list of all the toolkits available.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/linear/image_5.png)

In the sidebar that opens, choose Linear for the toolkit. Ensure the authentication is set to **OAuth2**. There is also support for **API Key** with Linear if you ever need it in the future. All the steps are the same. Just instead of Client ID and Secret, Composio will ask for the API key.

1. Here we are interested in setting up our own custom developer credentials, so to do that, enable **Use your own developer credentials**.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/linear/image_6.png)

Now, in the respective fields, paste the credentials you just copied from your newly created Linear Application.

1. Click **Create Linear Auth Config**, and the auth config should be created successfully.

By default, Composio will have the following scopes:

```plain text
admin - Admin Access
read - Read Access
write - Write Access
issues:create - Create Issues
comments:create - Create Comments
```

If you want to limit the scopes, you can do so by heading over to the **Manage Auth Config** tab and removing all the scopes that you don't need.

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/linear/image_7.png)

1. Once that's done, click **Connect Account**, and Linear will ask for your consent to authorize the application. Click **Authorize**, and Composio will handle the rest for you.

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/linear/image_8.png)

If everything goes well, you should see a Connection Successful screen.

Once done, copy the auth config ID (which starts with `ac_`) and use it in your application code via a secret manager.

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/linear/image_9.png)

Your custom Linear auth config is now ready to go! 🚀

---

## Test Linear Connection (Optional)

Composio recently added support for Playground, where you can test the connection and see how the tools work with an agent.

1. Navigate to your Linear Auth Config.

1. Click on the **Playground**.

1. Once you're there, try asking some related prompts, and if you did everything correctly, Composio should be able to access the details.

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/linear/image_10.png)

And that's it! You're all set to go with Linear! 🎉
