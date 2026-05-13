---
title: "Claude Code vs OpenAI Codex: I Tested Both on Problems That Actually Break AI Agents"
source: https://www.notion.so/composio/Claude-Code-vs-OpenAI-Codex-I-Tested-Both-on-Problems-That-Actually-Break-AI-Agents-35df261a6dfe8061ba5ad1e45ff79580
notion_page_id: 35df261a-6dfe-8061-ba5a-d1e45ff79580
last_synced: 2026-05-13T13:30:16Z
---

86 million vs 7.2 million. That's what the npm download counter looked like the week both new models dropped. Codex's 12x lead came almost entirely from four days after GPT-5.5 launched.

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/claude-code-vs-openai-codex/92f6ed091c.png)

Then I looked about it. The npm download numbers made Codex look 12× bigger. Then I dug into the data. Claude Code had stopped using npm months earlier. The 7.2 million number was mostly legacy installs.

Opus 4.7 vs GPT-5.5. Two complex builds. Real MCP failures. I went in with a pretty clear guess. The results did not follow it.

I'd been hearing the hype. Anthropic dropped Opus 4.7 in April 2026, they claim it's 60% less likely to drop subtasks in long sequences than 4.6. The same week, OpenAI shipped GPT-5.5. Their tagline? "Smarter, faster, fewer tokens, better tool use."

So I gave both agents the same two tasks: one MCP-heavy backend workflow, and one real-time React app. I used Composio for GitHub and Slack, added the kind of failures you actually hit in tool-based workflows, and watched how each agent handled it.

I thought one of them would clearly pull ahead. That did not happen. Claude Code and Codex failed in different places, recovered in different ways, and left me with a much clearer sense of when I would use each one.

### TL;DR

Claude Code was more deliberate. It checked MCP before coding, planned the architecture, shipped the larger implementation, and wrote a smoke test on its own.

Codex was leaner. It hit a tool-resolution failure on the PR triage task, handled it cleanly, and still shipped a working real-time UI with fewer files and slightly lower cost.

I did not get a clean winner. Claude felt better for tool-heavy, architecture-heavy work. Codex felt better when the task was scoped tightly and I wanted a compact implementation fast.

<table header-row="true">
<tr>
<td></td>
<td>Claude Code (Opus 4.7)</td>
<td>Codex / Cursor (GPT-5.5)</td>
</tr>
<tr>
<td>Problem 1 — PR triage</td>
<td>Completed, 1 PR scored</td>
<td>GitHub MCP blocked, empty report</td>
</tr>
<tr>
<td>Problem 2 — Collab UI</td>
<td>36 files, 12m 17s, 3ms WS broadcast</td>
<td>28 files, \~15 min, 5ms WS broadcast</td>
</tr>
<tr>
<td>TypeScript `any`</td>
<td>Zero</td>
<td>Zero</td>
</tr>
<tr>
<td>Est. API cost (both problems)</td>
<td>\~\$2.50</td>
<td>\~\$2.04</td>
</tr>
<tr>
<td>Largest component</td>
<td>123 lines</td>
<td>67 lines</td>
</tr>
<tr>
<td>WS smoke test</td>
<td> Passed (3ms)</td>
<td>Passed (5ms)</td>
</tr>
</table>
## How I tested them

I kept the test simple: same prompts, same machine, same repo, same `.env`, and the same Composio credentials for GitHub and Slack.

Claude wrote into `src/` and `code-review/`. Codex wrote into `gpt55-pr-triage/` and `gpt55-code-review/`. Neither agent got to see the other one’s work.

I was not trying to build a perfect academic benchmark. I cared about the stuff that matters in daily use: did it run, did it stay type-safe, did it recover when tools broke, how messy was the code, how long did it take, and what did it cost?

---

### How I connected Composio

