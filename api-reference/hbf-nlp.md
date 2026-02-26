# hbf-nlp API

The NLP processing service. Handles LLM calls, intent classification, entity extraction, test set evaluation, and NLP pipeline execution for agents.

**Base URL (local):** `http://localhost:2055`
**Swagger UI:** [http://localhost:2055/api](http://localhost:2055/api)

## Authentication

Requires a Bearer JWT token (typically the CORE_TOKEN for service-to-service calls).

## Key Endpoints

- **NLP Processing** — Classify intents, extract entities, run NLP pipelines
- **LLM** — Execute LLM-based processing steps
- **Test Sets** — Manage and evaluate NLP test sets
- **Models** — Model configuration and management

## OpenAPI Specification

{% swagger src="../specs/hbf-nlp.openapi.json" %}
[hbf-nlp.openapi.json](../specs/hbf-nlp.openapi.json)
{% endswagger %}
