Neovim to cursor

## Demo (avante.nvim + mcphub.nvim in action)

Now that the setup is done, we can install any MCP servers we want from MCP Hub and start using them with Avante.

Run the following command to open MCP Hub and view the marketplace:

```bash
:MCPHub
```

That’s pretty much it. You can now easily access all the local MCP servers available in the marketplace by following the instructions there.

That said, to run remote MCP servers, which in our case is Composio’s MCP server, we need to use the MCP URL.

Head over to [connect.composio.dev](https://connect.composio.dev/). If you do not already have an account, sign up first.

Next, we need to get the MCP URL and the API key. Scroll down a bit, and you should see both the URL and the API key, which is labeled `X-CONSUMER-API-KEY`. Copy both, as we will need them in the next step.

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/neovim-to-cursor/images/1.png)

Like any other MCP client, MCP Hub uses a `servers.json` file in your config directory. In my case, it is located at `/home/shricodev/.config/mcphub/servers.json`, and it holds all the server configurations.

Before moving further, export the API key you just copied from Composio:

> 💁 You can also paste the key directly into the config file, but that is not recommended since you might accidentally leak it, for example on GitHub.

```bash
export COMPOSIO_CONSUMER_KEY=<your_api_key>
```

I have added a few local servers, Git and Time, along with the Composio server. Here is what the `servers.json` file looks like:

```json
// 👇 ~/.config/mcphub/servers.json

{
  "mcpServers": {
    "composio": {
      "url": "<https://connect.composio.dev/mcp>",
      "headers": {
        "x-consumer-api-key": "${env:COMPOSIO_CONSUMER_KEY}"
      }
    },
    "time": {
      "args": ["run", "-i", "--rm", "mcp/time"],
      "command": "docker"
    },
    "git": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "--mount",
        "type=bind,src=/home/shricodev/codes,dst=/personal",
        "mcp/git"
      ]
    }
  }
}
```

As soon as you save the changes to the `servers.json` file, you should see Docker containers spin up automatically for the local servers through MCP Hub:

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/neovim-to-cursor/images/2.png)

If you are not using any remote MCP servers through Composio, that is pretty much it. You can start using everything right away.

But if you want to use MCP servers from Composio and you have configured the Composio part properly, you should see a list of all the available tools in the MCP Hub UI.

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/neovim-to-cursor/images/3.png)

Now, there is one small step left, which is connecting the tools you plan to use. In my case, that is Slack. In the sidebar, click **Connect Apps**, search for Slack, and connect it.

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/neovim-to-cursor/images/4.png)

Once connected, the application should appear as connected. This is basically your one place to plug in all the tools you want to use.

Just like that, you can access those tools directly from Neovim. You are not limited to a fixed number of applications, so connect as many as you need.

Here is a quick demo of me using these together with Slack:

[Video](https://youtu.be/Jwut-Uk0v1k)

Here's the local MCP server with Git demo:

[Video](https://youtu.be/_7q-TQh3bdM?si=QPUuup7qU-xgY2Nl)
