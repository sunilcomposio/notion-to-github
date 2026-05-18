---
title: "How to Build an MCP Server with FastMCP (stdio, Streamable HTTP, Cursor, and Claude)"
source: https://www.notion.so/composio/How-to-Build-an-MCP-Server-with-FastMCP-stdio-Streamable-HTTP-Cursor-and-Claude-35cf261a6dfe800b8dd5d9ab5c35e4e0
notion_page_id: 35cf261a-6dfe-800b-8dd5-d9ab5c35e4e0
last_synced: 2026-05-18T12:48:39Z
---

If you've spent any time wiring up tools for an AI agent, you know the pain: every model vendor has its own function-calling format, every integration is bespoke glue code, and every model upgrade breaks something downstream. 

MCP changes this by providing a shared interface for tools, data sources, and AI clients. Instead of rebuilding integrations for every model or app, you expose capabilities once through an MCP server and let compatible clients connect to them.

Anthropic released the Model Context Protocol in November 2024, and by spring 2025, OpenAI, Microsoft, and Google had adopted it. It has quickly become the de facto standard for connecting AI systems to external tools and data. That makes MCP worth learning now.

This guide teaches you how to build an MCP server from scratch, expose it over two transports: **stdio **and **streamable HTTP**, connect it to Claude Desktop and Cursor.  and provide you with a production checklist for hardening the production MCP servers before shipping them. The full code is available in the companion repo at the end

### tl; dr

- What is MCP
- MCP Components in Nutshell
- Build a Local MCP server (tools, resources, and prompts) 
- Connect to Hosted MCP Server (HTTP Streamable)
- MCP Server Advance Use Case
- Deployment Notes for Production MCP Servers
- Conclusion
- Frequently Asked Questions

Let's Begin

---

## What is MCP

An MCP server is a JSON-RPC 2.0 process that exposes three primitives to any compliant [MCP client](https://composio.dev/content/mcp-client-step-by-step-guide-to-building-from-scratch)[ ](https://composio.dev/content/mcp-client-step-by-step-guide-to-building-from-scratch)over stdio or streamable HTTP. Those primitives are **tools** (functions the model can call), **resources** (data the model can read), and **prompts** (reusable templates). That's the whole protocol. 

Everything else is an implementation detail as given in the comparison table.

<table header-row="true">
<tr>
<td>**Feature**</td>
<td>**Model Context Protocol (MCP)**</td>
<td>**Traditional API (REST / GraphQL / gRPC)**</td>
</tr>
<tr>
<td>**Primary purpose**</td>
<td>Give LLM-based agents a *single* way to fetch context **and** invoke side-effecting tools.</td>
<td>General machine-to-machine data exchange & business logic.</td>
</tr>
<tr>
<td>**Integration effort**</td>
<td>Once an agent speaks MCP, it can talk to any compliant server; only one SDK/wire-spec to learn.</td>
<td>Each API exposes its own spec/SDK; you integrate them one by one.</td>
</tr>
<tr>
<td>**Interaction model**</td>
<td>Stateful sessions + bidirectional messaging; supports long-running tasks and mid-job progress callbacks.</td>
<td>Stateless request/response; usually unidirectional.</td>
</tr>
<tr>
<td>**Streaming support**</td>
<td>Standardised via “Streamable HTTP” (SSE/WebSocket).</td>
<td>Not part of the REST spec; developers bolt on WebSockets/SSE ad hoc.</td>
</tr>
<tr>
<td>**Extensibility (adding new capabilities)**</td>
<td>The server can add new tools/resources without breaking clients; the agent discovers them at runtime.</td>
<td>Breaking changes need versioning, or clients must update to use new endpoints/types.</td>
</tr>
<tr>
<td>**Standardisation/wire format**</td>
<td>One JSON-Schema-driven spec for inputs & outputs, plus defined transports.</td>
<td>Multiple styles (REST, GraphQL, gRPC) with differing auth headers, error shapes, and media types.</td>
</tr>
<tr>
<td>**Maturity & tooling**</td>
<td>Rapidly growing, but still early; dozens of open-source servers and early commercial support.</td>
<td>Decades of best-practice guides, gateways, SDKs, APM, and monitoring.</td>
</tr>
<tr>
<td>**Performance path length**</td>
<td>Extra hop (agent → MCP → underlying API) adds a bit of latency; streaming mitigates waiting for large payloads.</td>
<td>Direct call; generally lower overhead for high-QPS, deterministic workloads.</td>
</tr>
<tr>
<td>**Typical sweet-spot use-cases**</td>
<td>Autonomous agents chaining multiple tools, dynamic workflows, and user-in-the-loop</td>
<td>CRUD data services, stable integrations, high-throughput micro-services.</td>
</tr>
</table>
If you want a deeper conceptual primer, check out the [Model Context Protocol explainer](https://composio.dev/blog/what-is-model-context-protocol-mcp-explained/), which covers the architecture in detail. For primer, here is how this all works!

---

## MCP Components in Nutshell

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/942a96ba16.png)

### The Architecture

The MCP architecture consists of several key components that work together to enable seamless integration:

1. **MCP Hosts**: Applications that want to use tools or data through MCP. Examples include Claude Desktop, Cursor, or any AI app that supports MCP.
2. **MCP Clients**: Client instances created by the host. Each client maintains a dedicated one-to-one connection with a single MCP server.
3. **MCP Servers**: Lightweight programs that expose tools, resources, and prompts through the MCP protocol.
4. **Local Data Sources**: Files, databases, codebases, or local services that an MCP server can access on the user’s machine.
5. **Remote Services**: External APIs or cloud services that an MCP server can call, such as Google Sheets, GitHub, Slack, or Postgres hosted in the cloud.

At a high level, the host does not talk directly to your database, file system, or API. Instead, it talks to an MCP client, which talks to an MCP server, which then talks to the actual data source or service. 

This simple diagram might help you understand the flow better:

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/01e1a4f86f.png)

