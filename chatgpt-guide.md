OpenAI recently launched ChatGPT Atlas, a fork of Chromium with Agentic capabilities. The UI is clean, rebuilt with SwiftUI, AppKit and Metal, but take that away and it’s the same capabilities, you can already access on ChatGPT’s website.

Is it really that hard to get agentic capabilities in your browser? Do you really need another browser for it? Turns out, no, a Chrome extension can do more than solve your problem. I spent my weekend building one, and here’s how you can make it too.

Here’s the demo:

Here’s the code for the project: [https://github.com/ComposioHQ/open-chatgpt-atlas](https://github.com/ComposioHQ/open-chatgpt-atlas)

## Why Chrome Extensions Are Actually Perfect For This

Before diving into the build, let me explain why a Chrome extension is the right approach. The first question I had to answer was: Can a Chrome extension do what an AI browser can do?

The answer is yes, and here's why:

**1\. Extensions have access to everything that matters:**

- They can take screenshots of the current tab

- They can inject JavaScript into any page

- They can listen to page navigation events

- They can create UI (sidepanel, popups, context menus)

- They run with elevated permissions, the webpage doesn't have

**2\. They're easier to distribute:**

- No install process, just add to Chrome

- Updates happen automatically

- Works on any OS that runs Chrome

- Users don't have to abandon their existing browser setup

**3\. They're cheaper to build:**

- No need to maintain a Chromium fork

- No need to handle browser-level features (tabs, bookmarks, updates)

- Focus purely on the agent capabilities

The UI is simple. All you need is a sidebar in the browser where the AI agent can take actions, and for anything the agent can't or won't do through browser automation, you can use MCP (Model Context Protocol) to route to external tools.

## Architecture

The first step was deciding on the LLM. There were three providers with top-tier models: OpenAI, Anthropic, and Google.

OpenAI and Anthropic both charge for their APIs, with no free tier. This means a lot of people won't be able to access or build on top of them without immediately hitting a paywall. I wanted this to be something other developers could fork and experiment with without worrying about bills.

Google, on the other hand, offers a generous free tier for Gemini models that most people can access and build on top of. The free tier gives you 150 requests per minute for gemini 2.5 pro, which is way more than you'll need unless you're running this commercially. Gemini 2.5 Computer Use is also cheaper and faster than Claude’s Computer Use with Sonnet 4.5.

Setting up a Chrome extension is actually pretty straightforward. The core file is the `manifest.json`It defines what the extension is and what permissions it needs. What we need is a Chrome extension that sits in the sidebar and can take actions on the open browser. This means we need:

- A `manifest.json` that declares permissions and entry points

- A `background.ts` file that runs as a service worker, listening for messages from the sidepanel

- A `content.ts` that gets injected into webpages and can extract page content and execute actions

- UI files: `sidepanel.tsx` (React), `settings.tsx`, and their corresponding HTML/CSS

Taking the above requirements into consideration, here's how the file directory looks:

```plain text
atlas-extension/
├── Core Extension Files
│   ├── manifest.json             // Chrome extension config
│   ├── background.ts             // Message router & coordinator
│   ├── content.ts                // Injected into pages, executes actions
│   ├── sidepanel.tsx             // Main chat interface (React)
│   ├── types.ts                  // TypeScript interfaces
│   ├── tools.ts                  // Composio tool definitions
│   ├── settings.tsx              // API key configuration
│   ├── settings.html
│   └── sidepanel.html
│
├── Config Files
│   ├── package.json
│   ├── vite.config.ts            // Build tool (bundles TS → JS)
│   ├── tsconfig.json
│   └── tsconfig.node.json
│
├── Styling
│   ├── sidepanel.css
│   └── settings.css
│
└── Assets
    └── icons/
        └── icon.png
// 16x16, 48x48, 128x128

```

## The Permissions That Make It Work

These are the permissions we need from Chrome that make a browsing agent work:

```plain text
"permissions": [
  "sidePanel",      // Create sidebar UI
  "storage",        // Save API keys, settings to chrome.storage.local
  "tabs",           // ⭐ CRUser types command in Sidepanel
  "history",        // Read browser history (for context)
  "bookmarks",      // Read bookmarks (for context)
  "webNavigation",  // Track when pages load/unload
  "scripting",      // Inject content scripts dynamically
  "contextMenus"    // Add right-click menu items
],

```

The most important one is `tabs`. This is what lets you capture screenshots of the current page, which is essential for computer use. Without screenshots, the AI is blind—it has no idea what the page actually looks like, so it can't make intelligent decisions about where to click or what to type.

The `scripting` Permission is also critical because it allows you to inject `content.ts` into any webpage dynamically. This is how you execute actions on the page—clicking buttons, filling forms, scrolling, etc.

## System Architecture: How Messages Flow

Here's how the pieces talk to each other:

The background.ts is the central nervous system. It’s always running, and it coordinates everything. When you send a message from the side panel, this worker routes it to the correct flow.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/chatgpt/image_1.png)

