---
title: "Per-User OAuth for AI Agents: Why It Matters and What to Look For"
source: https://www.notion.so/composio/Per-User-OAuth-for-AI-Agents-Why-It-Matters-and-What-to-Look-For-352f261a6dfe80aab1d6e88bfa06241f
notion_page_id: 352f261a-6dfe-80aa-b1d6-e88bfa06241f
last_synced: 2026-05-20T10:55:48Z
---

# Per-User OAuth for AI Agents: Why It Matters and What to Look For

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/per-user-oauth-for-ai-agents/7f79c49d76.png)

AI agents are crossing a line that traditional software never had to. They read your Slack, draft your emails, push code, update your CRM, and pay your invoices. To do that, they need keys to systems that belong to specific people. Not the application. Not the company. The person.

That is the entire reason per-user OAuth exists in the agent context, and it is the difference between a side project and something a customer will trust with their Gmail account.

This article breaks down what per-user OAuth means for AI agents, why shared credentials fall apart at scale, what the emerging standards look like, and the exact checklist to use when picking a platform to handle it. We will also show how [Composio](https://composio.dev/) approaches each of these problems so you do not have to assemble the stack yourself.

## The problem with the way most teams start

Most agent prototypes start with a single API key in an environment variable. It works for one developer, on one machine, for one demo. The moment a real user shows up, the model breaks.

API keys identify the calling application, not the user behind the action. Every request from the agent looks the same to the downstream service. There is no concept of consent, no scoping per user, no way to revoke one person's access without invalidating everyone, and no audit trail that says "this action happened because Sarah asked the agent to do it."

This becomes a real security problem fast. If the agent code accidentally passes the wrong user identifier, or an attacker tricks the agent into requesting data for a user who did not authorize it, the agent has no protocol-level defense. This is the classic confused deputy problem, and it scales horribly with autonomous systems that chain dozens of tool calls per task.

Composio's own [guide on AI agent infrastructure](https://composio.dev/blog/secure-ai-agent-infrastructure-guide) calls this hitting the "Authentication Wall." It is the moment a promising prototype stops being promising.

## What per-user OAuth actually solves

Per-user OAuth flips the model. Instead of the agent holding one master credential, each user grants the agent a scoped, revocable token tied to their own account. The agent acts with that user's identity, within the limits that user approved, for as long as that user allows.

Concretely, this gives you:

**Explicit consent.** The user sees what the agent is asking for and approves it. No assumed access.

**Scoped permissions.** The token can be limited to read-only access on a specific resource rather than full account control. If the agent only needs to read, that is all it gets.

**Short lifetimes.** Access tokens expire in minutes to an hour. Refresh tokens rotate. A leaked token is dangerous for a short window, not forever.

**Selective revocation.** Revoking one user's grant does not break the agent for everyone else.

**Identity in the audit log.** Every action traces back to a specific user, which is what SOC 2, HIPAA, and ISO 27001 actually require.

**Multi-tenant isolation.** Each user's tokens live in their own bucket, encrypted at rest. A bug in one tenant's workflow does not expose another tenant's data.

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/per-user-oauth-for-ai-agents/28ebb110d8.svg)

That last one matters more than people think. Composio's [user and session model](https://docs.composio.dev/docs/users-and-sessions) is built around exactly this idea: a user is an identifier from your app, every connection lives under that user's ID, and connections are fully isolated between users. The same agent code can serve thousands of users without any of them ever touching each other's data.

## The standards that are actually shaping this

The protocol layer is moving fast. There are three things worth knowing.

### OAuth 2.1 with mandatory PKCE

OAuth 2.1 is the current best-practice consolidation of OAuth 2.0. It makes PKCE (Proof Key for Code Exchange) mandatory and removes older, less secure flows. PKCE matters specifically for agents because most agents are public clients running in environments where you cannot reliably hide a client secret. PKCE prevents an attacker from intercepting an authorization code mid-flow.

If a platform you are evaluating does not enforce PKCE, that is a red flag.

### The MCP Authorization spec

