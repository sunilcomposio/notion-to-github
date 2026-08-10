# Apigee Alternatives and Competitors: 8 Tools Compared for 2026

Apigee is a leading API management platform. Google acquired it in 2016, and Gartner has named it a Magic Quadrant Leader in full lifecycle API management since 2018. Its current product, Apigee X, runs on Google Cloud. It can deploy proxies across 24 regions and cache content at more than 100 locations. It also works with Cloud Armor, Cloud IAM, customer-managed encryption keys, and VPC Service Controls. If you sell APIs or publish them for external developers, Apigee gives you a strong set of tools.

However, its pricing has two main parts. You pay for API calls, and you also pay an environment fee for each region. Standard API proxy calls cost \$20 per million. Environment fees start at about \$365 a month and can reach about \$3,431 a month for each region. By comparison, AWS charges \$1.00 per million requests for an HTTP API.

You must also pay separately for some advanced features. API Analytics costs \$20 per million analytics calls, and Advanced API Security costs \$350 per million API calls. You can use these features only with higher environment tiers, so you may need to buy a more expensive tier.

Apigee Edge reached end of life in March 2025, so its remaining users had to move to Apigee X.

## How I compared these tools

I divided users into two groups. The first group runs APIs as products for customers, partners, or external developers. These teams often need a developer portal, monetization, analytics, and partner onboarding. The second group uses APIs inside its own applications. These teams usually need routing, authentication, rate limits, and monitoring. Apigee works well for the first group, but it can offer more than the second group needs.

I compared each tool in six areas:

- **Pricing model:** Does it charge by call, environment, region, gateway service, core, or a flat rate?
- **Price transparency:** Does the vendor publish prices, or do you need to ask for a quote?
- **License and hosting:** Is it proprietary or open source? Can you use a managed service, a hybrid setup, or your own infrastructure?
- **AI and MCP support:** Can it manage model traffic and expose APIs as tools for agents?
- **Apigee features it can replace:** Does it include a gateway, developer portal, analytics, security, or monetization?
- **Best use case:** Who should use it, and what trade-offs should they expect?

## What Apigee costs and how Google calculates it

Google publishes Apigee's pay-as-you-go rates, so you can estimate entry-level costs. However, it does not publish subscription prices, so you must ask for a quote.

The evaluation tier is free for 60 days. After that, pay-as-you-go charges standard API proxy calls at \$20 per million, dropping to \$16 per million between 50 million and 500 million calls and \$13 per million above that. Extensible proxies, which run the more capable policy set, cost \$100 per million. Environment fees are charged per region and rise with environment type, from around \$365 a month at the entry level to roughly \$3,431 a month at the top, with 20, 50, and 100 proxy deployments included as you move up the three environment types and additional deployments billed by the hour.

- Multi-region deployment multiplies the environment fee, so resilience is a fixed cost increase rather than a variable one.
- API Analytics costs \$20 per million analytics calls, and Advanced API Security costs \$350 per million API calls, both available only on the higher environment types.
- Networking is billed separately at standard Google Cloud rates, including data egress and IP addresses.
- The three subscription tiers are quote-only. Reported enterprise spend commonly lands between \$2,000 and \$25,000 a month, with annual subscriptions ranging from around \$10,000 into six figures.
- Extensible proxies cost five times what standard proxies cost per call, so policy complexity has a direct price.

A small, single-region setup can start at about \$385 a month. However, teams report that a practical setup can cost about \$1,460 a month after they add environments, analytics, and normal traffic. This cost can make sense when you use Apigee to monetize APIs and onboard external partners.

## Apigee alternatives and competitors compared at a glance