## Computer Use: The Browser Automation Loop

Here’

**Step 1:** The agent captures a screenshot of the current page state using `chrome.tabs.captureVisibleTab()`. This screenshot is the agent's "eyes"—it sees what you see.

**Step 2:** The screenshot gets sent to Gemini along with your natural language intent ("click the login button") and the page's DOM structure (for additional context).

**Step 3:** Gemini analyzes the screenshot, identifies the login button visually, and returns a function call:

```plain text
{
  "action": "click",
  "coordinates": {"x": 450, "y": 320},
  "reasoning": "Found login button at top-right of page"
}

```

**Step 4:** `background.ts` receives this action and forwards it to `content.ts` running on the current webpage.

**Step 5:** `content.ts` executes the click at those coordinates, shows a blue visual indicator to show what happened, and reports success or failure.

**Step 6:** The loop repeats with a fresh screenshot of the new page state. If the click opened a modal, the next iteration sees the modal and can interact with it. If a page is loading, it waits and adapts.

This repeats up to 30 times per task. Each iteration adapts based on what it sees. It's not running a predetermined script—it's genuinely reacting to the current state of the page.

### How content.ts Executes Actions

When `background.ts` receives an `EXECUTE_ACTION` message from Gemini (e.g., `{type: 'EXECUTE_ACTION', action: 'click', coordinates: {x: 100, y: 200}}`), it relays this to `content.ts` running on the current webpage.

The content script's `executePageAction()` function handles 12 different browser actions. Here are the important ones:

**1\. Click:** Uses `document.elementFromPoint(x, y)` to find the element at those coordinates, then fires a click event. If a CSS selector is provided instead, it queries and clicks that element directly.

```plain text
case 'click':
  const element = document.elementFromPoint(x, y);
  if (element) {
    element.click();
    return { success: true, element: element.tagName };
  }
  break;

```

**2\. Fill:** Finds the input/textarea element, focuses it (which triggers any React state updates), then uses `keyboard_type()` to type the text character-by-character. This is important for React apps that listen for input events instead of just checking `.value`.

```plain text
case 'fill':
  const input = document.elementFromPoint(x, y);
  if (input && (input.tagName === 'INPUT' || input.tagName === 'TEXTAREA')) {
    input.focus();
    await keyboard_type(input, text);
    return { success: true };
  }
  break;

```

Why character-by-character? Because if you just set `.value = "text"`, React doesn't know the value changed. You have to dispatch keyboard events for each character so React's synthetic event system picks it up. This was one of those annoying things that took way too long to debug.

**3\. Scroll:** Scrolls the page (or a specific element) up/down/to-top/to-bottom by manipulating `scrollTop` and `scrollLeft` or using `.scrollIntoView()`.

**4\. Keyboard Type:** Types text one character at a time using `dispatchEvent(new KeyboardEvent('keydown'))` and `dispatchEvent(new KeyboardEvent('keyup'))`, mimicking real typing. This is actually faster than setting `.value` because it doesn't cause React to re-render the entire component tree on every character.

