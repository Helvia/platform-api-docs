# Authentication

All platform API services use JWT Bearer token authentication. Tokens are passed in the `Authorization` header:

```
Authorization: Bearer <token>
```

## Token Types

### User JWT (Console / Admin API)

Used by the hbf-console frontend and any external client calling hbf-core APIs on behalf of a user.

**How to obtain:**

```bash
# Production
curl -X POST https://api.helvia.ai/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "you@example.com", "password": "your-password"}'

# Local development
curl -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "admin@helvia.dev", "password": "ChangeMe123!"}'
```

> **Note:** The production URL above is a placeholder. Replace with your actual deployment URL.

The response includes an `accessToken` field — use that as the Bearer token.

### Service Token (CORE_TOKEN)

Used for service-to-service communication. Services like hbf-bot, hbf-nlp, and hbf-session-manager authenticate to hbf-core using a long-lived `CORE_TOKEN`.

This token is configured in each service's environment variables and is typically a JWT signed with the same secret as hbf-core.

**Where it's used:**
- `hbf-bot` → `hbf-core` (fetching tenant configurations)
- `hbf-nlp` → `hbf-core` (fetching tenant configs and NLP settings)
- `hbf-session-manager` → `hbf-core` (session data queries)

### Pipeline JWT

Some services (helvia-rag-pipelines, semantic-doc-segmenter) use their own authentication tokens, typically validated against the same JWT secret or passed through from hbf-core.

## Local Development

For local development, the `CORE_TOKEN` is set in the platform `.env` file and shared across all services via `docker-compose.yml` environment variables.

Use `make seed` to create an admin user, then call the login endpoint to get a user JWT for testing API calls.
