# I Tested 10 Popular AI Skills With ChatGPT: Here's What Actually Works

Many people start with the directory. They open it and connect a dozen of skills because this feels productive. A week later, they use one tool and forget about the permissions they gave to the others.

Keep your setup small. Choose two or three workflows that you use often, and connect one or two apps that contain the information you need.

## **What is a skill?**

A **skill** is a defined process. It tells ChatGPT how to complete a task in the same way each time, and it can contain instructions, examples, and code. You can use it as a standard procedure for a task. This is different from

A **ChatGPT app** gives ChatGPT access to another service. It connects ChatGPT to Gmail, Drive, Slack, or SharePoint, so it can read your information.

A **plugin** can contain skills and apps. Some plugins contain only skills, while others contain a skill and the app that it needs.

Skills use the [open Agent Skills standard](https://www.notion.so/33cf261a6dfe8004925ed6fc48b01aca). Each skill is a folder that contains a `SKILL.md` file. You can write it once and use it in ChatGPT, Codex, Claude, or Cursor.

### **Check these three limits first**

Personal Skills are available on Business, Enterprise, Healthcare, and Edu plans.

An admin must enable Personal Skills on Enterprise and Edu plans. OpenAI started to enable them automatically for Enterprise workspaces on 23 July, unless a workspace chose to disable them. The desktop app and the web app do not sync skills. If you create a skill on the web, you must also add it to the desktop app.

## **How to install a skill**

There are four ways to add one. For most people, asking ChatGPT to create it is the easiest place to start.

1. **Ask ChatGPT to create it**

Go to **Skills → Create → Create with chat**, or request a skill in a regular conversation. Eligible accounts already include `skill-creator`, so you can describe the workflow you want without writing YAML yourself.

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_1.png)

2. **Use the editor**

Go to **Skills → Create → Create with editor** when you want precise control over the instructions and supporting text.

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_2.png)

3. **Upload a folder**

Go to **Create → Upload from your computer** to import a skill from GitHub, a colleague or another trusted source.

   ![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_3.png)

4. **Install a shared skill**

Go to **Skills → Shared with me**, open the **•••** menu for the skill and select **Install**.

### **Review a skill before you install it**

ChatGPT scans uploaded skills, and it approves most of them immediately. It marks some as **Needs Review** and blocks risky ones.

The scan checks for clear problems, but you must also review the skill. A skill can contain instructions and code, so read its files before you install it.

## **How to decide if you need a skill**

Ask yourself one question: Do you explain the same format every week?

If you have typed “pull out the action items and owners” more than three times, you can create a skill for that task.

These 10 skills are useful for many people.

## TL;DR: Top 10 Skills at a Glance

| # | Skill | Source | Best for | Works in ChatGPT? |
| --- | --- | --- | --- | --- |
| 1 | Composio Skills | Composio Official | Giving skills real access to Gmail, Slack, Notion, CRMs and 1,500+ apps | Via MCP in ChatGPT Work |
| 2 | Skill Creator | OpenAI, built in | Creating and improving your own skills | Native |
| 3 | Humanizer | Community (blader) | Stripping AI tells from drafts | Upload the SKILL.md |
| 4 | Writing Guidelines | Vercel | Clearer, more consistent prose | Adapt with Skill Creator |
| 5 | Brand Guidelines | Anthropic | Applying your brand's colours and type consistently | Rebuild with your brand |
| 6 | Theme Factory | Anthropic | Reusable visual themes for decks and docs | Themes are portable text |
| 7 | Notion Meeting Intelligence & Knowledge Capture | OpenAI Plugins (Notion) | Meeting prep and turning chats into wiki pages | Plugin Directory; needs the Notion app |
| 8 | KPI Reporting & Market Sizing | OpenAI Plugins (Data Analytics) | Scorecards, WBR/QBR readouts, TAM/SAM/SOM | Upload individual SKILL.md files |
| 9 | Marketing Skills | Anthropic knowledge-work-plugins | Campaign briefs, competitive briefs, brand review | Upload individual SKILL.md files |
| 10 | Research Paper Review | BESSER-PEARL | Rigorous pre-submission paper review | Documented for ChatGPT |

## 1. Composio: Connect your work apps to ChatGPT in seconds

