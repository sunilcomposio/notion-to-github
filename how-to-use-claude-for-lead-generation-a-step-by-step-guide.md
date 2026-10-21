# How to Use Claude for Lead Generation: A Step-by-Step Guide

I remember the first time I built a lead list by hand, it took me a full afternoon. I had Apollo open in one tab, LinkedIn in another, and a spreadsheet in a third, and I copied each name and email one by one. At the end of the day, I had 40 leads, and many of the emails bounced.

Then I used Zapier to automate lead generation. However, a typical Zap follows a deterministic flow: you already know what happens next. That approach isn’t enough when lead generation requires judgment, like assessing whether a company is a good fit, finding the right decision-maker, and deciding which details to verify.

Now I’ve moved the entire workflow to Claude, and with the right tools connected, it takes a few minutes. A typical lead generation workflow uses tools like Apollo, ContactOut to find prospects and fill in missing details, plus Google sheet or CRM to save the results. Claude needs access to those tools to do more than give advice.

That’s where Composio comes in: it connects Claude to the apps I use, so I can manage the workflow from one chat instead of switching between tabs.

Claude finds the companies, finds the right people, checks if they fit, and gets their contact details. I review the list, and then I decide who to contact.

With [Composio Instant](https://composio.dev/instant), Claude isn’t limited to one data provider. If Apollo doesn’t return the details you need, Claude can try ContactOut or Hunter instead. You pay as you go for the tools you use from single wallet.

In this article, I show you how to set this up with Composio Instant, Apollo, and ContactOut. I also show you the exact prompts that I use for each step.

#### TL;DR

- **Composio** connects Claude to Apollo, ContactOut, and 1,500+ other apps through one connector.
- **Composio Instant** lets Claude use Apollo search and enrichment tools without an Apollo account or API key. You pay per call from your Composio balance.
- **Apollo** finds companies and people, and it adds company data, job posts, and news that help you qualify leads.
- **ContactOut** finds decision makers, gets emails and phone numbers from LinkedIn profiles, and verifies emails.
- **Claude** combines all of this, scores each lead, and writes them to Google Sheet and prepares first draft of your outreach.

---

### What You Need

You need three things before you start:

- A Claude account on the web or desktop app
- A Composio account, with some funds in your balance for Instant tools

You do not need an Apollo or ContactOut account for search and enrichment. You only need your own Apollo API key if you want Claude to save contacts, run sequences, or send emails in Apollo.

---

### **Step 1: Add Composio to Claude (One-time process)**

1. Open the [Composio \<\> Claude ](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Composio&connectorUrl=https%3A%2F%2Fconnect.composio.dev%2Fmcp).
2. This opens Claude's custom connector dialog.

![Claude Add custom connector modal with Composio details](https://framerusercontent.com/images/DqhDXxjNfL62jAqYhSL5pgdMRds.png)

1. Click **Continue**.
2. On the next screen, click **Add**.

![Custom connector settings with the Add button](https://framerusercontent.com/images/nshgizvMzvONe8Yo8tVHhhJcaCE.png)

1. You'll be redirected to Composio's sign-in page. Log in (or create a free account) and approve the connection.

![Composio sign-in page](https://framerusercontent.com/images/LM0g5VaoRv2MKvwViFFMBbR7EwU.png)

Then:

1. Return to Claude and confirm Composio shows as enabled under Connectors.

![Claude connectors list with Composio enabled](https://framerusercontent.com/images/DjZ7n5KypBHNiDnXBaYo1cWifnY.png)

You only need to do this once, no matter how many Google accounts you connect afterward.

#### Quick check

Start a new chat and ask:

> What Composio tools do you have available?

If Claude lists Composio's tools, you're ready to connect Google Sheets.

![Claude listing the available Composio tools](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how-to-use-claude-for-lead-generation-a-step-by-step-guide/01-composio-tools-check.webp)

---

### Step 2: Understand Composio Instant Tools

This part is important, because it changes what you pay and what you need to set up.

[Composio Instant](https://composio.dev/instant) runs tools like Apollo, ContactOut, Hunter on Composio’s own provider accounts. You get an initial \$2 credit.

Claude can use those tools without your own account or API key, and Composio charges each call to your Composio balance with zero-markup at verdor prices.

Instant tools typically handle data search, retrieval, and enrichment. For Apollo, these include the following tools

| Apollo tool | What it does |
| --- | --- |
| People search | Finds people by title, company, location, and more |
| People enrichment (single and bulk) | Adds details to a person record |
| Organization search | Finds companies that match your filters |
| Organization enrichment (single and bulk) | Adds company size, industry, funding, and more |
| Organization job postings | Shows the jobs that a company has open |
| News articles | Finds recent news about a company |

![Apollo Instant tools and their per-call prices in Composio](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how-to-use-claude-for-lead-generation-a-step-by-step-guide/02-apollo-instant-tools.webp)

Other Apollo tools work with your private Apollo data, for example contacts, sequences, deals, and emails. Instant does not cover these tools, so you must connect your own Apollo account to use them.

Same for ContactOut, Hunter, Clay, People Data Labs etc.

> ⚠️ Your Composio balance does not work as a strict spending limit, because Composio checks it after the calls. So add a small amount first, and check the execution logs in the Composio dashboard to see what each call costs. You can also turn Instant off at any time, and your connected accounts keep working.

---

### Step 3: Tell Claude Who Your Ideal Customer Is

Claude gives better results when you define your ideal customer first. I keep this as a short block of text, and I paste it at the start of each lead generation chat.

```text
My ideal customer:
- B2B SaaS companies
- 50 to 200 employees
- Based in the US or Canada
- Raised a Series A or Series B in the last 18 months
- Currently hiring sales reps (SDRs or AEs)

The people I want to reach:
- Head of Sales, VP of Sales, or Chief Revenue Officer
- Founder or CEO, if the company has fewer than 80 employees
```

Change this to match your own business. The more specific you are, the fewer bad leads Claude returns.

> 💡 If you use Claude Projects, put this text in the project instructions. Then Claude uses it in each chat, and you do not have to paste it again.

---

### Step 4: Find Companies With Apollo

Now ask Claude to find companies that match your ideal customer.

```text
Use Apollo through Composio to find 25 companies that match my ideal
customer. For each company, show the name, website, employee count,
location, industry, and latest funding round. Put the results in a table.
```

Claude uses Apollo’s organization search, and it returns a table. Review the list, and remove any company that you already know is a bad fit.

![Claude returning an Apollo company search as a table](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how-to-use-claude-for-lead-generation-a-step-by-step-guide/03-apollo-company-search.webp)

---

### Step 5: Qualify the Companies

A company can match your filters and still be a bad fit. So before you look for contacts, ask Claude to check for buying signals.

Apollo gives you two useful signals: open jobs and recent news. A company that hires five SDRs this month has a clear need to grow its sales team, and a company that just raised money has the budget to buy.

```text
For each company in the table, use Apollo to check:
1. Their open job postings. Count how many sales roles they have open.
2. Recent news from the last 90 days, such as funding, launches,
   or leadership changes.

Then give each company a score from 1 to 10 with this rubric:
- +4 if they have 2 or more open sales roles
- +3 if they raised funding in the last 12 months
- +2 if they had a relevant leadership change, such as a new VP of Sales
- +1 if they launched a new product
Show the score and one line that explains it. Sort the table by score.
```

Keep the top 10 companies, and drop the rest.

You can also use ContactOut to check the size of your target group before you spend credits. ContactOut’s **Count People** tool estimates how many people match your filters, and it does not use any credits.

```text
Use ContactOut to count how many people with the title Head of Sales,
VP of Sales, or CRO work at these 10 companies. Do not reveal any
contact details yet.
```

---

### Step 6: Find the Right People

Now find the decision makers at your top companies. You can use Apollo, ContactOut, or both.

**With Apollo:**

```text
Use Apollo people search to find the Head of Sales, VP of Sales, or CRO
at each of my top 10 companies. Show their name, title, and LinkedIn URL.
```

**With ContactOut:**

```text
Use ContactOut to list the key decision makers at each of my top
10 companies. Show their name, title, and LinkedIn URL.
```

I usually run both, and then I ask Claude to merge the results and remove duplicates. Each tool finds some people that the other one misses.

---

### Step 7: Get and Verify Contact Details

This step costs the most credits, so do it only for the people that you plan to contact.

First, check which profiles have contact details. ContactOut’s **Check LinkedIn Contact Availability** tool tells you if a profile has a work email, a personal email, or a phone number, and it does not use credits.

```text
For each person in the list, use ContactOut to check which ones have
a work email available. Do not reveal the emails yet. Show me the
list with a yes or no for each person.
```

Then get the emails for the people that have them:

```text
For each person with a work email available, use ContactOut to get
their work email. Then use ContactOut's Verify Email tool to check
each one. Mark each email as valid, invalid, accept-all, or unknown.
```

If ContactOut does not find an email for someone, try Apollo’s people enrichment for that person. Then verify that email with ContactOut too.

> ⚠️ Remove emails that are invalid, and be careful with accept-all emails. If you send to too many bad addresses, your email domain can get flagged as spam.

---

### Step 8: Save Your Leads

Now save the final list. Composio connects to most CRMs and spreadsheets, so you can choose where the leads go.

For CRMs and spreadsheets you will have to connect your accounts. Here are some guides for Google sheet and Hubspot

- [How to connect Google sheet to Claude](https://composio.dev/toolkits/googlesheet/framework/claude)
- [How to connect HubSpot to Claude](https://composio.dev/toolkits/hubspot/framework/claude)

**To a Google Sheet:**

```text
Create a Google Sheet called "Leads - October 2026" and add all
verified leads. Use these columns: Name, Title, Company, Website,
Email, Email Status, LinkedIn URL, Score, Score Reason.
```

**To Apollo** (needs your own Apollo API key):

```text
Create an Apollo contact for each verified lead, and add the score
reason as a note.
```

You can also save the leads to HubSpot, Salesforce, Attio, or another CRM in the same way. Ask Claude to connect the app, and it gives you a sign-in link.

---

### Step 9: Draft the First Email

Claude already knows why each lead fits, because it saw the job posts and the news. So it can write a short email that refers to a real reason.

Connect Claude to [Gmail](https://composio.dev/toolkits/gmail/framework/claude) or [Outlook](https://composio.dev/toolkits/outlook/framework/claude).

```text
Write a short first email for each of the top 5 leads. Use the
signal that gave them the highest score, such as their open sales
roles or their recent funding. Keep each email under 100 words.
Do not send anything. Show me the drafts first.
```

Read each draft before you send it, and fix anything that sounds wrong. Claude can make mistakes about names, titles, or news, so check the facts.

> 💡 Always tell Claude “Do not send anything” in the prompt. If you want Claude to send emails, give it a clear instruction to send them after you approve each one.

---

### One Prompt for the Full Workflow

After you test each step, you can run them all in one prompt:

```text
Use Composio with Apollo and ContactOut to do this:

1. Find 25 companies that match my ideal customer.
2. Check their open sales roles and news from the last 90 days,
   and score each company from 1 to 10 with my rubric.
3. Keep the top 10.
4. Find the Head of Sales, VP of Sales, or CRO at each company.
5. Check which people have a work email available, then get and
   verify those emails with ContactOut.
6. Save the verified leads to a new Google Sheet.
7. Draft a short first email for the top 5 leads, but do not send them.

Ask me before you do any step that costs more than 50 credits.
```

---

### Keep It Legal and Polite

Lead generation tools make it easy to send a lot of emails, so be careful.

- Check the rules where your leads live. GDPR applies to people in the EU, and CAN-SPAM applies in the US.
- Only contact people who have a real reason to care about your product.
- Include a clear way to opt out in each email.
- Do not upload personal emails to a cold outreach tool. Use work emails for B2B outreach.

---

### Wrapping Up

You do not need to use every tool in this article on day one. Start with Apollo through Instant, because you do not need an Apollo account for it. Then add ContactOut when you need better email coverage and email verification.

The most useful part of this setup is the qualification step. Claude checks each company for real buying signals before you spend credits on contact details, so you pay only for the leads that you plan to contact.

If you use a different tool for lead generation with Claude, let me know.
