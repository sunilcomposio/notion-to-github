# I Tested 10 Popular AI Skills With ChatGPT: Here's What Actually Works

Many people start with the directory. They open it and connect a dozen of skills because this feels productive. A week later, they use one tool and forget about the permissions they gave to the others.

Keep your setup small. Choose two or three workflows that you use often, and connect one or two apps that contain the information you need.

## What a skill is

[OpenAI changed these terms in July](https://www.notion.so/3a4f261a6dfe80a18000db60ee7ca655), but many online guides still use the old terms. Here is a simple explanation.

A **skill** is a defined process. It tells ChatGPT how to complete a task in the same way each time, and it can contain instructions, examples, and code. You can use it as a standard procedure for a task.

A [**ChatGPT app**](https://www.notion.so/29df261a6dfe8027abb8ca6f553a6e20) gives ChatGPT access to another service. It connects ChatGPT to Gmail, Drive, Slack, or SharePoint, so it can read your information.

A [**plugin**](https://www.notion.so/3b4f261a6dfe80f7bd03cce697d8bc81) can contain skills and apps. Some plugins contain only skills, while others contain a skill and the app that it needs.

Skills use the [open Agent Skills standard](https://www.notion.so/33cf261a6dfe8004925ed6fc48b01aca). Each skill is a folder that contains a `SKILL.md` file. You can write it once and use it in ChatGPT, Codex, Claude, or Cursor.

## Check these three limits first

Personal Skills are available on Business, Enterprise, Healthcare, and Edu plans.

An admin must enable Personal Skills on Enterprise and Edu plans. OpenAI started to enable them automatically for Enterprise workspaces on 23 July, unless a workspace chose to disable them. The desktop app and the web app do not sync skills. If you create a skill on the web, you must also add it to the desktop app.

## How to install a skill

You can install a skill in four ways. The first option is suitable for most people.

1. **Ask ChatGPT to create it**

Go to **Skills → Create → Create with chat**, or ask ChatGPT to create a skill in a normal conversation. Eligible accounts already use a `skill-creator` skill, so you do not have to write YAML. Describe what you need, and ChatGPT will create the file.

2. **Use the editor**

Go to **Skills → Create → Create with editor** if you want to control the exact text.

3. **Upload a folder**

Go to **Create → Upload from your computer** to add a skill from GitHub or a colleague.

4. **Install a shared skill**

Go to **Skills → Shared with me**, select the **•••** menu for the skill, and select **Install**.

### Find and share your skills

You can find your skills under **Plugins → Skills** in the sidebar. You can also go to `chatgpt.com/skills`.

To share a skill, select **•••** and then select **Share**. Choose people or groups, or copy the link.

### Review a skill before you install it

ChatGPT scans uploaded skills, and it approves most of them immediately. It marks some as **Needs Review** and blocks risky ones.

The scan checks for clear problems, but you must also review the skill. A skill can contain instructions and code, so read its files before you install it.

## How to decide if you need a skill

Ask yourself one question: Do you explain the same format every week?

If you have typed “pull out the action items and owners” more than three times, you can create a skill for that task.

These 10 skills are useful for many people.

## **TL;DR: The 10 Skills at a Glance**

| **#** | **Skill** | **Source** | **Best For** | **Works in ChatGPT?** |
| --- | --- | --- | --- | --- |
| 1 | Skill Creator | OpenAI official | Creating and improving other ChatGPT Skills | Native |
| 2 | Composio Skills | Composio official | Repeatable workflows involving external apps and tools | Useful with MCP setup |
| 3 | Impeccable | Community | Design critique, visual polish, layouts | Coding-agent focused |
| 4 | Writing Guidelines | Vercel official | Clearer, more consistent writing | Adaptable (copy rules) |
| 5 | Brand Guidelines | Anthropic official | Applying brand voice and visual standards | Rebuild with your brand |
| 6 | Canvas Design | Anthropic official | Posters, graphics, visual deliverables | Partly portable |
| 7 | Theme Factory | Anthropic official | Consistent themes across presentations and assets | Themes portable, Artifacts not |
| 8 | Matt Pocock Skills | Community | Structured thinking, review, problem-solving | Mostly coding-agent focused |
| 9 | Superpowers | Community | Planning and structured workflows | Codex/coding agents |
| 10 | Academic Research Skills | Community | Research pipeline: reading → writing → review | Codex-first; adaptable |

## [1. Skill Creator](https://github.com/openai/skills/tree/main/skills)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_1.png)

**ChatGPT fit: Native**

Eligible ChatGPT accounts include OpenAI's `skill-creator` by default. You do not need to find a repository, write the YAML frontmatter yourself or install a separate package.

Ask ChatGPT to create or modify a skill and it can invoke Skill Creator automatically. You can also start from **Skills → Create → Create with chat**.

Skill Creator is useful because a good skill is more than a saved prompt. It can turn a repeated process into a reusable workflow: what information to collect, what sequence to follow, what reference material to use and what the final output should look like.

One detail matters more than it first appears: the skill's **description**. OpenAI's own Skill Creator says the description is used to determine when a skill should be used, and recommends describing both what the skill does and the situations in which it should fire. The full instructions are loaded only after the skill triggers.

So this is weak:

```markdown
Reviews reports.
```

This is much better:

```markdown
Review weekly marketing reports for missing metrics, unsupported claims, inconsistent 
comparisons and unclear next actions. Use when the user asks to review, 
QA or prepare a marketing performance report.
```

The second tells ChatGPT when the skill belongs in the conversation.

In Codex, OpenAI also provides `$skill-creator`. Codex separately includes `$skill-installer`, which installs skills from curated sources or GitHub repositories. They are different tools: one creates, the other installs.

**Best use:** build skills around your own repeated work before filling your account with somebody else's.

## [2. Composio](https://github.com/composio-community/skills)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_2.png)

**ChatGPT fit: Useful when you need to connect apps like Slack, Outlook, Gmail that aren’t lobotomized**

Composio is interesting because it separates two things that are often bundled together: **knowing how to do a workflow** and **having access to the app required to do it**.

Its [Awesome Codex Skills repository](https://github.com/composio-community/awesome-codex-skills) contains a large collection of `SKILL.md` workflows, including a `composio-skills` directory built around external services. The skills are documented primarily for coding agents, but the underlying pattern transfers well: the skill teaches the agent how to operate an app, while Composio provides the tools and authentication needed to interact with it.

3 examples show why this is useful.

**Gmail Automation** covers workflows such as searching email, replying, creating drafts, working with labels and handling attachments. Instead of repeatedly explaining how you want your inbox handled, you can build a repeatable workflow around it, for example, finding customer emails that still need a response, summarizing them and preparing drafts.

**Notion Automation** works with pages, databases, blocks, comments and search. That makes it useful for workflows where ChatGPT needs to do something with the result of a conversation: create a project page, add an item to a database, update documentation or record a decision in the right place.

**Slack Automation** covers messages, channels, search, reactions, threads and scheduled messages. A skill could, for example, search a project channel for recent decisions, summarize what changed and prepare or post an update to the appropriate channel.

That is what makes the Composio collection more interesting than a directory of prompts. The skill contains the operating procedure; Composio supplies the connection that lets the agent act on the real service.

This is more useful than installing a vague skill called “sales assistant.”

**Important:** connecting the Composio MCP server does not automatically authorize Gmail, Slack, Notion, GitHub, or the other services in Composio's catalogue. Each service still requires its own authorization. When an app is first needed, Composio can generate an OAuth link for you to approve, or you can connect accounts in the Composio dashboard beforehand.

**Best use:** repeatable workflows where ChatGPT needs to move between conversation and real work happening in email, documents, project-management tools, CRMs and other external services.

## [3. Impeccable](https://github.com/pbakaus/impeccable)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_3.png)

**ChatGPT fit**: Not a good fit for ordinary ChatGPT; excellent for coding agents

Impeccable sounds like a general design critic. It is much more specific.

It is a frontend-design system for coding agents. It audits and improves interfaces using design guidance, commands, deterministic anti-pattern detection and project-level integrations.

Its current installer supports coding environments including Codex CLI, Claude Code, Cursor, Gemini CLI and others. The recommended project installation is:

`npx impeccable skills install`

and updates use:

`npx impeccable skills update`

For Codex, Impeccable also installs project hook configuration that the user must approve.That dependency on a project is the point. Impeccable can inspect frontend files, detect UI anti-patterns and participate in the edit loop. Its detector checks source files for design problems and common AI-generated interface patterns. A normal ChatGPT conversation is not the environment it was built around.

If you develop interfaces with Codex, Impeccable is worth looking at. If what you really want is “make my presentation or PDF look more professional,” importing Impeccable into ChatGPT is the wrong solution.

## [4. Writing Guidelines](https://github.com/vercel-labs/agent-skills)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_4.png)

**ChatGPT fit**: Good to adapt

The underlying guidelines are strong, and this is the most useful skill on the list for people who write.

The skill around them is shaped for code review. It reads files you point it at, it fetches the current guidelines from a URL before each review, and it reports findings as file and line references.

**To install in a coding agent:** run` npx skills add vercel-labs/agent-skills --skill writing-guidelines`

**To use it in ChatGPT:**

- Open Vercel's `writing-guidelines` repository and use its `command.md` as the reference material.
- Create a Personal Skill with Skill Creator and add those guidelines as a supporting reference.
- Tell the skill to review documents or text supplied in ChatGPT.
- Change the output format to identify findings by section, paragraph or quoted sentence instead of relying on repository file paths and line numbers.

The useful thing to keep is Vercel's editorial system: rules for voice, structure, headings, code examples, typography, concision and common AI-writing habits. The coding-agent wrapper is the part you adapt.

Vercel also publishes a separate **Web Design Guidelines** skill for reviewing interfaces. Do not confuse the two.

## [5. Brand Guidelines](https://github.com/anthropics/skills/tree/main/skills/brand-guidelines)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_5.png)

**ChatGPT fit**: Excellent template; replace Anthropic's brand system with your own.

Anthropic's Brand Guidelines skill applies Anthropic's visual identity to generated artifacts. It defines the company's colour palette, heading and body fonts, fallback fonts, and rules for applying colours to text, shapes and other visual elements. It does not contain your company's branding, and it is not a general brand-voice skill.

**To read it:** open `https://raw.githubusercontent.com/anthropics/skills/main/skills/brand-guidelines/SKILL.md` in a browser.

**To use it in Claude:** Anthropic says the example skills in this repository are already available on paid Claude.ai plans. In Claude Code, you can add Anthropic's skills marketplace with:

```markdown
/plugin marketplace add anthropics/skills
```

and install the example skills with:

```markdown
/plugin install example-skills@anthropic-agent-skills
```

**What to do with it in ChatGPT:** use the file as a template for your own Brand Guidelines skill. Replace Anthropic's colours and typography with your palette, approved typefaces, spacing and layout rules, logo guidance, and combinations that should never be used.

The useful idea is not Anthropic's particular visual style. It is turning a static brand guide into instructions an agent can apply repeatedly when creating documents, presentations and other visual work.

## [6. Canvas Design](https://github.com/anthropics/skills/tree/main/skills/canvas-design)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_6.png)

**ChatGPT fit:** Good concept to adapt; the original implementation depends on tools and files that may not transfer unchanged.

Canvas Design is one of the more interesting Anthropic skills because it does not start by drawing. It starts by defining a **design philosophy**.

The workflow has two stages: first, create an aesthetic direction based on ideas such as form, space, colour, composition and visual hierarchy; then express that philosophy as a highly visual PNG or PDF. Anthropic's instructions deliberately favour sparse text and strong visual composition rather than treating the result like a decorated document.

That philosophy-first process is the part worth borrowing for ChatGPT. Instead of asking for “a beautiful poster,” a similar skill could first decide on the visual language—typography, negative space, colour, rhythm and composition, and only then create the final design.

The original skill should not be assumed to work unchanged in ChatGPT, however. It references a local `./canvas-fonts` directory, tells the agent to obtain fonts when needed, expects downloadable `.md`, `.png` and `.pdf` files, and includes a final refinement step that refers to revising the underlying code. Those assumptions depend on the tools available in the environment where the skill runs.

ChatGPT itself can generate images, and supported ChatGPT Work environments can create files, so visual output is not inherently the problem. The question is whether the particular tools expected by this Anthropic skill are available when you import it.

**To use it in Claude:** Canvas Design is part of Anthropic's example skills collection. The same `example-skills@anthropic-agent-skills` installation described in the Brand Guidelines section applies. same repository and same commands as Brand Guidelines above. The raw file sits at `https://raw.githubusercontent.com/anthropics/skills/main/skills/canvas-design/SKILL.md`.

**Best use in ChatGPT:** borrow the two-stage method, define the visual philosophy first, then create the artifact with the image or file-generation tools available to you.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_7.png)

## [7. Theme Factory](https://github.com/anthropics/skills/tree/main/skills/theme-factory)

**ChatGPT fit**: Highly portable concept, the theme definitions are portable text you can lift into your own skill.

Theme Factory is essentially a reusable styling system. Anthropic ships it with ten preset themes, each defining a colour palette, heading font, body font and overall visual identity. The skill is designed to apply those choices consistently across things such as presentations, documents, reports and HTML pages.

The useful part is not the ten themes themselves. It is the idea of turning visual preferences into something an agent can reuse.

Instead of asking ChatGPT to “make this look professional” every time, you could create themes such as **Board Report**, **Product Launch**, **Editorial**, or **Dark Technical**, each with fixed colours, typography and usage rules. The same theme can then be applied whenever you create a new deck or document.

**To install:** same repository and same commands again.

The raw file sits at [https://raw.githubusercontent.com/anthropics/skills/main/skills/theme-factory/SKILL.md](https://raw.githubusercontent.com/anthropics/skills/main/skills/theme-factory/SKILL.md)

The themes themselves are colour and typography systems described in text, and those are portable. Lift the theme definitions into a skill you write for ChatGPT.

Anthropic's version also includes a visual theme showcase, asks the user to choose a theme before applying it, and can generate a new theme when none of the presets fit.

The concept transfers well to ChatGPT because most of the important information is just structured design guidance. Some implementation details such as available fonts or how the final artifact is produced, may still need to be adapted to the tools available in ChatGPT.

**Best use:** build a small library of visual styles you actually reuse instead of describing your preferred colours and typography from scratch every time.

## [8. Matt Pocock Skills](https://github.com/mattpocock/skills)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_8.png)

**ChatGPT fit:** Mostly built for coding agents, but some of the workflow ideas transfer surprisingly well.

Matt Pocock's skills collection is built around common ways coding agents fail: they start before they understand the task, lose important project context, produce overly verbose explanations, or make decisions without exposing the assumptions behind them.

The repository supports Claude Code, Codex and other coding agents. Most of the collection is engineering-focused, with skills for planning, debugging, architecture, testing, issue triage and repository documentation.

**To install in Codex or another supported agent:**

```markdown
npx skills@latest add mattpocock/skills
```

The installer lets you choose which skills to add. Claude Code also has a native `mattpocock-skills` plugin available through its plugin system.

The most interesting skill for ChatGPT users is **`grill-me`**.

It interviews you about a plan or design one question at a time, following each branch of the decision tree until the important assumptions have been made explicit. Instead of giving you a wall of generic questions, it gives its recommended answer with each question so you can react to a concrete proposal. Unlike many of the engineering skills in the collection, `grill-me` is deliberately stateless. It writes no repository files and is documented for non-code use as well.

That makes the pattern easy to adapt into a ChatGPT skill for things such as project briefs, campaign plans, business decisions or product ideas: make the agent challenge the plan before it starts producing the deliverable. Other skills in the collection go much deeper into coding-agent workflows. `grill-with-docs`, for example, records decisions in a `CONTEXT.md` glossary and architecture decision records, while other skills work with issues, repositories and code.

**Best use in ChatGPT:** borrow the interrogation pattern. For the full engineering workflow, use the collection inside Codex or another coding agent.

## [9. Superpowers](https://github.com/obra/superpowers)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_9.png)

**ChatGPT fit:** Built for coding agents, especially Codex; not a good fit for ordinary ChatGPT.

Superpowers is more than a collection of coding prompts. It describes itself as a complete software-development methodology built from composable skills that automatically guide an agent through the development process.

The important detail is that Superpowers adapts itself to the coding agent where it runs. Claude Code and some other environments can use startup hooks, while Codex uses its native skill-discovery system. Subagents, worktrees and other capabilities are also mapped to the tools provided by each supported agent rather than being implemented identically everywhere.

**To install in Codex:** Superpowers is available through the official Codex plugin marketplace. In the Codex app, open **Plugins**, find **Superpowers** in the Coding section and click **+**. In Codex CLI, open `/plugins`, search for `superpowers` and select **Install Plugin**.

**To install in Claude Code:** use Anthropic's official marketplace:

```markdown
/plugin install superpowers@claude-plugins-official
```

The author also maintains a Superpowers marketplace:

```markdown
/plugin marketplace add obra/superpowers-marketplace
```

then:

```markdown
/plugin install superpowers@superpowers-marketplace
```

Install Superpowers separately in each coding agent you use.

The full framework is not a good fit for ordinary ChatGPT because many of its workflows assume a software project with file access, shell commands, tests, git and other coding-agent capabilities. The individual ideas, brainstorm before implementation, write a real plan, test before declaring success and verify claims with evidence can still be borrowed for your own ChatGPT skills.

**Best use:** install the complete framework in Codex or another supported coding agent; borrow individual workflow ideas if you mainly use ChatGPT.

## [**10. Academic Research Skills**](https://github.com/voidful/academic-skills)

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/i-tested-10-ai-skills-with-chatgpt/image_10.png)

Academic Skills is a research-focused collection built around the Agent Skills open standard. It currently includes seven main workflows: reading papers, generating research ideas, designing experiments, writing mathematical proofs, drafting papers, reviewing submissions, and analysing fit with potential professors.

Together, they form a fairly complete research pipeline:

`read → brainstorm → design → prove → write → review → revise`

The useful part is that each stage has its own instructions rather than trying to turn “academic research” into one giant prompt. A paper-reading workflow can feed into idea generation, which can lead into experiment design and eventually paper writing and review.

**To install in Codex:**

```plain text
git clone https://github.com/voidful/academic-skills ~/.codex/skills/academic-research
```

The repository also documents project-level installation under `.codex/skills/academic-research`.

**The catch:** the documented OpenAI setup is primarily a Codex workflow. The repository assumes an agent environment with a skills directory and supporting files that can be read as part of the research process.

ChatGPT supports uploading Agent Skills, so individual parts of the collection may be portable, but I would not assume the complete repository will behave unchanged in an ordinary ChatGPT conversation. If you want to use it there, review the individual `SKILL.md` files and adapt the workflows you actually need.

**Best for:** researchers, PhD students and academics who want a structured workflow spanning paper reading, idea development, experiments, writing and review. The complete collection is most naturally suited to Codex or another agent environment with filesystem access.

## What I would actually install

Do not install all 10. If you use ChatGPT for ordinary knowledge work, I would start with **Skill Creator** and build one skill around something you genuinely repeat.

If your workflow needs live external services, look at **Composio** next. The combination is powerful: your skill defines how the work should be done; the app/MCP layer provides the data and actions required to do it. Then use collections such as **Vercel Writing Guidelines**, **Anthropic Brand Guidelines**, **Canvas Design** and **Theme Factory** as reference material. They show how experienced teams encode taste, standards and process into reusable instructions.

If you write software, the list changes. **Impeccable**, **Matt Pocock's skills** and **Superpowers** become much more interesting because Codex provides the repository, shell, git and agent environment those projects were actually designed around, and that is the useful rule for evaluating skills in 2026:

**Read the ****`SKILL.md`****, but also read the environment around it.**

A good skill tells an agent what to do.

A working skill also has access to everything it needs to do it.