Composio is interesting because it separates two things that are often bundled together: knowing how to do a workflow and having access to the app required to do it.

3 examples show why this is useful.

[**ChatGPT Gmail Automation**](https://composio.dev/toolkits/gmail) covers workflows such as searching email, replying, creating drafts, working with labels and handling attachments. Instead of repeatedly explaining how you want your inbox handled, you can build a repeatable workflow around it, for example, finding customer emails that still need a response, summarising them and preparing drafts.

[**ChatGPT Notion Automation**](https://composio.dev/toolkits/notion/framework/chatgpt) works with pages, databases, blocks, comments and search. That makes it useful for workflows where ChatGPT needs to do something with the result of a conversation: create a project page, add an item to a database, update documentation or record a decision in the right place.

[**ChatGPT Slack Automation**](https://composio.dev/toolkits/slack/framework/chatgpt)[ ](https://composio.dev/toolkits/slack/framework/chatgpt)covers messages, channels, search, reactions, threads and scheduled messages. A skill could, for example, search a project channel for recent decisions, summarise what changed and prepare or post an update to the appropriate channel.

That is what makes the Composio collection more interesting than a directory of prompts. The skill contains the operating procedure; Composio supplies the connection that lets the agent act on the real service.

#### How to install Composio in ChatGPT

**1. Open the MCP settings**

In ChatGPT Work, go to **Settings** and open the **Plugins** section. Inside the Plugins page, click the **MCP** tab.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_4.png)

**2. Add the server**

Click **Add server**. In the **Name** field, enter **Composio**; for **Type**, select **Streamable HTTP**; and in the **URL** field, paste the Composio MCP server URL:

```plain text
https://connect.composio.dev/mcp
```

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_5.png)

Click **Save**. The Composio server now appears in your MCP plugins list.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_6.png)

**3. Authenticate**

Click **Authenticate**, log in via Composio and click **Allow access** to authorise ChatGPT Work to use your Composio account.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_7.png)

**4. Start using Composio**

That is it. Composio tools are now available in ChatGPT Work. Ask it to work with your connected apps, and it will run the actions you authorise.

#### What you can do after connecting Composio

- Ask ChatGPT to triage this morning's inbox and draft replies to anything from a customer
- Turn a conversation into a Notion page in the right database
- Pull the last week of decisions from a Slack channel and post a summary
- Enrich a lead in your CRM and book the follow-up meeting

**Best use:** repeatable workflows where ChatGPT needs to move between conversation and real work happening in email, documents, project-management tools, CRMs and other external services.

## [2. Skill Creator](https://help.openai.com/en/articles/20001066-skills-in-chatgpt)

**ChatGPT fit: Native. It is already installed on eligible accounts.**

Eligible ChatGPT accounts include OpenAI's `skill-creator` by default. You do not need to find a repository, write YAML frontmatter or install anything.

Ask ChatGPT to create or modify a skill and it invokes Skill Creator automatically. You can also start from **Skills → Create → Create with chat**. It asks follow-up questions, drafts the skill and prompts you to install it.

Skill Creator matters because a good skill is more than a saved prompt. It turns a repeated process into a reusable workflow: what information to collect, what sequence to follow, what reference material to read and what the output should look like.

One detail matters more than it first appears: the **description**. ChatGPT uses the description to decide when a skill should fire. The full instructions only load after that decision.

So this is weak:

```plain text
Reviews reports.
```

This is much better:

```plain text
Review weekly marketing reports for missing metrics, unsupported claims,
inconsistent comparisons and unclear next actions. Use when the user asks
to review, QA or prepare a marketing performance report.
```

The second one tells ChatGPT both what the skill does and when it belongs in the conversation.

**Best use:** build skills around your own repeated work before filling your account with somebody else's.

## [3. Humanizer](https://github.com/blader/humanizer)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_8.png)

**ChatGPT fit: Excellent. It is a single Markdown file with no dependencies.**

Humanizer is the most-starred pure-instruction skill on GitHub, and it does one thing: rewrite AI-sounding text so it reads like a person wrote it, without changing what it says.

The skill is built on the Wikipedia "Signs of AI writing" guide maintained by WikiProject AI Cleanup. It lists 35 named patterns grouped by strength and frequency: staging sentences that signal importance instead of adding a fact, triads and dashes applied by rule, ordinary facts dressed up as pivotal, bold and title case on every list item, and so on. Then it walks through a draft, an audit ("what makes this so obviously AI generated?") and a final pass.

