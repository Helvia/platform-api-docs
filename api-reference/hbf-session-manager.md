# hbf-session-manager API

Manages the lifecycle of conversation sessions. Monitors active sessions, marks them as complete after inactivity, and triggers post-completion activities.

**Base URL (local):** `http://localhost:3002`
**Swagger UI:** [http://localhost:3002/api](http://localhost:3002/api) (non-production only)

## Authentication

Requires a Bearer JWT token (typically the CORE_TOKEN for service-to-service calls).

## Key Endpoints

- **Sessions** — Query and manage conversation session state
- **NLP Pipelines** — Pipeline integration checks and status

## OpenAPI Specification

{% swagger src="../specs/hbf-session-manager.openapi.json" %}
[hbf-session-manager.openapi.json](../specs/hbf-session-manager.openapi.json)
{% endswagger %}
