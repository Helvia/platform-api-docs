# Bot API (Agent Interaction)

The Bot API exposes a synchronous HTTP endpoint for interacting with AI Agents programmatically. It follows a text-in/text-out pattern — send a JSON request and receive the agent's response(s) in the same HTTP response.

**Production URL:** `https://bot-v5.helvia.ai`

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

## Usage Flow

A typical conversation follows this pattern:

1. **Trigger the greeting** — send a postback message to start a new session and get the agent's welcome message.
2. **Send a text message** — send the user's text and get the agent's response.
3. **Repeat step 2** for each turn of the conversation.

### Step 1: Trigger the Greeting

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

## Key Concepts

### Handle

The handle is the unique identifier for an AI Agent deployment. It stays the same across all requests. In the console, it's labeled as "Identifier" in the API deployment settings.

### Sender ID

The `sender.id` identifies the end user in the conversation. Use a consistent ID (e.g. a UUID) per user to maintain conversation context across messages. If you omit the `sender` object entirely, the system generates a default ID with the prefix `api_subscriber_`.

### Sessions

Sessions group message exchanges into conversations. A new session is created when:

- A message arrives from a **new `sender.id`** (first-time user).
- The **greeting is triggered** via a postback message (explicit session start).
- A message arrives from an existing user but the **inactivity timeout** has elapsed since their last message.

Within a session, the agent maintains conversational context (collected variables, flow state).

## Advanced Options

### Language Override

Force a specific language for the agent's response:

```json
{
  "handle": "...",
  "message": { "text": "Hello" },
  "sender": { "id": "user1" },
  "language": "EN"
}
```

### Tag Operations

Add or remove tags on the current session:

```json
{
  "handle": "...",
  "message": { "text": "Hello" },
  "sender": { "id": "user1" },
  "tagOperations": {
    "add": ["vip", "returning-customer"],
    "remove": ["new-user"]
  }
}
```

### Response Options

| Option | Effect |
|--------|--------|
| `includeMetadata` | Adds `responsesIds` and `nodesIds` arrays — useful for debugging or analytics |
| `includeStrippedMarkdownTextResponses` | Adds `responsesStrippedMarkdownText` array with plain text — useful for TTS |

```json
{
  "handle": "...",
  "message": { "text": "Hello" },
  "sender": { "id": "user1" },
  "options": {
    "includeMetadata": true,
    "includeStrippedMarkdownTextResponses": true
  }
}
```

## Error Responses

| Status | Error | Cause |
|--------|-------|-------|
| 400 | `Validator errors: ...` | Request body failed schema validation |
| 401 | `Unauthorized` | Missing or malformed `Authorization` header |
| 401 | `Invalid token` | JWT verification failed |
| 403 | `Unknown principal` | JWT `handle` doesn't match the request `handle` |
| 500 | `Server misconfiguration` | API auth secret not configured |

All errors return: `{ "status": "error", "error": "<message>" }`