It also supports voice matching. Paste two or three paragraphs of your own writing first, and Humanizer follows your rhythm, word choice and punctuation habits.

**To install in ChatGPT:** download the repository as a ZIP from GitHub, then go to **Skills → Create → Upload from your computer**. The `SKILL.md` sits at the repository root and the repo ships an `agents/openai.yaml` with the display name and default prompt, so it uploads cleanly.

**To use it:**

```plain text
Here's a sample of my writing for voice matching: [2–3 paragraphs]

Now humanize this text: [paste the draft]
```

**Best use:** the last pass on anything ChatGPT drafted that a human will actually read: emails, LinkedIn posts, customer replies, blog intros.

## [4. Writing Guidelines](https://github.com/vercel-labs/agent-skills/tree/main/skills/writing-guidelines)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_9.png)

**ChatGPT fit: Good to adapt. Keep the editorial rules, drop the file-path wrapper.**

Humanizer removes bad patterns. Vercel's Writing Guidelines add good ones. The underlying handbook covers voice, structure, headings, concision, code examples, typography and the AI-writing habits Vercel's own docs team sees most.

The skill around it is shaped for a coding agent: it reads files you point it at, fetches the live guidelines before each review and reports findings as file-and-line references. That wrapper is the part you replace.

**To use it in ChatGPT:**

1. Open the `writing-guidelines` folder and read `SKILL.md` and the reference it points to.
2. Create a Personal Skill with Skill Creator and paste the guidelines in as a supporting reference file.
3. Write a description like: *"Review documents or pasted text against the writing guidelines. Use when asked to review docs, check writing style or audit prose."*
4. Change the output format to identify findings by section, paragraph or quoted sentence instead of file paths and line numbers.

Vercel also publishes a separate **Web Design Guidelines** skill for reviewing interfaces. Do not confuse the two.

**Best use:** anyone who ships docs, help articles or long-form content and wants consistent house style without a human editor on every draft.

## [5. Brand Guidelines](https://github.com/anthropics/skills/tree/main/skills/brand-guidelines)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_10.png)

**ChatGPT fit: Excellent template. Replace Anthropic's brand system with your own.**

Anthropic's Brand Guidelines skill applies Anthropic's visual identity to generated artifacts. It defines a colour palette, heading and body fonts, fallback fonts and rules for applying colour to text, shapes and other elements. It does not contain your company's branding and it is not a general brand-voice skill.

**To read it:** open `https://raw.githubusercontent.com/anthropics/skills/main/skills/brand-guidelines/SKILL.md` in a browser.

**To use it in ChatGPT:** treat the file as a template. Ask Skill Creator to build a Brand Guidelines skill with the same structure, then replace Anthropic's colours and typography with your palette, approved typefaces, spacing and layout rules, logo guidance and the combinations that must never be used. Attach your brand PDF as a reference file if you have one.

The useful idea is not Anthropic's particular visual style. It is turning a static brand guide into instructions an agent applies every time it creates a document, deck or graphic.

**Best use:** teams that generate decks, one-pagers and social graphics in ChatGPT and are tired of fixing the colours afterwards.

## [6. Theme Factory](https://github.com/anthropics/skills/tree/main/skills/theme-factory)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_11.png)

**ChatGPT fit: Highly portable. The theme definitions are plain text you can lift into your own skill.**

Theme Factory is a reusable styling system. Anthropic ships it with ten preset themes, each defining a colour palette, heading font, body font and overall visual identity, and the skill applies the chosen theme consistently across presentations, documents, reports and HTML pages.

The useful part is not the ten themes. It is the idea of turning visual preferences into something an agent can reuse. Instead of asking ChatGPT to "make this look professional" every time, you create themes such as **Board Report**, **Product Launch**, **Editorial** or **Dark Technical**, each with fixed colours, typography and usage rules, and apply the same one whenever you build a new deck.

**To use it in ChatGPT:** the raw file is at `https://raw.githubusercontent.com/anthropics/skills/main/skills/theme-factory/SKILL.md`. The theme definitions are structured design text, so copy the ones you like into a skill of your own, add your own themes, and keep Anthropic's "ask the user to pick a theme first" step. Some implementation details, such as available fonts or how the final file is produced, depend on the tools in your ChatGPT environment.