| Tool | What it replaces | How it meters you | Public pricing | AI and MCP | Best for |
| --- | --- | --- | --- | --- | --- |
| Apigee | Full lifecycle API management | Per call plus environment fees per region | Partly, pay as you go rates published | Gemini assisted development on Google Cloud | API programs with external consumers and monetization |
| Kong | Gateway, portal, and AI gateway | Free self hosted, or per gateway service plus requests | Partly, open source free, Konnect consumption | AI Gateway governing LLM, MCP, and A2A traffic | Platform teams who want a gateway they control |
| Google Cloud API Gateway | Routing, auth, quotas, monitoring | Per call, with a free allowance | Yes, fully published | None built in, backends handle it | Serverless backends that never needed a full platform |
| AWS API Gateway | Routing, auth, throttling, caching | Per request, by API type | Yes, fully published | None built in, Lambda handles logic | APIs that live inside AWS and are not a product |
| Azure API Management | Gateway, portal, policy, analytics | Tiered capacity, or per call on Consumption | Yes, published across every tier | Azure AI services alongside | Microsoft estates with committed Azure spend |
| Tyk | Full lifecycle API management | Free self hosted, or usage based, or flat rate | No dollar figures published | MCP Gateway with per tool rate limiting in open source | Flat rate billing and a complete open source core |
| WSO2 | Full lifecycle API management | Free under Apache 2.0, paid support subscription | No published subscription rates | Multi gateway federation and MCP ready portals | Governing what you already run without migrating |
| MuleSoft | API management plus integration | Mule Flows and Messages, plus credits | No, negotiated | Agent Fabric: AI Gateway, Registry, MCP Bridge | Teams that need an integration platform too |
| Composio | Enterprise MCP gateway and agent tool access | Custom enterprise pricing | No public MCP Gateway price | Managed and custom MCP servers, tool routing, and governance | Giving enterprise agents secure, controlled access to tools |

#### 1. Apigee vs Kong: The Gateway You Run Yourself

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/apigee-alternatives-and-competitors/image_1.png)

Teams often compare Kong with Apigee because the two products have a clear operational difference. You can run Kong yourself, or you can use its managed service.

Kong Gateway is free and open source. It uses an NGINX engine that can handle more than 50,000 transactions per second on each node. You can configure it through an API, a web interface, or declarative files in a CI/CD pipeline. Mercedes-Benz and Australia Post use it. It suits platform teams that use Kubernetes and want to deploy a gateway across clouds, data centers, or hybrid environments. Kong AI Gateway can also manage LLM traffic, MCP calls, and agent-to-agent communication through the same policy layer as regular APIs. Apigee does not provide equivalent native support.

Apigee includes a developer portal, monetization, and advanced analytics. With Kong, you must assemble more of these capabilities yourself. The open-source edition also excludes the dashboard, role-based access control, and advanced plugins that many production teams need.

Konnect Plus bills per gateway service, and third-party analysis puts that near \$105 per service per month plus roughly \$34 per million requests, where every API endpoint and every LLM model you route counts as its own service. Developer portals carry published API caps with per-API charges above them, and SSO and audit logging sit behind enterprise. Self-hosted Kong Enterprise starts around \$30,000 to \$50,000 a year for small deployments; aggregated contract data puts typical mid-market and enterprise spend between \$40,000 and \$250,000 a year, and even the free edition carries a real cost of \$20,000 to \$100,000 a year once infrastructure and staff time are counted. Kong uses a different pricing model, but its costs still increase as you scale.

**Pros**

- A fully open-source, self-hosted gateway with no license cost and very high throughput on an NGINX engine
- Kong AI Gateway governs LLM, MCP, and agent-to-agent traffic through the same policy layer as your APIs, which Apigee does not do natively
- Runs anywhere across clouds, Kubernetes, or hybrid and is configured declaratively through your existing CI/CD

**Cons**

- Konnect Plus meters per gateway service, so a breadth-based bill replaces Apigee's per-call one rather than removing it
- The open-source edition omits the dashboard, RBAC, and advanced plugins most production teams end up needing
- SSO, audit logs, and advanced governance are Enterprise only, and self-hosted Enterprise starts near \$30,000 to \$50,000 a year

**Watch**

[Kong Gateway for beginners: API gateway tutorial](https://www.youtube.com/watch?v=20rOdqag4Dw)

#### 2. Apigee vs Google Cloud API Gateway: The Cheaper Product Google Already Sells

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/apigee-alternatives-and-competitors/image_2.png)

