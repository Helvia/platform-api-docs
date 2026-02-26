# semantic-doc-segmenter API

Document processing service. Ingests documents in various formats (PDF, DOCX, PPTX, HTML, MD, TXT), converts them to Markdown, segments them into semantic chunks, and optionally tags them using LLMs.

**Base URL (local):** `http://localhost:8082`
**Swagger UI:** [http://localhost:8082/docs](http://localhost:8082/docs)

## Authentication

Requires a Bearer JWT token.

## Key Endpoints

- **Documents** — Upload and manage documents
- **Jobs** — Track document processing jobs (ingestion, segmentation, tagging)
- **Debug** — Debugging and diagnostic endpoints

## OpenAPI Specification

{% swagger src="../specs/semantic-doc-segmenter.openapi.json" %}
[semantic-doc-segmenter.openapi.json](../specs/semantic-doc-segmenter.openapi.json)
{% endswagger %}
