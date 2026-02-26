# hbf-bot API

The chatbot executor service. Runs agent (tenant) configurations in chat channels including web-chat, Viber, Messenger, Slack, and Teams.

**Production URL:** `https://bot.helvia.ai` (placeholder — replace with actual URL)
**Local URL:** `http://localhost:3001`

hbf-bot has a minimal API surface and does not provide an OpenAPI specification. Its endpoints are documented below.

## Endpoints

### Health Check

```
GET /health
```

Returns the service health status.

**Response:** `200 OK`
```json
{
  "status": "ok"
}
```

### Reload Tenant

```
POST /reload/:tenantId
```

Reloads the configuration for a specific tenant (agent). Called by hbf-core when an agent's configuration is updated.

**Authentication:** CORE_TOKEN (Bearer)
**Path parameters:**
- `tenantId` — The tenant/agent ID to reload

**Response:** `200 OK`