Google offers two separate API products. Apigee is its enterprise API management platform, while Google Cloud API Gateway is a managed proxy based on Envoy. You configure API Gateway with an OpenAPI specification and place it in front of serverless services such as Cloud Run, Cloud Functions, and App Engine. It handles authentication, API key validation, quotas, usage limits, routing, and monitoring. The products have separate consoles and pricing. Depending on the workload, Apigee can cost about 7 to 100 times more.

API Gateway costs nothing up to two million calls a month, around \$24 at ten million calls, and roughly \$294 at a hundred million. Apigee's entry point alone is about \$385 a month before a single call, because environment fees are charged whether traffic arrives or not. Inbound data is free, and outbound uses standard Google Cloud network pricing, and calls are metered through the Service Control API, appearing on your bill as Service Control line items rather than a separate gateway charge, which confuses people the first time they look.

If your APIs are a product with external consumers, partners, or paying developers, the Apigee premium buys you the portal, the monetization engine, and the advanced analytics. Migrating between the two is not free, since Apigee policies and extensible proxy logic have no direct equivalent here, and there is no developer portal, no monetization, and no advanced security analytics. But for a team that overbought during an Edge migration, this is the cheapest correction available, and it does not even require leaving Google Cloud.

**Pros**

- Free up to two million calls a month, around \$24 at ten million and roughly \$294 at a hundred million, against an Apigee entry point near \$385 a month before traffic
- Fully managed and Envoy-based, with authentication, API key validation, quotas, usage limits, and monitoring built in
- Stays inside Google Cloud, so identity, logging, and networking work the way your team already expects

**Cons**

- No developer portal, no monetization, and no advanced security analytics, so it replaces only the gateway half of Apigee
- Aimed at serverless backends configured through an OpenAPI specification, with far less policy programmability than extensible proxies
- Apigee policies do not port across, so the migration is a rebuild rather than a redeployment

**Watch**

[Building and managing APIs for serverless with Google Cloud](https://www.youtube.com/watch?v=X1zTb3k60ow)

#### 3. Apigee vs AWS API Gateway: Commodity Pricing Inside AWS

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/apigee-alternatives-and-competitors/image_3.png)

If your backends run on AWS, AWS API Gateway can be a simple and low-cost choice.

You pay only for usage, and there is no seat fee or minimum commitment. AWS also does not charge you to create or deploy an API. HTTP APIs cost \$1.00 per million requests. REST APIs cost \$3.50 per million and include caching, usage plans, API keys, request validation, and WAF integration. WebSocket APIs cost \$1.00 per million messages plus \$0.25 per million connection minutes. Apigee charges \$20 per million standard proxy calls, so AWS HTTP API routing costs 20 times less per request. New accounts get a free tier covering a million REST calls, a million HTTP calls, a million WebSocket messages, and 750,000 connection minutes a month for the first twelve months, and since July 2025, new customers also receive up to \$200 in free tier credits. Aggregated data puts the median AWS API Gateway contract at about \$2,000 a year across 89 verified purchases.

Teams routinely pick REST APIs when HTTP APIs would do the same job and pay three and a half times more for unused features. Then, the fixed charges dominate small bills in a way per-request pricing hides: REST caching runs from about \$14.60 to \$2,774 a month regardless of traffic; VPC links bill from roughly \$7.20 to \$216 a month even at zero requests, and CloudWatch execution logs accumulate at \$0.50 per GB ingested, which at high volume can exceed the request charges entirely.

AWS API Gateway has no plugin system or developer portal, and it supports fewer protocols. You must implement custom logic in Lambda, which adds cold-start latency and invocation costs. It can replace Apigee's gateway functions, but it cannot replace the full platform.

**Pros**

- HTTP APIs at \$1.00 per million requests against Apigee's \$20 per million, with no minimum commitment and no charge for deploying APIs
- A twelve-month free tier covering a million calls a month across REST, HTTP, and WebSocket, plus up to \$200 in credits for new accounts
- Fully managed inside AWS, with a median contract near \$2,000 a year and native IAM, WAF, and CloudWatch integration