I did not wire GitHub and Slack into each agent by hand. I used [Composio ](https://dashboard.composio.dev)as the tool layer, connected GitHub and Slack there, and gave both agents the same credentials.

**Explore toolkits:** [docs.composio.dev/toolkits/introduction](https://docs.composio.dev/toolkits/introduction)

Claude Code handled that setup cleanly. Before it wrote code, it ran `/mcp`, confirmed the Composio server was reachable, and checked which GitHub and Slack tools were available.

Codex ran through Cursor with the same credentials, but it did not get the same tool access during execution. When it tried to call GitHub, it failed here:

```bash
ComposioToolNotFoundError: Unable to retrieve tool with slug
GITHUB_LIST_PULL_REQUESTS
```
---

### **Problem 1: GitHub PR triage**

**Prompt:**

```shell
You are a senior TypeScript engineer. Build a PR triage system with the following exact spec:

SCORING FORMULA (per PR):
- File count × 2
- Lines changed (additions + deletions, divided by 10, rounded down) × 1
- Missing labels: if no labels assigned, add 3
- No reviewers assigned: if reviewer list is empty, add 5

REQUIREMENTS:
1. Read all open PRs from this repo via GitHub MCP: composio-dev/composio
2. Score every PR using the formula above
3. Write a prioritized markdown report to ./output/triage.md — highest score first.
   Each row must include: PR number, title, URL, score breakdown, total score
4. For every PR with total score > 20, post a Slack alert to #dev-alerts
   via MCP. Message must contain: PR title, total score, and URL. Nothing else.
5. If any MCP call fails or rate limits, wait 5 seconds and retry up to 3 times before
   skipping that PR and logging the failure to ./output/errors.log
6. Strict TypeScript — no any. Modular structure across these files minimum:
   - src/scorer.ts
   - src/github.ts
   - src/slack.ts
   - src/report.ts
   - src/index.ts

Do not start writing code until you confirm both GitHub and Slack MCPs are live.
Run /mcp first and show me the connected tools before proceeding.
```
### What Claude Code built

Claude did the right thing first: it checked that MCP was actually available. Then it looked at the Composio tool schemas so it could work with the real GitHub response shapes instead of guessing them.

After that, it split the PR triage tool into eight files:

```shell
src/config.ts     — env loader
src/types.ts      — shared interfaces
src/scorer.ts     — pure scoring formula
src/github.ts     — MCP calls + retry
src/slack.ts      — alert posting
src/report.ts     — markdown generation
src/retry.ts      — 3-attempt × 5s helper
src/index.ts      — orchestrator
```
The part I liked was that retry logic came early. It had `retry.ts` in place before the GitHub calls were wired up, so failure handling was part of the design instead of something patched in later.

It also added `scripts/dry-run-from-mcp.ts`, which was useful. That script took real PR data already pulled through MCP and ran it through the production scoring/reporting code without touching Slack.

The actual run found one open PR: `#5`, titled `Test PR: Fix README Logo — Updated Title`. Claude scored it correctly:

```plain text
files(2) + lines(0) + labels(3) + reviewers(5) = 10
```
It wrote a clean `triage.md` and skipped Slack, which was the right call because the score was below the alert threshold.

- Estimated tokens: \~71,000
- Estimated cost: \~\$0.92
- TypeScript: zero `any`, clean typecheck
- Output: 8 modular files

### What Codex built

Codex also planned before coding, also produced zero `any`, also shipped 9 files with retry logic, error logging, and a `--dry-run` flag it added on its own.

The run failed because Cursor did not expose Composio’s MCP descriptors to GPT-5.5’s execution path. The GitHub call failed during tool resolution:

```shell
ComposioToolNotFoundError: Unable to retrieve tool with slug
GITHUB_LIST_PULL_REQUESTS
```
It retried 3 times, waited 5 seconds between each, logged the failure cleanly to `errors.log`, and produced an empty but structurally valid `triage.md`. 

That matters because the failure was handled cleanly. Codex did not crash, ignore the error, or invent output. The blocker was the environment: Cursor’s GPT-5.5 path did not have the same MCP access that Claude Code had natively.

- **Estimated tokens:** \~37,000
- **Estimated cost:** \~\$0.55
- **Zero ****`any`**, clean typecheck, graceful failure with full error log

> **Problem 1 verdict:** Claude Code completed the task. Codex failed at tool access through Cursor, but handled the failure correctly. In an environment where Composio is properly wired, this specific failure likely would not occur.

### Problem 2: Real-time collaborative code review UI

This was the point where the comparison got less obvious. I no longer expected one agent to clearly beat the other.

**Prompt:**

```shell
Build a real-time collaborative code review UI in React + TypeScript with these exact requirements:

CORE FEATURES:
1. Diff viewer — side-by-side and unified view toggle
   - Syntax highlighted (use highlight.js or prism, your choice — justify it)
   - Line numbers on both sides
   - Virtual scrolling for diffs > 300 lines (no DOM thrashing)

2. Inline comments
   - Click any line to open a comment thread on that line
   - Threads show commenter name, timestamp, comment body
   - Reply within a thread
   - Resolve/unresolve a thread
   - Resolved threads collapsed by default, expandable

3. Real-time sync
   - Use WebSockets (build a minimal Node.js WS server alongside the React app)
   - When reviewer A adds a comment, reviewer B sees it appear within 1 second
   - No page refresh required
   - Handle reconnection if WS drops (exponential backoff, max 5 retries)

4. Optimistic updates
   - Comment appears immediately for the person who posted it
   - If server rejects it, roll it back and show an error toast

5. Review status
   - Each reviewer can set status: Commented / Approved / Changes Requested
   - Status shown as colored badge next to reviewer name
   - Overall PR status derived from reviewer statuses (any Changes Requested = blocked)

TECHNICAL REQUIREMENTS:
- React 18 + TypeScript, no any
- Zustand for state management (not Redux, not Context for global state)
- TanStack Query for server state
- WebSocket server in Node.js + TypeScript (separate src/server/ folder)
- CSS Modules or Tailwind only — no styled-components
- No UI component libraries (build the components yourself)
- All components under 200 lines
- Full type coverage on WebSocket message payloads

SEED DATA:
- Hardcode a sample diff of at least 50 lines (you can use any real open source file)
- 2 hardcoded reviewers: "Alice" and "Bob"
- Pre-populate 3 comment threads on different lines so the UI isn't empty on load

Do not start until you outline your component architecture and state shape.
Show me the plan first, wait for my approval, then build.
```
Both agents received the same prompt. Neither saw the other's solution.

### What Claude Code built

Claude showed the architecture plan before writing anything. Then it built 36 source files in **12 minutes 17 seconds** wall-clock.

The store design was the first thing that stood out. Instead of treating optimistic updates as a side effect, it modeled them as first-class state:

```shell
type PendingOp =
  | { kind: "add_thread"; tempThreadId: string; anchorKey: string }
  | { kind: "add_reply"; threadId: string; tempCommentId: string }
  | { kind: "resolve"; threadId: string; previousResolved: boolean }
  | { kind: "set_status"; userId: string; previousStatus: ReviewerStatus };
```
That `previousResolved` and `previousStatus` captured pre-change state at intent time, so rollback is deterministic, not a best-effort guess. Most agents get optimistic updates wrong because they forget this step. Claude didn't.

Virtual scrolling was gated correctly: renders all rows for diffs under 300 lines, switches to `@tanstack/react-virtual` with `measureElement` above it, so dynamic row heights when threads expand don't break scroll offset.

WebSocket reconnection used a real backoff ladder: `[500, 1000, 2000, 4000, 8000]ms`, max 5 retries, with a "reconnecting…" UI state distinct from "connecting…" so the user knows what's happening the second time.

The smoke test, which Claude wrote unprompted, spun up two real WebSocket clients, walked them through add-thread → ack → broadcast → reply → status-change, and asserted round trip:

`[smoke] OK broadcast=3ms`

3ms. The spec said "within 1 second."

- **36 source files**, largest component 123 lines (well under the 200-line cap)
- **Estimated tokens:** \~121,000
- **Estimated cost:** \~\$1.58
- **Zero ****`any`**, clean typecheck, 12m 17s wall-clock

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/claude-code-vs-openai-codex/08fe209958.png)

### What Codex built

Codex also showed an architecture plan. Also produced zero `any`. Also passed the smoke test:

`[smoke] OK broadcast=5ms`

Also passed Zod validation both directions. Also shipped virtual scrolling, optimistic rollback, and WS reconnection with backoff.

But the first time I opened the UI, it crashed immediately. **Maximum update depth exceeded.** React was stuck in an infinite loop.

The cause was in `App.tsx`: a `useEffect` that called `hydrate()` on every render whenever `data` changed, without a guard to ensure it ran only once. Codex spotted the issue after I showed the stack trace and patched it cleanly:

```typescript
const hydratedFromQueryRef = useRef(false);
useEffect(() => {
  if (!data || hydratedFromQueryRef.current) return;
  hydrate(data.reviewers, data.threads);
  hydratedFromQueryRef.current = true;
}, [data, hydrate]);
```
One `useRef` guard, and the loop stopped. After that fix, the UI loaded without crashing.

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/claude-code-vs-openai-codex/9ca00f045e.png)

