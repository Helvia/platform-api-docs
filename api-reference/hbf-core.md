# hbf-core API

The core backend service for the Helvia.ai Platform. Manages organizations, users, agents (tenants), conversation flows, NLP configurations, and all platform settings.

**Base URL (local):** `http://localhost:8080`
**Swagger UI:** [http://localhost:8080/swagger-ui/index.html](http://localhost:8080/swagger-ui/index.html)

## Authentication

All endpoints require a Bearer JWT token — either a user token (from login) or a service CORE_TOKEN.

## Key Resource Groups

- **Authentication** — Login, registration, password management
- **Organizations** — Workspace management
- **Tenants** — Agent/bot configuration (the central resource)
- **Conversations** — Chat session data and history
- **NLP** — Intent and entity configuration
- **Flows** — Visual agent flow definitions
- **Knowledge Base** — Document collections for RAG
- **Analytics** — Usage metrics and reporting

## OpenAPI Specification

{% swagger src="../specs/hbf-core.openapi.json" %}
[hbf-core.openapi.json](../specs/hbf-core.openapi.json)
{% endswagger %}
