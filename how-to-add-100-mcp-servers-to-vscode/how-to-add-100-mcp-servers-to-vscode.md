How to add 100+ MCP servers to VS Code in minutes (Updated)

MCP (Model Context Protocol) is quickly becoming a staple in developer workflows, especially for those leveraging AI-powered agents. 

If you want to supercharge your VS Code environment by connecting it to a wide array of MCP servers—enabling everything from seamless API automation to collaborative coding—this guide walks you through the process in a clear, step-by-step manner

## TLDR

The blog focuses on 5 core aspects to get you MCP ready!

1. What are MCP Servers & What not 
1. How MCP Server Works 
1. Step by step guide to connect VS Code to Custom + Hosted MCP Servers 
1. How to use MCP with Vs Code Agent Mode 
1. Practical Use Cases with examples
To orchestrate the workflow, I will use composio fully managed [MCP Servers](https://dub.composio.dev/9wAhI83) as it has 200+ tool integrations and built in authentication. 

Let’s dive in

## What Are MCP Servers & What Not

Model Context Protocol (MCP) is an open standard that lets AI models interact with external tools and services using a unified interface. 

MCP acts as a bridge, allowing AI agents to access a diverse set of tools—ranging from file management utilities to cloud APIs—directly from your editor without defining or maintaining separate servers.

![Credits - Gerg Isenberg (YouTube)](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/how-to-add-100-mcp-servers-to-vscode/images/1.png)

Think of MCP as a universal adapter for your AI agent. With MCP, your agent can:

→ Query databases.

→ Manipulate files.

→ Automate ticketing systems.

→ Trigger CI/CD pipelines.

→ Fetch data from APIs.

All at once without leaving your code editor or juggling through multiple apps.



![sample flow for understanding - Credits - Giphy (Composio)](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/how-to-add-100-mcp-servers-to-vscode/images/2.png)

However, be considerate, MCP servers are **not**:

- **A direct API replacement:** MCP servers often wrap APIs, but they provide a standardized interface rather than replacing native API endpoints.
- **Inherently complex:** Many servers can be set up with minimal configuration, and templates are widely available.
- **A database:** MCP servers serve as bridges, not as primary data stores.
& only download MCP Server from trusted sources!

## How MCP Server Works

Under the hood, MCP follows a client-server architecture. Here is what happens using an example (leaving intricate details):

**Scenario**: Adding task/ ticket - fix login bug in website on high priority on [Linear](https://linear.app/).

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/how-to-add-100-mcp-servers-to-vscode/images/3.png)

→ You give a prompt in VS Code, e.g : *Add “****Fix login bug****” to my high-priority tasks.*

→ The VS Code (MCP Client) requests LLM (Agent) to process this prompt.

→ Agent derives a plan of action and connects to two MCP servers:
• One for task management system (e.g., Linear, or Jira) 
• Another for version control or code context (e.g., GitHub) 

→ The Linear MCP server interacts with the Linear Account to find and update the “high priority” task list with a new entry: *“Fix login bug”*. 

If needed extra context.

→ The Agent may call GitHub MCP to inspect relevant commit history or open issues to link context or add metadata.

→ Both MCP’s Servers send their responses back to the MCP client.

→ VS Code (MCP client) aggregates the responses and forwards them to the LLM Agent, which displays the outcome in VS Code — e.g. a confirmation message, a summary of changes, or even suggestions based on linked issues or logs.

If you want to delve deep into the details about depth, lifecycle, protocol, drawbacks and all things MCP check out this [interesting ](https://composio.dev/blog/what-is-model-context-protocol-mcp-explained/)blog.

Assuming you understood the flow, now let’s look at how to connect MCP Servers to VS Code (locally & online)

---

## How to Connect VS Code to MCP Servers: Step by Step

There are many ways to connect vs code to MCP server, but let’s follow the simplest route.

I will use [**calculator_mcp_server.py**](https://gist.github.com/DevloperHS/be05cdbaee2a66e9b26ffeb1257c9e91#file-calculator_mcp_server-py) for local MCP connections However feel free to build one yourself. 

For reference you can check out: [**MCP server: A step-by-step guide to building from scratch**](https://composio.dev/blog/mcp-server-step-by-step-guide-to-building-from-scrtch/)[. ](https://composio.dev/blog/mcp-server-step-by-step-guide-to-building-from-scrtch/)

A quick bit of info:

VS code / any client support 2 ways to add MCP servers 

- Local Version - You develop the server, define its functionality and expose it locally to be fetched by client. This is done using STDIO (standard input output) connection method.
- Hosted Version - Server developed by someone else and hosted somewhere. You gain access by using SSE (Server-Side Events) / HTTP connection method.
Keep the above in mind & let’s begin!

### 1. Prerequires

Before we begin, ensue you meet the following pre-requires.

- VS Code Latest Stable Build: If don’t download at [official site](/20ef261a6dfe80288798dbe8397bbf23) / upgrade.
- Python, Uv, NPX or NPM package manager installed. 
- Admin access to Vs Code `settings.json` file.
- Clear understanding of file paths in your os. (a must!)
Assuming you met the criteria, let’s move on.



### 2. Enable MCP in VS Code

Vs code by default has MCP server enabled. To verify hit` CTRL+SHIFT+P` (Windows & Linux) / `CMD+SHIFT+P `(Mac) and search **MCP**. 

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/how-to-add-100-mcp-servers-to-vscode/images/4.png)

### 3. Add Local MCP Servers to VS Code

Click MCP: Add MCP Server & Select Command (stdio). 

Now Follow the carefully on all tabs!

→ **Enter Command**: Enter command to run the server. Ensure using absolute path, so `python calculator_mcp_server` on windows becomes 👇

`"C:\\Users\\Harsh\\Documents\\mcp\\mcp\\Scripts\\python.exe" "C:\\Users\\Harsh\\Documents\\mcp\\calculator_mcp_server.py"`

→ **Enter Server ID**: Name your server - call it something meaningful.

→ **Choose Config Saving**: 

- **User Setting **- accessible everywhere, config get’s added to `settings.json` .
- **Workspace Settings **- only in current the project (highly suggested). This also creates a `mcp.json` in `.vscode` folder.
Here is a demo of me doing the same👇

[Video](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/f2fd0a8f-eca0-46fc-8887-19608e4dd97a/local.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ZEO2KXS%2F20260326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260326T082543Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDqewXnUlTkSM1zzFsLfP9Bn9NVWJXooGUuzKzn3jQK0AIhAIZ%2B11qFva5ff4gH7CdY7Va7ZXq5IXINGt1GBGzyWpZ1KogECMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxYsapQ67YPRka2imEq3AOt3pnjJT7g78jJBkcSIB7PwSefZEOZLczvyMRTQ4ke78JIrmew0Vam%2Fp7Jd4L3rBgWB8PW0BODWEiZSr4etD2c1jf%2B2QCjvkr6eXkosziyJNqtehPQRrs8vrol04BPcKZzEJ77GxSpBIhZG5hoKwQxKMCq4czodTPzukmDNKM8W2Ki1235%2F4P9nVT02p3TxjzEX%2Bf%2F%2BweqJSCMKcP%2FszMCaDUyFco094VTM83wn8cu9RpN6zGQCkKrzHPVo%2B%2FVGDdgHBGIlxP0qWoy4VJKEj3Ya5Z2NqKQN57G5dX7EpljwtgQ5ZUMiltZuekfdm%2BwncLGQrGlTv9nZt3Nn%2BKTFBnrc7d%2BOoG%2Fllue6ETjDU2WN5FMSXN0%2FajgrGB6NdQpE27K3NpeofSu1x77w5DRYiE654znhVrxi05DaP7HJ6XNnJic3tuLUudEG7vV%2FltNU%2F6Sw44gC32eJVJsbpVYtJhwVUAQi8Sf6ZTzfBYc2dD%2BAYmU7vWBS757%2FV8NsfrM4HNamHsaqA6UDLfq4UgR75dqhGVJS0fEtSaAJAhJhMZXFXCkSXdY87kHLGh3vbcVCZTyuleWiKIfVmd0QhwN9CAcyKLT%2BzaNnxVsQIcc73jn%2BES%2BiIEClgrmtzFItDCIvpPOBjqkAQ7EOxvxoVhKD3Qs7dfiXBIKfyVOPKFa1K4xoh2djJBiiy8JM%2B6puW%2FuJV7J6JQ8iUf1NH7jdWT6mcJ5uAj6TiHTrZ%2F5lQJ7Pw5DzmoI2BnuNJnoVwQGoN7bq1ClhSFTeP4OrmvmHE6ppFau1KKQ6gC%2FUUoreo845acKHhmlhd7pATGO0pXphqt5ZOFBdaGgyjTihE0vcNjylTIGWKc6yKD4DbN7&X-Amz-Signature=5dcef419628f3faa2d291dacda2ac94aab32bbf57aea17f7d62109d21f1fe474&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

To verify in copilot, choose agent mode & llm and prompt for calculation, the agent will detect the custom tools defined in `calculator_mcp.json` . Make sure to give the consent.

![add tool call in action using mcp](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/how-to-add-100-mcp-servers-to-vscode/images/5.png)
![tan tool call in action using mcp](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/how-to-add-100-mcp-servers-to-vscode/images/6.png)

![mcp server with agent mode in action](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/how-to-add-100-mcp-servers-to-vscode/images/7.png)

But how all this happened, time to dig behind the scenes?

Depending upon the workspace type, 2 files get created: `settings.json` and `mcp.json` . Both have similar structure.

```python
{
    "servers": {
        "calc_serve": {
            "type": "stdio",
            "command": "C:\\\\Users\\\\Harsh\\\\Documents\\\\mcp\\\\mcp\\\\Scripts\\\\python.exe",
            "args": [
                "C:\\\\Users\\\\Harsh\\\\Documents\\\\mcp\\\\calculator_mcp_server.py"
            ]
        }
    }
}
```

Important fields are:

- `name`: Name of MCP Server exposed to LLM. I named it `calc_serve`
- `type`:  what type of connection it is - **SSE / STDIO / HTTP**. 
  - Same need to be configured in transport parameter of mcp.run() withing sever
- `command` : Base command use to run the server 
  -  `python, npx, npm , uv` - are all potential candidates
  - Make sure to provide absolute path to exe files of same. Can do a `which / where` name in terminal
  - For env, it's better to use path for python present in .venv folder.
  - In case you select `User Settings` -> in `settings.json` path changes to \\ instead of \\\
- `args` : additional arguments can be passed.
  - If using stdio, ensure you use full path to the file.py
  - for `npm / npx/ yarn` **-y** or any **command line args** can be used here.
  - args should be provided in same manner as you would in terminal
A quick tip 💡

> Combine the `command` and `args` in same manner as defined and run in terminal, if it fails that means it's wrong way to run. This can help you fix mcp servers’ issue (command not found) easily.

Local are relief, but in production - hosted mcp servers are used. Let’s look at how to connect those to your vs code.

### 4. Add Hosted MCP Servers to VS Code

For demonstration I will use *composio-mcp*, because of its 200+ fully managed [MCP Servers](https://dub.composio.dev/9wAhI83) with built in authentication. Ya, it does my heavy lifting nowadays!

Let’s look at how to add one to vs code using HTTP. 

Follow the steps

→ Head to [Composio MCP Server](https://connect.composio.dev/) directory.

→ Ensure you are logged in, else signup (its free)

→ In the dashboard head to Install Section

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/how-to-add-100-mcp-servers-to-vscode/images/8.png)

→ Select VS code from the list click on Install. 

→ In the redirected page, Click on “Install in VS Code”

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/how-to-add-100-mcp-servers-to-vscode/images/9.png)

→ It will redirect to vs code, and in the MCP server Page, click Install

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/how-to-add-100-mcp-servers-to-vscode/images/10.png)

→ Once install it will ask you to authenticate, so click allow, login to the website, give the permissions and you are done!

![image](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/how-to-add-100-mcp-servers-to-vscode/images/11.png)

> Note : Go to `.vscode/mcp.json` and start the server (press restart). Ensure in terminal it shows `n` tools detected. A quick hack if the server is not showing tools.

Time to test it out in vs code agent mode!

---

## How to use MCP in Vs Code Agent Mode

Vs code allows agent mode to use MCP. So, let’s test it out. 

Let’s build a notion listicle page / short blog using mcp server we just added 👇

[Video](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/10f64780-3514-401d-b900-d7fa266923b8/notion_mcp_.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQ3NW2UP%2F20260326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260326T082544Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDVPg0My6YVjxYbE%2F50%2BwkTiDg12ysRHnhi7hHr05i%2FxwIgbvmGujW8Y9V6y4N2Hp%2B%2F6XmohcGcUqAbsOa%2Brcr4aNkqiAQIwP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKshLCb16%2FfinhlHGCrcA5LaBx%2BaayahgmzpbqQKkKHWMS%2FEJo8Pjn0ZtWwJBza4T8z%2B%2BWfxbPH6xlMGVFxMfBUx1MCmumjUxsH4%2F8F6Dx1Iy8OVMKHY%2Foyv73%2F2P456NEUbRDA%2F59sU%2FOtkjCLmHYiLy1D5jGqllwgDwl2CpglSMnnAw39YrPSEdCOfFfn10vxfRG1BVGafxwjQVPs9lzGJ%2F6Y6WS%2Bc2phgueT2jM9qeBqi2KoQqv47d7YN9Cjr7C5z4gqY4fzWEpAK5XjfRHEd2aai0aPx1AzpMDcEUcgFqROb2ioh5lbOx%2FlmL4gpQ6hzeTshO55GYqn27nhuE%2Fsdo76n%2BrnAukwLWa5o93j2prgSDSYty%2Brxiga02pyYiOUFp4LBUeRg%2Bve%2FG2tqCdKhFjNsU9vg3DkojA3VryqK6gxUgDxDJZpbpdTDsrXx7zptkjXKhX75Vk4b3TcEA29YWDM41%2FOk5bNSBOS3gtd17fB0v39nUr8MUwqM6ZsQYT3RTdiFBGvtjOg1Dsyg0fb%2Fys8PCSb4ncoRFm4NPVwqf8ays%2B43Tnk6Rbxc13t9Omm4u6m7o78d%2BOZnLsL872Erd4pdMejEb8rDCAVlAep9vsnxdDVPg7BxMLZxFsfvOAl%2BjKHbUEbUabycMOW8k84GOqUBQPa3uQGma5t05ATAIn7aLp5WlMF6lMlqLCEuWsFZhnXeXvs0v0rgvgJGDyrjap5BTTnmua%2BmSMMOFLXrok3eSL3O1AaElSphbDOf33FglX6nsNDq9VC0wrYSG6CGN0Dwuq6J%2BzJHZi%2FRXY%2BaoDapaebzF79dKXMR%2BcsklVQ612PUUT%2BS6JDhY3IqYLdhnIEMOYSmLTuWF%2Fjm5nT%2FYDDPZGmrjjHG&X-Amz-Signature=9506578bf5be0316f7d427d35cd994056dcc9e1123d1182ccd4e3a6231a31dd1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

For reference here are the steps:

→ Head to copilot

→ Switch to Agent Mode

→ Select Model - I choose Gpt4o, some models might perform even better.

→ Ensure agent have access to tools. To check click on 🛠️ icon and check if mcp server listed. If not enable it and recheck

→ Enter your query and let the agent do the job!

So, what changed in `vscode/mcp.json`  to enable **http **support? Let’s have a look

```json
{
  "servers": {
    "composio": {
      "type": "http",
      "url": "https://connect.composio.dev/mcp"
    }
  }
}
```

Most parameter configuration is gone except `name` and `url`  gets added. The url is responsible for connecting to the server and exposing pre-defined tools in composio notion toolset.

But there is more, much like building blocks, we can connect multiple mcps and productively get work done. Let’s explore it with a real use case!

---

##  Practical Use Cases with examples

MCP really shines when it comes at automating multiple tasks across multiple apps. Let’s look at how you can turn your ide into a full fledge working environment which can:

→ Find Issues

→ Create a linear ticket & milestone 

→ Send update to slack channel

all without leaving your ide. Check it out 👇

Prompt

```plain text
I need you to create me a Linear issue under this project: @web
https://linear.app/devloper-hs/project/park-manager-329e1aa897d8

Title the issue as: "Fix the circular import error asap! ⌛"
Add a comment to the issue saying: "Fix the circular import issue in app/__init__.py.!"

Then, post the status of whether the work was successful or not in my 'proj-park-manager' Slack channel. If it was successful, include a quick status update with the URL to the Linear issue: @web
https://devloperhs201-0lu6943.slack.com/archives/C08H18UBB19
```

 Results 👇

[Video](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/32dda01f-f6dd-4257-bfee-a672d25c1bf1/mcp-vs-code-coding-issue-tracker-agent_-_Made_with_Clipchamp.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQ3NW2UP%2F20260326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260326T082545Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDVPg0My6YVjxYbE%2F50%2BwkTiDg12ysRHnhi7hHr05i%2FxwIgbvmGujW8Y9V6y4N2Hp%2B%2F6XmohcGcUqAbsOa%2Brcr4aNkqiAQIwP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKshLCb16%2FfinhlHGCrcA5LaBx%2BaayahgmzpbqQKkKHWMS%2FEJo8Pjn0ZtWwJBza4T8z%2B%2BWfxbPH6xlMGVFxMfBUx1MCmumjUxsH4%2F8F6Dx1Iy8OVMKHY%2Foyv73%2F2P456NEUbRDA%2F59sU%2FOtkjCLmHYiLy1D5jGqllwgDwl2CpglSMnnAw39YrPSEdCOfFfn10vxfRG1BVGafxwjQVPs9lzGJ%2F6Y6WS%2Bc2phgueT2jM9qeBqi2KoQqv47d7YN9Cjr7C5z4gqY4fzWEpAK5XjfRHEd2aai0aPx1AzpMDcEUcgFqROb2ioh5lbOx%2FlmL4gpQ6hzeTshO55GYqn27nhuE%2Fsdo76n%2BrnAukwLWa5o93j2prgSDSYty%2Brxiga02pyYiOUFp4LBUeRg%2Bve%2FG2tqCdKhFjNsU9vg3DkojA3VryqK6gxUgDxDJZpbpdTDsrXx7zptkjXKhX75Vk4b3TcEA29YWDM41%2FOk5bNSBOS3gtd17fB0v39nUr8MUwqM6ZsQYT3RTdiFBGvtjOg1Dsyg0fb%2Fys8PCSb4ncoRFm4NPVwqf8ays%2B43Tnk6Rbxc13t9Omm4u6m7o78d%2BOZnLsL872Erd4pdMejEb8rDCAVlAep9vsnxdDVPg7BxMLZxFsfvOAl%2BjKHbUEbUabycMOW8k84GOqUBQPa3uQGma5t05ATAIn7aLp5WlMF6lMlqLCEuWsFZhnXeXvs0v0rgvgJGDyrjap5BTTnmua%2BmSMMOFLXrok3eSL3O1AaElSphbDOf33FglX6nsNDq9VC0wrYSG6CGN0Dwuq6J%2BzJHZi%2FRXY%2BaoDapaebzF79dKXMR%2BcsklVQ612PUUT%2BS6JDhY3IqYLdhnIEMOYSmLTuWF%2Fjm5nT%2FYDDPZGmrjjHG&X-Amz-Signature=90c7ec3a3fef902c47e38e0ada3b72282f2677d1bdb7ea1f147d9374c910228b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)

What I liked about this is - Agent 1st plans the outline, then figure out the tools and finally executes it based on settings in `.vscode/settings.json` . I hope you got an idea and now create powerful automation flows using same - Just make your prompt specific!

With this we have come to end of this informative blog, and here are my final thoughts!



---

## Final Thoughts

Integrating MCP servers with VS Code takes just a few minutes (with composio / anthropic / relevant sources) and unlocks a new level of automation and collaboration. 

Whether you’re automating tasks, connecting to cloud APIs, or enabling team-wide agentic workflows, MCP makes it easy to scale your productivity—one server at a time.

Ready to get started? Open your **`.vscode/mcp.json`**, add your first server, and watch your project development transform!


