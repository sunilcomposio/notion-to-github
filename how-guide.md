With the recent release of ChatGPT apps, especially the ChatGPT Apps SDK, developers can now build apps that run directly inside ChatGPT. 🤯

Currently, by default, OpenAI supports just the following apps: Booking.com, Canva, Coursera, Expedia, Figma, Spotify, and Zillow.

In the coming days, they plan to support developer-submitted apps by opening submissions later this year. This is a big moment for everyone who uses ChatGPT, especially developers who can build their own apps, and for the over 800 million ChatGPT users who get to try them.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_1.png)

So, you must know how to build your own apps with the ChatGPT Apps SDK. How cool would it be to create an app that supports over 500 apps and isn't limited to the default OpenAI-supported apps?

That's precisely what we'll cover, from understanding the SDK and running the project locally to temporarily hosting it on Ngrok to access it in ChatGPT.

So, without further ado, let's dive right in.

---

## TL;DR

To quickly summarize what we'll cover in this blog post, here's what we'll go through:

- What are ChatGPT apps and the Apps SDK?

- Installing Ngrok to host your localhost project on the internet with one command.

- How to use Rube to access OpenAI ChatGPT Apps.

- How to implement widgets in Next.js with ChatGPT Apps SDK + Rube MCP.

If you work with Next.js and want to learn how to build custom widgets for your ChatGPT Apps, this is the right place to start.

---

## What is ChatGPT Apps and the Apps SDK

ChatGPT Apps are third-party tools that you can run directly in your ChatGPT conversations. The best way to understand this is to think of it as a way to use these apps like Figma, Canva, Zillow, Spotify, and more, and do all sorts of work directly in them without ever leaving ChatGPT.

To give you a better overview, these are some things you can do with ChatGPT Apps:

- Say "Spotify, create me a playlist for my study session," and it'll build you a nice playlist in Spotify and play it directly in your chat.

- Say "Canva, create me a thumbnail for my blog post with this text," and it'll build you a nice thumbnail for your blog post.

You're seeing where this is headed. This is OpenAI's third attempt to make ChatGPT not just a chatbot, but an all-in-one platform with all apps directly available in it, so you never have to leave ChatGPT.

The first two were custom GPTs, but they didn't work well with the users. However, this is a powerful, more flexible approach that, if adopted, could mean ChatGPT becomes your all-in-one platform.

Well, then what's **ChatGPT Apps SDK?**

As the name suggests, it's pretty obvious; it's used to build apps that run inside ChatGPT. 🫠

To add more context, it's an open-source toolkit from OpenAI built on top of MCP that lets developers create apps that run directly in ChatGPT.

At a high level, this is how it works:

- Your app (e.g., Canva, Figma, etc.) runs as an MCP server that exposes its tools and input/output schemas directly to ChatGPT.

- Now, ChatGPT can invoke those tools and, optionally, render the UI components you provide in a sandbox.

Check out this video to get a high-level overview of ChatGPT Apps. 👇

---

## Building a ChatGPT App in Next.js with Widgets

The source code mainly consists of basic React code, so I won't explain it from scratch here. It's a GPT app designed to display Google Calendar meeting details with widget support.

This should be a good starting point for you. Feel free to build something similar for your use case with any apps you prefer.

Begin by cloning the project repository using the command below:

```bash
git clone https://github.com/shricodev/chatgpt-apps-sdk-demo-composio.git
```

### Step-1: Set Up Composio

> ℹ️ We'll use Composio to add integrations support to our application. You can choose any integration you like.

Before moving forward, you need to obtain a [Composio API key](https://platform.composio.dev/).

Go ahead and create an account on Composio, get your API key, and paste it into the `.env` file in the root of the project.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_3.png)

```plain text
COMPOSIO_API_KEY=<YOUR_COMPOSIO_API_KEY>
```

For this demo, I'll be using Google Calendar, so head over to the Composio dashboard to access the auth config ID for Google Calendar.

1. Create the auth config for Google Calendar.

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_4.png)

1. Create an auth config for Google Calendar. Note the external user ID as we will use it in the `.env` file.

Once done, copy the auth config ID (which starts with `ac_`). Now, add the auth config ID and the user ID to the `.env` file.

```plain text
COMPOSIO_USER_ID=<YOUR_COMPOSIO_USER_ID>
CALENDAR_AUTH_CONFIG_ID=<YOUR_COMPOSIO_CALENDAR_AUTH_CONFIG_ID>

```

Your final `.env` file should look something like this:

```plain text
COMPOSIO_API_KEY=<YOUR_COMPOSIO_API_KEY>
COMPOSIO_USER_ID=<YOUR_COMPOSIO_USER_ID>

CALENDAR_AUTH_CONFIG_ID=<YOUR_COMPOSIO_CALENDAR_AUTH_CONFIG_ID>

```

### Install Dependencies and Start the Project

Once your environment variables are configured, install the project dependencies:

```bash
pnpm install
```

After installation is complete, start the project with:

```bash
pnpm run dev
```

By default, the app runs on port `3000`. Double-check that it’s running on that port, as you’ll need this information later when setting up Ngrok.

### Step-2: Expose Your App with Ngrok

