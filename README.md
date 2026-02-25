# Helvia.ai Platform API Documentation

This repository hosts the consolidated OpenAPI specifications for all Helvia.ai Platform API services. Specs are published via GitHub Pages and consumed by GitBook.

## Hosted Specs

| Service | Spec URL |
|---------|----------|
| hbf-core | `https://helvia.github.io/platform-api-docs/specs/hbf-core.openapi.json` |
| hbf-nlp | `https://helvia.github.io/platform-api-docs/specs/hbf-nlp.openapi.json` |
| hbf-session-manager | `https://helvia.github.io/platform-api-docs/specs/hbf-session-manager.openapi.json` |
| helvia-rag-pipelines | `https://helvia.github.io/platform-api-docs/specs/helvia-rag-pipelines.openapi.json` |
| semantic-doc-segmenter | `https://helvia.github.io/platform-api-docs/specs/semantic-doc-segmenter.openapi.json` |

## How to Update

1. In the main platform repo, run `make docs-api` to re-export all OpenAPI specs
2. Copy the generated files from `docs/api/` into this repo's `specs/` directory
3. Commit and push — GitHub Pages will update automatically
4. GitBook picks up changes on next page load (specs are referenced by URL)

## Local Browsing

Open `index.html` in a browser to see links to all API specs rendered with Swagger UI.

## Structure

```
specs/
├── hbf-core.openapi.json
├── hbf-nlp.openapi.json
├── hbf-session-manager.openapi.json
├── helvia-rag-pipelines.openapi.json
└── semantic-doc-segmenter.openapi.json
index.html              ← Quick-browse page with Swagger UI
```
