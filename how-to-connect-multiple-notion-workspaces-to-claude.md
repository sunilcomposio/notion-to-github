# How to Connect Multiple Notion Workspaces to Claude

You connect Notion to Claude. It works beautifully. Then you try to add your client's workspace and find there's nowhere to put it.

Claude connects to **one Notion workspace at a time**. Switching to the other means disconnecting and reconnecting through Settings → Connectors and that isn't a per-chat toggle, it affects every session you have running.

If you're an agency, a consultancy, or a freelancer, this bites immediately. Your delivery wiki is in your workspace. The client's project tracker is in theirs. Half your job is moving information between the two.

The fix is Composio: one custom connector in Claude, holding as many Notion workspaces as you need, each labelled with the client's name.

> Compare the deliverable list in **acme-notion** against our scope doc in **internal-notion**. Flag anything in theirs that isn't in ours. Read only.

That question is impossible with the built-in connector, and it's most of an agency's scope-creep detection.

---

## Before you start

**Claude:**

- Custom connectors work on Free, Pro, Max, Team and Enterprise. **Free is limited to one custom connector**, so this would have to be it.
- On **Team/Enterprise**, only an Owner can add the connector at organisation level. Members then connect their own account to it.

**Composio:**

- A free account. 100K tool calls a month, unlimited connections, no card.

**Client side:**

- At minimum, guest access to the pages you want Claude to reach.
- The ability to complete an OAuth flow against that workspace. Some workspace owners restrict integrations.

---

## Step 1 — Add Composio as a custom connector

Composio publishes one shared MCP endpoint. Same URL for everyone, no API key, OAuth on click.

1. In Claude, open **Customize → Connectors** (`https://claude.ai/customize/connectors`).
2. Click the **+** button, then **Add custom connector**.
3. Fill in:
	- **Name:** `Composio`
	- **URL:** `https://connect.composio.dev/mcp`
	- **Advanced settings:** leave blank. No client ID, no secret, no headers; Composio handles auth automatically.
4. Click **Add**, then **Connect**. A browser window opens; sign in to Composio (or create the free account) and approve.
5. Back in Claude, confirm Composio shows with a **CUSTOM** badge and is toggled on.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-notion-workspaces-to-claude/image_1.png)

**Sanity check.** New chat: *"What Composio tools do you have available?"* You should get a short list of meta-tools.

> Composio doesn't dump every app's tools into your context, it exposes a handful that discover and run the right tool on demand, which is how one connector fronts 1,000+ apps without wrecking your context window.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-notion-workspaces-to-claude/image_2.png)

---

## Step 2 — Connect your first Notion workspace

Everything from here happens in chat:

> Connect my agency's Notion workspace to Composio. Label it **internal-notion**.

Claude returns an authorisation link. Click it, and then **slow down**, because Notion's consent screen has two things Slack's and Google's don't.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-notion-workspaces-to-claude/image_3.png)

**The workspace selector.** Notion authorisation covers exactly one workspace, and the screen picks one by default. Change it if it's wrong.

**The page picker.** Notion asks which pages the integration can access, and this is the real permission boundary. **Whatever you don't select, Claude cannot see** — you'll get "could not find page" errors later and waste an hour assuming the connection is broken.

**Whatever you don't select, Claude cannot see.** It will return "could not find page" errors later, and you will spend an hour assuming the connection is broken when the connection is fine.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-notion-workspaces-to-claude/image_4.png)

How to choose:

- **Your own workspace:** selecting broadly is usually fine.
- **A client workspace:** select the shared project space, not everything. Narrow is almost always right, and it's the answer you want to be able to give when they ask what your AI can see.
- **Either way, prefer a parent page** over hand-picking a dozen children. Children inherit, and it saves you the maintenance problem below.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-notion-workspaces-to-claude/image_5.png)

After approving, come back to Claude. It polls until the connection is Active, then confirms.

That label — `internal-notion` — is the mechanism. Composio calls it an **alias**, and it's how every future request routes to the right workspace. Aliases must be unique per toolkit.

---

## Step 3 — Connect the second, third, fourth workspace

Same conversation:

> Now connect another Notion workspace. Label it **acme-notion**.

Composio issues a **fresh** authorisation link and creates a separate connection. It does not overwrite the first, that's the whole point.

Same two checks every time: **workspace selector**, then **page picker**. If you're signed into several Notion workspaces in that browser, consider opening each auth link in an incognito window and signing in deliberately.

