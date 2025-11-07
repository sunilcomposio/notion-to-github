### AI Coding Assistants · Model Comparison · Agentic Integrations

Over the past few weeks, I’ve been exploring which AI coding assistant genuinely helps us ship faster **and** maintain code quality as we build real integrations via **Rube MCP** (which connects to 500+ apps).

To test this, I ran a head-to-head comparison: **Cursor Composer 1** versus **Cognition SWE-1.5**.

Same project brief. Same environment. The goal: build a working Chrome extension that uses Rube MCP to summarize selected text and push it to Notion.

Here’s what I found, with real timing data, debugging patterns, and cost insights.

---

## TL;DR

- **Cursor Composer 1** → Rapid prototyping and scaffolding; fastest to MVP but trades off robustness.

- **Cognition SWE-1.5** → Slower to start but with stronger reasoning, error handling, and modular architecture.

- Pick **Cursor** for experimentation; pick **SWE-1.5** for production.

---

## Why This Comparison Matters

The AI coding assistant market has become crowded fast. Every few weeks, there's a new model claiming to be the best at code generation. But when you're actually building production systems, especially agentic applications that need to orchestrate multiple services, the differences between tools become critical.

Two names keep coming up in developer conversations right now: Cursor Composer 1 and Cognition SWE-1.5. Both are positioned as next-generation coding assistants, but they take fundamentally different approaches to the same problem.

**Cursor Composer 1** has built a reputation for speed. It's deeply integrated into the Cursor IDE, offers lightning-fast autocomplete, and can scaffold entire projects in minutes. Developers love it for rapid prototyping and getting ideas into code quickly. The workflow feels seamless, and when you're in flow state, Cursor just gets out of your way.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/cursor/image_1.png)

**Cognition SWE-1.5**, on the other hand, is designed to think more like a senior engineer. It's the latest model from the team behind Devin, and it emphasizes reasoning depth over raw speed. SWE-1.5 is built to handle complex, multi-file projects where understanding context and making architectural decisions matters as much as generating syntactically correct code. It runs in Windsurf IDE and takes a more deliberate, thoughtful approach to code generation.

But here's the thing: most comparisons between these tools use synthetic benchmarks or isolated coding challenges. They'll test how well each model can solve LeetCode problems or generate boilerplate CRUD apps. That's useful data, but it doesn't tell you what really matters for modern development work.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/cursor/image_2.png)

---

## What We’ll Be Comparing These Models and Agents With

Both models were evaluated on the same **TaskFlow** project:

- Add a “Summarize with Rube MCP” context-menu option in Chrome.

- Send selected text to Rube MCP.

- Push summarized results to Notion.

- Display the last 5 summaries in a popup UI.

This required integration across Chrome APIs, Rube MCP streaming, and Notion’s REST interface, making it a real-world test of **multi-service orchestration**.

---

## Why MCPs Even Matter Here

Traditional APIs handle single-app use-cases.

**MCPs (Model Context Protocols)** let LLMs connect directly to tools — Notion, Slack, GitHub, etc. through structured, standardized endpoints.

Using Rube MCP as the intermediary, both models could trigger actions like:

- `NOTION_CREATE_NOTION_PAGE`

- `NOTION_APPEND_BLOCK_CHILDREN`

- `NOTION_QUERY_DATABASE`

This allowed us to measure each model’s ability to **orchestrate across tools**, not just write code.

---

## Setting up MCP Servers with Cursor & SWE-1.5

- **Common Setup:**

- **Cursor Composer 1:**

- **Cognition SWE-1.5:**

---

## Ideation and Setup

The goal was simple, build something that could **stress-test both models’ real-world reasoning and integration abilities**, not just their ability to generate boilerplate code.

So instead of another CRUD app or toy project, I picked a use-case that required **context handling, multi-service orchestration, and tool-aware logic**:

a Chrome extension that summarizes selected text using **Rube MCP** and pushes that data into **Notion**, all in one flow.

To make this fair and scalable, I built the entire project inside a **shared monorepo**, allowing both **Cursor Composer 1** and **Cognition SWE-1.5** to work on the same environment, same dependencies, same config, same API keys. This helped isolate what each model actually did differently in reasoning, debugging, and setup.

The initial project documentation, structure, and task list were scaffolded through **Rube MCP** connected to **Notion**, just to see how well the ecosystem handles planning and coordination between tools. Rube created the Notion planning page, while Cursor handled repo initialization and manifest setup.

Once the planning was in place, I spun up the **Chrome extension skeleton** and connected the **Rube MCP proxy** and **Notion API integration**.

For simplicity and reproducibility, both agents were given the *same prompt* and environment variables.

The setup went smoothly overall , no major hiccups except for Chrome’s limitations with environment variables, which SWE-1.5 later helped fix more elegantly. Cursor flew through setup quickly, while SWE-1.5 took a little longer but ensured a more stable foundation from the start.

The idea wasn’t just to “get it running,” but to observe **how each system approached reasoning** when faced with a multi-step, multi-context integration , the kind of task that really shows the difference between raw generation and agentic coordination.

### Prompt Used for Both:

```plain text
Create a Chrome extension called TaskFlow that adds a context menu action.
When a user right-clicks selected text and chooses "Summarize with Rube MCP,"
send the text to Rube MCP's summarization endpoint, then push the summary
to a Notion database. Include proper error handling and a simple popup
to show the last 5 summaries.
```

### Environment:

- Node.js + Chrome (Manifest V3)

- Rube MCP endpoint + proxy

- Notion integration via Composio

- Same browser, same test database, same runtime conditions

All setup notes, experiment logs, and generated code snapshots from both models are available here for reference:

