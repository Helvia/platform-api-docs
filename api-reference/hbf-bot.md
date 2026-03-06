# Bot API — Usage Guide

The Bot API exposes a synchronous HTTP endpoint for interacting with AI Agents programmatically. It follows a text-in/text-out pattern — send a JSON request and receive the agent's response(s) in the same HTTP response.

**Endpoint:** `POST https://bot-v5.helvia.ai/api/events`

CORS is fully open (`origin: *`), so browser-based clients can call the API directly.

## Prerequisites

Before using the API, you need two things from the console:

1. **Agent Handle** (labeled "Identifier" in the console) — the unique identifier of the AI Agent.
2. **API Token** (labeled "API Token" in the console) — a Bearer JWT token that authorizes requests.

To find these:
1. Open the console and navigate to your agent.
2. Go to **Designer → Deployments → API**.
3. Copy the **Identifier** (this is the `handle`).
4. Copy the **API Token** (this is the Bearer token for the `Authorization` header).

## Quick Start

Replace `<API_TOKEN>` and `<AGENT_HANDLE>` with your values and run these two commands to have a full conversation:

**1. Start a session (trigger the greeting):**

```bash
curl -X POST 'https://bot-v5.helvia.ai/api/events' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <API_TOKEN>' \
  -d '{
    "handle": "<AGENT_HANDLE>",
    "message": {
      "payload": { "type": "node", "data": "greeting" }
    },
    "sender": { "id": "test-user-1" }
  }'
```

**2. Send a message:**

```bash
curl -X POST 'https://bot-v5.helvia.ai/api/events' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <API_TOKEN>' \
  -d '{
    "handle": "<AGENT_HANDLE>",
    "message": {
      "text": "Hello, I need help with time management"
    },
    "sender": { "id": "test-user-1" }
  }'
```

**Response:**

Both calls return the same response format. A single message can produce one or more agent responses — the text of each is in `altText`:

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

Errors return: `{ "status": "error", "error": "<message>" }` with an appropriate HTTP status code (400, 401, 403, or 500).

## Request Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `handle` | string | Yes | Agent deployment identifier |
| `message.text` | string | * | User's text message |
| `message.payload` | object | * | Postback trigger (see below) |
| `sender.id` | string | Recommended | Unique user ID — use the same ID across messages to maintain session context |
| `sender.name` | string | No | User display name |
| `sender.email` | string | No | User email address |
| `language` | string | No | Language code override (e.g. `"EN"`, `"EL"`, `"DE"`) |
| `tagOperations` | object | No | Add/remove session tags (see Advanced Options) |
| `options` | object | No | Response formatting options (see Advanced Options) |

\* Provide either `message.text` (text message) or `message.payload` (postback), not both.

### Postback Messages

Postback messages trigger specific flow nodes, intents, or actions instead of sending text:

| `payload.type` | `payload.data` | Use case |
|-----------------|----------------|----------|
| `"node"` | node ID (e.g. `"greeting"`) | Trigger a specific flow node |
| `"intent"` | intent name | Trigger a specific intent |
| `"action"` | `{ "actions": [...] }` | Trigger an action with parameters |

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

| Option | Effect |
|--------|--------|
| `includeMetadata: true` | Adds `responsesIds` and `nodesIds` arrays — useful for debugging or analytics |
| `includeStrippedMarkdownTextResponses: true` | Adds `responsesStrippedMarkdownText` array with plain text — useful for TTS |

### Complete Example with All Options

```bash
curl -X POST 'https://bot-v5.helvia.ai/api/events' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <API_TOKEN>' \
  -d '{
    "handle": "<AGENT_HANDLE>",
    "message": {
      "text": "Hello"
    },
    "sender": {
      "id": "user-123",
      "name": "John Doe",
      "email": "john@example.com"
    },
    "language": "EN",
    "tagOperations": {
      "add": ["vip"]
    },
    "options": {
      "includeMetadata": true,
      "includeStrippedMarkdownTextResponses": true
    }
  }'
```

For the full request/response schemas and detailed error codes, see the **Bot API Reference**.