Since ChatGPT isn't running locally on your machine, you can't use [`localhost:3000`](http://localhost:3000/) when connecting to the app in ChatGPT.

You can host it on platforms like Vercel, but Ngrok is often faster and more convenient for development.

Ngrok lets you share your local project via a temporary public URL, which is perfect for quick testing without redeploying.

Even if you need to make changes, you won't have to push the changes to your repo to trigger the Vercel deployment. Ngrok can work directly from your local filesystem.

1. **Install Ngrok**

Make sure that you have [Ngrok](https://ngrok.com/) installed on your machine.

Visit this URL: [Ngrok Installation Guide](https://ngrok.com/), and find the relevant steps for your machine.

If you're someone like me who prefers Docker, you can use the following command to pull the Ngrok public image from Docker Hub.

```bash
docker pull ngrok/ngrok
```

Use whichever installation method fits your workflow.

1. **Start Ngrok**

Make sure that the project is running locally and listening on port 3000, as we will be exposing it to the internet.

It's as simple as typing out this command:

```bash
ngrok http 3000 // Change to the port your app is running on

```

And if you've followed the Docker steps, run the following command:

```bash
docker run --net=host -it -e NGROK_AUTHTOKEN=<NGROK_AUTH_TOKEN> ngrok/ngrok:latest http 3000 // Change to the port your app is running on

```

This will give you a public URL to access the project. Keep a note of this as we'll need it when setting up ChatGPT to add our new app.

Make sure to replace `<NGROK_AUTH_TOKEN>` with your actual ngrok auth token.

You can find it in your Ngrok dashboard. Log in to your [Ngrok](https://dashboard.ngrok.com/) account, and you'll find it in the dashboard.

### Step 3: Connect Your App to ChatGPT

Now, you should have a public URL for your app. Let's add it to ChatGPT.

It should look something like this: [`https://topographical-unmagnifying-halle.ngrok-free.dev`](https://topographical-unmagnifying-halle.ngrok-free.dev/)

Head over to ChatGPT settings, then under **Additional Settings>** **Apps and Connectors**, make sure **Developer mode is turned on**. This gives you access to add your own application.

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_5.png)

Click **Create**, and fill in the following details:

- MCP Server URL: The public URL you just got from Ngrok. Add `/mcp` at the end, because that's the endpoint in our code that handles the requests from ChatGPT.

- For Name and Description, you can put anything you want (show your creativity!)

- Authentication: Select **No Authentication**, as we'll handle authentication with Composio itself. Our app does not support OAuth by default.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_6.png)

### See It in Action

Let's see how all of this adds up. Here's a quick demo to give you an idea of our application.

Creating a Google Calendar meeting and viewing the details directly from GPT with widgets? That's really cool.

## Advanced: How to use Rube MCP with the ChatGPT Apps SDK?

You know what? You don’t need to worry about any of that if you don’t care about visual feedback or fancy widgets in ChatGPT. This is the right place to start.

> 💡 We only had to code all of that because I wanted to show you how to use Widgets with ChatGPT Apps + Rube MCP inside Next.js.

None of the coding is necessary. The whole point of ChatGPT apps is to give you direct access to your applications right inside ChatGPT, so if you want all your apps ready to go without extra setup, this is perfect.

Honestly, this is precisely what I’d recommend for your daily workflow. It’s simple, smooth, and gives you access to over 500 apps available on Rube. Pretty cool, right?

Here’s all you need to do:

1. Make sure that you are in Developer Mode as we discussed earlier.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_7.png)

1. In the same tab, click **Create**, and fill in the following details:

- MCP Server URL: [https://rube.app/mcp](https://rube.app/mcp)

- For Name and Description, you can put anything you want (show your creativity!)

Make sure you set up OAuth authentication, as we'll be using it to access the tools in ChatGPT.

![Image 7](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_8.png)

Now, click on **Create**.

1. It will now ask for some access; hit **Allow**.

![Image 8](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_9.png)

If everything goes well, your app should be available on ChatGPT. Make sure that you see all six actions appropriately listed, as these are the core actions Rube uses to manage authentication, prepare tool calls, and stuff.

![Image 9](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_10.png)

Here's all 6 Rube actions listed:

1. `RUBE_CREATE_PLAN`: Creates a complete step-by-step plan for LLMs.

1. `RUBE_MULTI_EXECUTE_TOOL`: Fast and parallel tool executor for tools discovered through `RUBE_SEARCH_TOOLS`.

1. `RUBE_REMOTE_BASH_TOOL`: Execute bash commands in a REMOTE sandbox for file operations, data processing, and system tasks.

1. `RUBE_REMOTE_WORKBENCH`: Process remote files or script bulk tool executions using Python code in a remote sandbox.

1. `RUBE_SEARCH_TOOLS`: Search for tools to execute the user task.

1. `RUBE_MANAGE_CONNECTIONS`: Manage connection with Rube.

This is to give you an idea of all the actions Rube will use to perform your tasks.

And... that's it.

Now, simply head over to your chat, use `@<APP_NAME>`, in my case `@Rube MCP Server`, and start talking.

As an example, try saying this:

> "@<APP_NAME> send an email to my ex at XYZ. Subject: Guess who. Body: Don’t worry, it’s just AI with Rube… or is it?"

(C’mon, try it, you’ll test two **connections** at once. 😉 Just kidding…)

---

## Wrapping Up

Alright, now you’ve got a clear idea of how the ChatGPT Apps SDK works, how to set up a custom server powered by Rube, host it with Ngrok, and use it right inside ChatGPT or plug in Rube and get going without any coding at all if you don't care about widgets.

In this post, we walked through everything from building your own custom server with Rube and widget support in Next.js to skipping the code entirely if you want to move fast.

Well, that's all for this! I will see you in the next one.
