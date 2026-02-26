# API Reference Overview

The Helvia.ai Platform exposes REST APIs through multiple services. Each service handles a specific domain of the platform's functionality.

## Services

| Service | Description | Production URL | Local URL |
|---------|-------------|----------------|-----------|
| **hbf-core** | Core backend API — manages organizations, agents (tenants), users, and all platform configuration | `https://api.helvia.ai` | `http://localhost:8080` |
| **hbf-nlp** | NLP processing service — handles LLM calls, intent classification, and NLP pipeline execution | `https://nlp.helvia.ai` | `http://localhost:2055` |
| **hbf-session-manager** | Session lifecycle management — tracks conversation sessions and triggers post-completion actions | `https://sessions.helvia.ai` | `http://localhost:3002` |
| **helvia-rag-pipelines** | RAG and semantic search — manages document collections, embeddings, and vector-based retrieval | `https://rag.helvia.ai` | `http://localhost:8081` |
| **semantic-doc-segmenter** | Document processing — ingests documents (PDF, DOCX, etc.), segments them into chunks, and tags them | `https://segmenter.helvia.ai` | `http://localhost:8082` |
| **hbf-bot** | Chatbot executor — runs agent configurations in chat channels (minimal API, no OpenAPI spec) | `https://bot.helvia.ai` | `http://localhost:3001` |

> **Note:** Production URLs above are placeholders. Replace with your actual deployment URLs.

## Common Patterns

All API services follow these conventions:

- **Content-Type**: `application/json` for request and response bodies
- **Authentication**: Bearer token via the `Authorization` header (see [Authentication](authentication.md))
- **Error responses**: Standard HTTP status codes with JSON error bodies
- **CORS**: Enabled for all origins in local development

## OpenAPI Specs

Each service (except hbf-bot) provides an OpenAPI specification. The specs are available:

- **At runtime** via each service's Swagger UI endpoint (see table above)
- **As static files** in the [`specs/`](../specs/) directory of this repo
