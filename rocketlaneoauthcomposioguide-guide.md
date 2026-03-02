In this guide, I will share the process for creating and configuring the auth config for Rocketlane. So, let's begin.

## Setting Up Rocketlane

In this section, we’ll go through the process of setting up Rocketlane and mainly creating a personal API key that you can use inside Composio.

### Step 1: Create a Rocketlane account

Visit [Rocketlane](https://www.rocketlane.com/login) and create a new account if you don't have one already.

Make sure that you choose a company mail, as it won't let you sign up with personal mails, e.g., ones ending with `@gmail.com`.

> **NOTE:** The API key that we will generate will be tied to this Rocketlane account.

### Step 2: Create a Personal API Key

Once you have a Rocketlane account, you can create a personal API key.

1. Navigate to your Rocketlane account.

1. In the bottom left corner, click on your profile icon and then click **Settings**. Scroll down to the **Advanced** section, where you can find the option to manage API keys. Click on the **Create API Key** button.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/rocketlaneoauthcomposioguide/image_1.png)

1. Give a nice name to your API key and click **Add**.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/rocketlaneoauthcomposioguide/image_2.png)

Once you do that, you will receive the API key. Make a note of this, as we will need it when setting up Composio.

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/rocketlaneoauthcomposioguide/image_3.png)

> 💁 **NOTE:** There isn't a way you can specify or limit scopes for a personal API key as you could with Stripe, GitHub, etc. Rocketlane does support role-based permissions (RBAC) at the account/project/customer level, so keys effectively get constrained by whatever the issuing user is allowed to access.

That's it. That’s all you need to set up on the Rocketlane side.

---

## Create the auth config in Composio

With your API key ready, navigate to the Composio dashboard for your project to configure the authentication settings for Rocketlane.

1. In the sidebar click on **create auth config**, choose Rocketlane from the toolkit.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/rocketlaneoauthcomposioguide/image_4.png)

1. Ensure the authentication is set to **API Key**. There is no support for **OAuth2** with RocketLane.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/rocketlaneoauthcomposioguide/image_5.png)

Now, click on **Create Auth Config**, and the auth config should be created successfully. This should get you to your new Auth Config page.

> ℹ️ Auth config is what defines how a toolkit authenticates across users (method + scopes + credentials/managed auth) so Composio can create and use connected accounts.

1. Once that's done, click **Connect Account**.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/rocketlaneoauthcomposioguide/image_6.png)

1. This should prompt you to add an external User ID, which is a unique way to identify each user. For me, I chose my gmail account; you can add yours or leave it to its default.

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/rocketlaneoauthcomposioguide/image_7.png)

1. Now, you will be prompted to enter your API key. Paste in your API key you just generated from Rocketlane.

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/rocketlaneoauthcomposioguide/image_8.png)

1. If everything goes well, you should see the connection status as **ACTIVE**, which verifies that Composio can successfully access your account.

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/rocketlaneoauthcomposioguide/image_9.png)

And with that, now Composio is all set up with Rocketlane.

If you want to limit the scopes from the Composio end, it's not possible as well. But, you can limit just the tools Composio has permission to run.

To get the tool names, head over to the **Tools & Trigger Types** and copy all the tools you want to limit.

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/rocketlaneoauthcomposioguide/image_10.png)

For example, if I want to only let Composio add an assignee to a task, I'll copy the **Add Assignee To Task** tool name and paste it in the **Manage Auth Config** tab and click **Save Auth Config**.

![Image 11](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/rocketlaneoauthcomposioguide/image_11.png)

Unless you specifically need to do it, don't touch it, as it could lead to unnecessary complexity.

Once done, copy the auth config ID (which starts with `ac_`) and use it in your application code via a secret manager.

Your custom Rocketlane auth config is now ready to go! 🚀

---

## Test Rocketlane Connection (Optional)

You can test Rocketlane in the playground, where you can test the connection and how the tools would work with an agent.

1. Navigate to your Rocketlane Auth Config.

1. Click on the **Playground**.

> **NOTE:** Since this is a completely new connection you will set up, you might have to repeat some earlier steps. Make sure you follow them properly, as above.

1. Once everything is done, try asking some related prompts, and if you did everything correctly, Composio should be able to access the details.

![Image 12](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/rocketlaneoauthcomposioguide/image_12.png)

And that's it! You're all set to go with Rocketlane! 🎉