Related: Anthropic's **Canvas Design** skill in the same repository is worth reading for its two-stage method: define a visual philosophy first (form, space, colour, hierarchy), then produce the artifact. The original leans on a local fonts directory and a code-refinement loop that will not transfer unchanged, but the philosophy-first pattern drops straight into a ChatGPT skill.

**Best use:** build a small library of visual styles you actually reuse instead of describing your preferred colours and typography from scratch every time.

## [7. Notion Meeting Intelligence & Knowledge Capture](https://github.com/openai/plugins/tree/main/plugins/notion)

**ChatGPT fit: Strong. These are OpenAI-published plugin skills built for the Notion app.**

OpenAI's Notion plugin bundles four skills, and two of them are exactly what knowledge workers keep rebuilding by hand:

**`notion-meeting-intelligence`** preps a meeting from workspace context. It confirms the goal, attendees and decisions needed, searches Notion for prior notes, specs and OKRs, picks the right template (status, decision, planning, retro, 1:1, brainstorming), then drafts an agenda or pre-read in Notion with owners, timeboxes and source links.

**`notion-knowledge-capture`** turns a conversation into a durable page. It classifies what you are capturing (decision, how-to, FAQ, wiki entry), finds the right database, extracts facts, decisions and rationale, creates the page against that database's schema and links it from hub pages.

The other two, `notion-research-documentation` and `notion-spec-to-implementation`, are useful if research synthesis or engineering handoff is part of your week.

**To install:** open **Plugins** in ChatGPT and search for Notion. Install the plugin and connect the Notion app when prompted. If your admin imports plugins from GitHub instead, note that this plugin declares an MCP server, so it may be labelled **Desktop only**.

**Best use:** any team that lives in Notion and wants meeting prep and decision logging to happen from the chat where the discussion took place.

## [8. KPI Reporting & Market Sizing](https://github.com/openai/plugins/tree/main/plugins/data-analytics)

**ChatGPT fit: Good. The individual skills are pure instruction files you can upload one at a time.**

OpenAI's Data Analytics plugin ships fifteen skills. The full plugin expects connected warehouses and a notebook runtime, and it declares MCP servers, so do not import the whole thing into web ChatGPT. Two of its skills, though, are plain `SKILL.md` files that work on whatever numbers you paste or upload.

**`kpi-reporting`** produces KPI readouts, scorecards, WBR/MBR/QBR updates and executive summaries. It forces the discipline most weekly updates skip: define the metric contract, pick the right comparison and target, separate validated drivers from guesses, and end with the operating implication.

**`market-sizing`** produces TAM/SAM/SOM estimates with transparent assumptions. It frames the market boundary, chooses a sizing method, separates evidence from assumption, runs sensitivity and states what would most improve confidence.

Both skills tell ChatGPT to stop and ask when a required data source is missing rather than substitute a weaker one, which is the behaviour you want in anything a leadership team will read.

**To install:** download the `kpi-reporting` or `market-sizing` folder from the repository (each is a `SKILL.md` plus an `agents/` folder), zip it and upload it through **Skills → Create → Upload from your computer**. The skills reference sibling skills like `$build-report` and `$visualize-data`; ChatGPT will simply produce the output inline if those are not installed.

**Best use:** anyone who owns a weekly or monthly metrics update, or who gets asked "how big is this market?" more than once a quarter.

## [9. Marketing Skills: Campaign Plan, Competitive Brief, Brand Review](https://github.com/anthropics/knowledge-work-plugins/tree/main/marketing/skills)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_12.png)

**ChatGPT fit: Strong. Written for Claude's desktop app, but the skills are plain Markdown and run anywhere.**

Anthropic's `knowledge-work-plugins` repository is organised by job: sales, marketing, finance, legal, HR, customer support, product management and more. Each plugin's `skills/` folder is a set of `SKILL.md` files that ChatGPT can upload directly. The marketing set is the most immediately useful:

**`campaign-plan`** generates a full campaign brief with objectives, audience, messaging, channel strategy, a week-by-week content calendar with dependencies and success metrics.

