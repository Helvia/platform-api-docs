# Helvia.ai Platform API Documentation

This repository hosts the OpenAPI specifications for all Helvia.ai Platform API services, published via GitHub Pages.

**GitBook** renders the interactive API reference at the organization level, using the OpenAPI specs hosted here. The GitBook space is managed entirely through the GitBook UI (no Git Sync) — specs are added by URL pointing to the GitHub Pages endpoints below.

## Hosted Specs (GitHub Pages)

| Service | Spec URL |
|---------|----------|
| hbf-core | [hbf-core.openapi.json](https://helvia.github.io/platform-api-docs/specs/hbf-core.openapi.json) |
| hbf-nlp | [hbf-nlp.openapi.json](https://helvia.github.io/platform-api-docs/specs/hbf-nlp.openapi.json) |
| hbf-session-manager | [hbf-session-manager.openapi.json](https://helvia.github.io/platform-api-docs/specs/hbf-session-manager.openapi.json) |
| helvia-rag-pipelines | [helvia-rag-pipelines.openapi.json](https://helvia.github.io/platform-api-docs/specs/helvia-rag-pipelines.openapi.json) |
| semantic-doc-segmenter | [semantic-doc-segmenter.openapi.json](https://helvia.github.io/platform-api-docs/specs/semantic-doc-segmenter.openapi.json) |

## Updating Specs

1. In the main platform repo, run `make docs-api` to re-export all OpenAPI specs
2. Specs are automatically copied to `specs/` if this repo is cloned alongside the platform
3. Commit and push — GitHub Pages updates automatically, GitBook refreshes specs every 6 hours

Or use the `/publish-api-docs` Claude Code skill which handles the full workflow.

## GitBook Setup

The GitBook space is **UI-managed** (not using Git Sync). To set up or modify:

1. Add each spec to the GitBook organization via **Organization → OpenAPI → Add specification** using the GitHub Pages URLs above
2. In the space, use **"Add new…" → "OpenAPI Reference"** to generate interactive API pages from each spec
3. Manual pages (Overview, Authentication, hbf-bot) are created directly in the GitBook editor — source content is in `api-reference/` for reference

## Local Browsing

Open `index.html` in a browser to browse all API specs with Swagger UI.

## Structure

```
specs/                      ← OpenAPI JSON specs (hosted via GitHub Pages)
api-reference/              ← Reference markdown for manual GitBook pages (copy-paste source)
index.html                  ← Quick-browse page with Swagger UI
```
