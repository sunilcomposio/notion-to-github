Figma with MCP

## Set Up the Figma MCP Server with Claude Code

💁 We’re going to use Composio to add Figma MCP server support to Claude Code.

Head over to [connect.composio.dev](https://connect.composio.dev/). If you don’t already have an account, sign up first.

Once you’re in the dashboard, scroll down a bit, and you should see **Claude Code** as one of the installation options.

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/figma-with-mcp/images/1.png)

If you’re using something else like OpenAI Codex, OpenClaw, or want to use it with an MCP URL, that option is there too.

For now, I’m going with Claude Code because it’s been the most helpful coding agent for me so far.

At this point, you’ve got two ways to authenticate:

1. OAuth
1. API Key
### OAuth (Recommended)

OAuth is the recommended option because you don’t have to deal with API keys yourself. Composio handles the authentication under the hood.

To install Composio in Claude Code using OAuth, just run:

```bash
claude mcp add --scope user --transport http composio https://connect.composio.dev/mcp
```

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/figma-with-mcp/images/2.png)

As you can probably tell, all we’re doing here is adding the Composio MCP under user scope and letting it handle authentication through the app. This is the safest option and the one I’d recommend.

### API Key

If you want to use an API key instead, you can still do it in one step. Just run:

```bash
claude mcp add --scope user --transport http composio https://connect.composio.dev/mcp --header "x-consumer-api-key: <your_consumer_key>"

```

Make sure to change the placeholder `<your_consumer_key>` with your actual consumer key.

> **💡 Note:** The only difference here is that with the API key approach, you’re authenticating by sending a request header with every request to the Composio server. As you can imagine, that consumer key is your API key, so keep it safe and don’t let it get compromised.

By default, Claude Code saves this to `~/.claude.json`, but I personally don’t like saving it globally.

So in the project where you actually plan to use Claude Code, copy that file into a local `.mcp.json` instead.

This makes it much easier to keep MCP servers scoped per project, which is super helpful once you start adding more of them for different setups.

Run this to copy the config into your current directory:

```bash
cp ~/.claude.json .mcp.json
```

---

## Verify the Connection

Now that Composio has been added to Claude Code, it’s time to verify the connection, or authenticate if you went with OAuth.

Open Claude Code and type `/mcp`, then select **Composio** and follow the browser login flow to authorize access to your Composio account. This part only applies if you’re using OAuth.

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/figma-with-mcp/images/3.png)

Once that’s done, you can confirm everything is working by listing your MCP servers with:

```plain text
/mcp list
```

You should see Composio in the list.

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/figma-with-mcp/images/4.png)

---

## Connect Figma

Now that all the groundwork is done, the only thing left is to connect Figma.

In the sidebar, click **Connect Apps**, search for **Figma**, and connect it. As simple as pressing one single **Connect** button.

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/figma-with-mcp/images/5.png)

Once connected, Figma should show up as connected. This is basically your one place to plug in all the tools you want to use.

And just like that, you’re done.

You can now clone any Figma design, no matter how complex it is.