Repeat per client. Composio enforces a cap on accounts per toolkit; the SDK default is 5, configurable between 2 and 10, so an agency past five active Notion clients should raise it or split across Composio projects.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-notion-workspaces-to-claude/image_6.png)

---

## Step 4 — Verify before you trust it

> List all my connected Notion accounts in Composio with their aliases and status.

Only connections marked **ACTIVE** can execute tools.

Then prove each alias reaches the workspace *and pages* you intended:

> For each connected Notion account, search for a page and tell me which workspace it came from, next to its alias.

This catches both failure modes at once: two aliases pointing at the same workspace, and a connection that authorised fine but has access to almost nothing because the page picker was skipped.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-notion-workspaces-to-claude/image_7.png)

---

## Step 5 — The Composio dashboard flow

Chat is faster for connecting. The dashboard is where you audit, rename, revoke and debug.

Go to **`dashboard.composio.dev`** and sign in with the account you authorised in Step 1.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-notion-workspaces-to-claude/image_8.png)

### First: know which surface you're on

Composio's dashboard splits in two, and this confuses everyone exactly once:

| Surface | What lives there |
| --- | --- |
| **For You** | Personal app connections for agents and MCP clients — the setup you just built |
| **Platform** | Developer projects: Playground, API Keys, Toolkits, Skills, Users, Sessions, Auth Configs, Triggers, Logs |

**Resources in one surface do not appear in the other.** If your Notion connections aren't where you expect, you're on the wrong surface. Use the **product switcher**.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-notion-workspaces-to-claude/image_9.png)

### For You — the click-through path

The point-and-click equivalent of Steps 2 and 4, and the better route when onboarding several clients in one sitting:

1. Open **For You** from the switcher.
2. Find **Notion** in the app list.
3. Click **Connect** and complete the OAuth flow; workspace selector, then page picker.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-notion-workspaces-to-claude/image_10.png)

1. Click **Connect on Notion again(Connect New) **for the next workspace. It adds a separate connection rather than replacing the first.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-notion-workspaces-to-claude/image_11.png)

1. Label each connection with the client name.

From here you can also **rename** a connection, **disconnect or delete** one (immediate), and **re-authorise** an expired one.

**Re-authorising is also how you widen page access.** If you need Claude to reach a page you didn't select originally, reconnect that workspace and include it at the picker.

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/how-to-connect-multiple-notion-workspaces-to-claude/image_12.png)

---

---

---

## FAQ

### **Can Claude connect to two Notion workspaces natively?**

No. One at a time, per Anthropic's own open issue, which names agency and freelance users as the affected group.

### **Can I add the connector twice under different names?**

No. Claude rejects a server URL that's already registered.

### **Does switching workspaces affect my other chats?**

Yes. Anthropic's issue notes that disconnecting and reconnecting affects all active sessions, not just the conversation you're in. It isn't a per-chat toggle.

### **Does this work in Cowork and Claude Code?**

Yes. Cowork uses the same Customize → Connectors flow. Claude Code takes the same endpoint as an HTTP MCP server, or Composio's native agent plugin via the CLI.

### **Do my workspaces get merged?**

No. Each connection keeps its own token and stays separately addressable. That's why alias routing works.

### **How many workspaces can I connect?**

Default cap is 5 per toolkit, configurable from 2 to 10.

### **What about pages a client shares with me as a guest?**

Guest-accessible pages come along with the workspace you connected, provided you select them at the page picker. composio it explicitly rather than assuming, guest access behaviour is the murkiest corner of this and worth verifying per client.

### **Can each person on my team have their own connections?**

Yes, Composio scopes connections per user, so two people never touch each other's credentials. Shared connections with per-user access control exist if you'd rather hold one agency connection per client.

### **Will clients see we've connected something?**

Yes, and they should. Integrations are visible to Notion workspace owners. Tell them first.

---

## The short version

Claude connects to one Notion workspace at a time, switching disrupts every active session, and the duplicate-connector workaround is blocked. It's an open issue with agency users named in it.

Add `https://connect.composio.dev/mcp` under **Customize → Connectors**. Connect each workspace with a `client-notion` alias. On every OAuth screen, check the workspace selector, then take the page picker seriously, that screen is the actual permission boundary, and skimming it is why most setups half-work.

Then write the guardrail block into a project per client before you run anything that writes.
