In this guide, I will share the process for customizing the auth config for Jira. So, let's begin.

## Setting Up Jira

In this section, we’ll go through the process of setting up Jira and creating a developer account.

> 💁 **NOTE:** If you already have a developer account and access to the Client ID and Client Secret, you can skip this section.

### Step 1: Create an Atlassian Developer Account

Visit the [Atlassian developer portal](https://developer.atlassian.com/console) and create a new developer account.

A developer account is required because you’ll need to create a new app, which must be linked to a developer account in order to work with the Jira API.

> 💁 **NOTE:** If you don't plan to use your own custom developer credentials, you can skip this entire section.

### Step 2: Create an Atlassian Application

Now, head over to the [Atlassian Apps Console](https://developer.atlassian.com/console/myapps) and create an application of **OAuth 2.0 Integration** type.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_1.png)

In the next section, name your application as you want and click **Create**.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_2.png)

### Step 3: Configure Authorization with Callback URL

Next, go to the **Authorization** tab in the left sidebar and click **Configure**. This will allow us to set up the Callback URL, which, in our case, will be Composio's Callback URL.

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_3.png)

There, you'll see an input field to add Composio's Callback URL. Enter the following URL and click on 
”**save changes**”:

```plain text
https://backend.composio.dev/api/v1/auth-apps/add

```

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_4.png)

That's all the setup needed for the **Authorization** tab. Now, we just need to configure the application's scopes.

### Step 4: Configure Scopes

Configuring scopes in Jira can be a bit tricky. To identify the exact names of the scopes you'll be working with, I suggest this method to make it easier to select each one.

Here are all the default scopes Composio supports for jira:

```plain text
read:jira-work

write:jira-work

manage:jira-project

manage:jira-configuration

read:jira-user

manage:jira-webhook

manage:jira-data-provider

read:servicedesk-request

manage:servicedesk-customer

write:servicedesk-request

read:servicemanagement-insight-objects

offline_access

read:sprint:jira-software

write:sprint:jira-software

read:board-scope:jira-software

write:board-scope:jira-software

read:project:jira

read:issue-type-scheme:jira
```

> This is a lot to look at in one line. I suggest you check out the scopes properly in the **Manage Auth Config** tab once we set up the auth config for Jira inside Composio in the next section.

Figure out all the scopes that you'll require in your workflow and keep a note of them.

Now, head back to the [Atlassian Apps Console](https://developer.atlassian.com/console/myapps) and click on the **Permissions** tab in the left sidebar.


Then you will see a page where all the Atlassian APIs are listed, as shown in the image below. From there, click the **“Add”** button next to the **Jira API**, and then proceed to configure the required scopes.


![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_5.png)

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_6.png)

You'll mostly be configuring these scopes inside the **User Identity API** and the **Jira API**. Click configure and tick the Classic or the Granular scopes that you plan on using from above.

Check the ones you need and click **Save**.

### Step 5: Copy the OAuth Credentials

Finally, head over to the **Settings** tab, and just a little below, you'll find the Client ID and Client Secret in the **Authentication Details** section.

Copy these as we will need them when setting up Composio (with custom developer credentials).

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_7.png)

---

## Create the auth config in Composio

With your OAuth2 credentials ready, navigate to the Composio dashboard for your project to configure the authentication settings for Jira.

1. Click on the **Create Auth Config** button to get a list of all the toolkits available.

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_8.png)


In the sidebar that opens, choose **Jira** for the toolkit. Ensure that the authentication method is set to **OAuth2**, as **Jira** also supports another authentication method (API Key).

1. For **Jira**, Composio does not provide managed OAuth credentials. Therefore, you will need to use your **own Custom developer credentials** to configure the authentication.

Now, in the respective fields, paste in the credentials you just copied from the Atlassian dashboard.

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_9.png)

1. Click **Create Jira Auth Config** and the auth config should be created successfully.

1. Once that's done, click **Connect Account,** and it will prompt you for your Jira subdomain.

For this, visit your Jira profile, and the URL should look something like this:

```plain text
https://<subdomain>.atlassian.net

```

The subdomain is what you'll need to enter here.

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_10.png)

If you chose to use custom credentials but didn't specify the relevant scopes, you will encounter this **Something went wrong** error.

![Image 11](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_11.png)

If everything went well, Composio can access your **Jira **account and everything you've configured in the Jira scopes.

1. If you want to limit the scopes from the Composio end, you can do so by heading over to the **Manage Auth Config** tab and removing all the scopes that you don't need.

![Image 12](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_12.png)

Once done, copy the auth config ID (which starts with `ac_`) and use it in your application code via a secret manager.

Your custom Jira auth config is now ready to go! 🚀

---

## Test Jira Connection (Optional)

Composio recently added support for Playground, where you can test the connection and how the tools would work with an agent.

1. Navigate to your Jira Auth Config.

1. Click on the **Playground**.

1. Once you're there, try asking some related prompts, and if you did everything correctly, Composio should be able to access the details.

![Image 13](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_13.png)

And that's it! You're all set to go with Jira! 🎉
