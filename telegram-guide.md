In this guide, we’ll walk through how to configure authentication for [**Telegram**](https://composio.dev/toolkits/telegram) using **Bot Token–based authentication** with Composio.

Telegram does **not** support OAuth.

All access is handled using a **Bot Token** generated via **BotFather**.

## Setting Up Telegram

In this section, we’ll create a Telegram bot and generate the API token required for Composio.

---

## Step 1: Create a Telegram Bot

1. Open Telegram and search for **@BotFather**

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/telegram/image_1.png)

1. Start a chat and type:

```plain text
/start
```

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/telegram/image_2.png)

1. Create a new bot:

```plain text
/newbot
```

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/telegram/image_3.png)

1. Enter:

  - **Bot name** (display name)

  - **Bot username** (must end with `bot`, e.g. `composio_test_bot`)

1. Once created, BotFather will return a **Bot Token**:

```plain text
1234567890:AAxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

**Important:**

Save this token securely; it grants full control over your bot.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/telegram/image_4.png)

## Telegram Bot Permissions

Telegram bots do not use scopes.

Instead:

- Permissions are implicitly granted via the **bot token**

- What the bot can do depends on:

  - Whether it’s added to a chat/group

  - Admin permissions (for groups/channels)

## Creating the Auth Config in Composio

Once you have the Telegram Bot Token, configure it in Composio.

## Step 2: Create a New Auth Config

1. Go to the **Composio Dashboard**

With your OAuth credentials ready, navigate to the Composio dashboard:

```plain text
https://platform.composio.dev/
```

1. Click **Create Auth Config**

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/telegram/image_5.png)

1. Select **Telegram **from the list of available toolkits

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/telegram/image_6.png)

1. Ensure authentication is set to **API KEY **

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/telegram/image_7.png)

## Step 3: Configure Authentication

1. Click **Connect Account**

1. Set **Authentication Type** to **Bearer Token**

1. Paste your **Telegram Bot Token**

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/telegram/image_8.png)

Telegram does **not require**:

- Client ID

- Client Secret

- Redirect URLs

- OAuth consent

---

## Scopes Supported by Composio for Telegram

Telegram does **not use OAuth scopes**.

- Access is controlled entirely by the bot token

- Composio forwards the token in API requests automatically

Example Authorization header:

```plain text
Authorization: Bearer <TELEGRAM_BOT_TOKEN>
```

## Base URL for Telegram API

All Telegram API requests are sent to:

```plain text
https://api.telegram.org
```

## How Authentication Works

When using Telegram with Composio:

- The bot token is stored securely in the **Auth Config**

- Composio injects the token in every API request

- No user login or OAuth flow is involved

- The bot acts as the authenticated entity

This makes Telegram a **very simple and reliable integration**.

---

## Test Telegram Connection (Optional)

You can verify the setup using **Composio Playground**.

1. Open your **Telegram Auth Config**

1. Navigate to **Playground**

1. Try a simple prompt like:

> **“Send a message to my Telegram bot”**

or

> **“Get recent updates from Telegram”**

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/telegram/image_9.png)

If the bot token is valid, Composio will successfully interact with Telegram.

## Final Step

Once everything is set up:

1. Copy the **Auth Config ID** (starts with `ac_`)

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/telegram/image_10.png)

1. Store it securely using a secret manager

1. Use it in your application code to authenticate Telegram via Composio

Your **Telegram integration** is now ready to use!