28 source files vs Claude's 36. Largest component: 67 lines. \~15 minutes wall-clock including an `npm install` stall that required a workaround.

The difference wasn't in what was implemented, every requirement was checked. The difference was in scale and granularity. Claude built 12 TSX components; Codex built 7. Claude's architecture is more decomposed, more separately testable, more "this is going into a production codebase." Codex's is more compact, more "this is the smallest thing that fully satisfies the spec."

The 9 TypeScript errors on first compile (8 missing CSS module declarations, 1 strict number narrowing) were fixed before the final run, so the end state was clean, but it required a fix pass that Claude's run apparently didn't.

- **28 source files**, largest component 67 lines
- **Estimated tokens:** \~99,000
- **Estimated cost:** \~\$1.49
- **Zero ****`any`** after fixes, clean typecheck, \~15 min wall-clock

> **Problem 2 verdict:** Both shipped working real‑time UIs that passed the smoke test. Claude’s came out cleaner on the first run. Codex needed one fix pass for a React loop, but after that it held up. Both are useful in different ways. Pick based on what you’re handing off to next.

---

### What actually broke, and what didn't

I promised "problems that actually break AI agents." Here's the honest accounting.

**What broke in Codex:** Tool resolution on Problem 1. `GITHUB_LIST_PULL_REQUESTS` wasn't accessible from GPT-5.5's execution path in Cursor. This isn't a Codex intelligence failure, it's an environment wiring failure. In a native Codex CLI setup with Composio properly configured, this likely passes. Worth knowing if you're running Codex through Cursor.

