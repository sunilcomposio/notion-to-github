OpenCode has recently gained significant popularity in the open-source space. It’s an alternative to Claude Code.

But what if I told you could 100x your Opencode experience with just one MCP?  Yes, with [Rube](https://rube.app/), that’s totally possible. But wait, what is possible?

After building a thousand managed MCP integrations and speaking with countless users, we found that while MCP is a force multiplier, it still has physical limitations. Adding even a single GitHub server will take 20k tokens from your LLM's context window; adding Jira/Linear, Supabase, etc., will essentially choke the models. There is nothing novel in it; most industry folks are already aware of this. 

So, how do we solve this? 

By exposing a few meta tools (Search, Planner, Bash, Remote Workbench, etc). When the agent searches for tools, the search tool fetches only relevant tools from Composio-managed apps, ensuring the LLM's context space remains clean. 

For complex issues, it uses a remote workbench or a bash script to chain multiple tools together, and instead of dumping the large artefacts directly into the context, Rube stores them in a file system and fetches the results as needed. 

There’s more to it: OAuth handling, healing loop when tool execution fails, and more. We are doing a full technical write-up for the same, will share on socials, so do [follow us ](https://x.com/composio)for Rube/Tool router breakdown.

Here in this blog post, I’ll walk you through how to set up Rube MCP with **Opencode**.

### TL DR;

- Rube MCP plugs into **OpenCode** once and instantly unlocks hundreds of tools without the annoying manual MCP wiring.

- Automate code reviews and send off clean summaries straight to Gmail or Slack using Rube’s mail tools.

- Backend setup becomes trivial when Rube creates and seeds Supabase databases with text instructions and LLM commands.

- Turn project insights into polished X threads and auto-save them into Notion with a single prompt.

- OpenCode + Rube turns repetitive dev work into background noise so you can actually focus on building.

---

## Install OpenCode

Let’s start with installing OpenCode.

Head to the terminal and install it using:

```shell
npm i -g opencode-ai
```

or use for the latest:

```shell
npm i -g opencode-ai@latest
```

Once done, verify it by:

```shell
opencode
```

It will open the TUI like this:

![opencode](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_1.png)

Now let’s connect it with Rube MCP.

---

## Connect OpenCode with Rube MCP

Connecting rube mcp with OpenCode is straightforward:

- Go to[ the Rube MCP](https://rube.app/) site.

- Signup / Login

- Head to Add Rube → MCP URL → Generate Token and copy it.

- Create a new folder `open-code` and open it.

- Inside, create a new file called `opencode.json` 

- Open it with File Explorer, nano/vim, and paste the following code. 

```shell
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "rube_mcp": {
      "type": "remote",
      "url": "https://rube.app/mcp",
      "enabled": true,
      "headers": {
        "Authorization": "Bearer <api-token>"
      }
    }
  }
}
```

Now open the OpenCode again and type `/mcp` and it will show:

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_2.png)

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_3.png)

You can even verify this using prompting OpenCode:

```shell
Do you have access to Rube MCP?
```

If you get an answer like this, it means we are all set:

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_4.png)

> Note: if you are new to MCP, don't change anything. If new to rube, for first time it ask for OAuth for each tool.

Now let's see a few use cases of Rube in OpenCode.

---

## 1. Using OpenCode to automate Code Review & Update 