**5\. Press Key:** Presses individual keys (Enter, Tab, Escape, etc.) by dispatching keyboard events. Useful for submitting forms or navigating through interfaces.

**6\. Key Combination:** Presses multiple keys simultaneously (Ctrl+A, Cmd+C, etc.) for complex keyboard shortcuts. This is how you can make the agent copy/paste or select all text.

**7\. Drag & Drop:** Simulates drag-and-drop by dispatching `mousedown`, `mousemove`, and `mouseup` events from source to destination coordinates. Useful for dragging sliders or reordering lists.

**8\. Hover:** Moves the mouse cursor to coordinates and fires `mouseover` and `mousemove` events. This is useful for triggering dropdowns or tooltips that only appear on hover. Each action returns a result object (e.g., `{success: true, element: 'BUTTON'}`) back through `background.ts` to the sidepanel, so Gemini can see what happened and decide on the next action. The content script also creates a visual indicator—a blue outline and pulsing circle at the action location—that disappears after 600ms. This gives you real-time feedback of what the agent is doing, which is surprisingly important for building trust. Without the visual feedback, the agent feels like a black box.

**Flow summary:**

Sidepanel calls streamWithGeminiComputerUse()

→ Background.ts captures screenshot

→ Gemini API receives screenshot + DOM

→ Gemini returns function calls

→ Background.ts forwards to content.ts

→ content.ts executes actions

→ Repeat up to 30 times

## Tool Router: External API Integration

Computer use is excellent for browser automation, but what if you need to send a Slack message? Or create a GitHub issue? Or search your Gmail?

That's where the Tool Router comes in. Instead of looping with screenshots and browser actions, you hand off the work directly to specialised external services via Composio's 500+ integrated tools.

The key difference: Computer use is iterative and visual (screenshot → analyze → act → repeat), while the Tool Router is a single API call to an external service. When you need to "send a Slack message," the Tool Router targets the Slack API, sends a single request, and the job is completed on their servers.

The Tool Router handles three critical features:

**1\. Discovery:** Searches across all available tools to find tools that match your task. Returns relevant toolkits with their descriptions, schemas, and connection status. For example, if you say "send an email," it searches and finds `GMAIL_SEND_MESSAGE`, `OUTLOOK_SEND_EMAIL`, etc., and returns them with their parameters so Gemini knows what to call.

**2\. Authentication:** Checks if you have an active connection to the required toolkit. If not, it creates an auth config and returns a connection URL using Composio's Auth Link. You complete authentication via this link, and your credentials are stored securely. 

**3\. Execution:** Loads authenticated tools into context and executes them. Supports parallel execution across multiple tools for efficiency. For example, if you say "find all emails from Bob and create a summary doc," it can: - Search Gmail in parallel - Process results - Call Google Docs API to create the summary - All in one flow

The beauty of this dual approach (Computer Use + Tool Router) is that you can mix them. You can use a computer to navigate to a page and extract information, then use the Tool Router to send that information via Slack. The agent picks which approach to use based on the task.

## The Sidepanel: Where You Actually Use It

The `sidepanel.tsx` is where you interact with the agent. It's a React component that renders in Chrome's sidebar (that panel that slides out from the right side of the browser).

Here's what it does:

**1\. Chat interface:** You type natural language commands ("click the login button", "fill out this form with my details", "send a summary of this page to Slack").

**2\. Live conversation history:** Displays the back-and-forth between you and the agent, including what actions it took and why.

**3\. Mode switcher:** Toggle between two systems:

- **Computer Use (Gemini):** For direct browser automation

- **Tool Router (Composio):** For external API calls to Gmail, Slack, GitHub, etc.

**4\. Visual feedback:** Shows when actions are executing, displays screenshots the agent is analyzing (if you want), and reports errors clearly.

The interface is intentionally minimal. You don't need a complex UI when the agent is doing all the work: just a text input and a conversation history.

