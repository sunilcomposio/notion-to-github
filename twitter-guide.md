## Setting Up Twitter/X

## Step 1: Create an X developer account

Go to [https://console.x.com/](https://console.x.com/) and log in to the Twitter/X account you want to associate with the developer app.

### Step 2: Create an App

Once your developer account is ready, you can create an app and generate the OAuth Client ID and Client Secret.

1. Click Apps in the left panel

1. Again, click on Create App

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/twitter/image_1.png)

1. Fill in the forms with your app details and the new client application

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/twitter/image_2.png)

1. Copy the credentials shown on the screen and keep them in a secure place

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/twitter/image_3.png)

### Step 3: Create Oauth 2.0 Client ID and secret

1. Now, refresh the page and click on the app you just created.

1. Click User authentication settings in the bottom-right corner.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/twitter/image_4.png)

1. Select the type of app for Oauth 2.0 Authentication.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/twitter/image_5.png)

1. Add Composio callback URL

```shell
https://backend.composio.dev/api/v1/auth-apps/add
```

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/twitter/image_6.png)

1. Click save changes and store the client ID and secret

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/twitter/image_7.png)

# Creating the Auth Config in Composio

With your OAuth credentials ready, navigate to the Composio dashboard for your project to configure Facebook authentication settings.

1. Click the **Create Auth Config** button to view a list of all available toolkits.

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/twitter/image_8.png)

In the sidebar, type Twitter. The authentication is already set to OAuth2 with prefilled scopes. You can remove scopes you don’t need.

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/twitter/image_9.png)

Scroll down and toggle to use your own developer credentials.

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/twitter/image_10.png)

Fill in with the Client ID, Client Secret from the last step, and the Bearer token from the first step and create the Twitter Auth Config.

Once you have set up the auth config for Twitter, you can go to the **Manage Auth Config** tab to make changes to any auth config fields. Here, you can also customise the auth scopes if needed. Default scopes are already pre-filled for most apps.

![Image 11](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/twitter/image_11.png)

Once done, copy the auth config ID (which starts with `ac_`) and use it in your application code via a secret manager.

You are all set with the new custom Twitter/X integration.
