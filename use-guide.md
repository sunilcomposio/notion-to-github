## The Rise of Codex

OpenAI's Codex has taken AI coding by storm, powering IDE, CLI, and cloud agents that write out code, fix bugs, and handle PRs at lightning speed. 

Developers everywhere are buzzing about its game-changing potential for faster builds. Recently saw an X tweet claiming they will not go with Claude's code because Codex is better at planning first.

But here's the catch with MCP integrations: Codex shines when pulling real-time context from external tools, yet wiring up each MCP server for GitHub, docs, or databases one by one is a tedious config nightmare that kills momentum.

What if you could plug in one MCP server that instantly unlocks 500+ apps, auto-selecting the perfect tools and methods for any task?

Enter Rube, the universal MCP hub. In this short blog, let's see how you can use it to supercharge your OpenAI Codex workflow across IDE, CLI, and the cloud.

## How to set up MCP for Codex

The benefit of Codex is that it can run in all 3 modes:

- **GUI Mode**: You can uninstall the Codex extension in VS Code, Cursor, or Windsurf. It reads your workspace, edits code, and runs tasks right in your favourite IDE.

- **CLI Mode**: Terminal fans can install Codex CLI via `npm install -g @openai/codex`, then run `codex` In their project for local agentic coding.

- **Cloud Mode**: Power users can delegate heavy tasks to Codex Cloud from the IDE or GitHub (@codex tags), freeing them to focus while it handles builds remotely. This is a unique edge over rivals.

- **Codex App**: The new Codex app. A very beautiful UI that's very intuitive to work with.

But to use rube, all requires a bit of setup, here is how to do it for all modes!

### 1. Set up MCP in Codex GUI Mode

I thought connecting remote MCP servers to the Codex extension was easy, but it's a major [issue](https://github.com/openai/codex/issues/6465), an open one right now on GitHub.
So, here's the clean workaround I found to use MCP with extension:

- Go to [Rube](https://rube.app/), sign up/log in, or whatever MCP servers you want to use.

- Head to [Rube Chat](https://rube.app/chat) → Use Rube → MCP URL → Generate Token. (You can use any other MCP URL)

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/use/image_1.png)

- Copy the token

- Install the [Codex extension](https://marketplace.visualstudio.com/items?itemName=OpenAI.chatgpt) for VS Code and log in.

- Then: ⚙️ → MCP Settings → + Add servers → **Streamable** **HTTP**:

- Put in the following details: 

  - Name: rube

  - URL: [https://rube.app/mcp](https://rube.app/mcp)

  - Headers:

    - Key: Authorisation

    - Value: Bearer <your-token>

- Save → Refresh → Authenticate → Done!

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/use/image_2.png)

To verify if mcp server is enabled or not, paste the following in the prompt box:

```plain text
Do you have access to [mcp-server]? Tell me all the task it can perform.
```

If it fetches the result with all tasks it can do or mcp details, you are all set.

You can check out the [config.toml](https://gist.github.com/DevloperHS/0bd80665579d80fccac163192c5b6448#file-config-toml) file for same, by going to ⚙️ → MCP Settings → Open config.toml file.

Now let’s set up the same for CLI.

### 2. Set up MCP in Codex CLI Mode

Terminal lovers, in the codex, we have 2 ways to setup mcp in the codex CLI:

- Config file - `config.toml` (manual, explicit)

- CLI commands (faster, recommended)

However, I find the **config file** offers full control in a single file and feels native.

Here is how to use the config file approach:

- Install the codex CLI using: `npm i -g @openai/codex`  (Make sure you have `npm` install)

- Initialise a project root directory. I used `codex-test`

- Create a folder named `codex` using `mkdir` . 

- Inside it creates a new file called `config.toml` and paste the following text from below and save it!

```plain text
[projects."/home/devloper_hs/codex-tests"]
trust_level = "untrusted"

[mcp_servers.rube]
enabled = true
url = "https://rube.app/mcp"

[mcp_servers.rube.http_headers]
Authorization = "Bearer <auth-token>"
```

**Notes**

- Replace the project path with your actual absolute path

- Replace `<auth-token>` with the token you already generated earlier

- `trust_level = "untrusted"` is the safe default

But those tied to terminal commands you can use:

```c#
codex mcp login <server-name> 
```

for streamable-http based mcp servers*. ****Make sure to authenticate!***

or stdio  based mcp servers  

```c#
codex mcp add <server-name> --env VAR1=VALUE1 --env VAR2=VALUE2 -- <stdio server-command>
```

Example: 

```c#
codex mcp add context7 -- npx -y @upstash/context7-mcp 
```

Next, let’s verify the connection.

### Verify Installation

Head to the root of the file and run `codex`, login and run `/mcp`

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/use/image_3.png)

To ensure it uses the MCP server, you can use the following prompt validation flow:

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/use/image_4.png)

> I liked the fact that instead of giving me rouge output, its actually gave me steps to validate. Btw I already validated the gmail earlier.

Now is the time to set up MCP for the codex using cloud mode.

### 3. Set up MCP in Codex Cloud Mode

Codex allows delegating tasks to a cloud instance, letting developers focus on other tasks.

Sadly, this has been long planned but not shipped. The blockers being: secure tool proxying, credential forwarding, deterministic replays and more.

A smart approach is to use the MCP process locally, then delegate responses and the rest of the work to the cloud.

### 4. Set up MCP in the Codex App

- Click on the Settings gear icon on the bottom left.

- Go to MCP Servers. You’ll see some servers that you can authenticate and use.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/use/image_5.png)

