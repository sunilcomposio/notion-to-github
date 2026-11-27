# Why engineering teams keep building MCP gateways

Sierra, Roblox, DoorDash, Uber, and Databricks have published approaches to governing how agents reach company systems. Their architectures differ, but the recurring decision is the same: put a shared control point between an agent and the tools it can use.

For platform teams designing an internal MCP gateway, these accounts are useful design references. They show where a proxy is sufficient, where policy needs application context, and where operating the gateway becomes an ongoing product responsibility.

This review compares five engineering problems: credential custody and identity, permissions on business data, tool discovery, usage limits, and safe operation as tools and policies change. The published accounts emphasize different problems; an omitted feature is not evidence that a company lacks it.

## What an MCP gateway controls

An MCP gateway sits between agents and the MCP servers or APIs they use. For requests routed through it, the gateway can enforce access policy, manage credentials, and record an audit trail. Its authority depends on which execution paths the organization actually routes through it.

MCP gives agents a common interface for discovering and calling tools. A gateway gives an organization a place to decide how those calls should work: which tools are available, whose credentials apply, what may execute, and what gets recorded. A production design also has to make that route convenient enough for people to use.

![Iceberg diagram: one endpoint above the surface, with identity and credentials, application policy, usage and audit, and ongoing maintenance below](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/why-engineering-teams-keep-building-mcp-gateways/01-the-work-behind-the-endpoint.webp)