🔗[TaskFlow Repo](https://github.com/VarshithKrishna14/TaskFlow-Extension/tree/main)

(Note: this repository reflects intermediate builds and architectural comparisons rather than a finalized release.)

---

## Refactoring and Fixing the Errors

Unlike most models that rush to code, SWE-1.5 approached the setup deliberately,  first validating the manifest, then configuring the proxy routing, before writing a single fetch call. It handled **streamed SSE parsing**, **dynamic MCP session management**, and **secure environment variable simulation** (without using `process.env`, which is unavailable in Chrome).

The model’s main slowdown came from debugging the `"process is not defined"` issue and refining Notion API permissions. However, it consistently **reasoned through root causes** rather than just patching over them, leading to a more maintainable final architecture.

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/cursor/image_3.png)

![Image 4](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/cursor/image_4.png)

**Key Debugging Insights:**

- SWE-1.5 caught and explained architecture-level problems.

- Cursor focused on local syntax and runtime fixes.

![Image 5](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/cursor/image_5.png)

- SWE-1.5’s error recovery reduced total debugging cycles by ~50%.

![Image 6](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/cursor/image_6.png)

---

## Building the Recommendation Pipeline

### Cursor Composer 1

- Single `background.js` file.

- Quick context menu and fetch logic.

- Minimal modularity; limited error awareness.

Example:

```javascript
const resp = await fetch(API_URL, { method: 'POST', body: JSON.stringify(payload) });
const data = await resp.json();
return data.result;
```

Simple and effective for MVPs, but lacks schema validation or structured error control.

### Cognition SWE-1.5

- Multi-file setup (`background.js`, `popup.js`, `config.js`, `proxy.js`)

- Robust handling of **Server-Sent Event (SSE)** streams from Rube MCP.

- Dynamic validation of responses and graceful fallbacks for malformed JSON.

- Added real-time notifications for user feedback within the Chrome environment.

Example:

```javascript
async function sendToRube(input) {
  const payload = {
    jsonrpc: "2.0",
    id: Date.now(),
    method: "tools.call",
    params: {
      name: input.action,
      arguments: { text: input.text }
    }
  };

  const resp = await fetch("http://localhost:3001/rube", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Accept": "application/json, text/event-stream"
    },
    body: JSON.stringify(payload),
  });
```

The extra rigor pays off , no silent failures, and cleaner recoveries.

---

## Comparing the Cost

**Cursor Composer 1**  took approximately 25 minutes to generate an **initial** Chrome extension , build the Notion integration, and complete the first end-to-end test.

It consumed around ~40-50K tokens (based on IDE reports), roughly $0.15-0.25. The model performed best during rapid iteration but required more manual debugging and direction from me.

In one case, the MCP stream parser completely missed malformed data, which led to an unhandled `SyntaxError`. While it was easy to patch, the model didn’t catch it autonomously.

Still, it was fast, ideal for “get it working, fix it later” projects.

**Cognition SWE-1.5**, by contrast, took around 45 minutes to reach a **comparable prototype stage** including validation, proper Notion API integration, and runtime testing.

It used about ~55-65K tokens (Windsurf showed higher usage), approximately $0.50-0.60 at the current pricing.

SWE-1.5 was more deliberate ,  it asked clarifying setup questions, implemented buffer-based stream parsing, and even added fallback logic for partial SSE data.

While the run was slower and more verbose, the final build was much more stable and production-ready. The model also reduced the number of debugging loops from six to three, nearly halving the iteration time post-deployment.

SWE-1.5 used ~30% more tokens but reduced debugging iterations by half.

For production, that’s a fair trade, faster QA cycles and fewer regressions.

---

## Benchmark Results

Here's how both models performed across all metrics:

## Developer Experience Feedback

**Cursor Composer 1:**

Feels like coding with a smart junior developer, fast, direct, but expects you to know the next step. Great for momentum.

**SWE-1.5:**

Feels like working with a senior teammate — anticipates pitfalls, asks clarifying questions, and won’t let you deploy something brittle.

**Debugging Iterations:**

- Cursor → ~6 loops

- SWE-1.5 → ~3 loops

Both models showed distinct approaches to code generation, but SWE-1.5 needed fewer retries. This comparison focuses on the development experience and code generation capabilities during the build phase. Both implementations successfully integrated with Rube MCP and generated valid API calls to Notion.

Final production deployment would require additional testing for:

- Chrome extension permission scopes

- Notion workspace-specific configurations

- Cross-browser compatibility validation

The goal here was to evaluate how each model approaches complex,
multi-service integration problems rather than measure production uptime.

---

## Final Thoughts

Both tools have distinct strengths depending on your development phase and requirements. This comparison reflects the development and integration testing experience rather than a fully deployed production application.

- **Cursor Composer 1** excels at **speed and iteration**, perfect for hackathons and prototypes.

- **Cognition SWE-1.5** delivers **depth and resilience**, best for team environments and long-term codebases.

If I were shipping fast, I’d pick **Cursor**.

If I were building something meant to last, I’d choose **SWE-1.5**.

The real mastery isn’t picking one model, it’s knowing *when* to switch.

- Local proxy server at `localhost:3001/rube`

- Connection to Composio’s MCP endpoint

- Bearer-authenticated Rube API key

- Notion integration tested via Composio sandbox database

- Used quick scaffolding commands

- Generated minimal proxy & Chrome service worker

- Reached a working call fast, but lacked deep error diagnostics

- Asked clarifying setup questions (proxy routing, authorization scope)

- Created more robust server logic with validation and fallback

- Correctly handled SSE streams and partial responses from MCP
