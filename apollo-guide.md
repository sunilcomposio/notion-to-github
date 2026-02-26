This guide walks you through creating an Apollo API key and configuring it in **Composio**.

---

## Overview

- **Authentication type:** API Key

- **OAuth required:**  No

- **Scopes:** Not supported

- **Access control:** Apollo plan + user role

Apollo uses **API key–based authentication**. Permissions are enforced internally by Apollo and **cannot be modified by Composio**.

---

## Step 1: Generate an API Key in Apollo

```markdown
https://app.apollo.io/
```

1. Log in to your Apollo account.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_1.png)

1. Navigate to:

```plain text
Admin Settings → Integrations
```

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_2.png)

1. Click on **Integrations**

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_3.png)

1. Click on **API → API Keys**

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_4.png)

1. Click on **Create a new Key**

1. Give the key a name.

### Apollo API Reference & Available Endpoints

In the same **API** section, Apollo displays a list of available REST endpoints that your API key can access.

Commonly used endpoints include:

```plain text
POST /api/v1/contacts/search
POST /api/v1/people/search
POST /api/v1/organizations/search
POST /api/v1/accounts/search
GET  /api/v1/users
```

These endpoints allow you to:

- Search and filter **contacts**

- Search **people and organizations**

- Retrieve **account and user data**

- Power lead enrichment and prospecting workflows

> The visibility of these endpoints confirms that your API key is active and ready to use.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_5.png)

1. Copy the generated API key.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_6.png)

> **Important:** Save the API key securely. You may not be able to view it again.

---

## Step 2: Understand Apollo Permissions (Important)

Apollo **does not support OAuth scopes**.

Access is determined by:

- The **Apollo subscription plan**

- The **role of the user** who created the API key

Typical accessible resources include:

- People search

- Company search

- Contact enrichment

- Basic sequence data (plan-dependent)

> Composio forwards the API key exactly as provided. Apollo enforces all permission limits.

## Step 3: Create Apollo Auth Config in Composio

1. Go to Composio Dashboard:

```plain text
https://platform.composio.dev/
```

1. Click on the **Create Auth Config** button to get a list of all the toolkits available.

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_7.png)

1. Click **Create Auth Config**

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_8.png)

1. Select **Apollo** from the toolkit list

1. Make sure the **Authentication Type** is API Key

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_9.png)

1. Click on **Connect Account**

![Image 10](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_10.png)

1. Paste your **Apollo API Key**

![Image 11](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_11.png)

1. Click **Connect Account**

---

## Step 4: How Authentication Works

When using Apollo with Composio:

- The API key is stored securely

- Composio injects the key into every request

- Apollo validates and authorizes the request

- No OAuth flow or redirects are involved

---

## Step 5: Test Apollo Connection (Optional)

You can test the integration using **Composio Playground**.

### Sample test prompt:

> **“Search for companies in the fintech industry”**

![Image 12](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_12.png)

If configured correctly, Apollo data will be returned successfully.

---

## Base URL (For Reference)

All Apollo API requests are sent to:

```plain text
https://api.apollo.io
```

---

## Final Step

Once configured:

1. Copy the **Auth Config ID** (starts with `ac_`)

![Image 13](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/apollo/image_13.png)

1. Store it securely using a secret manager

1. Use it in your application to authenticate Apollo via Composio

---