Figure 1. The work behind one endpoint. Original Composio illustration, inspired by [Sierra’s engineering iceberg](https://sierra.ai/blog/building-sierras-mcp-gateway-an-engineering-iceberg).

I work on Composio’s [MCP gateway](https://composio.dev/mcp-gateway), and building it and working through deployments with engineering teams of all sizes has shown me that connecting tools is only the first step. Once teams put those tools to use, buyers ask practical questions: Why did the bill increase? Does a tool have more access than it needs? What happens when a scheduled job runs through an employee’s account? Teams also need to manage cost, permissions, and account ownership.

## Roblox: credential isolation is only part of the problem

In an internal security test at Roblox, a hidden instruction in a GitHub issue persuaded a coding agent to upload credentials to a public repository. [Roblox’s account](https://about.roblox.com/newsroom/2026/07/roblox-unveils-security-research-tools-black-hat-bsides-las-vegas) explains why sandboxing alone leaves important risks unresolved.

In conversations with the Roblox engineering team and in its Black Hat presentation, the gateway appeared alongside agent identity, credential brokering, and network controls. Roblox has since [published the credential architecture](https://about.roblox.com/publications/how-roblox-gives-ai-agents-access-without-giving-them-credentials): the agent uses an identity tied to an engineer, a broker obtains short-lived access, and the gateway attaches vendor credentials outside the agent process. On this execution path, the agent requests an operation without receiving the vendor credential used to perform it.

The network controls matter as much as the gateway. A gateway controls the requests it receives; the surrounding environment has to control alternative routes.

In enterprise deployments, we have to examine both. Keeping a token away from an agent does not prevent it from misusing an authorized tool. An agent allowed to read a document and post to a repository may still move data between them. Buyers need to establish where credentials live, which routes exist, and which actions remain possible. Credential isolation answers one of those questions.

For a platform team, credential custody creates an enforcement point just before an authenticated request reaches a real system. The trusted execution service should bind the caller to an account, evaluate the final operation and arguments, and only then attach the credential. A model-supplied user ID is not proof of identity. Keeping secrets out of prompts is also insufficient if the agent can read them from its environment or files.

The boundary must cover alternative paths too: raw API proxies, batch calls, and code execution. If a path cannot enforce a required policy, restrict that path. Gateway credentials themselves still need protection, and the downstream service remains responsible for enforcing its own permissions.

## Sierra: access decisions need session context

Sierra is an enterprise AI company that builds customer-service agents for businesses. They have published a [comprehensive account](https://sierra.ai/blog/building-sierras-mcp-gateway-an-engineering-iceberg) of their internal MCP gateway implementation.

Sierra’s employees needed agents to access information across company systems. But an agent helping with one customer’s work could read another customer’s sensitive information and reuse it. Sierra built comprehensive controls into its gateway to prevent that.

The gateway checks data in three stages:

1. A deterministic process identifies which customers the data might be about.
2. A fast model narrows that list.
3. A slower model determines which customer, if any, the data concerns and how sensitive it is.

It logs customer-data access and blocks attempts to access sensitive data about more than one customer in the same session. Users can explicitly approve exceptions, such as an investigation involving multiple customers, outside the agent’s normal flow. Those approvals also go into the audit log.

Sierra also built a REST extension mechanism because existing MCP servers did not always support the full workflow. It lets the team add missing functionality, replace tools that do not fit employees’ workflows, and remove or replace potentially unsafe tools.

Our customers ask for something similar: keep certain records away from an agent, or let it complete a specific workflow safely. Choosing which tools an agent can use is not always enough. The gateway also needs to check which data those tools can access and under what conditions. Building those checks requires understanding each application and its data, so every new system adds work.

Sierra’s design also makes session boundaries a security decision. A platform team needs to define who can start a new session, how long access history persists, and who can approve an exception. Otherwise, a restriction that works within one conversation may not constrain the larger workflow.

## DoorDash: give agents the tools a workflow needs

DoorDash built a shared Agent Gateway so its agents could use internal APIs, engineering tools, and third-party services without each team building its own authentication, credential storage, and logging. Its [engineering account](https://careersatdoordash.com/blog/how-doordash-built-a-centralized-gateway-for-ai-agent-tool-access/) explains how it manages access and gives agents the tools they need for a task.

The gateway has two main parts. A proxy verifies callers, checks permissions, applies rate limits, adds credentials, and forwards requests. A registry stores the agents, servers, owners, authentication settings, policies, and tool catalogs the proxy needs.

A server might expose hundreds of tools, but a workflow may need only a few. DoorDash groups approved tools from multiple servers into bundles for specific tasks.

A coding agent can connect to one endpoint for selected GitHub, Jira, observability, and documentation tools. Filters determine which tools it sees based on the agent, user group, environment, or audience. The gateway checks permissions both when the agent discovers tools and when it calls them.

The gateway handles credentials without exposing them to the agent. It supports internal identity, stored vendor tokens, per-user OAuth grants, and team service principals. For each call, it records the caller, tool, result, latency, and cost information when the downstream system provides it. Rate limits protect those systems, and teams can test new limits in shadow mode to see which calls they would block before enforcing them.

Teams can register servers, approve tools, configure access, and build bundles themselves. DoorDash reports more than 200 registered MCP servers, more than 30 agents and services used by thousands of employees, and millions of tool calls each week.

Sierra’s example shows why access decisions need to account for customer data and session history. DoorDash’s account highlights tool selection too: agents need a useful set of approved actions for each task. It also shows why ease of use matters. Teams need a simple way to use the gateway, or they may connect directly to servers instead.

For example, a support agent might need ticket history, account details, and a tool to record a proposed resolution. A task-specific bundle can provide those tools without exposing every operation from each vendor. Administrators can inspect the tools included in the workflow, while the gateway checks whether each call is allowed. Someone still has to maintain that bundle as the workflow and services change.

## Uber: reuse existing APIs and discover tools as needed

Uber’s teams initially built their own MCP integrations, which duplicated infrastructure and made tools harder to discover and maintain. Its [engineering account](https://www.uber.com/us/en/blog/designing-mcp-gateway/) describes a shared gateway that exposes existing APIs as MCP tools without requiring teams to rewrite the underlying services. Uber reports hosting more than 800 MCP servers and more than 5,000 tools.

The gateway separates registration from execution. An MCP Registry stores tool definitions, ownership, and enablement settings. At runtime, a proxy translates MCP calls into HTTP, gRPC, or TChannel requests and converts the responses back into MCP results. It also proxies calls to native MCP servers, applies Uber’s access controls, and redacts sensitive data from tool responses.

Uber automates much of the registration work through AutoCrawler. It scans internal API definitions for new services and schema changes, generates tool schemas and descriptions, and discovers native MCP servers. But finding a tool does not make it available to agents. Every server and tool starts disabled, and the owning team must review and enable it. Changes to tool descriptions also require owner approval, with version rollback available.

At this scale, Uber also had to limit how much tool information agents load. Its Omni MCP interface lets an agent find a relevant server, discover its tools, fetch a specific tool’s schema, and then call it. The agent can discover what it needs in stages instead of loading thousands of tool definitions into its context.

Uber reduces response size too. Response Projection lets the agent specify the fields it needs, and the gateway removes the rest from the response. For coding agents, Code Mode provides CLI commands to list servers, search for tools, and make calls through the gateway. Agents can save results to files and read only the relevant parts. Uber says Code Mode is now its default for MCP tool use in coding agents.

DoorDash groups approved tools around a task. Uber adds another way to manage a large catalog: discover tools gradually and return only the data the agent needs. Its design also separates two decisions that become important as integrations grow: which APIs can become tools, and which tools their owners have approved for use.

Progressive discovery and code execution reduce context pressure, but they must preserve the same authorization boundary. A compact CLI is useful only if its calls remain governed. Review the operations inside a script or batch, not just permission to launch the script.

## Databricks: govern models and tools together

Databricks offers Unity Gateway as part of Unity Catalog. Its [governance architecture](https://www.databricks.com/blog/ai-gateway-governance-layer-agentic-ai) extends permissions, auditing, usage visibility, and cost controls to model and tool access. It also supports on-behalf-of user access for supported MCP execution paths, so those calls run with the requesting user’s permissions.

The [September release of the Unity Gateway CLI](https://www.databricks.com/blog/deploy-and-manage-coding-agents-scale-unity-gateway-cli) adds centrally managed coding-agent configurations, including models, MCP servers, skills, and spending policies. Administrators publish the settings, and the CLI applies them when developers launch a supported agent. Databricks says it uses the CLI to roll out models and cost optimizations across its own engineering organization.

For teams already using Databricks, this brings model and tool administration into the same system. Administrators can manage which models and tools developers use, track usage and spending, and update approved configurations centrally.

Teams still need to check which identity each downstream connection uses. Signing into a gateway does not, by itself, mean that a connected SaaS application acts as that user. Supported on-behalf-of connections preserve the user’s permissions; connections using a shared service account operate with that account’s permissions.

A shared service account may be appropriate for a team automation, but the destination may see that account rather than the employee who started the work. Recording the employee’s identity in a gateway log helps explain who initiated the action. It does not, by itself, reduce what the service account can access. Teams need to verify both who the gateway records and which permissions the destination actually enforces.

## The design choices compared

The useful comparison is by engineering problem. These are the choices emphasized in the accounts, followed by the decision an internal platform team still has to make.

| Engineering problem | Design choices in the accounts | Decision for your platform |
|---|---|---|
| Credentials and identity | Roblox: brokered credentials and isolation. Databricks: supported on-behalf-of access. | Which account executes the call, and can the agent reach its credentials or bypass the gateway? |
| Business-data permissions | Sierra: classification and session history. DoorDash: discovery and execution checks. | Is tool-level permission sufficient, or does the decision depend on the record and prior access? |
| Tool discovery and usability | DoorDash: task bundles. Uber: progressive discovery, projection, and Code Mode. | How does the agent find a small useful interface while every execution path remains governed? |
| Usage and cost | DoorDash: rate limits and available cost metadata. Databricks: centrally managed spending policies. | Are you limiting bursts, total calls, concurrency, or money? Which downstream costs are covered? |
| Change and operations | Uber: owner approval and rollback. Sierra: maintained REST extensions. Databricks: centrally managed configurations. | Who reviews changes, and what happens when a policy or connector can no longer be applied? |

These choices interact. A well-scoped tool can still use an overprivileged service account. A permitted operation can still exhaust an allowance. A safe tool definition can become unsafe after an upstream schema change. The gateway has to preserve the intended policy across those changes.

Two requests from our enterprise conversations show what this work involves.

## Permission needs a volume limit

One enterprise team I talked to wanted to limit how much agents could use paid APIs during operational investigations. The tools were already useful, but the team wanted a clear usage allowance and approval before a user could exceed it.

For example, a person might make five API calls during an investigation, while an agent might make 5,000. Both can use the same credentials and permitted operations, but the higher volume increases cost and load on the service.

Permission to use a tool does not set a spending limit. A model-token budget does not cap a separate vendor’s API bill. A per-minute rate limit can prevent bursts of traffic, but a long-running job can still use up a daily allowance.

Teams need to define what the allowance covers: requests per user, a team’s shared use of a service, or spending on one run. If calls have different costs, the counter needs to account for those differences. Teams also need rules for counting retries and calls running at the same time.

Administrators need to see who used the allowance and decide what happens when it runs out. The gateway should tell the agent to stop, wait, or request approval for more. A generic error may cause the agent to keep retrying.

For a hard allowance, reserve capacity atomically before dispatch so concurrent calls cannot all spend the same remaining balance. Count the downstream operations inside a batch, and define what happens to a reservation when a call fails. A timeout may mean the provider completed the operation; retrying a write can repeat its effect as well as its cost.

If the charge is only known after execution, a preflight check cannot guarantee an exact spending ceiling without a conservative reservation or a provider-side limit. Request counts and response bytes are useful controls, but neither is automatically a dollar budget.

![Flowchart: a tool call must pass a permission check and then an allowance check before the downstream operation runs](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/why-engineering-teams-keep-building-mcp-gateways/02-permission-and-allowance-checks.webp)

Figure 2. Permission and allowance answer different questions. A request can be authorized and still exceed the allowance for the task. Both checks belong before the next chargeable operation.

## A connector needs room for company policy

Another enterprise team wanted to adapt existing tools to its ServiceNow setup. It needed to change tool descriptions and inputs, lock some inputs to fixed values, and control which records agents could access.

For example, an administrator might require every new incident to use a specific assignment group and allow only certain priority values. The agent could fill in the description and affected service, while the organization controlled the assignment and priority rules.

The gateway needs to show the agent the allowed inputs and enforce those rules when it runs the tool. Hiding an input from the model does not guarantee its value. The gateway must set the fixed value or check that the submitted value is allowed. A default is not enough if the caller can override it.

![Diagram: the incident tool the agent sees versus the rules execution enforces, such as a fixed assignment group and allowed priority](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/why-engineering-teams-keep-building-mcp-gateways/03-servicenow-incident-policy.webp)

Figure 3. Example policy for incident creation in ServiceNow: narrow the tool’s inputs, then enforce the fixed values and constraints on execution.

The same care applies to reading records. If agents cannot access critical incidents, that rule must cover searches, lists, and requests for a specific incident by ID. When a request includes only an ID, the gateway may need to check the incident’s current priority before allowing access. Checking only a priority input would miss requests that do not include it.

API gateways already inspect and modify requests. MCP also gives the model a tool description and an input schema to help it choose and use a tool. When administrators change what a tool can do, they need to update both what the model sees and what the gateway allows.

Administrators also need to test rules with representative users, check the results, and then apply them more widely. A policy editor alone is not enough. Teams need a way to verify that the rules work as intended before rolling them out.

Policy must also survive changes to the tool itself. If a provider renames the field an enforced rule depends on, silently dropping that rule is unsafe. Define when to hold the tool for review, how to test a replacement schema, and whether rollback is still compatible with the current API.

## Building a gateway means owning its next request

These companies have good reasons to build their own gateways. Their internal services, security requirements, and workflows call for designs specific to their organizations. The teams that already run those systems may also be well placed to run the gateway.

But the work continues after launch. Providers change how authentication works, tools return different response formats, and connections lose access. Administrators need new policies as agents take on more tasks. The gateway team has to investigate these changes and keep the system working.

That takes engineering time, knowledge of the connected applications, and clear ownership of product and operational decisions. Generating the initial code does not remove those responsibilities. Without a team dedicated to them, gateway maintenance takes time away from building and improving agents.

At [Composio](https://composio.dev/mcp-gateway), we provide the recurring infrastructure: managed integrations and authentication, gateway access policies, and an audit trail, including access to existing MCP servers. With [hosted authentication and managed execution](https://docs.composio.dev/docs/authentication), provider credentials are stored and refreshed server-side and injected into requests; the caller receives results rather than the provider token. Custom OAuth app ownership does not require exporting those tokens into an agent runtime.

That foundation now extends to more of the decisions described here. Enterprise tool overrides can force argument values, shape the schema agents see, and hide response fields. Resource policies narrow what allowed tools may access. Quota controls limit call volume, with monitoring and enforcement modes. These capabilities depend on the features enabled for a deployment and the execution paths they cover; call quotas bound usage rather than calculate every provider’s charges.

The operational details matter as much as the policy editor. Tool overrides have drafts, tests, revision history, and rollback. Enforced rules affected by upstream schema changes can hold a tool for review. The advantage is being able to configure and operate those controls on maintained integrations, without building the credential lifecycle and each enforcement mechanism into every agent.

A managed gateway still leaves business decisions with the enterprise: who owns each automation, what it may change, which account it uses, and when a person must approve its actions. The people responsible for the business process need to make those decisions.

Token custody makes that enforcement practical, but it is not a complete security guarantee. Teams still need to verify downstream account privileges, policy coverage, bypass routes, and the handling of sensitive tool results. [Composio’s custody documentation](https://docs.composio.dev/docs/security/token-custody) distinguishes provider-token isolation from deployment ownership and control of the execution credentials.

This shared foundation leaves teams more time for the workflows that justify deploying agents.

Consider an operations agent that starts investigating an issue before an engineer opens a dashboard. It checks the affected service, compares recent deployments, reads relevant logs, and proposes a fix. It works within a query allowance, and its write permissions stop at opening a pull request. The engineer can then review the evidence and proposed change.

A revenue agent could use calls, support tickets, product usage, and CRM records to prepare an account plan. It could find a promise that has not been fulfilled, identify who is responsible, and draft a follow-up. Its access would need to stay within that account, with reliable connections between systems and approval before sending anything externally.

A procurement agent could check an invoice against a purchase order, review delivery records, and prepare an exception for approval. It would need to work across systems with separate permissions, with enough access to investigate but no authority to release payment.

Teams still need to understand their applications, test these workflows, and decide where people must review or approve the work. A gateway cannot make those business decisions for them.

A well-run gateway lets teams reuse connections, access policies, and controls across workflows. They can spend more time improving investigations, account plans, and procurement reviews because each new agent can use the shared infrastructure without starting another integration project.