The key point is that MCP servers serve as the integration layer. They hide the messy parts of authentication, API calls, file access, and business logic behind a clean protocol interface.

This separation of concerns makes MCP servers modular and maintainable. The AI app does not need to know how Google Sheets, GitHub, or a local database works. It only needs to know how to call the MCP server.

So how does this all connect?

### **How The Components Work Together**

Let's understand this with a practical example:

Say you're using Claude Code (an MCP host) to manage your project's budget. You want to update a budget report in Google Sheets and send a summary of the changes to your team via Slack.

- [C](https://code.claude.com/docs/en/mcp)[laude Code](https://code.claude.com/docs/en/mcp) (MCP host) initiates a request to the MCP client to update the budget report in Google Sheets and send a Slack notification.
- The MCP client connects to two MCP servers: one for Google Sheets and one for Slack.
- The [Google Sheets MCP](https://composio.dev/toolkits/googlesheets) server calls the Google Sheets API (remote service) to update the budget report.
- The[ Slack MCP ](https://composio.dev/toolkits/slack)server interacts with the Slack API (remote service) to send a notification.
- MCP servers send responses back to the MCP client.
- The MCP client forwards these responses to Cursor, which displays the result to the user.

This process happens seamlessly, allowing Cursor to integrate with multiple services through a standardized interface.

That's the working in a nutshell. From here on, we will focus on building.

---

## Build Local MCP Servers

We'll build something you'd actually deploy: a **GitHub issue search server** that lets your AI assistant search issues, fetch issue details, and pull recent pull requests from any repository. \[subject to change\]

We will follow a series of steps to make sure things go smoothly. This is also industry standard, so it's best to follow along. 

### Prequires

Before anything else, ensure you complete the prerequisites, as they will be the foundation for any MCP implementation we do.

You'll need:

- **Python 3.10+**
- **Node.js 18+** for TypeScript (if you choose to build with TypeScript)
- **Claude Desktop** or **Cursor** for testing

A common question users often ask: when should you pick Python vs. TypeScript? Here is an honest review:

- [Python ](https://www.python.org/downloads/)with [FastMCP](https://gofastmcp.com/getting-started/welcome) is faster for prototyping and works well for stdio servers. 
- [TypeScript ](https://www.typescriptlang.org/)shines for HTTP servers deployed to Cloudflare Workers, Vercel Edge, or any Node-based stack due to its type safety.

In case you are wondering, Composio is the ultimate integration platform, empowering developers to seamlessly connect AI agents with external tools, servers, and APIs with just a single line of code.

With the fully managed [**MCP Server**](https://composio.dev/blog/best-mcp-servers-for-chatgpt)[s](https://composio.dev/blog/best-mcp-servers-for-chatgpt), developers can rapidly build powerful AI applications without the hassle of managing complex integrations. 

> In a nutshell: Composio handles the infrastructure so developers can focus on innovation.

Next, let's set up the working environment.

---

### **Work Environment Setup**

We start by creating a project directory.

Navigate to your working folder and create a folder named **MCP,** or u can use the terminal command:

```shell
mkdir mcp_servers
cd mcp_servers
```
Next, create a virtual environment using:

```shell
python3 -m venv .venv
```
Now activate the environment with:

```shell
# Windows:
.venv\Scripts\activate

# Linux/Mac:
source .venv/bin/activate
```
Ensure you see (.venv) in Front of the terminal cwd path.

Finally, install the FastMCP package

```shell
pip install FastMCP
```
---

### Writing Server Code

Server code is where the entire MCP server logic lives. Create a new `server.py`  file and inside it paste the code below:

```python
# server.py

# imports
import base64
import os
import uuid

from typing import List
from fastmcp import FastMCP

mcp = FastMCP("Utility MCP Server")

# generate password utility
@mcp.tool()
def generate_password() -> str:

    """
    Generate a pseudo-random password for development use.

    This function creates a password by encoding a UUID4 string in base64
    and then reversing the resulting string. It's intended for use in
    development or testing environments where cryptographic security is not
    critical.

    Returns:
        str: A reversed base64-encoded UUID4 string, typically around 48 characters.
        May include alphanumeric characters, '+', '/', and '=' padding.

    Security Notes:
        - Uses the string form of UUID4 (not raw bytes), so effective entropy is lower (~190 bits of input chars)
        - Suitable for temporary or development credentials.
        - NOT recommended for production use.
        - For higher security, consider using secrets.token_urlsafe().

    Example:
        >>> generate_password()
        '==dGM0ZTcwLTI0ZTUtNDJlZC05Y2I1LTk3OGJjODAxMDAwNw=='
    """

    uuid_string = str(uuid.uuid4()).encode()
    base64_encoded = base64.b64encode(uuid_string).decode()
    return base64_encoded [::-1]

# list all the files in the directory
@mcp.tool()
def list_files(directory: str = ".") -> List[str]:
    """
    List all files and directories in a specified path.

    This tool allows the AI to explore the file system by returning a list 
    of names of the entries in the directory given by path.

    Args:
        directory (str): The path to the directory to list. Defaults to the 
        current working directory (".").

    Returns:
        List[str]: A list of filenames and directory names. If an error 
        occurs (e.g., directory not found), returns a list containing 
        the error message.
    """
    try:
        return os.listdir(directory)
    except Exception as e:
        return [f"Error: {str(e)}"]
        

# read the given file
@mcp.tool()
def read_file(filename: str) -> str:
    """
    Read and return the full text content of a file.

    This tool enables the AI to analyze the contents of specific text files 
    within the accessible file system.

    Args:
        filename (str): The path to the file that needs to be read.

    Returns:
        str: The complete string content of the file encoded in UTF-8. 
        If the file cannot be read, returns an error message starting 
        with "Error reading file:".
    """
    try:
        with open(filename, 'r', encoding='utf-8') as file:
            return file.read()
    except Exception as e:
        return f"Error reading file: {str(e)}"

# system info (resource)
@mcp.resource("data://sys-info")
def get_system_info() -> str:
    """
    Get a comprehensive system snapshot including time, hardware specs, 
    and real-time health metrics.
    """
    import platform
    import psutil
    import subprocess
    from datetime import datetime

    now = datetime.now()
    timestamp = now.strftime("%Y-%m-%d %H:%M:%S")
    day_name = now.strftime("%A")
    
    
    boot_time = datetime.fromtimestamp(psutil.boot_time())
    uptime = str(now - boot_time).split('.')[0]

    sys_type = platform.system()
    machine = platform.machine()
    cpu_count = psutil.cpu_count(logical=False)
    try:
        gpu_info = subprocess.check_output(
            ["nvidia-smi", "--query-gpu=gpu_name", "--format=csv,noheader"], 
            encoding="utf-8"
        ).strip()
    except Exception:
        gpu_info = "N/A (NVIDIA-SMI not found)"

    cpu_usage = psutil.cpu_percent(interval=None)
    memory = psutil.virtual_memory()
    disk = psutil.disk_usage('C:\\')

    return f"""
             --- Hardware Specs ---
            OS:        {sys_type} ({machine})
            CPU:       {cpu_count} Physical Cores
            GPU:       {gpu_info}

            --- Temporal Context ---
            Date/Time: {timestamp}
            Day:       {day_name}
            Uptime:    {uptime}

            --- Resource Utilization ---
            CPU Load:  {cpu_usage}%
            RAM Usage: {memory.percent}% ({memory.used // (1024**2)}MB / {memory.total // (1024**2)}MB)
            Disk (C:): {disk.percent}% Used ({disk.free // (1024**3)}GB Free)
            """

# general prompt
@mcp.prompt()
def helper_prompt(task: str, context: str = "") -> str:
    """General purpose prompt for any task with optional context."""
    base_prompt = f"""
Task: {task}

Please approach this systematically:
1. Break down the problem
2. Consider multiple solutions
3. Explain your reasoning
4. Provide clear, actionable steps
"""

    if context:
        base_prompt += f"\n\nAdditional context: {context}"

    return base_prompt

# api-design rules prompt
@mcp.prompt()
def api_endpoint_design_prompt(endpoint_name: str, purpose: str) -> str:
    """Specific prompt for designing RESTful API endpoints."""
    return f"""
Design a RESTful API endpoint: {endpoint_name}
Purpose: {purpose}

Please provide:
1. HTTP method and URL pattern
2. Request/response schemas with examples
3. Status codes and error handling
4. Authentication requirements
5. Rate limiting considerations
6. OpenAPI/Swagger documentation snippet

Follow REST conventions and include proper validation.
Use JSON for data exchange and meaningful HTTP status codes.
"""

# Run MCP server
if __name__ == "__main__":
    mcp.run()
    
# Run http stremable MCP Server (can be deployed / hosted)
# if __name__ == "__main__":
   # mcp.run(transport="streamable-http", host="0.0.0.0", port=8000)
```
Key points from the code:

- `mcp` : It's the Fast MCP object named as provided. Important to initialize at the start.
- `@mcp.tool()` : Decorator wraps the entire function underneath and exposes it as a tool to the MCP client. Docstrings are very important; a good docstring leads to fewer model failures. Includes `generate_password` , `list_files`, `read_file`tools.
- `@mcp.resource("data://sys-info")` : Decorator wraps the entire function and exposes it as a resource file that the client can use to fetch data. The path to the created resource is defined in `()` . Includes `get_system_info` resource
- `@mcp.prompt()` : Decorator wraps the entire function and exposes it to the client as a prompt builder, useful when the task requires a specific use-case prompt. Docstrings are important here. Includes `helper_prompt `, `api_endpoint_design_prompt` 
- `mcp.run()` : Runs the MCP server.   For a streamable HTTP MCP server, use `mcp.run(transport="streamable-http", host="0.0.0.0", port=8000)`

Overall, we are wrapping different functions and exposing them to the client as the MCP tool, resources, and server.

However, to use it with the client, additional config is required, so let's set that up. I am using a cursor for the demo.

Head to the Cursor and create a new folder `.cursor` , inside it creates a new file `mcp.json` (the config file) and paste the following setup.

```python
{
    "mcpServers": {
        "utility_server": {
            "type": "stdio",
            "command": "your-python-exe-path",
            "args": ["your file path"]
        }
    }
}
```
> Quick Tip: To verify this works is by pasting the command and args together; if they start the server, you are good to go; else, provide the full path. I usually go for path of .venv’s `python.exe`

Open **Cursor Settings,** then click **Tools & MCPs**. You will see your local MCP name. Click Enable, 

To check, open the terminal, switch to the Output tab, select the MCP server, and check for the connected message.

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/361cfb6272.png)

Now we can test our MCP Server by opening the chat/agents window and invoking any of the defined tools via the prompt. I invoked the password-generator, as I often find myself asking Claude to "think of a 16-character password" (which is terrible).

```python
Generate password + show mcp tool usage
```
Here is the output:

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/c3c08ae3c8.png)

Saw that Ran command, it means the MCP server asked me for permission. I kept it that way, as you never want to let an agent touch your system files. 

> Note: If docstring is poor, the mcp client might fall back to cli approach, the easiest way to fix this is add a rule to only use mcp servers for the task. You will find a similar approach in the project github repo as well.

All this is great, but setting it up for testing a local MCP server is a hassle. Let's look at an easier approach, MCP Inspector.

---

### **Test with MCP Inspector**

To test the MCP server's functionality, we will use the Fast MCP Inspector, a browser-based tool that connects to the server and lets us call tools directly, without an LLM layer. 

Run MCP Inspector, with:

```shell
npx @modelcontextprotocol/inspector python server.py
```
You'll see a localhost URL. Open it in your browser, click **Connect**, and you should see your server respond.

Now you can run and test your tools, resources, prompts, and more right from the browser, quite handy for developers testing MCP servers' performance and understanding response schemas. Check out the [MCP Inspector Guide](https://modelcontextprotocol.io/docs/tools/inspector) to learn more.

Personally, I find it best for testing prompts’ responses. Here is the response it generated:

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/44999b8dda.png)

Local servers are great for simple use cases, but for most industry use cases, they don't provide the required infrastructure. Let's explore better use cases.

---

## Connect to Hosted MCP Server (HTTP Streamable)

Having a local server is good, but not as great as the one used in production. Usually, these are hosted, secured by design, and optimized for multiple tool calls. 

Usually, these are servers written by expert developers, integrated with the app's internal mechanisms, and intended to serve as connectors. 

But here lies the issue: connecting to 30+ MCP servers for a single project is a pain. Composio solves this.

Let's look at how you can connect to the hosted Composio MCP server. You connect it once and use 1K+ tools directly, all secure, optimized tool calls. 

### Connect to Composio MCP Servers (Streamable HTTP )

Integrating with **Composio MCP** is incredibly simple and takes just 5 steps.

1. Visit the [Composio MCP ](https://dashboard.composio.dev/)page. Ensure you are logged in, or else sign up

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/532bada0e4.png)

2. In the dashboard, head to the Install Section

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/c8020190c5.png)

3. Select Cursor from the list, and click on Install.

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/7c43cc0374.png)

4. In the redirected page, click on “Install in Cursor.”

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/d8b7a76c2c.png)