**What broke in Claude Code:** The tail of the Problem 2 session contained repeated recap output, the final summary printed three times before the agent settled. Not destructive, but it's a known long-session issue with context-window pressure. The files were correct; the terminal got noisy.

**What didn't break in either:**

Neither leaked `any`. Both `--strict` typechecks passed. Neither hallucinated a Composio tool name, both used `GITHUB_LIST_PULL_REQUESTS` and `GITHUB_GET_A_PULL_REQUEST` correctly. Both implemented real retry loops with declared attempt counts and waits. Both wrote smoke tests. Both got the WS broadcast under 10ms.

That last list is the more important one. A year ago, `any` in a 30-file TS project written by an agent was expected. Getting Composio tool names right was a coin flip. Based on these runs, both agents look meaningfully better at the fundamentals than they did six months ago.

---

### **MCP Configuration: Claude Code vs Codex**

Both support MCP, but the setup differs:

**Claude Code** – Native HTTP + stdio. Drop this in `claude_desktop_config.json` or `.claude/mcp.json`:

```json
{
  "mcpServers": {
    "composio": {
      "url": "https://api.composio.dev/mcp/YOUR_URL",
      "headers": { "x-api-key": "YOUR_KEY" }
    }
  }
}
```
**Codex** – Also supports HTTP (Streamable) natively now. Add to `~/.codex/config.toml`:

