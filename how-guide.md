This guide shows how to connect Composio with OpenClaw to unlock access to 860+ tools beyond the default set and some practical use cases of it.

The focus is simple: expand capabilities while keeping authentication secure and data under your control. No fluff, no complex infrastructure - just plug in, connect your tools, and start building things that actually matter.

So let's get started with the setup!

---

## Adding Composio MCP to OpenClaw

Assuming you have open claw installed already, there are 3 ways to enable composio in open claw. I have listed all of them, though I prefer the 1st one the most!

In case you don’t have openclaw install you check out Docker Install Section of [Install OpenClaw](https://composio.dev/blog/how-to-setup-openclaw-and-use-saas-apps)

Get it done and then follow along.

### 1. Using direct prompt with your API Key

Before setting up openclaw, we need to configure the mcp server 

For configuring mcp server:

- **Create an account: **Go to [https://dashboard.composio.dev/](https://dashboard.composio.dev/) and sign up / login

- **Set up MCP: **Head to the **Connect to OpenClaw** and copy the prompt

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_1.png)

  - Head to OpenClaw and paste the prompt:

```markdown
Add a new MCP server called "composio" with transport type HTTP. Use the URL https://connect.composio.dev/mcp and add the header "x-consumer-api-key: your-api-key".
```

If first time login and authenticate and you are done!

### **2. Using Composio **`**npm**`** Plugin**

Composio recently allowed support for open claw plugin, i.e. no headache of configuration, simple one liner command set it all!

So, head to terminal and type

```c#
openclaw plugins install @composio/openclaw-plugin
```

Once done, setup your api key, following the steps:

- Log in at [**dashboard.composio.dev**](https://dashboard.composio.dev/)**.**

- Choose your preferred client (OpenClaw, Claude Code, Cursor, etc.).

- Copy your consumer key (`ck_...`).

In terminal run:

```c#
openclaw config set plugins.entries.composio.config.consumerKey "ck_your_key_here"
```

Finally restart the gateway:

```c#
openclaw gateway restart
```

All this does is setup the open claw configuration with consumer key in the Composio MCP.

Or alternatively, you can directly setup the mcp server , for more granular control.

### **3. Using MCP Porter**

As a secondary measure, you can activate mcp support through [*mcp_porter*](https://skillsmp.com/skills/openclaw-openclaw-skills-mcporter-skill-md)[ ](https://skillsmp.com/skills/openclaw-openclaw-skills-mcporter-skill-md) and then add composio. This keeps things organized and simple.

→ Head to Skills section, and search MCP Portal and click install.

→ Now go to terminal and restart the containers (openclaw & socat-proxy) using:

```c#
 cd /home/devloper_hs/openclaw && docker compose restart openclaw-gateway 
```

→ Then again go to skills and in MCP Porter, refresh the page. make sure it shows eligible

→ Now go to home folder where opencalw is installed: `/home/username/.openclaw/workspace/config`

→ Open the `mcporter.json` in any ide and paste the following & save: 

```c#
{
  "mcpServers": {
    "composio": {
      "baseUrl": "url_from",
      "headers": {
        "x-api-key": "api_key"
      }
    }
  },
  "imports": []
}

```

> The **url** and **api **key are the one from the pre-require step.

or if you want a stdio server setup you can use (might cause issue):

```c#
npm install -g composio-mcp
```

```c#
{
  "mcpServers": {
    "composio": {
      "command": "composio-mcp",
      "args": ["--api-key", "api_key"],
      "transport": "stdio"
    }
  }
}
```

```c#
# usage
npx mcporter call --stdio "npx @composio/mcp@latest setup https://backend.composio.dev/tool_router/<api-key>/mcp" <tool_name> <arg1>=<value1>
```

Now to make sure agent pick up the mcp servers perfectly let’s top the setup with skills file!

```c#
npx skills add https://github.com/composiohq/skills --skill composio --yes
```

Now we are all setup to experience openclaw seamlessly.

💡Fact: You can do all the steps in section through prompt in openclaw, but as it stores logs that ate accessible to other, it causes security concern, so cli approach is better. 

---

## Using OpenClaw 

Head back to the the dashboard and go to chat.

The nicest thing about clawd bot is you can chat with it to get things done, rather than prompting, and it understand you close to human. 

However, for this test, I just want it to fetch me hackathon emails, so here is my chat session!

> Note: It might hallucinate, as it's a personal project, however, take care of security as these chats are visible to anyone if they know your token.

You can even try it use with telegram. Steps are simple:

- Head to the telegram `BotFather`  channel and select your bot

- Click start and copy the pairing code

- Head to the terminal and paste:

```c#
cd /home/devloper_hs/openclaw && docker compose run --rm openclaw-cli pairing approve telegram pairing-code
```

Replace the pairing-code with one you copied.

In few seconds you will see an output like:

```c#
Container openclaw-openclaw-cli-run-58c012ca9af1 Creating
Container openclaw-openclaw-cli-run-58c012ca9af1 Created

🦞 OpenClaw 2026.2.17 (4134875) — Your AI assistant, now without the $3,499 headset.

Approved telegram sender [bot_id].
```

Now you can easily interact with openclaw, like I am doing!

I even tried using it to get some trendy tweets:

**Prompt**

```markdown
Use OpenClaw as an autonomous agent connected through Composio tools. 

Continuously monitor trending topics from Google Trends, X trending feeds, and other available trend sources, filter for high-engagement AI/tech/startup discussions, and extract concise context (keywords, sentiment, why it’s trending). 

Generate 2–4 short, opinion-neutral social posts per trend in a natural human tone optimized for Telegram readability, avoid hashtags spam, avoid misinformation, and deduplicate similar topics. 

Schedule execution every few hours, keep outputs brief (max ~4–5 lines each), and send the final approved posts directly to a specified Telegram chat using the Composio Telegram integration. 

Include lightweight safety checks, rate limits, and logging of sources used for each generated post.
```

**Output**

and even to create my own task-board (inspired by AlexFinn).

Prompt:

```markdown
Please build a task board for us that tracks all the tasks we are working on. I should be able to see the status of every task and who the task is assigned to, me or you. Movingg forward please we will put all tasks you and I work on into this board and update it in real time.
```

Output:

Best thing I like about it is, if done right, it can do things!

I hope you enjoy your days with open-claw securely using docker and have fun.

Make sure to stop the session once done using:

```c#
cd /home/devloper_hs/openclaw && docker compose down
```

With this let’s end this article with a key takeaway.

---

## Key Take Away!

Tools like open-claw seem amazing at first glance but can easily become a nightmare if not handled right - especially around security. 

The moment you hand broad permissions or API keys to an agent; you've opened a door you might forget, but it is even there. It doesn't know what's sensitive, it just acts. 

That’s where tool like [Composio ](https://composio.dev/)sidesteps this quietly by handling scoped access and managed credentials under the hood, so the agent does its job without holding the master keys to everything.

What other approach you find safeguards your privacy and sensitive data, do share in comments!