**Cons**

- No developer portal, no plugin system, and no monetization, so it covers only the routing and access layer
- Custom logic has to run in Lambda, adding cold start latency and per-invocation charges
- Fixed costs distort small bills: caching, VPC Links, and CloudWatch logs all bill regardless of request volume

**Watch**

[Building well-architected API Gateway APIs](https://www.youtube.com/watch?v=JDbkoICzQro)

#### 4. Apigee vs Azure API Management: Published Tiers and Committed Spend

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/apigee-alternatives-and-competitors/image_4.png)

Azure API Management provides the closest direct alternative to Apigee from another large cloud provider. Microsoft publishes prices for all its tiers, so you can estimate costs before you contact its sales team.

Tiers run from a serverless Consumption option at roughly \$3.50 per million calls with the first million free, through Developer at about \$48 a month with no SLA, Basic v2 near \$210, and Standard v2 near \$734, up to Premium at roughly \$3,000 per unit per month. Premium is the only tier with active configurations across multiple regions and self-hosted gateway entitlement, which includes four units per Premium unit before additional units are billed by the hour. The v2 tiers introduced in 2024 modernized the gateway architecture and pulled the entry price down considerably. You get a developer portal, policy engine, analytics, and API products in the box, which is a fuller feature set than either cloud gateway above.

API Management consumption draws down against an Azure consumption commitment, so an organization that has already committed spend to Microsoft can run its API program out of money that is otherwise sitting on the books.

Azure API Management is the right answer if you are already a Microsoft estate and the wrong one if you are not, because the value comes from proximity to what you already run. Right sizing is a genuine skill, and licensing specialists routinely find customers paying for Premium when Standard v2 would carry the workload at a quarter of the run rate. Only Standard v2 and above support virtual network integration, with full injection reserved for Premium, so network isolation requirements push you up the stack quickly.

**Pros**

- Published prices at every tier, from pay per call Consumption at roughly \$3.50 per million up to Premium, with no sales conversation needed
- Consumption draws down an existing Azure commitment, which can make API management effectively prepaid for Microsoft estates
- A full feature set including developer portal, policy engine, analytics, and API products, plus self-hosted gateways at Premium

**Cons**

- The economics only work if you are already a Microsoft shop, since the advantage comes from proximity and committed spend
- It's easy to overspend by sitting on Premium when Standard v2 would carry the load at a fraction of the cost
- Virtual network integration needs Standard v2 or higher and full injection needs Premium, so isolation requirements escalate the tier fast

**Watch**

[Azure API Management live demo and walkthrough](https://www.youtube.com/watch?v=PXtFq5wmGt0)

#### 5. Apigee vs Tyk: Flat Rate Billing and a Complete Open Source Core

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/apigee-alternatives-and-competitors/image_5.png)

The open-source edition of Tyk Gateway includes authentication, rate limits, quotas, and transformations under the MPL 2.0 license. Paid plans add the dashboard, hosted portal, and support. Kong also charges for its dashboard and role-based access control, so Tyk can give self-hosting teams more features before they buy a plan.

Tyk uses Go and depends only on Redis for rate limits and token storage. It supports REST, SOAP, GraphQL, gRPC, and TCP. It can import Swagger, OpenAPI 2, and OpenAPI 3 documents. It also supports OIDC, JWTs, bearer tokens, basic authentication, and client certificates. In addition, it can convert content between formats such as SOAP and GraphQL.

Tyk sells three tiers: Core is usage-based, Enterprise is custom for high-traffic, multi-region work, and Professional is a flat rate with unlimited APIs and unlimited API requests per month. There is also a fully self-hosted deployment where both the control plane and the data plane run inside your environment, which is why regulated customers in financial services, healthcare, and government use it.