```shell
[mcp_servers.composio]
transport = "http"
url = "https://api.composio.dev/mcp/YOUR_URL"
headers = { "x-api-key" = "YOUR_KEY" }
```
Both work. The difference in my test came from environment wiring (Cursor vs native CLI), not MCP support.

---

### **Performance Benchmarks**

The number most comparison posts get wrong. Actual API rates (verified):

- **Opus 4.7:** \$5 / M input, \$25 / M output
- **GPT-5.5:** \$5 / M input, \$30 / M output

Per-token, Opus 4.7 is actually slightly *cheaper* on output than GPT-5.5. The cost gap in this test came from token volume, not per-token pricing.

<table header-row="true">
<tr>
<td>Run</td>
<td>Agent</td>
<td>Est. tokens</td>
<td>Est. cost</td>
</tr>
<tr>
<td>Problem 1 : PR triage</td>
<td>Claude Opus 4.7</td>
<td>\~71,000</td>
<td>\~\$0.92</td>
</tr>
<tr>
<td>Problem 1 : PR triage</td>
<td>GPT-5.5</td>
<td>\~37,000</td>
<td>\~\$0.55</td>
</tr>
<tr>
<td>Problem 2 : Collab UI</td>
<td>Claude Opus 4.7</td>
<td>\~121,000</td>
<td>\~\$1.58</td>
</tr>
<tr>
<td>Problem 2 : Collab UI</td>
<td>GPT-5.5</td>
<td>\~99,000</td>
<td>\~\$1.49</td>
</tr>
<tr>
<td>**Total**</td>
<td>**Claude**</td>
<td>**\~192,000**</td>
<td>**\~\$2.50**</td>
</tr>
<tr>
<td>**Total**</td>
<td>**GPT-5.5**</td>
<td>**\~136,000**</td>
<td>**\~\$2.04**</td>
</tr>
</table>
Claude used about 1.4× more tokens. Total cost difference is roughly 23%, not the 5× the original numbers implied. If you're running these at scale, Claude is more expensive, but not in a "different planet" way. The premium buys you more granular components, a cleaner architecture, and an unprompted smoke test. Whether that's worth \~25% more cost depends on the codebase.

On Problem 1, Claude's extra cost bought you a completed run. Codex's cheaper run produced an empty report because the tool wasn't reachable. That's not a cost vs quality tradeoff, that's an environment constraint you need to solve regardless of which agent you pick.

---

### Where they actually differ

Both agents support HTTP and stdio MCPs. The difference is how they treat MCP in practice. Claude Code behaves like MCP is a native part of the workflow: it verifies tools before acting, builds error handling around tool failures, and treats the MCP server as a primary data source.

36 files vs 28. 12 components vs 7. 123 lines max vs 67. Claude builds more. Whether "more" is better depends on your team size, your code review culture, and how long the codebase is going to live. There's no universal answer.

**Long sessions get noisy with Claude.** The recap loop at the end of the UI run showed up clearly. On a 12-minute session, it was minor. On a 2-hour session, it would be more disruptive. Claude's long-context handling may be better than it was on 4.6, but it still is not clean at scale.

---

### Decision framework

<table header-row="true">
<tr>
<td>If you need...</td>
<td>Use</td>
</tr>
<tr>
<td>Native MCP tool access, verified before coding</td>
<td>Claude Code</td>
</tr>
<tr>
<td>Lowest cost per task</td>
<td>Codex</td>
</tr>
<tr>
<td>More decomposed, separately testable architecture</td>
<td>Claude Code</td>
</tr>
<tr>
<td>Compact implementation that satisfies the spec</td>
<td>Codex</td>
</tr>
<tr>
<td>Unattended runs where MCP environment is controlled</td>
<td>Claude Code</td>
</tr>
<tr>
<td>Fast prototyping, you'll review the output</td>
<td>Codex</td>
</tr>
<tr>
<td>Long sessions without recap drift</td>
<td>Codex</td>
</tr>
<tr>
<td>A smoke test written unprompted</td>
<td>Claude Code</td>
</tr>
</table>
---