The [Model Context Protocol authorization specification](https://modelcontextprotocol.io/specification/2025-03-26/basic/authorization) formalized OAuth as its standard in 2025. It mandates OAuth 2.1 with PKCE, requires Authorization Server Metadata discovery via RFC 8414, and supports Dynamic Client Registration via RFC 7591. The November 2025 update added step-up authorization, letting clients request additional scopes only when an operation actually requires them, rather than over-permissioning the initial token.

The spec also had a real-world security crisis to address. In late 2025, security researchers at Obsidian Security disclosed [one-click account takeover vulnerabilities](https://www.obsidiansecurity.com/blog/when-mcp-meets-oauth-common-pitfalls-leading-to-one-click-account-takeover) in remote MCP servers from several well-known organizations. The root cause: many MCP servers were implemented as OAuth proxies using a single static `client_id` to talk to the upstream SaaS authorization server. Once any user consented for that shared client_id, the SaaS auth server cached the decision. An attacker could then complete the MCP-layer consent themselves, send a crafted authorization link to a victim, and the upstream server would skip the consent prompt entirely because it had seen that client_id before. The authorization code would be issued to the attacker's redirect URI.

The fix is per-client identity and strict consent handling at the proxy layer. Composio's [Tool Router](https://composio.dev/toolkits/composio) gives each session a secure, user-scoped MCP URL rather than a shared endpoint, which sidesteps this class of attack structurally.

### The IETF "On-Behalf-Of" draft for AI agents

There is an active IETF draft, [draft-oauth-ai-agents-on-behalf-of-user](https://datatracker.ietf.org/doc/draft-oauth-ai-agents-on-behalf-of-user/), that extends OAuth specifically for agent delegation. It adds a `requested_actor` parameter so the consent screen shows the agent's identity (not just the app), and an `actor_token` parameter so the agent authenticates itself when exchanging the authorization code.

The result is an access token that documents the full delegation chain: the user delegated to this client application, which delegated to this specific agent. That chain is what makes after-the-fact auditing possible.

The draft is at revision 02 as of August 2025 and has not been adopted by the working group yet, but it points clearly at where the protocol layer is headed. Multi-hop delegation (agent A calling agent B on the same user's behalf) is still an open problem in the spec.

## Production architecture: where teams fail

Getting an access token is the easy part. Operating at production scale is where most homegrown OAuth implementations collapse. Five things tend to go wrong.

**Token storage.** Tokens have to be encrypted at rest, isolated per tenant, never logged, and never placed in LLM context. The last point is non-obvious and critical: if you put a refresh token in the prompt, a prompt injection attack can exfiltrate it. The pattern to use is brokered credentials, where the LLM never sees the token at all and a separate service makes the actual API call.

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/per-user-oauth-for-ai-agents/0a6cab3007.svg)

Composio's [secure infrastructure guide](https://composio.dev/content/secure-ai-agent-infrastructure-guide) explains this pattern directly: the LLM asks Composio to perform an action, Composio calls the upstream API with the stored credential, and the token never enters the model's context window.

**Refresh handling.** Refresh tokens need proactive coordination. Waiting for a 401 error and then refreshing creates race conditions, cascading retries, and unstable background jobs. Composio handles refresh automatically and only marks a connection as `EXPIRED` after multiple refresh attempts have failed, according to its [authentication docs](https://docs.composio.dev/docs/authenticating-tools).

**Scope discipline.** Composio requests sensible default scopes for each toolkit but lets you override them via [custom auth configs](https://docs.composio.dev/docs/custom-auth-configs). Tightening scopes shrinks the blast radius if something goes wrong. Most APIs still have coarse-grained scopes, which means even with discipline, agents tend to be over-permissioned. The mitigation is short token lifetimes and per-tool scoping where the API supports it.

**Branding and consent screens.** When users hit an OAuth consent screen that says "Composio wants to access your Gmail" instead of "YourProduct wants to access your Gmail," conversion drops and trust takes a hit. Composio's [white-labeling support](https://docs.composio.dev/docs/custom-app-vs-managed-app) lets you bring your own OAuth app credentials so the consent screen shows your brand. Use managed apps for prototyping and your own credentials for production.

**Multi-account per user.** Some users connect a personal Gmail and a work Gmail. The platform needs to model that without forcing them to share a user ID across both. Composio handles this with the connected account ID layered under the user ID, so a single user can have multiple accounts on the same toolkit, as documented in the [users and sessions guide](https://docs.composio.dev/docs/users-and-sessions).

## What to look for in a per-user OAuth platform for agents

If you are evaluating providers, these are the non-negotiables:

1. **Per-user token isolation** with encryption at rest and no cross-tenant leakage
2. **OAuth 2.1 with mandatory PKCE** for all OAuth flows
3. **Automatic, proactive token refresh** that does not depend on the client
4. **Short-lived access tokens** with rotating refresh tokens
5. **Granular scope configuration** so each integration uses least privilege
6. **Brokered credentials** so the LLM never sees raw tokens
7. **Selective per-user revocation** without affecting other users
8. **Audit trail on the delegation chain** for compliance
9. **White-label OAuth consent screens** for production trust
10. **Step-up authorization** to request new scopes only when needed
11. **Multi-account per user** for personal and work accounts on the same app
12. **MCP-native deployment** so any MCP-compatible client can use the same auth layer
13. **SOC 2 Type 2 and ISO 27001 compliance** as table stakes for enterprise customers
14. **SDK support across major frameworks** (LangChain, CrewAI, OpenAI Agents SDK, Claude Agent SDK, Mastra, Vercel AI SDK, LlamaIndex)

Composio covers every item on that list today. It supports 500+ toolkits, handles OAuth end-to-end with user-scoped tokens, is SOC 2 Type 2 and ISO 27001 compliant, and works as both a direct SDK and an MCP server. The full feature breakdown is on the [AgentAuth product page](https://composio.dev/agentauth) and the [comparison guide](https://composio.dev/content/ai-agent-authentication-platforms).

## What this looks like in code

Per-user OAuth with Composio compresses into a handful of lines. The pattern below follows the current SDK as documented in Composio's [authenticating tools guide](https://docs.composio.dev/docs/authenticating-tools).

First, install the SDK and set your API key:

```bash
pip install composio
export COMPOSIO_API_KEY=your_api_key_here
```
To trigger the OAuth flow for a specific user, use the hosted Connect Link pattern. This returns a redirect URL the user opens in their browser to complete authentication:

```python
from composio import Composio

composio = Composio(api_key="your_api_key")

# Use the "AUTH CONFIG ID" from your Composio dashboard
auth_config_id = "your_auth_config_id"

# Use a unique identifier for each user in your application
user_id = "user_1349_129_12"

connection_request = composio.connected_accounts.link(
    user_id=user_id,
    auth_config_id=auth_config_id,
    callback_url="https://your-app.com/callback"
)

redirect_url = connection_request.redirect_url
print(f"Visit: {redirect_url} to authenticate your account")
```
After the user completes the flow, Composio stores the tokens, links them to that user ID, and handles refresh automatically. The agent code never touches the token directly.

To fetch tools scoped to a specific user, pass that same user ID:

```python
from composio import Composio

composio = Composio(api_key="your_api_key")

user_id = "user_1349_129_12"

# Tools are automatically scoped to this user's connected accounts
tools = composio.tools.get(user_id=user_id, toolkits=["GITHUB", "GMAIL"])
```
Two users running the same workflow get two different sets of credentials applied transparently:

```python
tools_user_1 = composio.tools.get(user_id="user_1", toolkits=["GITHUB"])
tools_user_2 = composio.tools.get(user_id="user_2", toolkits=["GITHUB"])

# Each set of tools uses the respective user's credentials
# when invoked by an agent
```
That is the whole point of per-user OAuth: the auth layer disappears into the platform, and the agent code reads like it is talking to a single user even when it is serving thousands.

For a full working example with an LLM framework, see the framework-specific quickstart guides in [Composio's documentation](https://docs.composio.dev/docs).

## The takeaway

Per-user OAuth is not a feature you bolt onto an agent product later. It is the foundation that decides whether your agents can serve real customers at all. Shared API keys cap your ceiling at a single-user demo. Per-user OAuth opens the door to multi-tenant production deployment, enterprise compliance, and the kind of trust required to handle a customer's inbox, calendar, or revenue pipeline.

The protocol layer is still evolving. OAuth 2.1, the MCP authorization spec, and the IETF on-behalf-of draft are all converging on the same answer: explicit user consent, scoped delegation, audited token lifecycle, and isolation per user. Build for that model now and you will not need to retrofit later.

If you would rather skip the months of building it yourself, [start with Composio](https://composio.dev/) or read the [auth-to-action guide](https://composio.dev/blog/secure-ai-agent-infrastructure-guide) for the full architecture. The shortest path from prototype to production is using a platform that already solved this.