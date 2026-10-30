# How to use QuickBooks Online in Claude as a non-US user

Connecting QuickBooks Online to Claude should be simple.

Open Claude, connect QuickBooks, sign in, and start asking questions about your invoices, expenses, reports, and everything else sitting inside your books.

Except that flow currently breaks if your business is outside the US.

Intuit's official QuickBooks connector for Claude is [currently available only to US customers](https://quickbooks.intuit.com/community/other-questions-9/australia-claude-anthropic-quickbooks-connector-blocked-app-isn-t-available-for-use-in-your-country-please-give-au-date-376577).

So if you're using QuickBooks Online from somewhere else, you can end up with this slightly frustrating situation where both Claude and QuickBooks work perfectly fine on their own, but you still can't connect them. 🫠

There is another way though.

You can use [Composio](https://composio.dev/toolkits/quickbooks) to connect QuickBooks Online to Claude through its own QuickBooks integration. Composio operates differently from official MCP servers. It has its own managed OAuth app for QuickBooks, so the connection doesn't go through Intuit's official Claude connector, which is the part that's limited to US customers.

In this guide, I'll show you how to set that up.

## Why QuickBooks doesn't connect to Claude outside the US

Claude already has an official QuickBooks connector.

It can be used to work with accounting data directly from Claude, including things like reports, invoices, customers, and other QuickBooks data.

The problem isn't really the connector itself.

It's availability.

In October 2026, QuickBooks support [confirmed](https://quickbooks.intuit.com/community/other-questions-9/australia-claude-anthropic-quickbooks-connector-blocked-app-isn-t-available-for-use-in-your-country-please-give-au-date-376577) that the Claude integration was limited to US customers.

That means you can have:

- A valid QuickBooks Online subscription
- A Claude account
- Full access to your company books

...and still get blocked while connecting the two.

> 💁 **Small clarification:** This restriction is for Intuit's official Claude connector.

That's where Composio comes in.

## Connecting QuickBooks to Claude with Composio

Composio gives Claude another route to QuickBooks.

Instead of connecting QuickBooks directly through Claude's native connector, Claude talks to Composio, and Composio handles the QuickBooks connection.

The flow looks like this:

![Connection flow from Claude through Composio MCP to QuickBooks Online](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how-to-use-quickbooks-online-in-claude-as-a-non-us-user/01-quickbooks-composio-connection-flow.webp)

The [Composio QuickBooks toolkit](https://docs.composio.dev/toolkits/quickbooks) currently exposes **114 tools**.

That includes tools for things like:

- Profit and loss reports
- Invoices
- Customers
- Vendors
- Expenses
- Bills
- and many more...

Composio QuickBooks connector is managed which means Composio handles the OAuth flow, so you don't need to create your own Intuit developer app just to get started.

Cool. Let's connect it.

## Before you start

You'll need:

- A Claude account with custom connector support
- A [Composio account](https://dashboard.composio.dev/)
- A QuickBooks Online account

> ℹ️ This guide is for QuickBooks **Online**, not QuickBooks Desktop.

Also, one important thing before we start.

Using Composio gives you a different connection route, but Intuit still controls the final QuickBooks authorization. So if Intuit blocks a specific company or region during OAuth, Composio can't magically override that. 🙄

## Step 1: Connect Composio to Claude

First, let's connect Composio to Claude. This is pretty straightforward.

1. Click here: [**Connect Composio to Claude**](https://claude.ai/customize/connectors?connectorName=Composio&connectorUrl=https%3A%2F%2Fconnect.composio.dev%2Fmcp&modal=add-custom-connector).
2. This will take you directly to Claude's Custom Connector modal, with Composio's details already filled in.

![Claude Add custom connector modal with Composio details](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how-to-use-quickbooks-online-in-claude-as-a-non-us-user/02-claude-custom-connector-modal.webp)

3. Click **Continue**, then click **Add** in the next window.

Leave the **Authentication** and **OAuth Client** as is. Claude figures out the default option by itself. Click **Add**.

![Custom connector authentication and OAuth client settings](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how-to-use-quickbooks-online-in-claude-as-a-non-us-user/03-connector-authentication-settings.webp)

1. Now you'll see a list of tools exposed through the Composio MCP. You can restrict certain tools right from here, although I'd suggest leaving them as they are unless you really know what you're doing.

![Composio connector tool permissions in Claude](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how-to-use-quickbooks-online-in-claude-as-a-non-us-user/04-composio-tool-permissions.webp)

1. Return to Claude and make sure Composio is enabled.

![Composio connector enabled in Claude](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how-to-use-quickbooks-online-in-claude-as-a-non-us-user/05-composio-enabled-in-claude.webp)

And that's it. Composio is now connected to Claude. ✌️

### Quick check

I normally test the connection before doing anything first.

Ask Claude:

> What Composio tools do you have access to?

![Claude listing the Composio tools it has access to](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how-to-use-quickbooks-online-in-claude-as-a-non-us-user/06-composio-tools-check.webp)

Now, let's connect QuickBooks Online.

## Step 2: Connect your QuickBooks Online account

Now ask Claude to connect QuickBooks through Composio.

You can use something like:

> Connect my QuickBooks Online account using Composio. Give me the authorization link.

Claude should start the QuickBooks connection flow and return an authorization link.

![Claude returning a QuickBooks Online authorization link](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how-to-use-quickbooks-online-in-claude-as-a-non-us-user/07-quickbooks-authorization-link.webp)

Open it.  Stick with the default values for all the fields shown in the connection screen.

![Composio QuickBooks connection screen with default values](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how-to-use-quickbooks-online-in-claude-as-a-non-us-user/08-composio-quickbooks-connection-screen.webp)

You'll be taken to Intuit, where you can sign in to QuickBooks and approve the connection.

![Intuit screen for connecting Composio to QuickBooks Online](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/how-to-use-quickbooks-online-in-claude-as-a-non-us-user/09-intuit-quickbooks-sign-in.webp)

If you have access to multiple QuickBooks companies, choose the one you want to use with Claude.

Approve the requested permissions. Once the authorization finishes successfully, return to Claude.

Your QuickBooks account should now be available through Composio.

> ⚠️ If Intuit shows a country restriction during this step, the connection hasn't gone through. Composio uses a different integration route, but the final authorization still depends on Intuit.

## Step 3: Make sure Claude is connected to the right company

Before asking Claude to create invoices or change anything inside QuickBooks, I would test the connection with a read-only request.

Ask:

> Check my QuickBooks connection through Composio and show me the company name and currency. Don't create or update anything.

Claude should query the connected QuickBooks account and return the company details.

Check the company name and currency.

If they match the QuickBooks account you expected, the connection is working.

Now we can try something more useful.

Ask:

> Get my profit and loss report from QuickBooks for last month. Summarize total income, expenses, and net profit.

If Claude returns the report, you're pretty much done.

## Things you can do with QuickBooks inside Claude

Once QuickBooks is connected, you can start using Claude as an interface for a lot of the repetitive stuff you'd normally do manually.

For example:

**Check unpaid invoices**

> Find my unpaid invoices in QuickBooks and show the customer, outstanding amount, and due date.

**Review expenses**

> Get my expenses from QuickBooks for last month and show me the biggest categories.

**Look up customers**

> Find the customer named Acme in QuickBooks and show me their recent invoices and outstanding balance.

**Prepare an invoice**

> Prepare an invoice in QuickBooks for an existing customer. Show me everything for approval before creating it.

> 💁 QuickBooks contains real financial data. For anything that creates, edits, or deletes records, I would always ask Claude to show you the action first and wait for approval.

## That's pretty much it

The native QuickBooks connector inside Claude is currently limited to US customers.

But if you're outside the US, Composio gives you another route:

1. Connect Composio to Claude
2. Connect QuickBooks Online through Composio
3. Verify the company with a read-only request
4. Start using QuickBooks tools directly from Claude

Once it's connected, you can ask Claude to pull reports, check invoices, look up customers, review expenses, and handle other supported QuickBooks actions without jumping between tabs all day.

Pretty handy. ✌️

## FAQ

### Can I connect QuickBooks Online to Claude outside the US?

Claude's official QuickBooks connector is currently limited to US customers.

Composio provides a separate QuickBooks integration you can use with Claude, although the final OAuth authorization still depends on Intuit and the QuickBooks company you're connecting.

### Do I need to create an Intuit developer app?

No.

Composio provides managed OAuth for the QuickBooks integration, so you can connect your account without setting up your own Intuit developer application.

### Can Claude create invoices in QuickBooks?

Yes.

Composio's QuickBooks toolkit includes tools for creating and working with invoices, along with many other QuickBooks actions.

For financial write actions, I'd recommend asking Claude to show you exactly what it's about to do before executing anything.

### Is this the same as Claude's official QuickBooks connector?

No.

Claude's native QuickBooks connector is provided through Intuit.

This setup connects Claude to QuickBooks through Composio's QuickBooks toolkit instead.

You can see the available QuickBooks tools in the [Composio documentation](https://docs.composio.dev/toolkits/quickbooks).