5. You will be redirected to Cursor, and in the MCP server Page, click Install

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/2563e21065.png)

Once installed, click on Needs Authentication, then Authorize at redirect, and done!

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/9ee5cd10fe.png)

You can connect to apps in advance in the Connect Apps section; most of them use OAuth. (optional). If you don't, the agent will prompt you to connect at runtime.

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/2523496c50.png)

To test the integration, open the Composer, establish a connection to the app, and ask it to perform actions.

But how did all this happen?

It turns out that if you go to your cursor MCP & Tools Page, select the ✏️ icon, and see the configuration as:

```json
{
  "mcpServers": {
    "composio": {
	    "type":"http",
      "url": "https://connect.composio.dev/mcp",
      "headers": {}
    }
  }
}
```
The **http **here means a streamable HTTP server. This allows the MCP server to operate as a normal web server (accessible via a URL) rather than as a local subprocess on your machine (for local MCP servers)

And interestingly, for any MCP to run in HTTP streamable mode, you have to replace:

```json
if __name__ == "__main__":
    mcp.run()
```
to 

```json
if __name__ == "__main__":
    mcp.run(transport="streamable-http", host="0.0.0.0", port=8000)
```
And you are done, pretty handy, isn’t it?

Now, let's look at an advanced use case to see where Composio MCP helps.