Its MCP Gateway, launched in May 2026, is the most developed agent story of any independent gateway here, and most of it sits in the open-source edition: five-level rate limiting, per-consumer RBAC, filtered tool discovery, and OAuth 2.1-protected resource metadata. Rate limits apply per tool, so an agent that exhausts its allowance on one tool receives a 429 for that tool alone rather than losing access to everything. Upstream OAuth client credentials auth and the control plane management layer are enterprise edition.

As of mid-2026 Tyk's pricing page publishes no dollar figures at all, so every commercial tier is a sales conversation, and SSO and SAML sit on higher tiers. The cloud trial is also short at 48 hours, though it needs no credit card and no call.

**Pros**

- The most complete open-source core here under MPL 2.0, with auth, rate limiting, quotas, and transformations all included free
- A flat-rate Professional tier with unlimited APIs and unlimited monthly requests, which is the direct answer to per-call billing
- An MCP Gateway with per-tool rate limiting, per-consumer RBAC, and filtered discovery, mostly available in the open-source edition

**Cons**

- No dollar figures published on any commercial tier, so budgeting requires a sales conversation
- SSO and SAML are restricted to higher tiers, and the dashboard and portal hosting are commercial only
- The cloud trial lasts just 48 hours, which is tight for evaluating an API platform properly

**Watch**

[Create your first API in the Tyk Dashboard](https://www.youtube.com/watch?v=aLh9dXcb76k)

#### 6. Apigee vs WSO2: Apache 2.0, and a Way To Avoid Migrating

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/apigee-alternatives-and-competitors/image_6.png)

WSO2 API Manager is fully open source under the Apache 2.0 license, and it does not restrict product features to a paid edition. It includes full lifecycle API management, design tools, a Java-based gateway, a developer portal, and an OAuth 2.0 key manager. It supports REST, SOAP, GraphQL, WebSockets, webhooks, and streaming APIs. You can host it in the cloud, on your premises, or in a hybrid environment. A paid subscription gives you support and security updates.

In March 2026 WSO2 launched an API platform built around multi-gateway federation. The platform discovers and governs APIs already running on Kong, Amazon API Gateway, and Azure API Management without requiring migration, presenting a single developer portal where humans and agents can find the organization's entire portfolio of APIs and MCP servers regardless of where each one is hosted. If you are staring at hundreds of Apigee proxies and pricing a rebuild, the option to put governance over what you already run instead of moving it deserves a look before you commit to a migration budget. Monetization and analytics come through Moesif, which WSO2 acquired in 2025, and support prepaid, pay-as-you-go, and outcome-based models.

The Java and JVM stack is heavyweight to run compared with lightweight Go or Envoy-based gateways, and reviewers consistently describe a steep learning curve where documentation exists but examples are thin. Running in production without a subscription means no vendor security updates and no round-the-clock support, which most enterprises will not accept, and subscription rates are not published. The managed option is a separate product called Choreo rather than a hosted version of the same thing.

**Pros**

- Fully open source under Apache 2.0 with no feature paywall, covering the full API lifecycle across REST, SOAP, GraphQL, and streaming
- Multi-gateway federation governs APIs already running on Kong, Amazon API Gateway, and Azure API Management without migrating them
- One developer portal exposing both APIs and MCP servers to humans and agents, with monetization through Moesif

**Cons**

- A heavyweight Java and JVM stack with a steep learning curve and sparse examples
- Production use without a paid subscription means no vendor security updates and no round-the-clock support
- Subscription pricing is not published, and the managed offering is a separate product in Choreo

**Watch**

[WSO2 API management: an overview](https://www.youtube.com/watch?v=DQpU2xdcJNM)

#### 7. Apigee vs MuleSoft: When You Need Integration As Well

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/apigee-alternatives-and-competitors/image_7.png)

Your APIs may need to coordinate calls across SAP, Salesforce, and several databases. In that case, you need integration tools as well as API management. Apigee can proxy, secure, and analyze APIs, but it does not help you build these integrations. MuleSoft supports both tasks.

Its Anypoint Platform combines full lifecycle API management with an integration engine, a large connector library, DataWeave transformations, Anypoint Exchange, and Runtime Fabric for on-premises workloads. Its 2026 Agent Fabric adds an AI Gateway and a registry for agents in your organization. It also includes an MCP Bridge, which can turn existing MuleSoft APIs into tools that agents can call.