- Or, add server

- Choose STDIO or Streamable HTTP

- Add your MCP server

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/use/image_6.png)

- And you’re done.

So, with this, we are all set to look at a few use cases of Codex!

---

## Use Cases of Codex + MCP

Codex is mainly meant for coding, but with [Rube MCP](/25df261a6dfe804791b6fe4a3709d774) it can handle a variety of tasks. Here are 3 use cases I like to use as a developer.

### 1. Organising Old Project Commits into an Existing Sheet for Refresher

Integrations: [Google Sheet ](https://composio.dev/toolkits/googlesheet)+ [GitLab](https://composio.dev/toolkits/gitlab)

```plain text
Create a new googsheet and add last 10 code activity summary (PRs, commits, stale branches) from my [project] GitHub and local repository. Also give me the sheet link at the last.
```

& here is the output:

This gives me a rough idea of where I left off and start from there.  No more mundane checks.

But the use cases don’t stop here.

### 2. Creating Changelogs / Project Report on the fly

Integrations used: [GitHub](https://composio.dev/toolkits/github), [Google Doc](https://composio.dev/toolkits/googledoc)s

```plain text
Create a new Google Doc and generate a clean changelog report for my [project] GitHub repository using the all merged commits in anice formatted way and give me doc link.
```

& here is the output

This allows me to turn my project changelogs into a well-formatted Google Doc and send them the pdf / Doc. They can use it to verify the progress, ask for further clarification or suggest some changes

By far, I have shared productivity stuff; it's time for an actual development use case!

### 3. Extracting Design Tokens from Figma File

Integrations used:[ Figma](https://composio.dev/toolkits/figma)

This one has been game changer for me.

To be frank, I am not good with frontend, so I take inspiration form figma designs. However, manually converting the design to code was a headache.

But now I fire up Codex and ask it to extract design tokens I can use to replicate the frontend in a similar style.

```markdown
Analyze the Figma design file at https://www.figma.com/design/ZoE3NGZObwwNBr9Iyv8X9G/Shopping-Website -- Community -? node-id=2-
tokens), and generate a markdown file named design.md at root of project containing a professional design system reference.
Include token definitions, usage guidelines, naming conventions, and example mappings suitable for engineers to directly
implement in code.
```

Here’s the output

Ofc, I make a few changes here and there before implementing, but this now codex + rube combo makes life so easier.

And here is a bonus one for content creators!

### Turning Codex into your Content Creator Devlogs in Medium Blogs Draft

Integrations used:[ ](https://composio.dev/toolkits/figma)[Google Docs](https://composio.dev/toolkits/googledocs), [Notion](https://composio.dev/toolkits/notion)

Many developers love writing devlogs (summaries of what they did) while building a product. But with Codex + mcp, you can analyse each dev log on a day-by-day basis and create a blog that you can then share with the world. Similar to how big companies. 

Here is the prompt I used:

```plain text
You are tasked with creating an SEO-optimized blog post from development logs for Medium publication.

**Requirements:**
1. Read the @devlog file content based on chronological order (earliest to latest based on days)
2. Transform technical devlogs into a narrative-driven blog post with personal storytelling
3. Use Rube MCP's googledocs tool to create and format the document
4. Follow this structure:
   - Engaging title with primary keyword
   - Compelling hook/introduction
   - Main body with 3-5 naturally flowing sections
   - Personal insights and lessons learned
   - Conclusion with takeaways
   - Call-to-action

**Writing Style:**
- First-person perspective ("I discovered...", "My journey with...")
- Story-driven narrative that connects technical milestones
- Natural transitions between sections (avoid forced segmentation)
- Balance technical detail with accessibility
- Include challenges, breakthroughs, and reflections

**SEO Optimization:**
- Include relevant keywords naturally in headers and content
- Use H2/H3 headings for structure
- Add meta description (150-160 characters)
- Include 3-5 tags for Medium
- Optimize readability (short paragraphs, varied sentence length)

**Formatting in Google Docs:**
- Bold for emphasis and subheadings
- Italics for technical terms or emphasis
- Bullet points for key takeaways
- Code blocks where appropriate
- Proper spacing between sections

Execute the task by:
1. Analyzing devlog files chronologically
2. Creating the narrative structure
3. Applying proper formatting
4. Writing the blog post with Rube MCP googledocs tool

Output the Google Docs URL when complete.
```

And here is the output it created: 


By default, Codex doesn’t have access to Google Docs, but MCP can help with that. So, we let Codex handle the generation process, and rube handled the Google Docs. 

In short, this process allows anyone to turn their weekly builds into a full-fledged blogpost or content worth sharing.

I hope you got an idea of what’s all possible.

Now let’s look at the final lesson!

## Final Thoughts

Even with vibe coding pushing software development forward, most people still treat Codex/MCP like a magic box. That’s the actual problem.

These tools don’t work magically. They need **clear, structured prompts** to give good output. Most people don’t have that skill yet, so results fall apart.

That’s where tools like Rube help - by enforcing structure and combining prompts with real tool support, instead of guesswork.

Going forward, this becomes the new normal. Using these tools well won’t be optional.

So, don’t waste time. Go to [**rube.app**](http://rube.app/), connect it with Codex, and start building or automating your work.
