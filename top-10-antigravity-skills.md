---
title: "Top 10 Antigravity Skills"
source: https://www.notion.so/composio/top-10-antigravity-skills-341f261a6dfe80f9beb5ed28b0b2eae3
notion_page_id: 341f261a-6dfe-80f9-beb5-ed28b0b2eae3
last_synced: 2026-04-27T12:16:01Z
---

I use Antigravity almost every day while building things. In the beginning, I was just running agents and connecting tools, but it still felt like I was doing a lot of work outside the flow.
Then I came across agent skills. They are basically plug-and-play capabilities that let your agent connect with different tools and actually get things done. I got curious and started trying them out in my own workflow.
That is when things started to click. I was not jumping between tools or writing extra logic for every step. I could use a skill and keep everything inside one flow. It made the whole setup feel much more natural and a lot less messy.
So in this article, I am sharing 10 useful skills you can use with Antigravity, what they do, and where they can help in real workflows.
### 1. **Tool Integration with Composio**
The [Composio skill](https://github.com/ComposioHQ/skills) is what connects your agent to external tools and services. It gives you access to 1000+ tools through a single interface, so you don't have to deal with separate APIs, auth flows, or custom integrations for each one. 
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/1207c842-699f-405e-bed9-e77e4cbff1d7/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666R6DOWMS%2F20260427%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260427T121539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICiKfM09OusEBpjwrugxtJdDCFZ5ugmuqzSTCCI5JF2uAiB3nrlkLvgUx1amsd2xI%2BchdxfRNH3V0%2FwbwQr5iw6x9CqIBAjE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ouiOS%2FVzFVS4wqeKtwD4TsAYYX86CrejYrtrks%2FIOZNkBmGl6YTXZEn53PUhnUGEgv1Y%2Fa3hO08%2B9DHzryjtkbErKR935MMs2%2BZYH4vZsxrSLXj0U9ZGlsiL3ygSF7T5bOGzVexA5V6x5aOWzlyrhAKgbxeIWbi6IS%2FJbHx2N9rgfqhHJ%2Fs2QnBTmfnEqynyWoCJ6tIbVuV%2BSAv66nWkwheTCyXgWjhZvWIP47QIQHl8mlwMrTt2Xdkh%2B2Br1vJ5gcxpIlqUbvifGsYDRY7toiBsUl4T2wNtit%2BU4A708fepZwoi8U%2Bg2H6a2mbzYt9gdyydl3b4vRC9Ck6yfOFLxSa1F2HxL4v88g2DWelKuTn5ogKkQzR2rLM5LdPIuZk0yQQZvTZeungoGfqGPwewollyDGJHOPpr%2BD8S14k9FcBxBncrFgNNFOty9sL1ptP6PbjSvGUSQ3XRE57XmBx1niNh0r%2BT9RUaywGO7ByEmluREnfmtC5NTug51nR2Z0cO4Ryb%2FRYlh48P%2BnOJVRpK7pFy6s8Nxm9Qr4zruITBmwMnH%2BzZYtWaP3ZDS84FGR%2BM8bYiFuKHJ8HgNEbSWdUc5gNvX4y2zlqwBQp%2BoFKeAwROFpn6JEmRVjAxY2%2FaSagYdeUzoP55pJwSGcw1Yu9zwY6pgE7zYBSYfqNQKUP4nzCPp8lpzrO859HlCbPWNjIE33IQ%2Fx83axFHo%2BiYzs8u5woUOBPYjC4GpIFCWFmRoRRNKrj5xl%2F1WOfNeTKoUBwXEF9ADmTixkxKMt7OcrluutmglVO9je0%2BkBVhW%2FVs4b%2BM57m8Vudgcese2FCefPpshXV9jW1IZPGnqSwZbSYx4Vp2ezuzyl4sBZ7D2kPWw5x5c31SmSqtkM%2F&X-Amz-Signature=30220d8b711bb5ecf7b597cc20cfb6c53c1dab7d3b31b36ab460aa812912e89f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
Everything shows up as simple actions your agent can call when needed. It also takes care of things like sessions, tokens, and maintaining stable connections, so you do not have to worry about the setup.
In real use, your agent can find a tool, connect to it, and run actions as part of one flow. It can pull data from one place, use it, and trigger something somewhere else without extra code. You can also chain multiple steps, handle errors, and react to events, which makes it work well for real workflows.
**Why it matters**
- No need to deal with OAuth or token handling for every tool
- You can chain actions across tools in one run
- Works with different frameworks, so you are not locked in
- Handles sessions, errors, and execution flow for you
- Supports triggers, so your agent can react to events
**Who is it useful for: **If your agent needs to talk to more than one tool, you will need this. It is useful for anyone building multi-step workflows or connecting to tools like GitHub, Slack, or other platforms where setup usually takes time.
**How to install it**
```plain text
npx skills add composiohq/skills
```
## 2. Web Data Extraction with Apify
The [Apify skill ](https://blog.apify.com/introducing-apify-agent-skills/)lets your agent pull real, structured data from the web, not just search results. It can scrape platforms like Instagram, YouTube, Google Maps, Amazon, and more, returning clean JSON that your agent can use directly. 
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/f9230fd6-3f3b-40ea-a538-1cf14e2afaae/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666R6DOWMS%2F20260427%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260427T121539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICiKfM09OusEBpjwrugxtJdDCFZ5ugmuqzSTCCI5JF2uAiB3nrlkLvgUx1amsd2xI%2BchdxfRNH3V0%2FwbwQr5iw6x9CqIBAjE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ouiOS%2FVzFVS4wqeKtwD4TsAYYX86CrejYrtrks%2FIOZNkBmGl6YTXZEn53PUhnUGEgv1Y%2Fa3hO08%2B9DHzryjtkbErKR935MMs2%2BZYH4vZsxrSLXj0U9ZGlsiL3ygSF7T5bOGzVexA5V6x5aOWzlyrhAKgbxeIWbi6IS%2FJbHx2N9rgfqhHJ%2Fs2QnBTmfnEqynyWoCJ6tIbVuV%2BSAv66nWkwheTCyXgWjhZvWIP47QIQHl8mlwMrTt2Xdkh%2B2Br1vJ5gcxpIlqUbvifGsYDRY7toiBsUl4T2wNtit%2BU4A708fepZwoi8U%2Bg2H6a2mbzYt9gdyydl3b4vRC9Ck6yfOFLxSa1F2HxL4v88g2DWelKuTn5ogKkQzR2rLM5LdPIuZk0yQQZvTZeungoGfqGPwewollyDGJHOPpr%2BD8S14k9FcBxBncrFgNNFOty9sL1ptP6PbjSvGUSQ3XRE57XmBx1niNh0r%2BT9RUaywGO7ByEmluREnfmtC5NTug51nR2Z0cO4Ryb%2FRYlh48P%2BnOJVRpK7pFy6s8Nxm9Qr4zruITBmwMnH%2BzZYtWaP3ZDS84FGR%2BM8bYiFuKHJ8HgNEbSWdUc5gNvX4y2zlqwBQp%2BoFKeAwROFpn6JEmRVjAxY2%2FaSagYdeUzoP55pJwSGcw1Yu9zwY6pgE7zYBSYfqNQKUP4nzCPp8lpzrO859HlCbPWNjIE33IQ%2Fx83axFHo%2BiYzs8u5woUOBPYjC4GpIFCWFmRoRRNKrj5xl%2F1WOfNeTKoUBwXEF9ADmTixkxKMt7OcrluutmglVO9je0%2BkBVhW%2FVs4b%2BM57m8Vudgcese2FCefPpshXV9jW1IZPGnqSwZbSYx4Vp2ezuzyl4sBZ7D2kPWw5x5c31SmSqtkM%2F&X-Amz-Signature=bf4df9b7634962b125c6673bee0f58283807ab45bb5d163dd4f002ddc1c882c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
You can also build and run custom scrapers, so you are not limited to predefined sources. In practice, your agent can collect live data, track updates, and combine multiple sources in one flow, working with fresh information that it can actually act on.
**Why it matters**
- Gives your agent access to real-time web data
- Supports many platforms out of the box
- Returns structured data your agent can directly use
- Let's you build and run custom scrapers
- Helps avoid relying on outdated information
**Who is it useful for: **This is useful if your agent needs live external data. It works well for tracking prices, monitoring competitors, collecting leads, following trends, or building data pipelines.
**How to install it**
```plain text
npx skills add apify/agent-skills
```
## 3. Real-World Communication with Twilio
[Twilio](https://www.twilio.com/en-us) lets your agent send messages, make calls, and handle communication over SMS, WhatsApp, and voice. It turns your agent from just processing data into something that can actually reach people. You can use it for notifications, alerts, OTP verification, reminders, or automated voice calls.
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/16e656f7-1fe4-4596-bfe9-ca0f40cd6f11/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666R6DOWMS%2F20260427%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260427T121539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICiKfM09OusEBpjwrugxtJdDCFZ5ugmuqzSTCCI5JF2uAiB3nrlkLvgUx1amsd2xI%2BchdxfRNH3V0%2FwbwQr5iw6x9CqIBAjE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ouiOS%2FVzFVS4wqeKtwD4TsAYYX86CrejYrtrks%2FIOZNkBmGl6YTXZEn53PUhnUGEgv1Y%2Fa3hO08%2B9DHzryjtkbErKR935MMs2%2BZYH4vZsxrSLXj0U9ZGlsiL3ygSF7T5bOGzVexA5V6x5aOWzlyrhAKgbxeIWbi6IS%2FJbHx2N9rgfqhHJ%2Fs2QnBTmfnEqynyWoCJ6tIbVuV%2BSAv66nWkwheTCyXgWjhZvWIP47QIQHl8mlwMrTt2Xdkh%2B2Br1vJ5gcxpIlqUbvifGsYDRY7toiBsUl4T2wNtit%2BU4A708fepZwoi8U%2Bg2H6a2mbzYt9gdyydl3b4vRC9Ck6yfOFLxSa1F2HxL4v88g2DWelKuTn5ogKkQzR2rLM5LdPIuZk0yQQZvTZeungoGfqGPwewollyDGJHOPpr%2BD8S14k9FcBxBncrFgNNFOty9sL1ptP6PbjSvGUSQ3XRE57XmBx1niNh0r%2BT9RUaywGO7ByEmluREnfmtC5NTug51nR2Z0cO4Ryb%2FRYlh48P%2BnOJVRpK7pFy6s8Nxm9Qr4zruITBmwMnH%2BzZYtWaP3ZDS84FGR%2BM8bYiFuKHJ8HgNEbSWdUc5gNvX4y2zlqwBQp%2BoFKeAwROFpn6JEmRVjAxY2%2FaSagYdeUzoP55pJwSGcw1Yu9zwY6pgE7zYBSYfqNQKUP4nzCPp8lpzrO859HlCbPWNjIE33IQ%2Fx83axFHo%2BiYzs8u5woUOBPYjC4GpIFCWFmRoRRNKrj5xl%2F1WOfNeTKoUBwXEF9ADmTixkxKMt7OcrluutmglVO9je0%2BkBVhW%2FVs4b%2BM57m8Vudgcese2FCefPpshXV9jW1IZPGnqSwZbSYx4Vp2ezuzyl4sBZ7D2kPWw5x5c31SmSqtkM%2F&X-Amz-Signature=dd17c3c5046924c65319ab7b42e35dcda92a5128b615f1e69d6907018071afec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
Your agent can trigger messages or calls as part of a workflow, like sending alerts, confirming actions, or verifying users. It supports both outbound and inbound communication, including text-to-speech, call handling, and message tracking.
**Why it matters**
- Let your agent interact with real users
- Supports SMS, WhatsApp, and voice in one place
- Enables OTP and verification workflows
- Works for alerts, notifications, and escalations
- Helps complete workflows without human handoff
**Who is it useful for: **Useful for any workflow that needs human interaction. This includes DevOps alerts, customer notifications, verification flows, support systems, and any case where your agent needs to reach someone directly.
**How to install and use it**
```plain text
npx skills add openclaw/skills-twilio
```
## 4. Multimodal AI with Replicate
The [Replicate skill ](https://replicate.com/)gives your agent access to a wide range of AI models for things beyond text. It includes models for image generation, video, audio, speech, and more, all available through a simple API without needing to manage GPUs or setup. This means your agent can generate visuals, process audio, or work with video as part of its workflow.
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/60e64578-c53b-4a0c-a23e-27c6c971f7eb/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666R6DOWMS%2F20260427%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260427T121539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICiKfM09OusEBpjwrugxtJdDCFZ5ugmuqzSTCCI5JF2uAiB3nrlkLvgUx1amsd2xI%2BchdxfRNH3V0%2FwbwQr5iw6x9CqIBAjE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ouiOS%2FVzFVS4wqeKtwD4TsAYYX86CrejYrtrks%2FIOZNkBmGl6YTXZEn53PUhnUGEgv1Y%2Fa3hO08%2B9DHzryjtkbErKR935MMs2%2BZYH4vZsxrSLXj0U9ZGlsiL3ygSF7T5bOGzVexA5V6x5aOWzlyrhAKgbxeIWbi6IS%2FJbHx2N9rgfqhHJ%2Fs2QnBTmfnEqynyWoCJ6tIbVuV%2BSAv66nWkwheTCyXgWjhZvWIP47QIQHl8mlwMrTt2Xdkh%2B2Br1vJ5gcxpIlqUbvifGsYDRY7toiBsUl4T2wNtit%2BU4A708fepZwoi8U%2Bg2H6a2mbzYt9gdyydl3b4vRC9Ck6yfOFLxSa1F2HxL4v88g2DWelKuTn5ogKkQzR2rLM5LdPIuZk0yQQZvTZeungoGfqGPwewollyDGJHOPpr%2BD8S14k9FcBxBncrFgNNFOty9sL1ptP6PbjSvGUSQ3XRE57XmBx1niNh0r%2BT9RUaywGO7ByEmluREnfmtC5NTug51nR2Z0cO4Ryb%2FRYlh48P%2BnOJVRpK7pFy6s8Nxm9Qr4zruITBmwMnH%2BzZYtWaP3ZDS84FGR%2BM8bYiFuKHJ8HgNEbSWdUc5gNvX4y2zlqwBQp%2BoFKeAwROFpn6JEmRVjAxY2%2FaSagYdeUzoP55pJwSGcw1Yu9zwY6pgE7zYBSYfqNQKUP4nzCPp8lpzrO859HlCbPWNjIE33IQ%2Fx83axFHo%2BiYzs8u5woUOBPYjC4GpIFCWFmRoRRNKrj5xl%2F1WOfNeTKoUBwXEF9ADmTixkxKMt7OcrluutmglVO9je0%2BkBVhW%2FVs4b%2BM57m8Vudgcese2FCefPpshXV9jW1IZPGnqSwZbSYx4Vp2ezuzyl4sBZ7D2kPWw5x5c31SmSqtkM%2F&X-Amz-Signature=616928ca02a1e9fab6bfe96af48e0134a19dbfa1f0e3e7792bd70e7a7d626fde&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
Your agent can pick the right model, run it, and use the output directly. It also supports fine-tuning and lets you work with production-ready models that scale automatically, so you can build real features without worrying about infrastructure.
**Why it matters**
- Adds image, video, audio, and speech capabilities to your agent
- No need to manage GPUs or ML infrastructure
- Access to a large collection of ready-to-use models
- Supports fine-tuning for custom use cases
- Scales automatically based on usage
**Who is it useful for: A**nyone building agents that need more than text. This includes generating images, creating videos, processing audio, or building multimodal applications without having to handle ML infrastructure.
**How to install and use it**
```plain text
npx skills add replicate/skills
```
## 5. Data Insights with Metabase
The [Metabase skill](https://github.com/metabase/agent-skills) connects your agent to your internal business data. It lets your agent query databases using natural language, generate and run SQL, create dashboards, and surface insights using the semantic layer your team has already set up. 
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/72a56d40-7518-4885-ae0b-5dc18603ba40/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666R6DOWMS%2F20260427%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260427T121539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICiKfM09OusEBpjwrugxtJdDCFZ5ugmuqzSTCCI5JF2uAiB3nrlkLvgUx1amsd2xI%2BchdxfRNH3V0%2FwbwQr5iw6x9CqIBAjE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ouiOS%2FVzFVS4wqeKtwD4TsAYYX86CrejYrtrks%2FIOZNkBmGl6YTXZEn53PUhnUGEgv1Y%2Fa3hO08%2B9DHzryjtkbErKR935MMs2%2BZYH4vZsxrSLXj0U9ZGlsiL3ygSF7T5bOGzVexA5V6x5aOWzlyrhAKgbxeIWbi6IS%2FJbHx2N9rgfqhHJ%2Fs2QnBTmfnEqynyWoCJ6tIbVuV%2BSAv66nWkwheTCyXgWjhZvWIP47QIQHl8mlwMrTt2Xdkh%2B2Br1vJ5gcxpIlqUbvifGsYDRY7toiBsUl4T2wNtit%2BU4A708fepZwoi8U%2Bg2H6a2mbzYt9gdyydl3b4vRC9Ck6yfOFLxSa1F2HxL4v88g2DWelKuTn5ogKkQzR2rLM5LdPIuZk0yQQZvTZeungoGfqGPwewollyDGJHOPpr%2BD8S14k9FcBxBncrFgNNFOty9sL1ptP6PbjSvGUSQ3XRE57XmBx1niNh0r%2BT9RUaywGO7ByEmluREnfmtC5NTug51nR2Z0cO4Ryb%2FRYlh48P%2BnOJVRpK7pFy6s8Nxm9Qr4zruITBmwMnH%2BzZYtWaP3ZDS84FGR%2BM8bYiFuKHJ8HgNEbSWdUc5gNvX4y2zlqwBQp%2BoFKeAwROFpn6JEmRVjAxY2%2FaSagYdeUzoP55pJwSGcw1Yu9zwY6pgE7zYBSYfqNQKUP4nzCPp8lpzrO859HlCbPWNjIE33IQ%2Fx83axFHo%2BiYzs8u5woUOBPYjC4GpIFCWFmRoRRNKrj5xl%2F1WOfNeTKoUBwXEF9ADmTixkxKMt7OcrluutmglVO9je0%2BkBVhW%2FVs4b%2BM57m8Vudgcese2FCefPpshXV9jW1IZPGnqSwZbSYx4Vp2ezuzyl4sBZ7D2kPWw5x5c31SmSqtkM%2F&X-Amz-Signature=01ea0d5441aa6190c6ad62f97cc6880b50f11fda0ae1b74e3ad3e499243b7415&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
This means your agent is not just working with external data; it is working with the data that actually matters to your business.
Your agent can explore data, detect patterns, create visualizations, and even manage dashboards. It can turn plain questions into queries, refine results, and keep everything within your existing data permissions and structure.
**Why it matters**
- Gives access to internal business data, not just public data
- Converts natural language into SQL queries
- Helps generate insights and dashboards automatically
- Works within your existing data governance setup
- Reduces the need for manual data analysis
**Who is it useful for: **Useful for teams that rely on internal data, like operations, finance, product, and analytics. It is also helpful for developers building agents that need to monitor systems or generate insights from structured data.
**How to install it**
```plain text
npx skills add metabase/agent-skills
```
## 6. Infrastructure and Edge Control with Cloudflare
The [Cloudflare skill](https://github.com/cloudflare/skills) gives your agent access to Cloudflare’s full developer and infrastructure stack. It covers topics such as Workers, storage, networking, security, and AI tools, and helps your agent choose the right service for the task. Instead of relying on static docs, the agent receives up-to-date guidance and patterns for building or managing systems.
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/33cbe1b7-fb9f-4c1c-9334-4b38e4cf593c/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666R6DOWMS%2F20260427%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260427T121539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICiKfM09OusEBpjwrugxtJdDCFZ5ugmuqzSTCCI5JF2uAiB3nrlkLvgUx1amsd2xI%2BchdxfRNH3V0%2FwbwQr5iw6x9CqIBAjE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ouiOS%2FVzFVS4wqeKtwD4TsAYYX86CrejYrtrks%2FIOZNkBmGl6YTXZEn53PUhnUGEgv1Y%2Fa3hO08%2B9DHzryjtkbErKR935MMs2%2BZYH4vZsxrSLXj0U9ZGlsiL3ygSF7T5bOGzVexA5V6x5aOWzlyrhAKgbxeIWbi6IS%2FJbHx2N9rgfqhHJ%2Fs2QnBTmfnEqynyWoCJ6tIbVuV%2BSAv66nWkwheTCyXgWjhZvWIP47QIQHl8mlwMrTt2Xdkh%2B2Br1vJ5gcxpIlqUbvifGsYDRY7toiBsUl4T2wNtit%2BU4A708fepZwoi8U%2Bg2H6a2mbzYt9gdyydl3b4vRC9Ck6yfOFLxSa1F2HxL4v88g2DWelKuTn5ogKkQzR2rLM5LdPIuZk0yQQZvTZeungoGfqGPwewollyDGJHOPpr%2BD8S14k9FcBxBncrFgNNFOty9sL1ptP6PbjSvGUSQ3XRE57XmBx1niNh0r%2BT9RUaywGO7ByEmluREnfmtC5NTug51nR2Z0cO4Ryb%2FRYlh48P%2BnOJVRpK7pFy6s8Nxm9Qr4zruITBmwMnH%2BzZYtWaP3ZDS84FGR%2BM8bYiFuKHJ8HgNEbSWdUc5gNvX4y2zlqwBQp%2BoFKeAwROFpn6JEmRVjAxY2%2FaSagYdeUzoP55pJwSGcw1Yu9zwY6pgE7zYBSYfqNQKUP4nzCPp8lpzrO859HlCbPWNjIE33IQ%2Fx83axFHo%2BiYzs8u5woUOBPYjC4GpIFCWFmRoRRNKrj5xl%2F1WOfNeTKoUBwXEF9ADmTixkxKMt7OcrluutmglVO9je0%2BkBVhW%2FVs4b%2BM57m8Vudgcese2FCefPpshXV9jW1IZPGnqSwZbSYx4Vp2ezuzyl4sBZ7D2kPWw5x5c31SmSqtkM%2F&X-Amz-Signature=af48303067859f04a2a6e4592e80fbd6c2cea18cd00ac852286c024832c1bfd7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
Your agent can work with deployments, manage infrastructure, apply security rules, and build or update edge services. It can also generate production-ready setups using Cloudflare tools and follow best practices without you needing to look everything up manually.
**Why it matters**
- Covers a wide range of infrastructure and edge services
- Helps choose the right tools based on use case
- Uses up-to-date documentation and patterns
- Supports building and deploying directly on Cloudflare
- Useful for both development and security workflows
**Who it is useful for: **Useful for developers and teams working with deployment, infrastructure, or edge computing. It is especially helpful for platform engineers, security workflows, and anyone building or managing apps on Cloudflare.
**How to install and use it**
```plain text
npx skills add cloudflare/skills
```
## 7. Programmatic Video Creation with Remotion
The [Remotion skill](https://www.remotion.dev/) lets your agent create videos using code. It is built around the Remotion framework, where videos are defined using React components, animations, and data. This means your agent can describe a video and generate it without using a traditional editor. 
It covers things like animations, captions, charts, media handling, and timing, so everything needed to build a video is handled through code.
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/25258d12-78e5-4998-abb3-715b68654092/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666R6DOWMS%2F20260427%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260427T121539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICiKfM09OusEBpjwrugxtJdDCFZ5ugmuqzSTCCI5JF2uAiB3nrlkLvgUx1amsd2xI%2BchdxfRNH3V0%2FwbwQr5iw6x9CqIBAjE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ouiOS%2FVzFVS4wqeKtwD4TsAYYX86CrejYrtrks%2FIOZNkBmGl6YTXZEn53PUhnUGEgv1Y%2Fa3hO08%2B9DHzryjtkbErKR935MMs2%2BZYH4vZsxrSLXj0U9ZGlsiL3ygSF7T5bOGzVexA5V6x5aOWzlyrhAKgbxeIWbi6IS%2FJbHx2N9rgfqhHJ%2Fs2QnBTmfnEqynyWoCJ6tIbVuV%2BSAv66nWkwheTCyXgWjhZvWIP47QIQHl8mlwMrTt2Xdkh%2B2Br1vJ5gcxpIlqUbvifGsYDRY7toiBsUl4T2wNtit%2BU4A708fepZwoi8U%2Bg2H6a2mbzYt9gdyydl3b4vRC9Ck6yfOFLxSa1F2HxL4v88g2DWelKuTn5ogKkQzR2rLM5LdPIuZk0yQQZvTZeungoGfqGPwewollyDGJHOPpr%2BD8S14k9FcBxBncrFgNNFOty9sL1ptP6PbjSvGUSQ3XRE57XmBx1niNh0r%2BT9RUaywGO7ByEmluREnfmtC5NTug51nR2Z0cO4Ryb%2FRYlh48P%2BnOJVRpK7pFy6s8Nxm9Qr4zruITBmwMnH%2BzZYtWaP3ZDS84FGR%2BM8bYiFuKHJ8HgNEbSWdUc5gNvX4y2zlqwBQp%2BoFKeAwROFpn6JEmRVjAxY2%2FaSagYdeUzoP55pJwSGcw1Yu9zwY6pgE7zYBSYfqNQKUP4nzCPp8lpzrO859HlCbPWNjIE33IQ%2Fx83axFHo%2BiYzs8u5woUOBPYjC4GpIFCWFmRoRRNKrj5xl%2F1WOfNeTKoUBwXEF9ADmTixkxKMt7OcrluutmglVO9je0%2BkBVhW%2FVs4b%2BM57m8Vudgcese2FCefPpshXV9jW1IZPGnqSwZbSYx4Vp2ezuzyl4sBZ7D2kPWw5x5c31SmSqtkM%2F&X-Amz-Signature=eac85b583b0bf9d30e860eb56b281dd2be771f3b7179d9f9db8ed571f5f6f14e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
Your agent can generate videos from data, text, or events, and control every part of the output. Since everything is code, the videos are repeatable, editable, and easy to update. It also supports advanced features like animations, audio sync, and visual effects, making it useful for more than just simple clips.
**Why it matters**
- Turns data and text into videos automatically
- No need for manual editing or design tools
- Supports animations, charts, captions, and effects
- Videos are version-controlled and repeatable
- Useful for automated and scalable content creation
**Who is it useful for: **Useful for teams that need to generate videos as part of workflows. This includes marketing content, data reports, product updates, or any case where a video output is more useful than text or static files.
**How to install it**
```plain text
npx skills add remotion-dev/skills
```
## 8. API Testing and Validation with Postman
The [Postman skill](https://learning.postman.com/docs/agent-mode/skills) gives your agent a way to test and validate APIs before using them in a workflow. It acts as a pre-check layer, ensuring endpoints are working, responses are correct, and nothing breaks mid-run. It can also generate tests, update collections, and work with various APIs, including REST, GraphQL, and more.
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/13ca0139-9c6e-495a-87f8-c506bbc69871/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666R6DOWMS%2F20260427%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260427T121539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICiKfM09OusEBpjwrugxtJdDCFZ5ugmuqzSTCCI5JF2uAiB3nrlkLvgUx1amsd2xI%2BchdxfRNH3V0%2FwbwQr5iw6x9CqIBAjE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ouiOS%2FVzFVS4wqeKtwD4TsAYYX86CrejYrtrks%2FIOZNkBmGl6YTXZEn53PUhnUGEgv1Y%2Fa3hO08%2B9DHzryjtkbErKR935MMs2%2BZYH4vZsxrSLXj0U9ZGlsiL3ygSF7T5bOGzVexA5V6x5aOWzlyrhAKgbxeIWbi6IS%2FJbHx2N9rgfqhHJ%2Fs2QnBTmfnEqynyWoCJ6tIbVuV%2BSAv66nWkwheTCyXgWjhZvWIP47QIQHl8mlwMrTt2Xdkh%2B2Br1vJ5gcxpIlqUbvifGsYDRY7toiBsUl4T2wNtit%2BU4A708fepZwoi8U%2Bg2H6a2mbzYt9gdyydl3b4vRC9Ck6yfOFLxSa1F2HxL4v88g2DWelKuTn5ogKkQzR2rLM5LdPIuZk0yQQZvTZeungoGfqGPwewollyDGJHOPpr%2BD8S14k9FcBxBncrFgNNFOty9sL1ptP6PbjSvGUSQ3XRE57XmBx1niNh0r%2BT9RUaywGO7ByEmluREnfmtC5NTug51nR2Z0cO4Ryb%2FRYlh48P%2BnOJVRpK7pFy6s8Nxm9Qr4zruITBmwMnH%2BzZYtWaP3ZDS84FGR%2BM8bYiFuKHJ8HgNEbSWdUc5gNvX4y2zlqwBQp%2BoFKeAwROFpn6JEmRVjAxY2%2FaSagYdeUzoP55pJwSGcw1Yu9zwY6pgE7zYBSYfqNQKUP4nzCPp8lpzrO859HlCbPWNjIE33IQ%2Fx83axFHo%2BiYzs8u5woUOBPYjC4GpIFCWFmRoRRNKrj5xl%2F1WOfNeTKoUBwXEF9ADmTixkxKMt7OcrluutmglVO9je0%2BkBVhW%2FVs4b%2BM57m8Vudgcese2FCefPpshXV9jW1IZPGnqSwZbSYx4Vp2ezuzyl4sBZ7D2kPWw5x5c31SmSqtkM%2F&X-Amz-Signature=2e8412dde894481a48eaddd26ce19838b9b8d81b32a772d1e334cc48361f68f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
Your agent can check APIs, catch issues early, and even automatically create or update test cases. This helps avoid failures during execution and keeps workflows stable when dealing with multiple external services.
**Why it matters**
- Prevents workflows from failing due to broken APIs
- Automatically generates and runs API tests
- Supports multiple API types in one place
- Helps maintain reliability across workflows
- Reduces manual testing effort
**Who is it useful for: **Useful for any agent that depends on APIs. It is especially important for multi-step workflows, platform engineering, QA automation, and systems where reliability is critical.
**How to install it**
```plain text
npx skills add postman/agent-skills
```
## 9. Backend and Database Workflows with Supabase
The [Supabase skill](https://supabase.com/docs/guides/getting-started/ai-skills) provides access to backend capabilities like databases, authentication, storage, realtime updates, and edge functions. It also includes strong Postgres best practices, helping with query optimization, performance tuning, and handling common issues around auth, sessions, and permissions.
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/a6c495fc-3e26-47e6-9d44-05889a63ff4c/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666R6DOWMS%2F20260427%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260427T121539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICiKfM09OusEBpjwrugxtJdDCFZ5ugmuqzSTCCI5JF2uAiB3nrlkLvgUx1amsd2xI%2BchdxfRNH3V0%2FwbwQr5iw6x9CqIBAjE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ouiOS%2FVzFVS4wqeKtwD4TsAYYX86CrejYrtrks%2FIOZNkBmGl6YTXZEn53PUhnUGEgv1Y%2Fa3hO08%2B9DHzryjtkbErKR935MMs2%2BZYH4vZsxrSLXj0U9ZGlsiL3ygSF7T5bOGzVexA5V6x5aOWzlyrhAKgbxeIWbi6IS%2FJbHx2N9rgfqhHJ%2Fs2QnBTmfnEqynyWoCJ6tIbVuV%2BSAv66nWkwheTCyXgWjhZvWIP47QIQHl8mlwMrTt2Xdkh%2B2Br1vJ5gcxpIlqUbvifGsYDRY7toiBsUl4T2wNtit%2BU4A708fepZwoi8U%2Bg2H6a2mbzYt9gdyydl3b4vRC9Ck6yfOFLxSa1F2HxL4v88g2DWelKuTn5ogKkQzR2rLM5LdPIuZk0yQQZvTZeungoGfqGPwewollyDGJHOPpr%2BD8S14k9FcBxBncrFgNNFOty9sL1ptP6PbjSvGUSQ3XRE57XmBx1niNh0r%2BT9RUaywGO7ByEmluREnfmtC5NTug51nR2Z0cO4Ryb%2FRYlh48P%2BnOJVRpK7pFy6s8Nxm9Qr4zruITBmwMnH%2BzZYtWaP3ZDS84FGR%2BM8bYiFuKHJ8HgNEbSWdUc5gNvX4y2zlqwBQp%2BoFKeAwROFpn6JEmRVjAxY2%2FaSagYdeUzoP55pJwSGcw1Yu9zwY6pgE7zYBSYfqNQKUP4nzCPp8lpzrO859HlCbPWNjIE33IQ%2Fx83axFHo%2BiYzs8u5woUOBPYjC4GpIFCWFmRoRRNKrj5xl%2F1WOfNeTKoUBwXEF9ADmTixkxKMt7OcrluutmglVO9je0%2BkBVhW%2FVs4b%2BM57m8Vudgcese2FCefPpshXV9jW1IZPGnqSwZbSYx4Vp2ezuzyl4sBZ7D2kPWw5x5c31SmSqtkM%2F&X-Amz-Signature=a1ada2192f2df17213bc8c500eb82d3d6aeb65b5c217d0b245200b2474c9bf69&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
It can be used to create and manage databases, run queries, handle user authentication, and build backend logic as part of a workflow. It also guides how to write efficient SQL, avoid performance bottlenecks, and follow patterns that are suitable for production use.
**Why it matters**
- Full backend capabilities in one place
- Helps generate optimized and secure SQL
- Covers auth, storage, realtime, and edge functions
- Includes production-ready patterns
- Reduces common database mistakes
**Who it is useful for: **Useful for developers building full-stack or backend-heavy workflows, especially when working with databases, authentication, or Postgres in production.
**How to install and use it**
```plain text
npx skills add supabase/agent-skills
```
## 10. Payments and Billing with Stripe
The [Stripe skill](https://github.com/stripe/ai/blob/main/skills/stripe-best-practices/SKILL.md) helps handle payments, subscriptions, and billing workflows with the right APIs and patterns. It guides how to use modern Stripe features like Checkout Sessions, Payment Intents, and webhooks, while avoiding outdated methods. It also supports working with events, customer data, and payment states in a structured way.
![](https://prod-files-secure.s3.us-west-2.amazonaws.com/35adec70-dbef-4f42-9214-62ac8cdc4d75/61a7ed54-451b-4a00-b10d-bf2112637a85/image.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666R6DOWMS%2F20260427%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260427T121539Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICiKfM09OusEBpjwrugxtJdDCFZ5ugmuqzSTCCI5JF2uAiB3nrlkLvgUx1amsd2xI%2BchdxfRNH3V0%2FwbwQr5iw6x9CqIBAjE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ouiOS%2FVzFVS4wqeKtwD4TsAYYX86CrejYrtrks%2FIOZNkBmGl6YTXZEn53PUhnUGEgv1Y%2Fa3hO08%2B9DHzryjtkbErKR935MMs2%2BZYH4vZsxrSLXj0U9ZGlsiL3ygSF7T5bOGzVexA5V6x5aOWzlyrhAKgbxeIWbi6IS%2FJbHx2N9rgfqhHJ%2Fs2QnBTmfnEqynyWoCJ6tIbVuV%2BSAv66nWkwheTCyXgWjhZvWIP47QIQHl8mlwMrTt2Xdkh%2B2Br1vJ5gcxpIlqUbvifGsYDRY7toiBsUl4T2wNtit%2BU4A708fepZwoi8U%2Bg2H6a2mbzYt9gdyydl3b4vRC9Ck6yfOFLxSa1F2HxL4v88g2DWelKuTn5ogKkQzR2rLM5LdPIuZk0yQQZvTZeungoGfqGPwewollyDGJHOPpr%2BD8S14k9FcBxBncrFgNNFOty9sL1ptP6PbjSvGUSQ3XRE57XmBx1niNh0r%2BT9RUaywGO7ByEmluREnfmtC5NTug51nR2Z0cO4Ryb%2FRYlh48P%2BnOJVRpK7pFy6s8Nxm9Qr4zruITBmwMnH%2BzZYtWaP3ZDS84FGR%2BM8bYiFuKHJ8HgNEbSWdUc5gNvX4y2zlqwBQp%2BoFKeAwROFpn6JEmRVjAxY2%2FaSagYdeUzoP55pJwSGcw1Yu9zwY6pgE7zYBSYfqNQKUP4nzCPp8lpzrO859HlCbPWNjIE33IQ%2Fx83axFHo%2BiYzs8u5woUOBPYjC4GpIFCWFmRoRRNKrj5xl%2F1WOfNeTKoUBwXEF9ADmTixkxKMt7OcrluutmglVO9je0%2BkBVhW%2FVs4b%2BM57m8Vudgcese2FCefPpshXV9jW1IZPGnqSwZbSYx4Vp2ezuzyl4sBZ7D2kPWw5x5c31SmSqtkM%2F&X-Amz-Signature=4dd5ab1dbf46e3d3bee660f68a8f2580737d4004feb5fd75692ef5b0ab10bd76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
**Why it matters**
- Helps build reliable payment and billing workflows
- Uses up-to-date Stripe APIs and patterns
- Supports subscriptions, checkout, and webhooks
- Reduces errors in payment handling
- Works for both one-time and recurring payments
**Who it is useful for: **Useful for developers building products that involve payments, subscriptions, or billing. This includes SaaS platforms, e-commerce systems, and any workflow that needs to handle transactions or customer billing.
**How to install and use it**
```plain text
npx skills add stripe/ai--skill stripe-best-practices
```
## Conclusion
Once you start using skills like these, Antigravity stops feeling like just another tool and starts feeling like a system that can actually get things done end to end. Each skill handles a specific part of the workflow, but together they cover everything from data and infrastructure to communication, content, and payments.
You do not need all of them at once. The real value comes from picking the ones that match your workflow and slowly building around them. Over time, you end up with flows that are cleaner, more reliable, and require less manual effort.
If you are already using Antigravity, trying out even a couple of these skills can change how you approach building. It is less about wiring things together and more about letting the system handle the heavy lifting.