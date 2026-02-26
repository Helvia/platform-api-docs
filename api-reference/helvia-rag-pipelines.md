# helvia-rag-pipelines API

The RAG (Retrieval Augmented Generation) service. Manages document collections, generates embeddings, and performs vector-based semantic search. Used by modern agents for knowledge retrieval.

**Base URL (local):** `http://localhost:8081`
**Swagger UI:** [http://localhost:8081/docs](http://localhost:8081/docs)

## Authentication

Requires a Bearer JWT token.

## Key Endpoints

- **Pipelines** — Manage RAG pipelines and their configurations
- **Admin** — Administrative operations (collection management, health checks)
- **Search** — Semantic vector-based search across document collections

## OpenAPI Specification

{% swagger src="../specs/helvia-rag-pipelines.openapi.json" %}
[helvia-rag-pipelines.openapi.json](../specs/helvia-rag-pipelines.openapi.json)
{% endswagger %}