MuleSoft publishes no list prices. New customers buy Integration Starter, Integration Advanced, or the API Management Solution, metered on Mule Flows, which count how many integrations you have deployed, and Mule Messages, which count volume, with Automation Credits and Agentforce Flex Credits stacking on top as separate meters. Benchmark data puts the median contract at \$55,150 a year across 74 verified deals; first-year cost commonly runs two to three times the subscription; implementations typically take six to eight months; and premium connectors for SAP and Oracle are estimated at \$10,000 to \$15,000 each annually. Salesforce has owned MuleSoft since 2018 and completed its purchase of Informatica in November 2025.

**Pros**

- Combines full lifecycle API management with a real integration engine, connector library, and DataWeave transformation in one platform
- Anypoint Exchange gives you a reusable asset catalog, and Runtime Fabric runs workloads on-premise when auditors require it
- Agent Fabric adds an AI Gateway, an agent Registry, and an MCP Bridge that exposes existing APIs as agent-callable tools

**Cons**

- Substantially more expensive than Apigee, with a median contract near \$55,150 a year and no published prices at all
- Metered on deployed flows and message volume, plus automation and Agentforce credits, so most contracts span four consumption meters
- Implementations typically take six to eight months and need specialist developers, which is a long way from deploying a proxy

**Watch**

[MuleSoft 101 demo: building APIs and integrations on Anypoint Platform](https://www.youtube.com/watch?v=b7Ecn-z8ArY)

#### 8. Apigee vs Composio: A Governed MCP Gateway for Enterprise Agents

![](https://raw.githubusercontent.com/sunilcomposio/notion-assets/main/assets/apigee-alternatives-and-competitors/image_8.png)

Composio [MCP Gateway](https://composio.dev/mcp-gateway) gives every AI agent one managed endpoint for tools and MCP servers. You can put Composio's managed toolkits and your own MCP servers behind the same gateway. It then applies access policies, manages credentials, and records activity in one place.

The gateway gives each agent only the tools and actions it needs. You can define policies for an organization, team, or user, and you can block specific actions. Each team can use its own MCP URL and request access to restricted tools. Composio also keeps credentials out of agent configuration. It supports OAuth connections and service accounts, refreshes tokens automatically, and revokes access when a user leaves.

Composio records the user, team, agent, server, tool, action, time, and outcome for every call. It stores audit metadata without storing tool-call payloads, and you can set retention from seven days to one year. It also supports SAML or OIDC single sign-on, SCIM 2.0 provisioning, CSV exports, and SIEM workflows. Composio is SOC 2 Type II and ISO 27001 certified. You can use Composio Cloud, deploy it in your VPC, or host it yourself.

The gateway also helps agents use large tool catalogs. Its tool router returns only the tools that match a task, so the agent uses less context. It can run large responses in an isolated code sandbox and return a short summary. It can also handle rate limits, repair malformed parameters, adapt to schema changes, and retry recoverable calls. These capabilities apply across Claude, GPT, Gemini, and in-house models.

Composio serves a different use case from Apigee. Apigee manages APIs for developers, partners, and customers. Composio manages how AI agents discover and use tools. Choose it when you need one security, authentication, policy, and audit layer across enterprise agents and MCP servers. [Learn more about Composio MCP Gateway](https://landing-new-git-claude-codex-work-continuation-2d37d2.preview.composio.dev/mcp-gateway).

**Pros**

- Combines more than 1,000 managed toolkits and custom MCP servers behind one endpoint
- Provides action-level access controls, managed authentication, team policies, and central revocation
- Records every tool call and outcome in one audit view, with configurable retention and SIEM-ready exports
- Supports SSO, SCIM, Composio Cloud, VPC deployment, and self-hosting
- Reduces context use through runtime tool routing, sandboxed execution, and compact results

**Cons**

- Focuses on agent-to-tool traffic, so it does not replace Apigee's developer portal or API monetization features
- Enterprise MCP Gateway pricing is not public, so you must contact Composio
- Your team still designs and operates the agents that use the gateway

**Watch**

## Other Apigee competitors worth knowing

- Gravitee is the strongest name not in the main list, with open-source roots and unusually good support for event-driven and asynchronous APIs alongside REST, which matters if Kafka, MQTT, or webhooks are part of your estate.
- KrakenD is a high-performance, stateless open-source gateway written in Go and is worth a look if raw throughput and simple declarative configuration matter more than a full lifecycle platform.
- Apache APISIX is the other major open-source gateway, built on a lightweight architecture with a large plugin ecosystem.
- Zuplo takes a programmable approach where policies are TypeScript code rather than configuration, which suits teams who would rather write logic than fill in forms.
- IBM API Connect and Red Hat 3scale are the established enterprise options, and both come up in shortlists where an existing IBM or Red Hat relationship is already in place.
- TIBCO Mashery is now part of Boomi, which acquired it alongside APIIDA to build out its API management line, so evaluating it means evaluating Boomi's roadmap.
- Boomi itself is worth a mention for teams who need integration as well as API management but find MuleSoft's pricing out of reach, since it publishes a \$99 a month pay-as-you-go entry point. Related: Boomi Alternatives
- Hasura solves a different problem by generating a GraphQL layer over your data sources, and Postman covers design, testing, and documentation rather than runtime traffic. Neither is a gateway, though both appear in Apigee comparisons.

## How to pick an alternative to Apigee

Start with one question: do customers and partners use your APIs, or do your own applications use them?

If customers, partners, or external developers use your APIs, you may need a developer portal, monetization, and analytics. In that case, compare Apigee, Azure API Management, MuleSoft, Tyk, and WSO2.

If your APIs support your own applications, you may only need a gateway near your backends. Use Google Cloud API Gateway for workloads on Google Cloud, or use AWS API Gateway for workloads on AWS.

You can also narrow the list by your main requirement. Choose Kong or Tyk if you want to run the gateway yourself. Consider WSO2 if you want to govern existing gateways without moving every API. Consider MuleSoft if you also need integration and orchestration. Consider Composio if you need one governed MCP endpoint for agents, managed and custom servers, access policies, authentication, and audit logs.

| Your main reason for leaving Apigee | Narrow it down | Start with |
| --- | --- | --- |
| Per call and per region fees on internal traffic | Your backends run on Google Cloud | Google Cloud API Gateway (free to two million calls) |
| Per call and per region fees on internal traffic | Your backends run on AWS | AWS API Gateway (HTTP APIs at \$1.00 per million) |
| You want published prices and a full feature set | Your company already commits spend to Microsoft | Azure API Management (commitment drawdown) |
| You want to run the gateway yourself | You are a Kubernetes first platform team | Kong (open source, plus an AI Gateway) |
| You want to run the gateway yourself | You want the fullest free feature set | Tyk (MPL 2.0, complete open source core) |
| Consumption billing makes budgeting impossible | You want one fixed number at scale | Tyk Professional (flat rate, unlimited requests) |
| You cannot face migrating every proxy | Govern what you already run, where it runs | WSO2 (multi gateway federation) |
| Your APIs need integration behind them | Connectors, transformation, orchestration | MuleSoft, or Boomi for a lower entry price |
| You need to govern tool access for enterprise agents | You need one endpoint, scoped access, managed auth, and audit logs | Composio MCP Gateway |

## Frequently asked questions

### What is Apigee used for?

Apigee is Google Cloud's full lifecycle API management platform. Organizations use it to design, secure, publish, analyze, monetize, and scale APIs. It is most useful when customers, partners, or external developers use those APIs. It includes an API gateway, a developer portal, programmable policies, analytics, threat protection, and monetization. You can deploy it in the cloud, on your premises, or in a hybrid environment.

### How much does Apigee cost?

You can evaluate Apigee free for 60 days. With pay-as-you-go pricing, standard API proxy calls cost \$20 per million. The price falls to \$16 per million for 50 million to 500 million calls, and then to \$13 per million above 500 million. Extensible proxies cost \$100 per million calls. You also pay an environment fee for each region. It starts at about \$365 a month and can reach about \$3,431 a month. API Analytics costs \$20 per million calls, and Advanced API Security costs \$350 per million calls. Google does not publish subscription prices, but reported enterprise spending ranges from about \$2,000 to \$25,000 a month.

### What happened to Apigee Edge?

Apigee Edge reached end of life in March 2025. Current deployments use Apigee X, which runs on Google Cloud and works with Cloud Armor, Cloud IAM, customer-managed encryption keys, and VPC Service Controls. Older guides may describe Edge Microgateway or features that are no longer available. Check the publication date before you follow them.

### Apigee vs Kong: Which should I choose?

Choose Apigee if you want a managed platform with a developer portal, monetization, and advanced analytics. It suits teams that prefer usage fees over running gateway infrastructure. Choose Kong if your platform team wants to control the gateway and deploy it in different environments. Kong Konnect Plus charges about \$105 for each gateway service per month, according to third-party analysis, so you still pay more as you add services.

### Apigee vs API Gateway: What is the difference?

Google offers them as separate products, and each one has its own console and pricing. Google Cloud API Gateway is a managed proxy based on Envoy. You configure it with an OpenAPI specification, and it provides authentication, quotas, and routing for serverless backends. It is free for up to two million calls a month. Apigee provides a full API management suite for organizations that offer APIs as products. Depending on your workload, it can cost about 7 to 100 times more.

### Apigee vs AWS API Gateway: Which is cheaper?

AWS API Gateway costs much less for routing. HTTP APIs cost \$1.00 per million requests, and REST APIs cost \$3.50 per million. Apigee charges \$20 per million standard proxy calls, and it also charges an environment fee. Apigee includes a developer portal, monetization, advanced security analytics, and programmable policies. AWS API Gateway does not include these features. Choose AWS when your APIs support internal systems or your own applications and cost is your main concern.

### Apigee vs MuleSoft: What is the difference?

Apigee focuses on API management. MuleSoft's Anypoint Platform also includes an integration engine, connectors, DataWeave transformations, and Anypoint Exchange. Choose MuleSoft if your APIs must coordinate work across SAP, databases, and SaaS applications. However, it usually costs more. The median contract is about \$55,150 a year, and MuleSoft does not publish list prices. It charges for deployed flows and messages, and an implementation often takes six to eight months.

### Is there an open-source Apigee alternative?

Yes. Tyk provides authentication, rate limits, quotas, and transformations in its open-source edition under the MPL 2.0 license. WSO2 API Manager uses the Apache 2.0 license and includes full lifecycle API management. Kong Gateway is also free and open source, but its dashboard and role-based access control require a paid plan. You can also consider Gravitee, KrakenD, and Apache APISIX.

### What is the cheapest Apigee alternative?

For internal API routing, cloud gateways usually cost the least. Google Cloud API Gateway is free for up to two million calls a month, and AWS HTTP APIs cost \$1.00 per million requests. If you want to manage the gateway yourself, Tyk, Kong, and WSO2 offer open-source editions with no license fee. However, you still pay for infrastructure and staff time. Tyk Professional offers a flat rate if you need predictable enterprise costs.

### Which Apigee alternative is best for AI agents and MCP?

Tyk's MCP Gateway provides five-level rate limits, role-based access control for each consumer, filtered tool discovery, and OAuth 2.1 resource metadata. Most of these features are available in its open-source edition. Kong AI Gateway manages LLM, MCP, and agent-to-agent traffic through the same policies as regular APIs. WSO2 lists APIs and MCP servers in one portal, while MuleSoft Agent Fabric adds an agent registry and MCP bridge. Composio is useful when enterprise agents need controlled access to applications and custom MCP servers. It puts more than 1,000 managed toolkits and your own servers behind one endpoint. It also provides action-level policies, managed authentication, runtime tool routing, and a central audit trail.