## AI Coding Tool Costs

I started with Claude Sonnet 4.5 in Cursor. I set a $50 budget and figured that would last me at least a week. It was gone in three days.

The problem with Sonnet isn't that it's bad at coding—it's excellent at coding. The problem is that it's a token-guzzling machine. Here's where the tokens went:

**1\. Redundant documentation files:** Sonnet loves creating `TECHNICAL_IMPLEMENTATION.md`, `ARCHITECTURE.md`, `CHANGELOG.md`, and other markdown files that have no real utility except maybe giving Claude context on the changes it made. Highly inefficient when you're trying to complete a project quickly.

**2\. Verbose explanations:** Every code change comes with a three-paragraph explanation of *why* it made the change. Great for understanding, terrible for token efficiency.

**3\. Full-file rewrites:** Instead of making targeted edits, Sonnet often rewrites entire files. If you have a 500-line file and need to change one function, Sonnet will regenerate all 500 lines. That's 500 output tokens instead of 20.

Here's what my Cursor usage looked like with Sonnet 4.5:

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/chatgpt/image_2.png)

In the first three days, I'd burned through most of my $50 budget. I was stretching it by switching to Composer mode (which is slower but more thoughtful), but even that wasn't sustainable.

Then Anthropic launched Haiku 4.5, which performs at the same level as Sonnet 4. I was sceptical—usually "performs at the same level" means "performs at 80% of the level for niche tasks"—but I was desperate.

I switched to Haiku 4.5 midway through the project. I completed the remaining work for $30 total.

Here's the difference:

![Image 3](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/chatgpt/image_3.png)

**Key observations:**

**Haiku 4.5:**

- More focused changes, fewer tokens per edit

- Rarely creates unnecessary documentation files

- Makes targeted edits instead of full-file rewrites

- Suggestion acceptance rate is actually higher (because suggestions are more precise)

**Sonnet 4.5:**

- Better at high-level architecture decisions

- More verbose explanations (good for learning, bad for budget)

- More likely to rewrite everything

- Suggestion acceptance rate lower (because it suggests more changes per edit)

**The verdict:** For extension development—or really any project where you know roughly what you need to build—Haiku 4.5 is 95% as good for 30% of the cost.

The 5% where Sonnet is better? Initial architecture decisions, figuring out how to structure something you've never built before, debugging really weird issues. But for "implement this feature" or "fix this bug," Haiku is more than good enough.

## What Claude Got Wrong (So You Don't Waste Hours Like I Did)

Let me save you some pain by documenting the places where Claude Code absolutely struggled. These aren't bugs in Claude—they're gaps in its understanding of Chrome extension architecture and Gemini's API.

### Issue 1: Text Input Wouldn't Work

**Symptoms:** The agent could click buttons, scroll pages, and navigate between screens. But it couldn't type text into input fields. Every time it tried, nothing happened.

**Claude's diagnosis (first 10 attempts):** "The coordinates must be wrong. Let me try calculating them differently."

**Claude's diagnosis (next 10 attempts):** "Maybe the input isn't focused. Let me add a focus event first."

**Claude's diagnosis (next 10 attempts):** "The timing might be off. Let me add delays between keystrokes."

**Actual problem:** Gemini requires human-in-the-loop permission for tasks it considers sensitive, like typing text. By default, it blocks text input actions entirely unless you explicitly tell it not to.

```plain text
// In your Gemini API config
const response = await fetch('<https://generativelanguage.googleapis.com/v1beta/>...', {
  method: 'POST',
  body: JSON.stringify({
    contents: [...],
    tools: [...],
    safety_settings: [
      {
        category: "HARM_CATEGORY_DANGEROUS_CONTENT",
        threshold: "BLOCK_NONE"  // ← This is what you need
      }
    ]
  })
});

```

You can also manually reduce the guardrails Gemini has active by default. There's a `safety_settings` parameter that lets you control how conservative the model is about "dangerous" actions.