---

## Using MCP Servers for Advanced Use Case (Financial Agent)

Current financial institutions struggle with manual, time-intensive investment research that keeps analysts buried in data entry for weeks, delays critical decisions by days, lacks real-time risk visibility, creates compliance blind spots, and demands hiring more analysts just to manage portfolio volume - all while losing talent and clients

Let's see how the analyst can build financial agents and leverage Composio MCP to connect to 10-15 financial data sources (Yahoo Finance, SEC filings, Google Sheets, Bloomberg, etc.) and enrich their analysis by aggregating structured APIs, unstructured web scraping, and internal data warehouses.

The financial analyst agent will:

- Aggregate financial data (Yahoo Finance, Google Sheets with company data, SEC filings via web scraping), 
- Run Claude analysis to generate investment thesis. 
- Create formatted reports in Google Docs, 
- Track portfolio changes (pull from Excel sheet), 
- and send Executive Summaries to stakeholders via email with embedded tables. (HTML format)

All done in parallel!

```python
Web Scraping (composio & search api) + Google Sheets (composio) → Claude Analysis → Google Docs (composio) → Gmail (composio)
```
#### Prerequisites

- Claude Code / Cursor / VS Code. I am going with Claude
- [Composio Account](https://dashboard.composio.dev/login), login/signup

Assuming you have pre-requisites met, let's set up Composio on Claude.

#### Set up Composio MCP in Claude

To get started: 

- Visit the [Composio MCP ](https://dashboard.composio.dev/)page. Ensure you are logged in, or else sign up
- In the dashboard, head to the Install Section
-  Select Claude from the list, and click on Install.
- On the next page, select the MCP tab, then copy the command & paste it into the shell/terminal.

	```python
claude mcp add --scope user --transport http composio https://connect.composio.dev/mcp
	```
	Ensure `/mcp`  shows composio as an option. It might require authentication, so authenticate once

	![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/fastmcp-server/b81ffb7c9c.png)

> (Optional ) Once done, head to the Connect Apps page and click on **Connect **to connect Gmail, Google Drive, Google Sheets, and Google Docs. In case you ignore, agent will ask for these connections on runtime

Perfect, we only need to configure the document in the workspace

#### Set up Workspace for Agent

We will use Google Drive as our workspace to make the file accessible from everywhere:

-  Head to drive, create a new folder named `financial_data`  & from the URL, copy the folder ID.
- Inside, add an Excel file in the format & fill the data as:
	- Companies: List of all targeted companies with their analysis
	- Portfolio Performance: How the portfolio is performing over time, all metrics for targeted companies
	-  Contact: List of all internal contacts: Portfolio Managers, Limited Partners, Investors,  Board Managers, and more
	- Report Archive: Past report filing (sec)
- Then add a Google Doc template for an investment report. 

Make sure to copy the ID / URL of both files

> Easiest way is to use the template given in the GitHub Repo and let any AI Agent turn them in the above given format. Then copy paste that in newly created google sheet & google doc file.

Now is the time to run the agent. 

#### Run Financial Analyst Agent

To run the agent, open Claude Code and paste the [financial_agent_prompt](https://gist.github.com/DevloperHS/06982896ddfc20c8ace1af213dff2ebf). You will be prompted to fill in the details with the copied values from earlier, and add your email address:

```python
GOOGLE_SHEETS_ID: [your value]
GOOGLE_DOC_TEMPLATE_ID: [your value]
GOOGLE_DRIVE_FOLDER_ID: [optional]
GMAIL_SENDER_ADDRESS: [your gmail]
```
Once done, the agent handles the rest. Here is a demo of what it looks like, in production.

<video src="file://%7B%22source%22%3A%22attachment%3A1e01d6aa-64f0-4aed-a206-7d044bf8d271%3Afinancial-agent-using-composio-mcp.mp4%22%2C%22permissionRecord%22%3A%7B%22table%22%3A%22block%22%2C%22id%22%3A%22360f261a-6dfe-8067-b10b-c7b5616ba2e9%22%2C%22spaceId%22%3A%2235adec70-dbef-4f42-9214-62ac8cdc4d75%22%7D%7D"></video>

The agent also works on multiple portfolio reports. I am testing it with six different portfolios; you can adjust the count as needed. 

Same setup, but with multiple portfolio data. Make sure to copy each one's ID, and use the [financial_agent_prompt_for_multiple portfolio](https://gist.github.com/DevloperHS/06982896ddfc20c8ace1af213dff2ebf#file-financial_agent_prompt_for_multiple-portfolio-txt) for the job.

**Output**

<video src="file://%7B%22source%22%3A%22attachment%3A3634eb31-49b5-4756-85ba-2a9ca5ab79a3%3Afinancial-agent-using-composio-mcp-multiple-usecase.mp4%22%2C%22permissionRecord%22%3A%7B%22table%22%3A%22block%22%2C%22id%22%3A%22360f261a-6dfe-8000-aa89-f2df82c4f004%22%2C%22spaceId%22%3A%2235adec70-dbef-4f42-9214-62ac8cdc4d75%22%7D%7D"></video>

Note that, despite explicitly stating that Claude never created a subagent, this is because composio already has parallel execution enabled and, behind the scenes, uses subagents.

Hope this gave you an idea of how hosted MCP servers work, and tools like Composio MCP provide a universal tooling layer.

However, deploying a hosted MCP server for production is a different game. Let's look at a few rules to help ensure the servers don't break in real-world use.

---

## Deployment Notes for Production MCP Servers

Though we didn't cover a hosted MCP server build, if you want to build your hosted MCP server (local + later deployed), follow the production checklist before you deploy & put an MCP server in front of real users:

- **Logging to stderr or files only**, never stdout for stdio servers
- **Errors caught on every tool**: return a clean error message, don't let exceptions kill the connection
- **Inputs validated**: type hints (Python) or Zod (TypeScript) catch most issues; add explicit checks for things like SQL injection or path traversal if you're doing database/file operations
- **External API calls are rate-limited / Use Composio MCP**: your tool runs whenever the LLM decides to call it, which can be a lot. Let Composio MCP handle it.
- **Secrets in environment variables**, never hardcode in servers, never commit in production.
- **Tool descriptions for the LLM**: Write specific, one-purpose per tool. Five small tools beat one giant tool. The approach we followed.
- **Schema versioning**: when you change a tool's signature, bump your server's version so clients can adapt. Think API versioning.
- **Health check endpoint**: for HTTP servers, expose `/health` for your load balancer. Provides transparency to users.
- **Auth on production HTTP servers**: Use Bearer for internal, OAuth for public. But never both for the same for public/internal (private)

With this, we have come to the end of this comprehensive guide. Here is what matters the most.

---

## Conclusion

As AI transforms software development, MCP will play an increasingly important role in creating seamless, integrated experiences.

Whether you're building custom MCP servers or leveraging pre-built solutions like Composio MCP, the protocol enables powerful enhancements to AI capabilities through external tools and data sources.

The full code for the Python versions is available on GitHub.  

In case you need a few ideas, check out how to build an [MCP client](https://composio.dev/blog/mcp-client-step-by-step-guide-to-building-from-scratch/) that talks to your server, or move up the stack and build an [MCP-powered agent](https://composio.dev/blog/the-complete-guide-to-building-mcp-agents/) that orchestrates multiple servers at once. Happy building.

I hope you had a great learning experience - happy building with Composio! 🚀

---