For the 1st task, we will ask OpenCode to find bugs across all files in a code repository, prepare a report, and send it to [Gmail](https://rube.app/apps). (can use [Slack](https://rube.app/apps)[ ](https://rube.app/apps)as well)

Paste the following prompt:

```shell
Act as an autonomous code auditor. Given access to a source code @ repository, recursively scan all files, detect bugs, vulnerabilities, logical errors, performance issues, and bad practices, classify them by severity, suggest concrete fixes, and generate a structured report (summary, critical issues, file-wise findings, recommendations). Use Rube MCP to handle the Gmail task and send the full report to devloper.hs2015@gmail.com. At the end, output a concise execution summary of all tasks completed. Prioritize real execution over explanation; keep results actionable and production-ready.
```

In simple terms, the prompt asks the LLM to review the codebase, identify bugs, generate a summary, and email it to the user. The output should include a summary and a link to email. And all this should be done using rube’s Gmail tool. 

Yup, we don't have Gmail support in Antigravity, but rube helps here!

And here’s the Output it generated:

> Note: you may need to login to Gmail, if using first time.

For simplicity, the report is kept short. Feel free to expand it using a format you prefer. 

Now let’s look at the 2nd use case!

---

## 2. Using Rube MCP to handle Supabase Database for Apps

For the next task, let’s ask OpenCode to handle the database creation via Rube MCP for a vehicle parking management app. 

Paste the following prompt:

```shell
Build a minimal Parking Management System using Python, Supabase, HTML, and CSS with a Google Material Minimal aesthetic.

Project Setup
Create a new folder named `vms` and build the entire application inside it.
Create a `.venv` using `python3` and activate it before installing dependencies.
Use Supabase project ID: `<supabase-project-id>`.

Phase 1: Database Setup (via rube_mcp)
Use rube_mcp to create database `parking-system` and models:
User(id, name, email, created_at)
Admin(id → User.id, role, created_at) [predefined]
ParkingLot(id, name, location, total_spots)
ParkingSpot(id, lot_id → ParkingLot.id, spot_number, is_available)
Reservation(id, user_id → User.id, spot_id → ParkingSpot.id, start_time, end_time, status)
Establish all relationships and seed: 1 Admin, 3 ParkingLots, 10–15 Spots per lot, 5–10 Users, 8–12 Reservations.

Constraints
All Supabase operations via rube_mcp, production-ready code, env vars for secrets, clean error handling.

```

**In summary: **prompt the LLM to create a parking system model and define the relationship, and then pull in mock data defined in Supabase’s `student-db` using crud api’s. 

Yup, we don't have Supabase support in OpenCode, but rube helps here!

Here is the output:

Notice how OpenCode defined the task, and worked on it incrementally, while OpenCode + rube mcp handled the heavy lifting

Now let’s look at the final individual task, which we will do with OpenCode + Rube

---

## 3. Using Rube MCP to generate a Build Log-based Social Media Post for X

For the final task, we will ask OpenCode to draft us an X thread by analysing the entire project (handled by OpenCode) and save it to Notion (handled by rube mcp)
Here is the prompt:

```shell
Analyze the entire @vehicle-parking-app project to identify its key problems, milestones, and achievements. Then, write a 5-tweet engaging X(Twitter)thread using a problem–solution narrative. The first tweet should hook readers, and the last should include a call-to-action. Keep each tweet under 250 characters, separated with "1/". Use only bold or italic formatting (no headings or code). Save the final formatted thread to the Notion page at https://www.notion.so/appdev1/Project-Devlogs-2e3b43216e448044941ec4ea71ca30cc. Use rube_mcp for notion. 
```

In essence, the prompt asks OpenCode to analyse the project, find its key problems, milestones and achievements, use them to generate an engaging X thread based on given instructions and save it to the given Notion sheet.
How it worked out for me:

The OpenCode didn’t just create the task:

-  It launched its subagents to focus on analysing the project 

- while it performs the notion page retrieval, 

- Then use the results of the subagent to generate the thread and 

- Add it into given Notion page

Pretty handy, isn’t it?

But so far, we have looked at individual use cases. OpenCode + rube combination can do more. Let’s give it a final challenge!

---

## Capstone Task

For the final task, let’s ask rube to create an image tools webapp that has 3 -4 functionalities, similar to [pinetools.com](http://pinetools.com/), but in Python, Flask, core web dev frameworks and most heavy lifting done by rube integrations for AI tools. 

Here goes the prompt:

```shell
Build a simple MVP-level image creation web app as a new project named image-tools. 

Inside the project, first create and activate a Python 3 virtual environment (venv), then build the full application. 

Use Python (Flask) for the backend, SQLAlchemy for the local database, and HTML, CSS, and vanilla JavaScript for the frontend. 

The UI/UX should follow a Google-inspired minimalist design — clean, centered, and modern. 

The app will have four AI-powered tools: Post Complement Image Creation, Theme Change, Background Removal, and Prompt-Based Image Generation. 

All image creation features should use Google Gemini API through Rube MCP (gemini) integration, where clicking a tool’s button triggers Rube MCP in the backend to process the request and return results. 

Flask handles routing and data flow, while Rube manages all AI automation for scalable, real-time image generation.

Follow the best security practices.
```

In a nutshell, the prompt asks OpenCode to build an image editing tool that can use rube mcp to:

- Generate complement image to the provided post

- Change the theme of a given image (defaults)

- Remove the background of the provided image 

- Generate an image based on a prompt

while keeping the UI**-UX** minimal and simple (google inspired)

Here is the output:

Not only did OpenCode make the website one-shot with the power of Opus 4.5, but it also handled the DB (SQLite), multiple tool integrations, state handling, Ruber MCP routing, and UI/UX. 

This concludes our final build. Here are a few of my final thoughts on OpenCode + rube integrations.

---

## Conclusion

With OpenCode and Rube, the repetitive task became child’s play, and the best part is that this same workflow can be incorporated into various domains.

Though OpenCode has been developed for the last 15 years, it packs some serious punch and with Rube MCP as a partner, the only limit is your imagination.

So, head to Rube MCP, connect it to OpenCode, pull up some PRD doc, give it to Claude opus 4.5 / Claude sonnet 3.5 / Gemini3 models with a kickass prompt, and let the tool do its thing while you focus on software/automation architecting part. 

Happy Building.

---

## FAQ

**Q1. How do I integrate Rube MCP into OpenCode?**

**Answer:** Generate a token from the Rube dashboard and paste the provided code directly into your `opencode.json` config.



**Q2. How do I verify that Rube is successfully connected to OpenCode?**

**Answer:** In OpenCode check the bottom or use `/mcp` command editor, in the popup, and confirm that the Rube tool appears in your active list.


**Q3**. **Do I need to configure credentials for all 500+ tools manually?**

Answer: No, you configure Rube once, and it dynamically handles connections, requiring only a one-time OAuth login the first time you use a specific tool.



**Q4. Can Rube handle tasks for platforms not natively supported by Antigravity?**

**Answer:** Absolutely; Rube acts as a bridge to fetch data and perform actions on external platforms like Figma, Supabase, and Gmail that the native IDE doesn't support yet.



**Q5**. **How can I optimize the code generation quality for complex tasks like Figma conversions?** **Answer:** Ensure you provide rich context, such as clearly naming Figma layers and color styles, so the model understands the specific structural requirements.

- Replace the `<api-token>` with copied token and save it:
