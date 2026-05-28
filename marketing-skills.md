---
title: "Top Marketing Skills for AI Agents (Claude, Codex & More)"
source: https://www.notion.so/composio/Marketing-Skills-36ef261a6dfe80a1bee8febe9d003e1d
notion_page_id: 36ef261a-6dfe-80a1-bee8-febe9d003e1d
last_synced: 2026-05-28T14:22:35Z
---

## What Are Skills?

[Skills](https://support.claude.com/en/articles/12512176-what-are-skills) are reusable instruction packs in markdown files that make AI agents better at specific tasks.

Instead of prompting your agent from scratch every time, a skill gives it domain-specific knowledge, workflows, examples, rules, and best practices. 

For example:

- An SEO skill can teach the agent how to run a technical audit, generate schema, and optimize for AI citations.
- Meta ads and Google Ads skill can guide it through account structure checks, wasted spend analysis, and creative fatigue diagnosis. You can spot what’s going right and what’s not in just minutes.
- A design skill can help it extract visual systems from screenshots and generate better frontend implementation specs.
- A video skill can teach it how to generate Remotion or Manim code without hallucinating the API.

In simple terms: Skills turn general-purpose agents into specialized operators.

They are especially useful for marketing because most GTM work is not one isolated task. It is a chain:

Research → strategy → content → design → publishing → measurement → iteration.

Skills help agents follow those workflows with more consistency. And if you’re running a small company or bootstraping your business where you absolutely don’t have time or patience which most of us don’t, this is gold.

## How To Install Skills

Most skills are installed by cloning the skill repo or adding it to your agent’s skills/plugins directory.

The exact steps depend on the client you are using. 

### Claude Code

For Claude Code, most skills can be installed by adding them to your [Claude skills](https://composio.dev/content/top-claude-skills) directory.

A typical flow looks like this:

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone [skill-repo-url]
```
Then restart Claude Code or Claude Cowork so the new skill is discovered.

If the skill is packaged as a Claude plugin, follow the plugin install instructions in that repo instead. Some repos include a SKILL.md, while others include a full plugin folder with its own manifest and setup steps.

### **Codex**

For Codex, skills can be installed into your [Codex skills](https://composio.dev/content/top-codex-skills) directory:

```bash
mkdir -p ~/.codex/skills
cd ~/.codex/skills
git clone [skill-repo-url]
```
Then restart Codex or start a new session so the skill is picked up. 

### **OpenClaw / OpenCode / Other Agents**

Most [OpenClaw skills](https://composio.dev/content/top-openclaw-skills) can be installed by cloning the skill repository into your OpenClaw skills directory or adding it through OpenClaw’s configured plugin/skills path.

A typical flow looks similar:

```bash
mkdir -p ~/.openclaw/skills
cd ~/.openclaw/skills
git clone [skill-repo-url]
```
or just the simple command

```javascript
openclaw skills install git:owner/repo@ref
```
## Connecting Skills with MCPs

You can also connect Composio through MCP so the agent can access tools like Notion, LinkedIn, Typefully, Ahrefs, Google Search Console, Google Ads, and Meta Ads during the workflow.

For Claude Code

```javascript
claude mcp add --scope user --transport http composio https://connect.composio.dev/mcp
```
Then in Claude Code,

```javascript
/mcp
```
Select Composio and complete the OAuth flow.

Once connected, Claude can use both local skills and live app data in the same workflow.

Check our guides for [Cowork](https://composio.dev/toolkits/outlook/framework/claude-cowork), [Codex](https://composio.dev/toolkits/outlook/framework/codex), [OpenClaw](https://composio.dev/toolkits/outlook/framework/openclaw), [Hermes](https://composio.dev/toolkits/outlook/framework/hermes-agent), and [OpenCode](https://composio.dev/toolkits/outlook/framework/opencode).

## SEO / GEO Skills for Agents

### 1. GEO / AEO SKILL - AI SEARCH OPTIMIZATION

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/b276418b4b.png)

AEO has become quite popular nowadays, but not many are good at handling it. This skill optimizes the content to get cited by ChatGPT, Perplexity, Google AI Overviews, and Gemini. Furthermore, it scores citability, generates llms.txt, adds schema markup, and outputs a client-ready PDF audit report.

**Usage**

- Run a full GEO audit on any URL → get citability score + prioritized fix list across 13 dimensions.
- Generate an llms.txt file so AI crawlers understand the product site structure and index it correctly.
- Optimizes existing blog posts for AI citation with E-E-A-T signals, entity clarity, and FAQ schema injection
- Produce a branded PDF GEO report ready to send to a client or CMO, for contrast agencies charge \$2K-\$12K for this

**Prompt**

```plain text
Run a GEO audit on [URL]. Score: citability, AI crawler access, brand mention presence, schema markup, content factual density, and E-E-A-T signals. Output a full report with prioritized fixes. Then generate an llms.txt file and FAQ schema JSON-LD for the top 3 pages.
```
To learn more, check out[ https://github.com/zubair-trabzada/geo-seo-claude](/369f261a6dfe80b6af90f156145afe5f?pvs=25) by Zubair.

---

### 2. SEO + GEO FULL STACK SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/1bc6c5c6bb.png)

Your entire SEO + GEO workflow packaged into 20 skills covering: keyword research, content writing, technical audits, and rank tracking for both traditional search and AI platforms using Claude code, OpenClaw, hermes.

All scored against the CORE-EEAT benchmark (80 items) and CITE domain rating (40 items).

**Usage**

- Runs keyword research on Ahrefs → content gap analysis → full draft → AEO/GEO optimization in a single chained workflow
- Audit any page against 80 CORE-EEAT quality criteria and get a scored report with line-level fixes
- Generate programmatic SEO pages at scale from a keyword list and a content template
- Optimize for AI citations across ChatGPT Search, Perplexity, and Google AI Overviews simultaneously

**Prompt**

```plain text
Run the full SEO + GEO workflow for [topic/URL].

Step 1: keyword research and gap analysis vs [competitor].

Step 2: write a 1,500-word article scored against CORE-EEAT.

Step 3: apply AEO optimization - add FAQ schema, HowTo markup, and question-phrased headings.

Step 4: output a GEO readiness score.
```
You can explore more at: https://github.com/aaron-he-zhu/seo-geo-claude-skills by arron

---

### 3. CLAUDE-SEO -UNIVERSAL SEO SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/21ee610bfe.png)

Its the most complete SEO skill available for Claude Code right now. 

25 sub-skills and 18 sub-agents covering everything from technical crawl health to e-commerce SEO to international hreflang validation. 

While the GEO/AEO skill above is focused purely on AI citability, this one covers the full traditional + AI search spectrum in one package with updated Quality Rater Guidelines.

**Usage**

- Run `/seo audit` on any domain → get a full technical health report scored across crawlability, E-E-A-T, Core Web Vitals, schema, and internal linking
- Generate programmatic SEO pages at scale from data sources with built-in quality safeguards
- Build "X vs Y" and "alternatives to X" comparison pages that convert → one of the highest-ROI page types for SaaS
- Validate and generate hreflang for multi-language sites without a separate tool subscription

**Prompt**

```plain text
Run /seo audit [URL]. Score: technical health, E-E-A-T signals, schema markup, Core Web Vitals, and internal linking. Then run /seo geo [URL] for AI citation readiness. Output a combined report with a prioritized fix list ordered by traffic impact.
```
Learn more at: [https://github.com/AgriciDaniel/claude-seo](https://github.com/AgriciDaniel/claude-seo) by Daniel.

---

### 4. SEO-GEO OPTIMIZER -MULTI-PLATFORM AI CITATION SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/f319d0004b.png)

This skill is purpose-built for one thing: getting your content cited across every AI platform simultaneously. Covers Google, Bing, ChatGPT, Perplexity, Claude, Gemini, Grok, voice assistants, and social. 

If you want to yap, my AI-referred traffic grew 527% between January and May 2025 -this skill is built for that reality.

**Usage**

- Analyze any HTML, Markdown, or React/JSX file for traditional SEO + AI platform citability in a single pass
- Extract entities, generate schema, and add author credential signals that increase citation probability by up to 40%
- Score content against freshness signals -content under 30 days old gets 3.2x more AI citations
- Output multi-format audit reports ready for clients or internal teams

**Prompt**

```plain text
Run a full SEO + GEO analysis on [URL or file]. Score for: traditional search (Google, Bing), AI platforms (ChatGPT, Perplexity, Claude, Gemini), and voice assistants. Extract entities, flag missing author credentials, and generate FAQ schema. Output a prioritized fix list with estimated citation impact per fix.
```
Learn more at: [https://github.com/199-biotechnologies/claude-skill-seo-geo-optimizer](https://github.com/199-biotechnologies/claude-skill-seo-geo-optimizer) by Boris Djordjevic.

---

## Paid Ads Skills 

### 5. CLAUDE ADS AUDIT SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/8ff2653199.png)

Running successful paid campaigns and auditing is a struggle point for many growth teams, this skill fixes that.

It first runs 250+ checks across Google, Meta, YouTube, LinkedIn, TikTok, and Microsoft Ads. Based on the result, invoke's parallel agents per platform that score accounts with weighted rubrics, flag wasted spend, and generate AI-powered creative variants.

> This is where Composio comes in. Instead of managing separate auth flows, tools, and platform-specific actions, teams can use Composio to securely connect accounts, invoke the right tools, and generate optimized audit and paid campaign reports faster.

**Usage**

- Audit a Google Ads account → get wasted spend breakdown by campaign, ad group, and keyword in one pass
- Detect creative fatigue across Meta campaigns → auto-generate 5 replacement hooks per underperforming ad set
- Run a CPA spike diagnosis → trace root cause to bid strategy, audience overlap, or landing page mismatch
- Get a CMO-ready performance report with scores, benchmarks, and an action plan - all from the terminal

**Prompt**

```plain text
Audit this Google Ads account data [attach export]. Run: wasted spend analysis, quality score breakdown, bid strategy assessment, and creative fatigue check. Score each area 0–100 using 2026 benchmarks. Output a prioritized fix list and generate 3 replacement ad copy variants for the lowest-scoring ad groups.
```
You can learn more at [https://github.com/AgriciDaniel/claude-ads](https://github.com/AgriciDaniel/claude-ads) by Daniel.

---

### 6. CLAUDE MARKETING

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/7bde967ec5.png)

These skills are your full marketing department baked into a skill pack. 

Comes with six/6 paid media skills covering: 

- Google Ads, Meta Ads, 
- Microsoft Ads, 
- competitive ad analysis, 
- waste detection, 
- and account structure review 

All this with parallel execution and budget-weighted aggregate health scoring from A to F. Also ships with Klaviyo, Shopify, GA4, and Looker Studio skills if you need the full stack.

**Usage**

- Run a unified multi-platform audit across Google, Meta, and Microsoft simultaneously → get a single aggregate health score with platform breakdowns
- Pull competitor ad creative from public ad libraries → extract messaging patterns, creative formats, and positioning gaps
- Systematic waste analysis across Google and Meta → produces uploadable exclusion lists with thematic categorization
- TikTok Spark Ads + TikTok Shop creative strategy, pixel, Events API, and audience targeting in one skill

**Prompt**

```plain text
Run a cross-platform paid media audit across [Google Ads / Meta / Microsoft]. Use parallel agents per platform. Score each on: account structure, budget efficiency, creative fatigue, audience overlap, and attribution health. Output a unified A-F health score with a platform-by-platform action plan and wasted spend total.
```
Learn more at: [https://github.com/thatrebeccarae/claude-marketing](https://github.com/thatrebeccarae/claude-marketing) by Rebecca.

---

### 7. LIVE AD DATA + SEO SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/a0207470b4.png)

Skill’s generous free tier offers connection with Search Console and Bing Webmaster Tools. 

Premium (\$9/mo) includes live data from Google Ads, Meta, LinkedIn, HubSpot, Klaviyo, and Mailchimp. 

The differentiator is real account data, not exports, not static screenshots. It follows directly through MCP into Claude Code. Catches issues you'd never find from an export alone.

**Usage**

- Run `/seo-audit` post-site-migration → agent catches indexation drops and canonicalization issues in real time
- Connect live Google Ads → analyze search terms and find wasted spend without exporting CSVs
- Monitor SEO health continuously with `/seo-monitor` → get alerts when rankings drop before clients notice
- Pull LinkedIn and HubSpot data for full-funnel attribution from ad click to CRM deal

**Prompt**

```plain text
Connect to my Google Ads account via Cogny MCP. Run a search term analysis for the last 30 days. Find: wasted spend on irrelevant queries, keywords with high spend and zero conversions, and ad groups with Quality Score below 5. Output a negative keyword list ready to upload and 3 RSA copy improvements per underperforming ad group.
```
Learn more at: [https://github.com/cognyai/claude-code-marketing-skills](https://github.com/cognyai/claude-code-marketing-skills) by COGNY 

---

## Content Research & Intelligence

### 8. FIRECRAWL WEB INTELLIGENCE SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/f3b34f1047.png)

Not something new, similar to firecrawl-claude-plugin, but helps the content team's hours by providing claude code / openclaw/ hermes access to the live web.

This allows the agent to scrape any page as clean markdown, take full-page screenshots, extract structured data via JSON schema, and crawl entire doc sites. Handles JS rendering and anti-bot automatically.

**Usage**

- Scrape competitor pricing pages weekly → output structured comparison to a Sheet with zero manual copying
- Crawl a competitor's entire blog → extract all topics covered → identify content gaps in your own strategy
- Pull product reviews from G2, Capterra, or Reddit → auto-summarize into a customer voice report
- Run firecrawl x competitor-analysis \[URL\] in one command to get a full messaging teardown

**Prompt**

```plain text
Use Firecrawl to scrape [competitor URL]. Extract: headline, value prop, pricing tiers, key feature claims, and CTA copy. Then scrape [second competitor URL] and output a side-by-side comparison table with a 'gap for us' column.
```
You can learn more at [https://github.com/firecrawl/firecrawl-claude-plugin](https://github.com/firecrawl/firecrawl-claude-plugin) by firecrawl

---

### 9. DEEP  MULTI-STEP RESEARCH SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/c4cad3cee9.png)

This is the skill to reach for before writing any piece of content, building a pitch deck, or scoping a competitive landscape. It stops the agent from hallucinating sources by building a verifiable outline first.

It's a structured two-phase research workflow with human-in-the-loop control at every stage. 

- Phase 1 generates an extensible research outline for your approval. 
- Phase 2 does the deep investigation. 

The skill set covers academic research, technical comparison, market research, competitor analysis, due diligence, and investment analysis. 

Works with Claude Code, OpenCode, and Codex. 

**Usage**

- Research any competitor landscape → outline → deep investigation → structured report with verified sources
- Run market analysis before launching a campaign: ICP validation, category positioning, pricing benchmarks
- Due diligence on a new client, market, or partnership -company research, industry trends, risk signals -all structured and cited
- Academic literature surveys and benchmark reviews before writing technical content

**Prompt**

```plain text
Run a deep research workflow on [topic]. Phase 1: generate a research outline covering [market size / key players / positioning / trends / gaps]. I'll approve before you proceed. Phase 2: investigate each section with sourced evidence. Output a structured report with citations, a competitive matrix, and 3 key insights I can use for [content/campaign/pitch].
```
Learn more at: [https://github.com/Weizhena/Deep-Research-skills](https://github.com/Weizhena/Deep-Research-skills) by 

---

### 10. AI COMPETITIVE MARKETING INTELLIGENCE SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/7dea41cf85.png)

This skill contains 15 marketing skills, each with parallel subagents support, covering the full marketing intelligence stack. 

It acts as a dedicated competitive intelligence agent that audits any competitor's full online presence (messaging, positioning, SEO, ads, social, and brand voice) to output a client-ready PDF report.

Also ships with :  

- `market-competitors` for competitive intelligence on any competitor's online presence.
- `market-funnel` : for funnel analysis,
- `market-brand` : for brand voice extraction, 
-  `market-proposal` : for auto-generating client proposals from audit output.

**Usage**

- Run a full competitive intelligence audit on any competitor → messaging teardown, positioning gaps, SEO comparison, and ad creative analysis in one pass
- Extract a competitor's brand voice from their website and content → use it to sharpen your own differentiation
- Funnel analysis from landing page to checkout → score each step and get a prioritized CRO fix list
- Auto-generate a client proposal from an audit output → strategy, timeline, and pricing all drafted from the research

**Prompt**

```plain text
Run /market competitors [competitor URL]. Extract: headline messaging, value proposition, pricing strategy, key feature claims, target ICP signals, and brand tone. Compare against [your URL]. Output a side-by-side positioning matrix, identify the 3 biggest gaps we can exploit, and draft a differentiation statement for our homepage hero.
```
Learn more at: [https://github.com/zubair-trabzada/ai-marketing-claude](https://github.com/zubair-trabzada/ai-marketing-claude) by Zubair.

---

## Animation & Content Creation

### 11. REMOTION VIDEO SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/8865908205.png)

Product launch videos can make or break a product lead generation strategy, and launch videos are in high demand, so they are very expensive. With these skills you can build your own one, practically for 0\$

This skill is the official Remotion guidance baked into a SKILL.md. It gives Claude Code the knowledge to generate React-based programmatic videos without hallucinating the API.

Describe a video, get production code.

**Usage**

- Generate a launch announcement video from your product copy - hook, feature highlights, CTA- fully coded in React
- Build a weekly metrics video that pulls live data and renders as an MP4 automatically
- Create social-ready short-form video assets (16:9, 9:16) from existing blog content
- Render music videos from audio files and lyrics using the acestep-simplemv pattern

**Prompt**

```plain text
Using Remotion, build a 30-second product launch video for [product name].

Scene 1: bold headline on dark background.

Scene 2: 3 feature callouts with staggered animations.

Scene 3: CTA with logo.

Export at 1080x1920 for Instagram Reels. Follow Remotion best practices throughout.
```
You can learn more at [https://github.com/mxyhi/ok-skills](https://github.com/mxyhi/ok-skills) by mxyhi.

---

### 12. GSAP SKILLS 

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/f2445c2cea.png)

Official GSAP skills published by GreenSock Animation Studio. 

If you're building marketing pages with scroll animations, hero transitions, or text reveals, this is the skill that can stop Claude from hallucinating GSAP's API.

Contains eight skills covering: 

- core API, 
- timelines, 
- ScrollTrigger, 
- plugins, 
- React/Vue/Svelte integration, vanilla JS, 
- and performance. Following Webflow's acquisition of GSAP, 

Best part: all formerly paid Club GSAP plugins (SplitText, MorphSVG, etc.) are now free -this skill covers all of them. 

Works with Claude Code, Codex, Cursor, Windsurf, and 40+ agents, including Open Claw

**Usage**

- Add ScrollTrigger-powered scroll animations to any marketing page -parallax, pinned sections, counter animations -without getting the syntax wrong
- Build SplitText headline reveals and character-by-character animations for hero sections that actually perform
- Integrate GSAP correctly into React with the `useGSAP` hook -proper scoping, cleanup, and no memory leaks
- Chain complex timelines for product demo sequences and onboarding flows

**Prompt**

```plain text
Using GSAP with ScrollTrigger, build scroll animations for [page/component]. Include: hero headline SplitText reveal on load, feature cards staggered fade-in on scroll, and a pinned section with horizontal scroll. Use the useGSAP hook for React. Follow GSAP best practices for cleanup and performance.
```
Learn more at: [https://github.com/greensock/gsap-skills](https://github.com/greensock/gsap-skills)  by GreenSock.

---

### 13. CLAUDE CODE VIDEO TOOLKIT

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/9322e4e58a.png)

This official Remotion skill went viral in January 2026, garnering 6M+ views and 25k+ installs in its first week.

If you need explainer animations for technical products, this is it. Also covers screen recording via Playwright, YouTube clipping, and FFmpeg post-processing. 

Three separate AI pipelines (Claude, Gemini, Kimi K2.5) for generating Manim animations from text or images. 

Comes with 55+ working examples across physics, mathematics, CS, cosmology, and finance. 

Works with all mazor agents.

**Usage**

- Generate Manim scenes from text descriptions or images for technical explainers
- Record app flows with Playwright → Claude drives your app, clicks through workflows, fills forms -all recorded to video
- Create physics, math, or finance animations without touching Manim syntax
- Chain Remotion for launch videos with Manim for technical education content

**Prompt**

```plain text
Create a Manim animation explaining [concept]. Generate the scene code from scratch. Include: [visual elements], animations showing [process/relationship], and text overlays. Then render it as an MP4 and optimize for YouTube (16:9, H.264 codec).
```
Learn more at: [https://github.com/wilwaldon/Claude-Code-Video-Toolkit](https://github.com/wilwaldon/Claude-Code-Video-Toolkit) by wilwaldon.

---

## Webpage Design

### 14. MARKETING PAGE GENERATION SKILL - FRONTEND

Here is another design skill, tailored to marketing page generation, which, surprisingly, Gemini 3.1 Pro does best but lacks color and aesthetic appeal.

These skills instruct Claude/OpenClaw to forgo safe, generic aesthetics and commit to bold design decisions. 277,000+ installs.

Best part? Automatically triggers for any frontend task - landing pages, campaign pages, dashboards.

**Usage**

I use it to :

- Generate a complete SaaS landing page with hero, features, social proof, pricing, and CTA sections in a single pass.
- Build campaign-specific microsites from a brief - product, ICP, and tone - without touching Webflow.
- Rebuild an underperforming landing page with a distinct visual identity from a reference URL.
- Scaffold a complete marketing site with dark mode, responsive layout, and conversion-optimized structure.

It's a great one and I personally use it to build landing pages for products that lead to a high conversion rate. Really a must have!

**Prompt**

```plain text
Build a landing page for [product]. Target audience: [ICP]. Tone: [direct/editorial/minimal]. Must include: hero with headline + subhead + CTA, 3-feature section, social proof strip, and pricing table. Choose a bold, distinctive aesthetic - not generic SaaS. Output production-ready HTML/CSS/JS or React + Tailwind.
```
Give it a try at https://github.com/anthropics/claudecode/tree/main/plugins/frontend-design

---

### 15. DESIGN SKILL

Design is an essential aspect of conversion flow; it all starts there. Having a great design that tells the product's story leads to better conversions. But AI-generated designs are slop.

This skills fix that. It extracts design systems from reference UI images and generates implementation-ready design tokens, component specs, and style guides.

This kills the back-and-forth between designer and agent.

**Usage**

- Feed a screenshot of any UI → get a full design system extracted with colors, spacing, typography, and component patterns.
- Generate brand-consistent design tokens for a new product from a single reference image.
- Audit the existing UI for design consistency issues and obtain a structured list of fixes.
- Bootstrap a complete design system from scratch using decision-tree prompts before a single line of code.

**Prompt**

```plain text
Extract the design system from this UI screenshot [attach image]. Output: color palette with hex values, typography scale, spacing system, component inventory, and border/shadow tokens. Then generate a DESIGN_SYSTEM.md I can reference in all future builds.
```
I use this one almost every week. 

Learn more at: [https://github.com/daymade/claude-code-skills](https://github.com/daymade/claude-code-skills) 

---

### 16. CLAUDE DESIGN SKILLS  PACK - 3D, WEBGL & ANIMATION 

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/1243e8714b.png)

If the Frontend Design skill (skill #16 above) is your everyday landing page builder, this is what you reach for when the brief says "we want something that feels like Apple.com."

Comes with 27 plugins covering the full modern web design stack: Three.js, GSAP, React Three Fiber, Framer Motion, Babylon.js, A-Frame, PixiJS, Lottie, Rive, Spline, and more. 

Organized into installable bundles so you only load what you need. Built for agencies and studios doing high-end marketing pages that need to look like they cost \$50K.

**Usage**

- Build a Three.js WebGL hero section with PBR materials, lighting, and mouse-parallax interaction -no 3D expertise required
- Add Lottie animations to any marketing page from an existing After Effects export -correct implementation, no playback bugs
- Create Rive interactive brand mascots or state-machine animations that respond to user actions
- Build Spline 3D product configurators embedded directly into landing pages

**Prompt**

```plain text
Build a marketing hero section using Three.js. Include: a rotating 3D product model with PBR materials, ambient and directional lighting, mouse-parallax camera movement, and a GSAP ScrollTrigger fade-out on scroll. Export as a self-contained React component. Follow performance best practices -lazy load the canvas, use instanced meshes, cap to 60fps.
```
Learn more at: [https://github.com/freshtechbro/claudedesignskills](https://github.com/freshtechbro/claudedesignskills) by freshtechbro

---

## Content Creation & Editorial

### 17. CONTENT OPS SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/8512d30ed2.png)

Your entire content operations workflow under one skill.

It contains 8 skills that cover: production, audit, repurposing, and distribution. Works as a full editorial pipeline: from brief to published inside Claude Code or OpenClaw.

Simply put, it's a Swiss Army knife for the content and growth team as well.

**Usage**

- Run a full content audit: feed your sitemap → agent scores each page by intent, keyword alignment, and conversion potential → outputs prioritized fix list
- Repurpose one long-form piece → 5 tweet threads + 3 LinkedIn posts + 1 newsletter section in one command
- Generate a 12-week content calendar from your ICP, product pillars, and keyword list
- Auto-draft blog posts from raw notes or voice transcripts, following your brand voice file

**Prompt**

```plain text
Run a content ops workflow for [topic/URL]. Step 1: audit existing content for gaps vs [keyword cluster]. Step 2: generate 4 new content briefs targeting missed keywords. Step 3: draft post #1 in full using our brand voice [attach]. Step 4: repurpose it into 3 LinkedIn posts and a tweet thread
```
Get your content pipeline started: https://github.com/superamped/ai-marketing-skills by superramped.

While the content pipeline works, but it may still feel ai-generated, so pair it with next skill and see the organic growth.

---

### 18. HUMANIZER SKILL - DE-SLOP YOUR CONTENT

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/3b3f6c0436.png)

The biggest problem with social media right now is AI-generated slop. If you use AI to generate posts and post them directly, you need to stop!

Instead, use the Humanizer Skill.

It's an 8-pass editing system that detects and kills AI writing patterns before your content goes live. Strips banned vocabulary (delve, tapestry, foster), breaks structural tells, and forces organization & soul back into the copy.

**Usage**

- Paste any AI-generated blog post → get a rewrite that sounds like a person with opinions wrote it, not a language model.
- Run on cold email sequences before sending → eliminates the "this was clearly written by AI" reply-killing patterns.
- Match a specific human voice by feeding 2-3 paragraphs of your own writing as a style reference before humanizing.
- Use after every content ops skill output. This makes the pair unstoppable for publishing-ready content at scale.

**Prompt**

```plain text
Humanize this text: [paste]. First, scan for AI vocabulary, structural tells, and rhythm patterns. Then rewrite in 3 passes:

- kill AI words, break parallel structures, add human texture and opinion.

- Run an audit pass

- list everything still obviously AI then do a final rewrite.

Match this voice sample: [paste 2 paragraphs of your own writing].
```
Link: [https://github.com/jpeggdev/humanize-writing](https://github.com/jpeggdev/humanize-writing) by jpeggdev

---

### 19. CLAUDE BLOG SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/7a22baea13.png)

If you are looking for a blog skill that ships a complete article rather than just a draft, this is it.

It's a full-lifecycle blog engine containing: 30 sub-skills, 5 agents, 12 content templates.

Every draft passes a 5-gate delivery contract (Capability, Format, Visual, Content Review, Asset Integrity) scored against a 100-point rubric. Nothing below 90 reaches out. 

Dual-optimized for Google's SEO Core Update and AI citation platforms.

Unique feature: it auto-sources images from Pixabay/Unsplash/Pexels, generates hero images via Gemini, builds SVG charts in 7 styles, and embeds YouTube with AI-crawler-friendly fallbacks if instructed.

**Usage**

- Write a full blog post from a keyword or outline → passes 5 quality gates → arrives with hero image, inline charts, and FAQ schema already embedded
- Optimize existing posts for AI citation: answer-first formatting, citation capsules, 120-180 word passage blocks for passage-level indexing
- Generate a series of topically clustered articles from a keyword list with automatic internal linking
- Produce social cards and OG images for every post as part of the same workflow

**Prompt**

```plain text
Write a full blog post targeting [keyword]. Follow the 5-gate delivery contract. Include: E-E-A-T signals with author credentials, FAQ schema for the top 5 questions, a hero image sourced from Unsplash, 2 inline SVG charts, and YouTube embed for [video URL]. Dual-optimize for Google and ChatGPT/Perplexity citation. Output the complete HTML file.
```
Learn more at: [https://github.com/AgriciDaniel/claude-blog](https://github.com/AgriciDaniel/claude-blog)  by Daniel.

---

## Growth & Outreach

### 20. COLD-OUTREACH SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/9da23a34a1.png)

This skill generates high-converting cold outreach across email, SMS, and LinkedIn DM. It uses Alex Hormozi Frameworks for cold outreach.

It uses battle-tested Hormozi offer frameworks baked directly into the skill. Not generic copy - structured around irresistible offer construction.

**Usage**

- Generate a 5-email cold outreach sequence from competitor research or market intel
- Pull strongest pain observations and proof points from sales research → craft Hormozi-framed hooks
- Create SMS and LinkedIn DM variants from the same core message framework
- A/B test different offer structures against your ICP

**Prompt**

```plain text
Generate a cold outreach campaign using Alex Hormozi frameworks. Target: [ICP]. Pain point: [specific problem]. Solution: [your offer]. Output: 5-email sequence with subject lines, 3 SMS variants, and 3 LinkedIn DM variations. Each should follow Hormozi's problem-solution-offer arc without sounding salesy.
```
You can learn more at [https://clawskills.sh/skills/staybased-cold-outreach](https://clawskills.sh/skills/staybased-cold-outreach) by staybased

---

### 21. MARKETING-PSYCHOLOGY SKILL

![](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/assets/marketing-skills/cad4c877a5.png)

This is the skill every marketer should use. If you are someone who wants to write a funnel that converts through a landing page, this skill is a must-have. 

It applies cognitive biases, mental models, and behavioral science frameworks directly to marketing copy, CRO, and funnel design. Includes Cialdini, Kahneman, and Ariely's strategy and research process.

**Usage**

- Rewrite homepage copy using scarcity, social proof, and anchoring biases
- Audit landing pages for friction points -identify where choice paralysis or decision fatigue kills conversions
- Test multiple versions of CTAs against different cognitive frames
- Build funnel sequences using reciprocity and commitment biases

**Prompt**

```plain text
Analyze this copy: [paste]. Apply 12 cognitive bias principles to improve conversion: Cialdini's authority/scarcity/social proof, Kahneman's loss aversion, anchoring effects. Rewrite the headline, value prop, and CTA using 3 different psychological frameworks. Output all 3 variants with notes on which biases each leverages.
```
You can learn more at [https://github.com/coreyhaines31/marketingskills/tree/main/skills/marketing-psychology](https://github.com/coreyhaines31/marketingskills/tree/main/skills/marketing-psychology) by coreyhaines31

---

## Final Thoughts

Use a chain of skills to get the output that makes the product position unique and well-framed.

Simple example:

- Use the Content Ops skill to draft the long-form piece
- Pass it to marketing-psychology skill rewrites every headline and CTA against 12 cognitive bias principles
- Humanizer strips every AI tell across 8 passes → what's left sounds like your sharpest human writer, not a model hitting word count
- Cold Outreach pulls the strongest pain observations and proof points from that copy → lands in your ICP's inbox as a Hormozi-framed 5-email sequence and 3 LinkedIn DMs.

The hard part isn't the skills - it's orchestrating the flow between them. That's where Composio comes in handy.

It handles the auth layer, manages tool calls across every integration, and keeps context intact as output moves from one skill to the next. No glue code. No dropped state. No manual handoffs.

Install it once. Let it run the pipeline while you focus on what AI still can't do - knowing your customer well enough to say something worth reading.