**Time wasted:** 2+ hours across multiple sessions

**The kicker:** This is documented in Google's Gemini Computer Use guide, but Claude never thought to check there. It was convinced it was a coordinate or timing issue.

**Lesson:** When working with computer use models, always check their specific documentation for permission and safety settings BEFORE debugging for hours. The model might be refusing to do something, not failing to do it.

### Issue 2: Screenshot Capture Hell

**Symptoms:** The computer use loop would start, send the first screenshot, Gemini would respond with an action, and then the extension would crash when trying to capture the second screenshot.

**Claude's diagnosis (attempt 1-5):** "The screenshot might be too large. Let me try compressing it more."

**Claude's diagnosis (attempt 6-10):** "Maybe the format is wrong. Let me try converting PNG → JPG."

**Claude's diagnosis (attempt 11-15):** "Let me try JPG → PNG instead."

**Claude's diagnosis (attempt 16-30):** Variations of the above, trying different quality settings, different compression libraries, different encoding methods.

**Actual problem:** Chrome extensions can't capture screenshots from the sidebar context. The sidebar runs in its own isolated context and doesn't have access to the main window's visual content.

**The wrong approach (what Claude kept trying):**

```plain text
// This doesn't work from sidepanel context
const screenshot = await chrome.tabs.captureVisibleTab();
// Error: No tab found

```

**The right approach:**

```plain text
// You need to query for the main window's active tab first
const tabs = await chrome.tabs.query({active: true, currentWindow: true});
if (tabs[0]) {
  const screenshot = await chrome.tabs.captureVisibleTab(tabs[0].windowId, {
    format: 'png'
  });
}

```

The difference is subtle but critical. The sidebar doesn't have a concept of "current window" in the same way a content script does. You have to explicitly query for the active tab and specify its window ID.

**Time wasted:** 1+ hour, 30+ different approaches

**Moment of realization:** I finally found this buried in a GitHub issue from 2019 where someone else had the exact same problem. It's not in Chrome's official documentation.

**Lesson:** Claude doesn't understand Chrome extension context boundaries. When it fails to capture something that should work, check if you're in the right context (background vs content vs sidepanel vs popup).

This specific fix resolved the frustration from a long debugging session where I was trying every possible variation of the screenshot API without understanding the fundamental issue.

### Issue 3: Permission Manifest Confusion

**Symptoms:** Some Chrome APIs would work in development but fail in production after packaging the extension.

**Claude's diagnosis:** "The manifest permissions must be incomplete. Let me add more permissions."

**What Claude did:** Added every permission that sounded remotely related: `activeTab`, `tabs`, `<all_urls>`, `webRequest`, etc.

**Actual problem:** Chrome has different permission requirements for MV3 (Manifest V3) extensions vs MV2. Claude kept suggesting MV2 patterns because most Stack Overflow answers are from the MV2 era.

**The fix:** Understanding the difference between MV3's service workers and MV2's background pages, and adjusting the manifest accordingly.

**Time wasted:** 30 minutes

**Lesson:** Always check which manifest version you're using and make sure Claude's suggestions match that version. The APIs are similar but the permission model is different.

## Try It Yourself

I've open-sourced the code at [github.com/composiohq/open-chatgpt-atlas](http://github.com/composiohq/open-chatgpt-atlas). Here's how to get started:

**Setup (5 minutes):**

1. Clone the repo: `git clone ...`

1. Install dependencies: `npm install`

1. Build the extension: `npm run build`

1. Load in Chrome: Go to `chrome://extensions`, enable Developer Mode, click "Load unpacked", select the `dist` folder

1. Get a Gemini API key: [https://ai.google.dev/](https://ai.google.dev/)

1. Open the extension, go to Settings, paste your API key

**First task:** Open any webpage, click the extension icon, and try: "Click the search button and type 'AI browser agents'"

Watch the blue flashes as it executes each action. If it fails, check the console for errors (right-click the extension → Inspect).


