# Bot API — Usage Guide

The Bot API exposes a synchronous HTTP endpoint for interacting with AI Agents programmatically. It follows a text-in/text-out pattern — send a JSON request and receive the agent's response(s) in the same HTTP response.

**Production URL:** `https://bot-v5.helvia.ai`

CORS is fully open (`origin: *`), so browser-based clients can call the API directly.

For the full request/response schemas, authentication details, and error codes, see the **Bot API Reference**.

## Prerequisites

Before using the API, you need two things from the console:

1. **Agent Handle** (labeled "Identifier" in the console) — the unique identifier of the AI Agent.
2. **API Token** (labeled "API Token" in the console) — a Bearer JWT token that authorizes requests.

To find these:
1. Open the console and navigate to your agent.
2. Go to **Designer → Deployments → API**.
3. Copy the **Identifier** (this is the `handle`).
4. Copy the **API Token** (this is the Bearer token for the `Authorization` header).

## Usage Flow

All interactions go through a single endpoint: `POST /api/events`. A typical conversation follows this pattern:

1. **Trigger the greeting** — send a postback message to start a new session and get the agent's welcome message.
2. **Send a text message** — send the user's text and get the agent's response.
3. **Repeat step 2** for each turn of the conversation.

### Step 1: Trigger the Greeting

This starts a new conversation (session) and returns the agent's opening message.

```bash
curl -X POST 'https://bot-v5.helvia.ai/api/events' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <API_TOKEN>' \
  -d '{
    "handle": "<AGENT_HANDLE>",
    "message": {
      "payload": {
        "type": "node",
        "data": "greeting"
      }
    },
    "sender": {
      "id": "<UNIQUE_USER_ID>"
    }
  }'
```

### Step 2: Send a Text Message

```bash
curl -X POST 'https://bot-v5.helvia.ai/api/events' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <API_TOKEN>' \
  -d '{
    "handle": "<AGENT_HANDLE>",
    "message": {
      "text": "Hello, I need help with time management"
    },
    "sender": {
      "id": "<UNIQUE_USER_ID>"
    }
  }'
```

### Sample Response

A single user message can produce one or more agent responses. The text of each response is in `result.responses[N].altText`.

```json
{
  "status": "ok",
  "result": {
    "responses": [
      {
        "id": "TX_b8eb307f-cf33-48fb-bc51-a0e7e6a7d817",
        "type": "text",
        "options": ["Hi! I'm the Productivity Coach."],
        "altText": "Hi! I'm the Productivity Coach."
      },
      {
        "id": "TX_1bfda168-dd49-4dc0-b9fc-56ce9cb5d141",
        "type": "text",
        "options": ["How can I help you today?"],
        "altText": "How can I help you today?"
      }
    ]
  }
}
```

See the **Bot API Reference** for the full response schema, including optional metadata and stripped markdown fields.

## Key Concepts

### Handle

The handle is the unique identifier for an AI Agent deployment. It stays the same across all requests. In the console, it's labeled as "Identifier" in the API deployment settings.

### Sender ID

The `sender.id` identifies the end user in the conversation. Use a consistent ID (e.g. a UUID) per user to maintain conversation context across messages. If you omit the `sender` object entirely, the system generates a default ID with the prefix `api_subscriber_`.

### Sessions

Sessions group message exchanges into conversations. A new session is created when:

- A message arrives from a **new `sender.id`** (first-time user).
- The **greeting is triggered** via a postback message (explicit session start).
- A message arrives from an existing user but the **inactivity timeout** has elapsed since their last message. The timeout is configurable in the console under **Designer → Settings → Configuration**.

Within a session, the agent maintains conversational context (collected variables, flow state).

## Advanced Options

The `POST /api/events` endpoint supports several optional fields for advanced use cases. See the **Bot API Reference** for the full schema.

### Language Override

Force a specific language for the agent's response by including `"language": "EN"` (or `"EL"`, `"DE"`, etc.) in the request body.

### Tag Operations

Add or remove tags on the current session using the `tagOperations` field:

```json
"tagOperations": {
  "add": ["vip", "returning-customer"],
  "remove": ["new-user"]
}
```

### Response Options

Include additional data in the response using the `options` field:

- `includeMetadata: true` — adds `responsesIds` and `nodesIds` arrays, useful for debugging or analytics.
- `includeStrippedMarkdownTextResponses: true` — adds `responsesStrippedMarkdownText` array with plain text, useful for TTS or plain-text displays.
