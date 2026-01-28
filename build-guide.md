## Overview

This blog is your guide to building an AI agent from the ground up. We'll use Next.js to create the full application, from the front end to the agent backend, and power the agent with all the tools from different toolkits, as we call it (Slack, Gmail, GitHub... all you can think of) using Composio. 🔥

You'll learn how to set up the project, build a clean chat UI, and then bring it to life with an agent that can think and respond. We'll also cover how to manage chat state, implement streaming for real-time interactions, and keep track of different conversations.

![Image 1](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/build/image_1.png)

Think of it this way: you're building an agent that can perform actions across many platforms. For example, it could be:

- An agent for[ Slack](https://composio.dev/toolkits/slack)

- An agent for[ GitHub](https://composio.dev/toolkits/github)

- An agent for [Gmail](https://composio.dev/toolkits/gmail)

- An agent for [Notion](https://composio.dev/toolkits/notion)

- ...and over[ 877* others](https://composio.dev/toolkits), all combined in one.

By the end, you'll have a working AI agent that can use various toolkits to help you with pretty much everything, not just talk.

Here's a quick demo of the agent in action:

---

## Prerequisites

Before we start, make sure you have a few things set up. Don't worry, nothing too complicated! 😉

- **Node.js**: You'll need Node.js installed on your machine to run the Next.js project. You can use npm as the package manager or choose bun, yarn, whatever suits you.

- **Basic understanding of React and Next.js**: We'll walk you through everything, but it helps if you're somewhat familiar with React and how Next.js handles routing.

- **Composio Account & API Key**: This is super important! You'll need to sign up for a Composio account and grab your API key. This key is what lets your agent connect to all those awesome toolkits. You can sign up for an account [here for free](https://platform.composio.dev/auth).

- **An LLM API Key (e.g., OpenAI, Anthropic, or others)**: Your AI agent needs a brain! You'll need an API key from either OpenAI, Anthropic, or whatever you prefer. For this, I'll be using OpenAI Agents SDK, so steps can change a little if you choose any other.

That's pretty much all you need to have ready.

---

## Step 1: Setting up the Next.js Project

Alright, let's get started by getting our project up and running. We'll need to install a few dependencies (mainly OpenAI Agents SDK and Composio) right from the start.

### 1.1 Create a Next.js Application

First, open up a terminal and run the following command to spin up a new Next.js app. For the demo, we'll name it `all-in-one-agent` (or whatever you prefer).

```bash
npx create-next-app@latest all-in-one-agent \
    --typescript --tailwind --eslint --app --src-dir \
    --import-alias "@/*"

```

If you are asked any prompts, simply stick to the defaults.

Next, we'll add the necessary packages for AI, chat functionality, and Composio integration. Open your `package.json` file and add the following dependencies:

```json
{
  "name": "all-in-one-agent",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint"
  },
  "dependencies": {
    "@composio/core": "^0.5.4",
    "@openai/agents": "^0.4.2",
    "clsx": "^2.1.1",
    "dotenv": "^17.2.3",
    "lucide-react": "^0.563.0",
    "next": "16.1.4",
    "react": "19.2.3",
    "react-dom": "19.2.3",
    "tailwind-merge": "^3.4.0",
    "zod": "^4.3.6"
  },
  "devDependencies": {
    "@tailwindcss/postcss": "^4",
    "@types/node": "^20",
    "@types/react": "^19",
    "@types/react-dom": "^19",
    "eslint": "^9",
    "eslint-config-next": "16.1.4",
    "tailwindcss": "^4",
    "typescript": "^5"
  }
}

```

After updating `package.json`, run the installation command from your terminal to fetch the packages.

```bash
npm install

```

This completes the initial setup. Your project now has the basic structure and all the necessary dependencies to work with.

---

## Step 2: Building the Chat Interface

Let's start with the UI. But, wait? Wouldn't that be a lot to include in the blog here?

Absolutely! Explaining just JSX doesn't make sense and makes the blog unnecessarily long. If you know frontend, that's cool; there's no rocket science, it's basic React. If not, you can check out the code [here](https://github.com/shricodev/all-in-one-agent-composio).

> And frontend in big 2026 feels a relic of the past, Just kidding, I am bad at it so, I let Opus handle it and as you can say from the purple gradients. Didn’t want to put more effort here.

This won't be a tutorial overload, but it will give you a general idea of what each component does. I'll share and explain the code, covering just what you need to know.

---

## Step 3: Creating the Agent Backend

Now, we're going to handle the agent core logic, which is responsible for understanding user requests, orchestrating tool usage, and streaming the response to the client.

### 3.1 The Chat API Route

The `app/api/chat/route.ts` file is the entry point for all chat interactions. It initiates a Server-Sent Events (SSE) stream to provide real-time updates from the agent to the client.

```typescript
// 👇 api/chat/route.ts
import { NextRequest } from "next/server";
import { runAgentWithEvents } from "@/lib/agent";
import { createSSEMessage } from "@/lib/utils";
import type { AgentEvent } from "@/lib/types";

// SSE endpoint for chat with streaming agent events
export async function POST(request: NextRequest) {
  try {
    const body = await request.json();
    const userId = body.userId || process.env.DEFAULT_USER_ID;
    const message = body.message;
    const toolkits: string[] = Array.isArray(body.toolkits)
      ? body.toolkits
      : [];

    // ... input validation ...

    // Create a TransformStream for SSE
    const encoder = new TextEncoder();
    const stream = new TransformStream();
    const writer = stream.writable.getWriter();

    const emitEvent = async (event: AgentEvent) => {
      // ... writes SSE messages to the stream ...
    };

    // Run agent in background and stream events
    (async () => {
      try {
        await runAgentWithEvents(userId, message, emitEvent, toolkits);
      } catch (error) {
        // ... error handling ...
      } finally {
        await writer.close();
      }
    })();

    return new Response(stream.readable, {
      headers: { "Content-Type": "text/event-stream" /* ... */ },
    });
  } catch (error) {
    // ... error handling ...
  }
}

```

Now comes the fun part. This route creates an SSE connection. The key action happens in the self-invoking async function, which calls `runAgentWithEvents(userId, message, emitEvent, toolkits)`.

This passes the user's message to the core agent logic and uses the `emitEvent` callback to stream `AgentEvent` updates back to the client.

> But what are these toolkits that we're passing to the `runAgentWithEvents` function? They're from Composio. I'll explain how everything is wired up with Composio in the next section.

### 3.2 The Core Agent Logic

The heart of our AI agent resides in `lib/agent.ts`. This file configures the agent's personality and instructions and contains the main function for running it.

```typescript
// 👇 lib/agent.ts
export async function runAgentWithEvents(
  userId: string,
  userMessage: string,
  emitEvent: EventEmitter,
  toolkits?: string[],
): Promise<string> {
  // ... event emission for "connected" ...

  const agent = await createAgentForUser(userId, toolkits);
  const memory = getOrCreateMemorySession(userId);

  // ... event emission for "message_start" ...

  const streamResult = await run(agent, userMessage, {
    session: memory,
    stream: true,
  });

  let streamedContent = "";

  for await (const event of streamResult) {
    if (event.type !== "raw_model_stream_event") continue;

    const rawEvent = event.data as {
      type: string;
      delta?: string;
    };

    if (
      rawEvent.type === "output_text_delta" &&
      typeof rawEvent.delta === "string"
    ) {
      streamedContent += rawEvent.delta;

      emitEvent({
        type: "message_delta",
        // ... event data ...
      });
    }
  }

  // ... event emission for "message_complete", "auth_required", "done", and error handling ...

  return streamedContent || streamResult.finalOutput;
}

```

The main function, `runAgentWithEvents`, uses the run function from `@openai/agents` to process the user's message. It then iterates through the `streamResult` to emit `message_delta` events, allowing the frontend to display the AI's response as it's being generated.

### 3.3 Agent Event Types

To ensure type-safe communication between the backend and frontend, `lib/types.ts` defines all the types for all streamed events.

```typescript
// 👇 lib/types.ts
// Agent event types for SSE streaming
export type AgentEventType =
  | "connected"
  | "activity_start"
  | "activity_update"
  | "activity_complete"
  | "message_start"
  | "message_delta"
  | "message_complete"
  | "tool_call_start"
  | "tool_call_complete"
  | "auth_required"
  | "error"
  | "done";

export interface AgentEvent {
  type: AgentEventType;
  timestamp: string;
  data: AgentEventData;
}

```

All of this basically helps the UI react to specific states, like when the agent is thinking (`activity_start`), using a tool (`tool_call_start`), or sending parts of a message (`message_delta`).

---

## Step 4: Integrating and Using Composio Toolkits

This is where our LLM becomes an AI agent. Composio allows the agent to access and use over 10,000 tools from more than 500 different toolkits (like Slack, GitHub, Gmail, etc.) to perform real-world tasks. The connection is established in two key places: fetching connection details and then equipping the agent with those tools.

### 4.1 Establishing the Connection to Composio

The `lib/composio.ts` file is responsible for communicating with the Composio backend. It securely initializes the Composio client and fetches the necessary connection details for a specific user, which are then passed to the agent.

It's that simple.

```typescript
// 👇 lib/composio.ts
import { Agent, hostedMcpTool } from "@openai/agents";
import { getMcpConnectionDetails } from "./composio";

// ... other code ...

// Create agent for a user with optional toolkit filtering
export async function createAgentForUser(
  userId: string,
  toolkits?: string[],
): Promise<Agent> {
  // 1. Fetch Composio connection details
  const mcpDetails = await getMcpConnectionDetails(userId, toolkits);

  const agent = new Agent({
    name: "Codev Agent",
    instructions: SYSTEM_INSTRUCTIONS,
    model: MODEL,
    // 2. Give Composio tool router access to the agent
    tools: [
      hostedMcpTool({
        serverLabel: "composio",
        serverUrl: mcpDetails.url,
        headers: mcpDetails.headers,
      }),
    ],
  });

  return agent;
}

```

The `getMcpConnectionDetails` function is the core of this file. It uses `composio.sessions.getMcpDetails` from the Composio SDK to get a unique URL for that user. This essentially ensures that the agent actions are closely tied to the correct user.

### 4.2 Equipping the Agent with Tools

Now, with the connection details fetched, we can pass them to the agent in `lib/agent.ts`. Then the agent gets configured with the `hostedMCPTool`.

```typescript
// 👇 lib/composio.ts
import { Agent, hostedMcpTool } from "@openai/agents";
import { getMcpConnectionDetails } from "./composio";

// ... other code ...

// Create agent for a user with optional toolkit filtering
export async function createAgentForUser(
  userId: string,
  toolkits?: string[],
): Promise<Agent> {
  // 1. Fetch Composio connection details
  const mcpDetails = await getMcpConnectionDetails(userId, toolkits);

  const agent = new Agent({
    name: "Codev Agent",
    instructions: SYSTEM_INSTRUCTIONS,
    model: MODEL,
    // 2. Give Composio tool router access to the agent
    tools: [
      hostedMcpTool({
        serverLabel: "composio",
        serverUrl: mcpDetails.url,
        headers: mcpDetails.headers,
      }),
    ],
  });

  return agent;
}

```

Long story short, here's the two-step process in action:

1. We first call `getMcpConnectionDetails(userId, toolkits)` to get the secure URL and headers from Composio.

1. We then instantiate the `hostedMcpTool` from the `@openai/agents` SDK. The `serverUrl` and `headers` from `mcpDetails` are passed directly into its configuration.

Something this simple gives our agent access to all the toolkits available in the Composio Tool Router. Now, the agent can intelligently query the tools and perform any requested actions from any of the connected toolkits.

---

## Step 5: Managing State and Conversations

We've already laid the groundwork for 90% of our AI Agent. Now, let's just add a few quick enhancements to make it even better, such as managing conversation history and user sessions.

### 5.1 Managing Conversations on the Frontend

The `hooks/use-conversation.ts` is a simple React hook that handles the client-side management of the conversation history and integrates with local storage.

```typescript
// 👇 hooks/use-conversation.ts
import { useState, useEffect, useCallback } from "react";
import { Conversation, ChatMessage } from "@/lib/types";
import { generateId } from "@/lib/utils";

// ... LOCAL_STORAGE_KEY constant ...

export function useConversations() {
  const [conversations, setConversations] = useState<Conversation[]>([]);
  const [activeConversationId, setActiveConversationId] = useState<
    string | null
  >(null);

  // ... useEffect to load from local storage ...

  const createConversation = useCallback((initialMessage?: ChatMessage) => {
    // ... logic to create a new conversation ...
  }, []);

  const addMessageToConversation = useCallback(
    (conversationId: string, message: ChatMessage) => {
      // ... logic to add a message to a specific conversation ...
    },
    [],
  );

  // ... other functions like deleteConversation, updateConversationTitle ...

  return {
    conversations,
    activeConversationId,
    setActiveConversationId,
    activeConversation: activeConversationId
      ? conversations.find((conv) => conv.id === activeConversationId)
      : undefined,
    createConversation,
    addMessageToConversation,
    // ... other returned functions ...
  };
}

```

This is a simple hook that you can find almost anywhere on the internet, which you can use to work with local storage just tweaked for our usecase.

This hook gives a centralized way to manage conversations. It uses `useState` to track all conversations and the `activeConversationId`. It also uses `useEffect` to load and save conversations to `localStorage`, making sure that chat history is preserved across sessions.

### 5.3 Storing User Session Information

The `app/api/session/route.ts` handles the backend logic for user sessions, ensuring all the information remains persisted.

```typescript
// 👇 api/session/route.ts
import { NextRequest, NextResponse } from "next/server";
import { getDb } from "@/lib/db";
import { Composio } from "composio-client-sdk";
import type { UserSession } from "@/lib/types";

// ... Composio client initialization ...

export async function GET(request: NextRequest) {
  // ... authentication ...

  const db = await getDb();
  let session = await db
    .collection<UserSession>("sessions")
    .findOne({ userId });

  if (!session) {
    // Create new session if none exists
    const composioSession = await composio.sessions.createSession({ userId });
    session = {
      userId,
      composioSessionId: composioSession.id,
      connectedToolkits: [],
      onboardingCompleted: false,
      createdAt: new Date(),
      updatedAt: new Date(),
    };
    await db.collection("sessions").insertOne(session);
  } else if (!session.composioSessionId) {
    // Update existing session if Composio session ID is missing
    const composioSession = await composio.sessions.createSession({ userId });
    session.composioSessionId = composioSession.id;
    await db
      .collection("sessions")
      .updateOne(
        { userId },
        { $set: { composioSessionId: session.composioSessionId } },
      );
  }

  return NextResponse.json({ success: true, data: session });
}

```

The entire function of this route is to ensure that a UserSession is created or retrieved for every user. If a session does not exist, it creates one with `composio.sessions.createSession` and stores it.

This pretty much concludes the application logic.

You can always find the entire source code for the project here: [shricodev/all-in-one-agent](https://github.com/shricodev/all-in-one-agent.git)

---

## Running the Application

Now that you know how the logic works under the hood, let me show you how to run the application.

### Set Up Environment Variables

Ensure you have created a .env.local file at the root of your project with the following API keys:

- `OPENAI_API_KEY`: Your API key for OpenAI (or your chosen LLM provider).

- `COMPOSIO_API_KEY`: Your API key for Composio.

- `COMPOSIO_USER_ID`: Your User ID for Composio.

### Start the Development Server

Open your terminal, navigate to your project directory, and run the following command:

```bash
npm run dev

```

This will start the Next.js development server at `localhost:3000`.

### Access the Application

Once the server is running, open your web browser and navigate to: `http://localhost:3000`

You should now see your AI agent chat interface. Start experimenting, and you'll realize how powerful this agent can be in your daily workflow. 🔥

---

## Conclusion

Awesome! By now, you should have a good idea of how to build a streaming AI agent with Next.js and how to hook it up with tons of integrations using Composio. You've gone through setting up the frontend, getting the backend agent working, and connecting it to all sorts of cool tools.

![Image 2](https://raw.githubusercontent.com/sunilcomposio/notion-to-github/main/images/build/image_2.png)

This is really just the beginning of what you can do! Try adding more features and even more integration support to your bot to make it fit your exact needs. It can seriously come in handy.

If you got a bit stuck while coding along, you can find the source code here: [shricodev/all-in-one-agent](https://github.com/shricodev/all-in-one-agent).

So, that's it for this blog. Thanks a bunch for reading! See you next time. 🫡

Love to build cool stuff like this? I regularly build such stuff every few weeks. Feel free to reach out to me here:

- **GitHub**: [github.com/shricodev](http://github.com/shricodev)

- **Portfolio**: [techwithshrijal.com](http://techwithshrijal.com/)

- **LinkedIn**: [linkedin.com/in/iamshrijal](http://linkedin.com/in/iamshrijal)
