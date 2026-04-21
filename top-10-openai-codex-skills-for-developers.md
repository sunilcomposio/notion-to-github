---
title: "Top 10 OpenAI Codex Skills for Developers"
source: https://www.notion.so/composio/Top-10-OpenAI-Codex-Skills-for-Developers-347f261a6dfe80b88daade5536b39dfe
notion_page_id: 347f261a-6dfe-80b8-8daa-de5536b39dfe
last_synced: 2026-04-21T10:53:10Z
---

Codex gets a little better and more useful once you add the right skills on top.
Out of the box, it can already code and help with day-to-day coding stuff. But skills make the workflow a lot sharper in actual development work.
![](file://%7B%22source%22%3A%22attachment%3Aca51b54c-9fb1-4afd-800a-b13e93dac72a%3AUntitled_design(52).png%22%2C%22permissionRecord%22%3A%7B%22table%22%3A%22block%22%2C%22id%22%3A%22347f261a-6dfe-80f5-bbe5-df45aea75b44%22%2C%22spaceId%22%3A%2235adec70-dbef-4f42-9214-62ac8cdc4d75%22%7D%7D)
Testing apps, fixing CI, connecting tools, working with MCP servers, and overall automating the boring parts of development.
These are some of the best Codex skills I'd recommend for developers right now.
> ⚠️ These are the ones that I've picked from my own experience working with Codex. There could be some great ones I've missed, but feel free to drop them in the comments.
Also, the order doesn't really matter. This isn't in any particular order. We’ve a super good curated repo for Skills for Codex: [https://github.com/ComposioHQ/awesome-codex-skills](https://github.com/ComposioHQ/awesome-codex-skills)
Before jumping in, one quick thing: there are a few ways to install skills depending on where they come from.
---
## TL;DR
If you just want the useful picks, here’s the list:
- **webapp-testing:** Helps Codex test real browser flows so you can catch UI issues earlier.
- **connect:** Lets Codex work across tools like GitHub, Slack, and Notion instead of staying stuck in the repo.
- **mcp-builder:** Helps build and evaluate MCP servers easily.
- **gh-fix-ci:** Looks into failing GitHub Actions runs and helps figure out what actually broke.
- **notion-spec-to-implementation:** Turns rough Notion docs into plans, tasks, and something you can actually execute.
- **security-threat-model:** Helps Codex think through security risks, trust boundaries, and attack paths in a repo.
- **frontend-skill:** Makes Codex better at frontend and less AI-looking and generic.
- **create-plan:** Breaks work down before implementation instead of prompt-first coding.
- **cli-creator:** Turns repeated commands, scripts, or API calls into a reusable CLI.
- **stop-slop:** Cleans up AI-sounding writing in docs, READMEs, and blog posts.
Overall, these improve Codex for actual development work, not just code generation.
---
## How to install Codex skills
For official OpenAI skills:
```bash
$skill-installer <skill-name>
```
For OpenAI experimental skills:
```bash
$skill-installer install https://github.com/openai/skills/tree/main/skills/.experimental/<skill-name>
```
For community skills, the easiest way is to do a manual install in the local Codex skill folder:
```bash
mkdir -p ~/.agents/skills
git clone <repo-url> ~/.agents/skills/<skill-name>
```
For Composio skills, there’s also an installer script:
```bash
git clone https://github.com/ComposioHQ/awesome-codex-skills.git
cd awesome-codex-skills/awesome-codex-skills
python skill-installer/scripts/install-skill-from-github.py --repo ComposioHQ/awesome-codex-skills --path <skill-name>
```
> 💡 **NOTE:** Codex usually detects newly installed skills automatically, but if it doesn’t show up, restart.
---
## 1. <span discussion-urls="discussion://348f261a-6dfe-8048-8478-001cb2d95fe0">Composio connect</span>
> ℹ️ Connects Codex to external apps so it can take real actions across your workflow.
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/09b49eb3-336f-4c81-80fb-7f65d3cea258/Screenshot_From_2026-04-20_19-56-43.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46675WESFTA%2F20260421%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260421T105310Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQCidoe2Q%2B7A2I99z7bqsJ8GA3a%2BkMxNy%2Fyw4tE3jXS6kwIgRi8h94zc9DKeGqfN3m6CbejwNJn%2Fln4bWGqDciKiKG0q%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDC6RykBiF61m5LT5lCrcA7rT38glwxVMT8OnLPztXiF9h7kiDAJd1GPdF2%2B6ilD%2FmQ4n9C0Q5AwsEPp6BVrtjDSL9s6D7x2TZigOWXvdtNLN2Gt4fiCr70kUVm0DFw8%2F3OuNE%2Fptbtt%2FKf9d7h%2B7JssUiHQkbMZC8UnNQCP4QeNOmCXX9yCWqGH5Fo7oDBWypoMbj5UrmqYrV%2BiU0f5ACXWkBMJCclv0i5XOOqfogRPH48Imyn6eRv%2B%2FZDgPx1v5Ny5ImELD6hr91gOnsiDGIiGTExEvuoShCe1PSdVGnPMTYO3mS5JUWKG01DnWNUyvEaICx%2BZt3Vi8XSQXVkVWpOkNtUuxad30loo4a4E%2FO8ZhLw7Xs7AHI%2BgO7Exvmc8BfbDCuU4KYFgmffwet0Wns5zXPBv7pqM9OZRB31D%2BzVUNrCucPyqSyqlqhZ%2F5bRqejcoXUxdY1GFFE1HztWyIthB%2FSdUfS1P7%2BVvDkUI6wATJ2OERRtg%2FwtCi3vMbAab6Xo3uYyffmN%2BXaBPip%2B%2BJlJ3KXL1tOGMtQaU%2BfOtzmczFi2suGTwaDo7wE3LDfJyktamKo6Qv%2FEvu%2B8t7YzGXHvWRx62DkI2hyoO2LtYuDQmi0INwl9%2Bsw9ItLZDPCt3DW8gAwnDfs1Q%2FyrPoMOCInc8GOqUBnvaIcpFFgTlqZKpzZLCwx%2FWe76qNbkzi33qWPQ0%2BhR6Uipo0dkTV2aQf62%2B%2FeabjyKphDTJ4RSg%2BPTx4RvNRLPlGesOSFgMEsYIz7haowEAARuc1stAWAs9jMGWVdumBWDFKGWG8LWYMtWT6eMV1fjau2jkopJL7GIsE24VKMkgCRQDEn8jCV3HNvHxaczbeXEQvWNhxOQebEHEaiaur9RjuUJIl&X-Amz-Signature=34a0b9c3759e8ce7a2ee3b16fbaae44fdb8f594dec93ca9d37ec9bdd1ef66ae8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
With `connect`, you can hook Codex into external tools and let it do things across your workflow. GitHub, Notion, Slack, and a bunch more.
Pretty nice when you want to go beyond your local repo and work with the mess you already deal with every day.
### What it does
- Connects Codex to apps like GitHub, Slack, and Notion
- Lets it work beyond your local repo and across the apps you already use
- Useful when your work lives across tools, not just inside the repo
### Install
```bash
git clone https://github.com/ComposioHQ/awesome-codex-skills.git
cd awesome-codex-skills/awesome-codex-skills
python skill-installer/scripts/install-skill-from-github.py --repo ComposioHQ/awesome-codex-skills --path connect
```
## 2. <span discussion-urls="discussion://348f261a-6dfe-8043-b136-001c92021656">[webapp-testing](https://github.com/ComposioHQ/awesome-codex-skills/tree/master/webapp-testing)</span>
> ℹ️ Adds a proper web app testing workflow on top of Codex.
This is one of the easiest skills to justify adding.
If you're building anything with a UI, you'll eventually need to test flows and help verify behavior instead of just writing code and hoping for the best. 👀
### What it does
- Helps Codex test real user flows in your web app
- Useful for checking whether UI changes actually work in the browser
- Good for catching broken behavior before you ship
All of it is powered by Playwright, which is one of the very few non-slop things to come out of Micro”slop”. 😮‍💨
### Install
```bash
git clone https://github.com/ComposioHQ/awesome-codex-skills.git
cd awesome-codex-skills/awesome-codex-skills
python skill-installer/scripts/install-skill-from-github.py --repo ComposioHQ/awesome-codex-skills --path webapp-testing
```
---
## 3. [mcp-builder](https://github.com/ComposioHQ/awesome-codex-skills/tree/master/mcp-builder)
> ℹ️ Helps build and evaluate MCP servers.
If you're spending time around MCPs, this one is very easy to like.
Instead of vibe-coding your way into some half-working server, this gives Codex a more structured way for building and evaluating MCP servers.
### What it does
- Helps build and evaluate MCP servers
- Helps reduce trial-and-error setup
- Useful if you build tools for agents often
### Install
```bash
git clone https://github.com/ComposioHQ/awesome-codex-skills.git
cd awesome-codex-skills/awesome-codex-skills
python skill-installer/scripts/install-skill-from-github.py --repo ComposioHQ/awesome-codex-skills --path mcp-builder
```
---
## 4. [gh-fix-ci](https://github.com/ComposioHQ/awesome-codex-skills/tree/master/gh-fix-ci)
> ℹ️ Inspect failing GitHub Actions checks, summarize failures, and propose fixes.
This one is just useful. That's it.
CI failures are just the worst. And especially debugging them is even worse. Opening GitHub, visiting the failing workflow, getting the errors, and trying to make sense of it is just truly time-consuming. Frankly, it's part of the dev workflow, but sometimes it just gets irritating.
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/fdac1a3c-b4da-4b8b-9e43-659976c2bc1b/Screenshot_From_2026-04-19_21-32-59.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46675WESFTA%2F20260421%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260421T105310Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQCidoe2Q%2B7A2I99z7bqsJ8GA3a%2BkMxNy%2Fyw4tE3jXS6kwIgRi8h94zc9DKeGqfN3m6CbejwNJn%2Fln4bWGqDciKiKG0q%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDC6RykBiF61m5LT5lCrcA7rT38glwxVMT8OnLPztXiF9h7kiDAJd1GPdF2%2B6ilD%2FmQ4n9C0Q5AwsEPp6BVrtjDSL9s6D7x2TZigOWXvdtNLN2Gt4fiCr70kUVm0DFw8%2F3OuNE%2Fptbtt%2FKf9d7h%2B7JssUiHQkbMZC8UnNQCP4QeNOmCXX9yCWqGH5Fo7oDBWypoMbj5UrmqYrV%2BiU0f5ACXWkBMJCclv0i5XOOqfogRPH48Imyn6eRv%2B%2FZDgPx1v5Ny5ImELD6hr91gOnsiDGIiGTExEvuoShCe1PSdVGnPMTYO3mS5JUWKG01DnWNUyvEaICx%2BZt3Vi8XSQXVkVWpOkNtUuxad30loo4a4E%2FO8ZhLw7Xs7AHI%2BgO7Exvmc8BfbDCuU4KYFgmffwet0Wns5zXPBv7pqM9OZRB31D%2BzVUNrCucPyqSyqlqhZ%2F5bRqejcoXUxdY1GFFE1HztWyIthB%2FSdUfS1P7%2BVvDkUI6wATJ2OERRtg%2FwtCi3vMbAab6Xo3uYyffmN%2BXaBPip%2B%2BJlJ3KXL1tOGMtQaU%2BfOtzmczFi2suGTwaDo7wE3LDfJyktamKo6Qv%2FEvu%2B8t7YzGXHvWRx62DkI2hyoO2LtYuDQmi0INwl9%2Bsw9ItLZDPCt3DW8gAwnDfs1Q%2FyrPoMOCInc8GOqUBnvaIcpFFgTlqZKpzZLCwx%2FWe76qNbkzi33qWPQ0%2BhR6Uipo0dkTV2aQf62%2B%2FeabjyKphDTJ4RSg%2BPTx4RvNRLPlGesOSFgMEsYIz7haowEAARuc1stAWAs9jMGWVdumBWDFKGWG8LWYMtWT6eMV1fjau2jkopJL7GIsE24VKMkgCRQDEn8jCV3HNvHxaczbeXEQvWNhxOQebEHEaiaur9RjuUJIl&X-Amz-Signature=ef4f05c59c85dbbd7c47d11d731633d9799a611e524e9ae19c0aa842ba4a8ede&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
### What it does
- Looks at failing GitHub Actions runs and summarizes the cause
- Suggests likely fixes
- Saves a ton of time on repetitive CI work
### Install
```bash
git clone https://github.com/ComposioHQ/awesome-codex-skills.git
cd awesome-codex-skills/awesome-codex-skills
python skill-installer/scripts/install-skill-from-github.py --repo ComposioHQ/awesome-codex-skills --path gh-fix-ci
```
Nothing glamorous here, just a genuinely useful skill that almost everyone runs into sooner or later.
---
## 5. [notion-spec-to-implementation](https://github.com/ComposioHQ/awesome-codex-skills/tree/master/notion-spec-to-implementation)
> ℹ️ Turn Notion specs into implementation plans, tasks, and progress tracking.
A lot of work is somewhere in a doc that isn't well organized.
Usually a Notion page with a vague title, a bunch of bullet points, and maybe a few sentences that are actually worth reading.
### What it does
- Converts rough specs into implementation plans
- Helps track progress from doc to the actual execution
- Overall, great for teams that already live inside Notion
### Install
```bash
git clone https://github.com/ComposioHQ/awesome-codex-skills.git
cd awesome-codex-skills/awesome-codex-skills
python skill-installer/scripts/install-skill-from-github.py --repo ComposioHQ/awesome-codex-skills --path notion-spec-to-implementation
```
Very handy if your team lives in Notion, or if you’re someone like me. 🫩
---
## 6. [security-threat-model](https://github.com/openai/skills/tree/main/skills/.curated/security-threat-model)
> ℹ️ Helps generate threat models specific to the repository.
This is one of those least-used skills, but one of the most important when the time comes.
This skill helps Codex think through the security side of a repo in a more structured way. Things like trust boundaries, possible attack paths, and where things could go wrong.
### What it does
- Generates repo-specific threat models
- Useful for auth-heavy or sensitive systems
- Helps identify trust boundaries and attack surfaces
### Install
```bash
$skill-installer security-threat-model
```
Not something you’ll use every single day, but when you’re building anything auth-heavy, a production app, or even something slightly sensitive, it’s a really good one to have
---
## 7. [frontend-skill](https://github.com/openai/skills/tree/main/skills/.curated/frontend-skill)
> ℹ️ Makes Codex better at frontend, not just raw UI generation.
Finally, something that pushes Codex away from the same generic AI-looking UIs.
Not anything hard to make sense of, it just improves the overall feel of the UI. Especially handy for those who work on the UI most of the time.
### What it does
- Helps improve frontend implementation quality
- Helps with UI structure, polish, and consistency
- Useful when frontend is a big part of your workflow
### Install
```bash
$skill-installer frontend-skill
```
If you're planning to use Codex to also clone a few Figma designs every now and then, [figma-implement-design](https://github.com/openai/skills/tree/main/skills/.curated/figma-implement-design) is another must-have.
```bash
$skill-installer figma-implement-design
```
That one is more for when you already have the design ready and want Codex to turn it into something closer to production code.
---
## 8. [create-plan](https://github.com/ComposioHQ/awesome-codex-skills/tree/master/create-plan)
> ℹ️ Create a clear execution plan before the actual implementation.
The simplest yet most useful one on the list here.
Sometimes the best thing to do is stop for a second and think the idea through properly instead of immediately going with the prompt.
### What it does
- Breaks the task into a clean execution plan
- Makes implementation steps easier to review
- Helps reduce messy prompt-first coding (like most of us do)
### Install
```bash
git clone https://github.com/ComposioHQ/awesome-codex-skills.git
cd awesome-codex-skills/awesome-codex-skills
python skill-installer/scripts/install-skill-from-github.py --repo ComposioHQ/awesome-codex-skills --path create-plan
```
---
## 9. [cli-creator](https://github.com/openai/skills/tree/main/skills/.curated/cli-creator)
> ℹ️ Build a CLI **for** Codex from API docs, an SDK, a local script, or even a cURL command.
This is a very personal pick.
Instead of repeating the same scripts, commands, and API calls again and again, this helps Codex turn them into a proper CLI you can actually reuse. From docs, SDKs, curl commands, local scripts, whatever.
Pretty great if you like moving repeated workflows into something reusable.
Which, let's be honest, is one of those few satisfying things left in software after AI slop.
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/f06645ca-4715-4c4f-9e39-cc88ad306f3f/automate.webp?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46675WESFTA%2F20260421%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260421T105310Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQCidoe2Q%2B7A2I99z7bqsJ8GA3a%2BkMxNy%2Fyw4tE3jXS6kwIgRi8h94zc9DKeGqfN3m6CbejwNJn%2Fln4bWGqDciKiKG0q%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDC6RykBiF61m5LT5lCrcA7rT38glwxVMT8OnLPztXiF9h7kiDAJd1GPdF2%2B6ilD%2FmQ4n9C0Q5AwsEPp6BVrtjDSL9s6D7x2TZigOWXvdtNLN2Gt4fiCr70kUVm0DFw8%2F3OuNE%2Fptbtt%2FKf9d7h%2B7JssUiHQkbMZC8UnNQCP4QeNOmCXX9yCWqGH5Fo7oDBWypoMbj5UrmqYrV%2BiU0f5ACXWkBMJCclv0i5XOOqfogRPH48Imyn6eRv%2B%2FZDgPx1v5Ny5ImELD6hr91gOnsiDGIiGTExEvuoShCe1PSdVGnPMTYO3mS5JUWKG01DnWNUyvEaICx%2BZt3Vi8XSQXVkVWpOkNtUuxad30loo4a4E%2FO8ZhLw7Xs7AHI%2BgO7Exvmc8BfbDCuU4KYFgmffwet0Wns5zXPBv7pqM9OZRB31D%2BzVUNrCucPyqSyqlqhZ%2F5bRqejcoXUxdY1GFFE1HztWyIthB%2FSdUfS1P7%2BVvDkUI6wATJ2OERRtg%2FwtCi3vMbAab6Xo3uYyffmN%2BXaBPip%2B%2BJlJ3KXL1tOGMtQaU%2BfOtzmczFi2suGTwaDo7wE3LDfJyktamKo6Qv%2FEvu%2B8t7YzGXHvWRx62DkI2hyoO2LtYuDQmi0INwl9%2Bsw9ItLZDPCt3DW8gAwnDfs1Q%2FyrPoMOCInc8GOqUBnvaIcpFFgTlqZKpzZLCwx%2FWe76qNbkzi33qWPQ0%2BhR6Uipo0dkTV2aQf62%2B%2FeabjyKphDTJ4RSg%2BPTx4RvNRLPlGesOSFgMEsYIz7haowEAARuc1stAWAs9jMGWVdumBWDFKGWG8LWYMtWT6eMV1fjau2jkopJL7GIsE24VKMkgCRQDEn8jCV3HNvHxaczbeXEQvWNhxOQebEHEaiaur9RjuUJIl&X-Amz-Signature=97136b22b18137f1a535817af73102ac180994d10c79f2b2753f9a9f5c4ba26a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
### What it does
- Helps Codex turn repeated commands or API calls into a real CLI
- Useful when you keep doing the same workflow again and again
- Good for making scripts and tooling more reusable
### Install
```bash
$skill-installer cli-creator
```
---
## 10. [stop-slop](https://github.com/hardikpandya/stop-slop) (Nice to have)
> ℹ️ A skill file for "removing AI tells from prose"
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/03107d19-e02e-4386-84e9-2741f3d9c0a7/stopslop.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46675WESFTA%2F20260421%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260421T105310Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQCidoe2Q%2B7A2I99z7bqsJ8GA3a%2BkMxNy%2Fyw4tE3jXS6kwIgRi8h94zc9DKeGqfN3m6CbejwNJn%2Fln4bWGqDciKiKG0q%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDC6RykBiF61m5LT5lCrcA7rT38glwxVMT8OnLPztXiF9h7kiDAJd1GPdF2%2B6ilD%2FmQ4n9C0Q5AwsEPp6BVrtjDSL9s6D7x2TZigOWXvdtNLN2Gt4fiCr70kUVm0DFw8%2F3OuNE%2Fptbtt%2FKf9d7h%2B7JssUiHQkbMZC8UnNQCP4QeNOmCXX9yCWqGH5Fo7oDBWypoMbj5UrmqYrV%2BiU0f5ACXWkBMJCclv0i5XOOqfogRPH48Imyn6eRv%2B%2FZDgPx1v5Ny5ImELD6hr91gOnsiDGIiGTExEvuoShCe1PSdVGnPMTYO3mS5JUWKG01DnWNUyvEaICx%2BZt3Vi8XSQXVkVWpOkNtUuxad30loo4a4E%2FO8ZhLw7Xs7AHI%2BgO7Exvmc8BfbDCuU4KYFgmffwet0Wns5zXPBv7pqM9OZRB31D%2BzVUNrCucPyqSyqlqhZ%2F5bRqejcoXUxdY1GFFE1HztWyIthB%2FSdUfS1P7%2BVvDkUI6wATJ2OERRtg%2FwtCi3vMbAab6Xo3uYyffmN%2BXaBPip%2B%2BJlJ3KXL1tOGMtQaU%2BfOtzmczFi2suGTwaDo7wE3LDfJyktamKo6Qv%2FEvu%2B8t7YzGXHvWRx62DkI2hyoO2LtYuDQmi0INwl9%2Bsw9ItLZDPCt3DW8gAwnDfs1Q%2FyrPoMOCInc8GOqUBnvaIcpFFgTlqZKpzZLCwx%2FWe76qNbkzi33qWPQ0%2BhR6Uipo0dkTV2aQf62%2B%2FeabjyKphDTJ4RSg%2BPTx4RvNRLPlGesOSFgMEsYIz7haowEAARuc1stAWAs9jMGWVdumBWDFKGWG8LWYMtWT6eMV1fjau2jkopJL7GIsE24VKMkgCRQDEn8jCV3HNvHxaczbeXEQvWNhxOQebEHEaiaur9RjuUJIl&X-Amz-Signature=b0c19dff9e207ca3cf3abd616ecac505151b5ff3bf51107b8f22000dd771aaa2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
This is more of a bonus pick. Something I discovered just a few days back.
It’s not really a coding skill. It’s more for cleaning up that obvious AI-written tone in docs, blog posts, release notes, READMEs, and whatever else you end up writing with Codex.
### What it does
- Removes the obvious AI writing patterns (em-dashes and all)
- Helps docs and READMEs sound more natural and human-written
- Great if you use Codex for both code and writing
### Install
```bash
mkdir -p ~/.agents/skills
git clone https://github.com/hardikpandya/stop-slop.git ~/.agents/skills/stop-slop
```
Very optional. But pretty fun and useful if you work on docs and READMEs more often.
---
## Conclusion
Don’t try to install all of them at once. You probably won’t need all of them anyway.
Start with the ones that already fit where you work, and try to expand from there.
If you use Codex often, adding a few of these can make a real difference. I’m not even a huge Codex user myself, but ever since GPT-5.4, I’ve been going back and forth between Codex and Claude Code a lot more.
These are just some of my personal favorites.
Most of these come from OpenAI’s own [skill catalog](https://github.com/openai/skills) for Codex and the [awesome-codex-skills](https://github.com/ComposioHQ/awesome-codex-skills) repo. Go take a look, you might find a few better ones that fit your workflow even more.