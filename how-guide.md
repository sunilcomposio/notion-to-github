Google recently released Gemini 3, Nano Banana Pro and its agentic IDE Antigravity, and it's exploding in popularity. AI, Builders & Business community are hyping it up like crazy. 

But when it comes to tools, it's limited. Though one can add MCPs for use cases, it's hectic to connect every applications MCP servers manually

So, what if one has an MCP that you configure once, and it connects to 500+ products at once while intelligently figuring out which MCP tools and methods are needed?

Enter Rube, a universal MCP & in this short blog, let's see how you can use it to supercharge your AI-assisted development.

### TL DR;

1. Rube MCP plugs into Antigravity once and instantly unlocks hundreds of tools without the annoying manual MCP wiring.

1. You can automate code reviews and send off clean summaries straight to Gmail / Slack using Rube’s mail tools.

1. Backend setup becomes trivial when Rube creates and seeds Supabase databases with text instructions and LLM commands.

1. Frontend devs can turn Figma wireframes into clean HTML/CSS by letting Rube fetch designs and Antigravity generate UI code.

1. Antigravity + Rube turns repetitive dev work into background noise so you can actually focus on building.

---

## Connect Antigravity with Rube MCP

Before doing anything else, one needs to connect it once. Process is straightforward:

- Go to[ Rube MCP](https://rube.app/) site.

- Signup / Login

- Head to Add Rube → MCP URL → Generate Token

- Copy the code to put in place of  `RUBE API KEY` in `mcp.json`.

- Head back to Antigravity, `…` dot in prompt editor → MCP -> Manage MCP Server → View raw config

- Paste the command in [mcp.json ](https://gist.github.com/DevloperHS/19fb43ee702fd736a7f252da9ca9ec60#file-mcp-json)

- Perform OAuth, wait till it completes

- and boom, Rube added to the MCP list.

For verification, head to Manage MCP Servers, refresh and check the tool. It will look something like this:

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_1.png)

> Note: if you are new to MCP, don't change anything. If new to rube, for first time it ask for OAuth for each tool.

Now let's see a few use cases of  Rube in Antigravity.

---

## 1. Using Rube MCP to automate Code Review & Update 

Senior developer often analyse intern codebases, pointing out errors, summarising them and sending them to their Slack/email.

This can be a time-consuming, mundane & repetitive task. So, they can delegate it to Antigravity + Rube combo.

So here is a prompt to paste in the agent window prompt box (much like VS Code)

**Prompt**

```plain text
Find the bugs in @agent.py @requirements.txt @__init__.py . 
Categorise them in low , medium high priority and create a short summary analysis with proper formating. 
Then using @mcp:rube_mcp: Send the summary to devloper.hs2015@gmail.com with title "Code Analysis Summary".
```

In simple words, the prompt asks the LLM to review the codebase, identify bugs, generate a summary, and send it as an email to the user. The output should include a summary and a mail link. And all this should be done using rube’s Gmail tool.

Yup, we don't have Gmail support in Antigravity, but rube helps here!

Now click `>` And wait till it's complete. If this is 1st time, rube will ask you to connect your Gmail account to send mail, but that's just OAuth for you.

Finally, it will come back with the draft/task it performed and an email link. Pretty transparent 😅

Here I am performing the same:

> Note: You can even ask it to send to team slack, but I went with gmail, as this is just a demo.

Hopefully, now the intern will find the email and fix it all and send a pull request again :)



---

## 3. Using Rube MCP to handle Supabase Database for Apps

Backend developers often need to create different database models for different projects. This can be time-consuming, mundane and repetitive.

They can leverage Rube MCP to connect to Supabase, build the database in one shot, and use APIs to pull real-time information. Best part - no manual handling of pesky database issues, all handled by rube!

Here is a prompt to do so:

```plain text
Build a minimal Student Grade CRUD application using Flask, Jinja, HTML, and CSS. The aesthetic should be "Google Material Minimal"—clean white backgrounds, subtle shadows, rounded corners, and excellent typography.

Phase 1: Database Setup (via rube_mcp)
Use the `rube_mcp` tool to interact with Supabase.
1. Create a table named `students` in the database `student-data`.
2. The schema should include: `id` (serial/int, primary key), `name` (text), `subject` (text), `grade` (int), and `last_updated` (timestamp).
3. Immediately seed this table with 5-10 realistic mock entries (e.g., "Alice Smith", "Mathematics", 92).

Phase 2: Flask Application
Create a single-file Flask app (or standard folder structure if preferred) that connects to this Supabase instance.
1. API/Routes:
   - GET `/`: Render the dashboard showing all students.
   - POST `/add`: Add a new student.
   - POST `/update/<id>`: Update a student's grade.
   - POST `/delete/<id>`: Remove a student.
2. UI/UX:
   - Use Jinja2 for templating.
   - Style using vanilla CSS (no external frameworks like Bootstrap).
   - Design: Center-aligned card layout. The table should look like a Google Docs file list or Google Classroom roster. Use soft gray borders (#e0e0e0) and the system font stack (Inter/Roboto/San Francisco).
   - Add a "Add Student" floating action button (FAB) or a clean top bar button.

Ensure the code is production-ready, handles database connections securely, and renders the frontend cleanly.
```

