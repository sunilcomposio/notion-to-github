LangChain recently introduced [LangSmith Agent Builder](https://smith.langchain.com/), a no-code tool to build agents with natural languages and its making quite the buzz.

Instead of relying on user setting up the nodes and edges, it allows a chat-based interactions, where user just need to define the goal and system generates the prompt, connects tools and setup triggers automatically.

It built on top of [LangChain’s deep agent’s ](https://docs.langchain.com/oss/python/deepagents/overview) framework and supports planning, persistent memory, and multi-step tasks for complex workflow.

However, when it comes to tool’s I felt it pretty limited, even connecting mcp was not straightforward, I had to struggle a bit to use remote MCPs.

So, I did a manual digging and found the issues to both problems, and this blog covers the fix along with few use cases.

Let’s get started!

---

### TL; DR

- **LangSmith** Agent Builder lets you build AI agents through chat instead of wrestling with nodes and edges, but the MCP connection is secretly broken.

- Rube MCP unlocks 900+ tools instantly-Gmail, Calendar, LinkedIn, Exa-without auth headaches or tool selection chaos once you apply the manual fix.

- Email triage becomes autopilot when agents fetch unread Gmail, categorize by urgency, and send consolidated summaries without you lifting a finger.

- Calendar briefings turn intelligent when agents pull today's schedule, research external contacts via LinkedIn, and email you a personalized day-ahead report.

- LinkedIn candidate sourcing gets surgical precision as agents calibrate with 5 samples, refine criteria iteratively, then deliver 30 qualified profiles matching your exact requirements.

---

## Set Up MCP Server in LangSmith Agent Builder

For demonstration, we will go with hosted[ Rube MCP](https://rube.app/) server as it allows me to access 900+ tools without worrying about auth, tool selection, query management and tool calling orchestration. 

Here is what to do:

- Head over to [Rube ](https://rube.app/chat)& Login / Create Account

- In left side panel select Use Rube → MCP URL → Copy it!

- Now head to LangSmith agent builder and Login / Create Account (important that you select no-code experience)

- Within LangSmith Agent Builder, click ⚙️ Settings → MCP Servers → Add MCP Servers

- In the new modal put 

- After few seconds, a pop up will appear asking to verify, hit verify and you need to re-login to rube / the tool.

However, it’s not over yet, you can’t use the tool yet!

If you go now to agent builder workspace, select* Create manually instead,* you will only see default tools (probably a bug), so to fix it, you need to click on MCP → fill in same details and revalidate. 

After few seconds you will see tools listed under the mcp server name you added, in our case it’s RUBE. 

![Rube MCP Server Showing Tools](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_1.png)

> NOTE: Though I used rube mcp server for demo, you can use any other using same technique, till they don’t fix the issue!

Now let’s see how mcp server work together with LangSmith Agent Builder to handle daily odd jobs.

---

## How to Use MCP server with LangSmith Agent Builder 

The process at Agent Builder is slightly different than rest of the tool you might have used!

It actually a single [agent.md](http://agent.md/) file behind the scenes, with addition of folders like tools and skills as needed. 

When you create agent, these file changes in real time based on instructions and can later be reviewed.

However, the above complexity is hidden behind a chat interface that allow user to prompt what they want to build, and the system take care of the rest. 

So, let’s use it for ease of understanding, by building 3 agents (easy, medium, complex).

### 1. Email Triage Agent

The 1st agent will be a simple one, use the agent to fetch all the emails and triage them into Important, General & Rubbish.

Put in the following prompt to in the chat window & hit enter.

```plain text
Build a agent that uses Rube MCP to fetch only unread emails from Gmail. For each email, analyze the sender, subject, and content, then triage into exactly three labels: Important, General, Ignore based on urgency, relevance, and action required. After processing all unread emails, return a single consolidated summary with bullet points grouped by label, including sender, subject, and a one-line intent per email. Do not modify, reply, or archive emails. Output the summary only after the task fully completes.
```

Now the agent builder will ask you few clarifying questions:

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_2.png)

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_3.png)

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_4.png)

Once you answer these, like I have done, it will combine metadata, toolbox, instructions and generate an agent overview. 

Hit create and your agent is created just like that!

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_5.png)

To test the agent, in the left chat window enter:

```plain text
Triage my unread Gmail emails using and email me the summary
```

Result:

As can be seen, it calls the gmail tool via rube mcp we added and did the task for us!

> **Note**: To keep things simple, I have kept the text in markdown (apparently no rendering support in Gmail). Feel free to optimize it further.



### 2. Daily Calendar Briefing Agent

Next up let’s spin up an agent that fetches the calendar and other tools, generates a daily briefer and send it to gmail. 

Enter the following prompt in the chat window:

```plain text
You are a daily calendar assistant. Each morning, get today’s date (dd-mm-yyyy), fetch all events for the day, sort them by time and importance, and identify busy periods, gaps, and back-to-back meetings. Briefly research external meetings if needed using tavily_linkedin_search accessed via rube mcp. Send a concise email summary of the day via Gmail.
```

> Ensure you add the TAVILY_API_KEY & GEMINI_API_KEY by selecting ⚙️ at top as well, for it work fine.

Now to test:

```plain text
send me 21/01/2016 briefing please
```

Results:

As can be seen, it calls the rube calendar & Gmail tool via rube mcp we added and sent me a email. It seems I have a scheduled travel, so it wrote a customized message for me. (ignore markdowns)



### 3. LinkedIn Candidate Sourcing Agent

As for last agent, let’s make an agent that can filter out / source candidate from LinkedIn based on provided criteria. 

We will use rube mcp to call exa-tool and pass in the inputs and let it handle the rest. 

Paste the following prompt in chat window:

```plain text
You are an expert LinkedIn candidate sourcing agent. When a user requests candidates, first gather role requirements (skills, seniority, location, constraints) and ask clarifying questions if needed. Begin with a calibration search of exactly 5 candidates using the Exa search protocol via Rube MCP, returning real LinkedIn profiles only (name, role, company, qualifications, LinkedIn URL). 

Never fabricate data. Ask for feedback and refine criteria iteratively until the user confirms alignment. Only after explicit confirmation, run a full-scale Exa search via Rube MCP (default 30 candidates), list candidates in chat. Exclude candidates already at the hiring company. Clearly state limitations if results are sparse. Prioritize precision, transparency, and iteration over volume.
```

> Ensure you add EXA_API_KEY and GEMINI_API_KEY in ⚙️ at top. Also select Gemini 3.0 Model from Model Selector 

Let’s test it out:

```plain text
Source LinkedIn candidates for a Senior Backend Engineer. Focus on Python, Django, AWS, 5–8 years' experience, based in India.
```

Results:

Amazing it called the exa tool via rube mcp, we added earlier and showed potential candidate in chat based on criteria's.

This is just a glimpse of what is automations are possible with LangSmith Agent Builder & and rube mcp.  Feel free to expand these base examples as per your liking and get rid of those boring mundane task.

Time to look at the final take!

---

## Conclusion

LangSmith Agent Builder paired with Rube MCP turns complex agent workflows into effortless no-code magic, handling everything from email triage to candidate sourcing.

Though the toolset starts limited, a quick MCP server like rube unlocks 900+ tools, letting you automate daily mundane task with chat-based prompts.

So, connect Rube to LangSmith, drop in a goal like "build me a reseach agent…” add API keys, connect to tools/ mcp’s and watch it build, run, and deliver - while you reclaim your time for high-impact work.

Happy Automating.

---

## FAQ

**Q1: How do I fix LangSmith Agent Builder not showing MCP tools after connection?**

A: Click "Create manually" in Agent Builder workspace, select MCP, re-enter server details and revalidate -tools will appear under your MCP server name after verification.

**Q2: What is Rube MCP and why use it with LangSmith Agent Builder?**

A: Rube MCP is a hosted server providing 900+ pre-authenticated tools (Gmail, Calendar, LinkedIn, Exa) that integrate with LangSmith through OAuth 2.1 without manual auth configuration.

**Q3: Can LangSmith Agent Builder create agents without coding experience?**

A: Yes, LangSmith uses chat-based prompts to auto-generate system prompts, connect tools, and build agents—no node setup required, built on LangChain's Deep Agents framework.

**Q4: What agents can I build with LangSmith and Rube MCP integration?**

A: You can build email triage agents, calendar briefing bots, LinkedIn candidate sourcing tools, and any automation combining 900+ tools through natural language instructions.

- name: rube, 

- URL: [https://mcp.notion.com/mcp](https://mcp.notion.com/mcp),

- select OAuth 2.1 & hit save.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_6.png)