**`competitive-brief`** researches competitors and produces a positioning and messaging comparison: recent news, content strategy, review-site themes, a 2x2 positioning matrix and the gaps nobody has claimed.

**`brand-review`** checks a draft against your brand voice, style guide and messaging pillars, flags deviations by severity and gives before/after fixes, including unsubstantiated claims and missing disclaimers.

The same folder has `email-sequence`, `seo-audit`, `performance-report` and `draft-content`. The sales plugin next door has call prep and pipeline review; finance has reconciliation and variance analysis.

**To install:** open the skill folder on GitHub, download it, and upload it via **Skills → Create → Upload from your computer**. Some skills reference a `CONNECTORS.md` placeholder for CRM or analytics tools; delete those lines or point them at your Composio connections.

Once installed, the skills fire on plain requests. For example:

```plain text
Build a campaign plan for our Q4 launch of the new analytics dashboard.
Target: mid-market SaaS ops leads. Budget: $40k. Channels: LinkedIn, email,
webinar. I need a six-week content calendar with owners and success metrics.
```

```plain text
Review this landing-page draft against our brand voice and messaging
pillars. Flag anything off-tone, any unsupported claims and any missing
disclaimers, and give me a before/after fix for each.
```

**Best use:** marketers who want a repeatable brief-to-plan-to-review loop instead of starting from a blank prompt each campaign.

## [10. Research Paper Review](https://github.com/BESSER-PEARL/research-agent-skills)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_13.png)

**ChatGPT fit: Strong. The maintainers document ChatGPT as a supported target.**

Research Paper Review is a focused skill for evaluating an academic paper before submission or revision. Give it a PDF, LaTeX source or pasted text, and optionally the target venue and paper type.

It produces a structured review: summary, strengths, weaknesses, numerical consistency checks, venue-specific recommendations and a prioritised list of changes. That last part is what makes it more useful than a generic paper summary.

Its narrow scope is the advantage. Larger academic skill collections try to cover the whole pipeline from reading to writing and usually assume a Codex-style environment with a filesystem. This one does one repeatable task well, and the maintainers explicitly list ChatGPT among the assistants it works in.

**To install:** open the `research-paper-review/SKILL.md`, either upload the folder through **Skills → Create → Upload from your computer** or paste the contents into Skill Creator. Then provide the paper and, if relevant, the intended venue.

```plain text
Review this paper as a full research-paper submission for NeurIPS.
Check its claims, evidence, numerical consistency, strengths and weaknesses,
then prioritise the revisions.
```

**Best use:** a rigorous second-pass review before submitting or revising an academic paper.

## Also worth a look

Three more that did not make the ten but fit specific jobs:

- **Public Equity Investing** (OpenAI Plugins): 23 skills for listed-company work including `company-tearsheet`, `earnings-deep-dive`, `memo-builder` and `meeting-prep`. No MCP dependency, so it imports cleanly. Narrow audience, very deep.
- **Canva plugin skills** (OpenAI Plugins): `canva-brand-check` and `canva-design-feedback` give structured, read-only critique of a design against your Brand Kit. Needs the Canva app connected.
- **Dropbox plugin skills**: organise, find and share Dropbox content from chat. Useful if Dropbox is where your files live.

## What I would actually install

Do not install all 10. If your work touches live tools, start with **Composio**. Most useful skills need to read an inbox, a channel or a database before they can do anything, and Composio is the layer that gives ChatGPT that access. Then add **Skill Creator** and build one skill around something you repeat every week. The combination is the whole point: the skill defines how the work should be done, and Composio provides the data and the actions to do it.

Add **Humanizer** as the last pass on anything a human will read. From there, pick the one that matches your job: **Notion** if that is where your team's memory lives, **KPI Reporting** if you own a metrics update, the **Marketing skills** if you run campaigns.

Use **Writing Guidelines**, **Brand Guidelines** and **Theme Factory** as reference material. They show how experienced teams encode taste and standards into instructions an agent can follow.

And the rule for evaluating any skill you find in 2026:

**Read the ****`SKILL.md`****, but also read the environment around it.**

If a skill wants a shell, a repository or a `scripts/` folder it runs on its own, it belongs in Codex. If it wants a description, some reference files and your actual work, it belongs in ChatGPT. A good skill tells an agent what to do. A working skill also has access to everything it needs to do it.