**In summary: **prompt the LLM to create a Google minimal style UI-UX-based CRUD app (for demo purposes) and pull in mock data defined in Supabase’s `student-db` using crud api’s. Here, the database was created using `rube_mcp`. 
Yup, we don't have Supabase support in Antigravity, but rube helps here!.

Here is how it worked for me! 

> NOTE: For demo purposes, I have used a CRUD app example, but same flow can be expanded to complex project as well

**Pro Tip**

Always provide enough context for the LLM to perform the job as expected. For this example, this translates to adding: 

- db name - student-db

- column name - id, name, subject, grade, last_updated

- mock data - in tuple pairs.

With this power in hand, backend devs can create a robust and scalable database by just giving the right instructions!

Now off to the final use case of today!

---

## 3. Using Rube MCP to generate Frontend Code from Figma Mockups

Frontend developers often have to convert the Figma designs shared into webpages. They can leverage Composio’s Rube MCP to connect to Figma, fetch the designs, and generate the frontend code for them—best fact: no need to handle the conversion details; all done by rube.

Here is a prompt to do so!

**Mockup **(used for demo purposes only)

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how/image_2.png)

**Prompt**

```plain text
Here is the figma file url https://www.figma.com/design/CHX6G247vkQFDYh84Qv9CS/Low-fi-Wireframe-Template--Community-?node-id=123-0&p=f&t=6OH8UhfkjgLRPLpE-0. 

Implement a blog based ui base on the given wireframe. Keep ui minimal and clean, similar to google (materialistic design). 

Only Tech stack to be used: HTML, CSS. Create the project in new folder called blog-space. I only need Mocup UI for client. 

Make sure to fetch the file using rube_mcp and then only start designing
```

In a nutshell, the prompt provides the LLM with a Figma URL and asks it to replicate the wireframe into an HTML blog page with CSS, all in the same folder. 

Again, we don't have Figma support in Antigravity, but rube helps here!.

**Output**

**Pro Tip **

For generating a complex frontend, follow these guidelines for optimised results:

- Name the layers; include additional details, such as row and column numbers, in the layer names themselves. E.g `memo-card-grid-row0-col0` (the leftmost corner).

- Name the colour styles. E.g. ❌ #FFFFFF, ✔️ background-color-white

- Name the file properly. Please don’t keep it generic.

These guidelines provide the model with enough context to generate the complex frontend architecture that developers are expected to deliver. 

These were some use cases; the list is endless. But here is the final verdict!

---

## Conclusion

With Antigravity and Rube, the repetitive task became child’s play, and the best part is that this same workflow can be incorporated into various domains.

Though Antigravity is new, it packs some serious punch and with Rube MCP as a partner, the only limit is your imagination.

So, head to Rube MCP, connect it to Antigravity, pull up some PRD doc, give it to Gemini 3 / Nano Banana Pro with a kickass prompt, and let the tool do its thing while you focus on ideating and planning the part. 

Happy Building.

---

## FAQ

**Q1. How do I integrate Rube MCP into Antigravity?**

**Answer:** Generate a token from the Rube dashboard and paste the provided command directly into your Antigravity’s `mcp.json` raw config.



**Q2. How do I verify that Rube is successfully connected to Antigravity?**

**Answer:** Head to "Manage MCP Servers" in the editor, refresh the page, and confirm that the Rube tool appears in your active list.


**Q3**. **Do I need to configure credentials for all 500+ tools manually?**

Answer: No, you configure Rube once, and it dynamically handles connections, requiring only a one-time OAuth login the first time you use a specific tool.



**Q4. Can Rube handle tasks for platforms not natively supported by Antigravity?**

**Answer:** Absolutely; Rube acts as a bridge to fetch data and perform actions on external platforms like Figma, Supabase, and Gmail that the native IDE doesn't support yet.



**Q5**. **How can I optimise the code generation quality for complex tasks like Figma conversions?** **Answer:** Ensure you provide rich context, such as clearly naming Figma layers and colour styles, so the model understands the specific structural requirements.