### What I'm doing now

Claude Code is my default for anything where MCP access matters, where the architecture will outlive the sprint, or where I want the agent to validate its own output. The unprompted smoke test on Problem 2 is the thing I keep pointing people at. It was the clearest “this agent has actually shipped things like this before” behavior I saw across either run.

Codex through Cursor is what I reach for when the task is self-contained, the spec is tight, and I want something working fast and cheap. The Problem 1 MCP failure is an environment issue I can fix with proper Composio wiring. With that fixed, I would expect that run to pass.

The bigger point was this: both agents passed the TypeScript bar, both got the WS broadcast under 10ms and neither hallucinated a tool name. The fundamentals are solid on both sides. The differences are in architecture philosophy, environment integration, and cost per token, not raw capability. That's a much more useful framing than "which model is smarter."

If I ran this again, I'd give Codex a properly wired MCP environment from the start. And I'd add a third problem, something with a long, multi-turn conversation to really test Claude's recap drift.

Neither agent is perfect. Both are getting better fast. What mattered most in practice: build something real. You'll figure out which one works for you in an afternoon.

---

### Sources & code

- Full test code: [github.com/VarshithKrishna14/Claude-codex-test](https://github.com/VarshithKrishna14/Claude-codex-test)
- Claude PR triage: `src/`
- Claude collab UI: `code-review/`
- GPT-5.5 PR triage: `gpt55-pr-triage/`
- GPT-5.5 collab UI: `gpt55-code-review/`
- Composio MCP dashboard: [dashboard.composio.dev](https://dashboard.composio.dev/)
- Thomas Wiegold's OpenCode comparison: [thomas-wiegold.com/blog/i-switched-from-claude-code-to-opencode](https://thomas-wiegold.com/blog/i-switched-from-claude-code-to-opencode/)
- Morph's OpenCode vs Claude Code deep dive: [morphllm.com/comparisons/opencode-vs-claude-code](https://www.morphllm.com/comparisons/opencode-vs-claude-code)
- Claude Opus 4.7: [anthropic.com/news/claude-opus-4-7](https://www.anthropic.com/news/claude-opus-4-7)
- GPT-5.5: [openai.com/index/introducing-gpt-5-5](https://openai.com/index/introducing-gpt-5-5)

---

## FAQ

### **Q1: Which agent should I use for my daily work?**

**A:** It depends on the shape of the work. For greenfield features with real architectural choices, especially real-time UIs, I’d start with **Claude Code**. For tight, self-contained tasks where you want something fast, cheap, and easy to run in CI, I’d reach for **Codex**. Many developers will probably keep both installed.

### **Q2: Is Claude Code really 5× more expensive?**

**A:** Not in this test. The comparison used public API pricing for Opus 4.7 and GPT-5.5, and the token gap was only about 1.4×. Most of the cost difference came from per-token pricing, not wildly different token usage. For smaller tasks, or if you use cheaper Claude models like Sonnet, the gap narrows.

### **Q3: Can I run Codex with HTTP MCPs like Composio?**

**A:** Yes. Codex supports Streamable HTTP MCP servers. Add an `[mcp_servers]` block in `~/.codex/config.toml`, set `transport = "http"`, and provide the `url` and auth `headers`. That removes the need for the older `stdio` proxy adapter.

### **Q4: Would the results change with a different MCP provider?**

**A:** Possibly. Composio was not the main variable here. The bigger issue was that MCP access paths differed between Claude Code and Codex-through-Cursor environments. That kind of wiring issue could show up with any provider. The difference is less about the tool vendor and more about how each agent environment exposes tools.

---
