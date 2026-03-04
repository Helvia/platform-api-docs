# Helvia.ai Platform API Documentation

This repository hosts the **curated** OpenAPI specifications for all Helvia.ai Platform API services, published via GitHub Pages.

**GitBook** renders the interactive API reference at the organization level, using the OpenAPI specs hosted here. The GitBook space is managed entirely through the GitBook UI (no Git Sync) — specs are added by URL pointing to the GitHub Pages endpoints below.

## Hosted Specs (GitHub Pages)

| Service | Spec URL | Source |
|---------|----------|--------|
| hbf-core | [hbf-core.openapi.json](https://helvia.github.io/platform-api-docs/specs/hbf-core.openapi.json) | Live (production) |
| hbf-nlp | [hbf-nlp.openapi.json](https://helvia.github.io/platform-api-docs/specs/hbf-nlp.openapi.json) | Local export |
| hbf-session-manager | [hbf-session-manager.openapi.json](https://helvia.github.io/platform-api-docs/specs/hbf-session-manager.openapi.json) | Local export |
| helvia-rag-pipelines | [helvia-rag-pipelines.openapi.json](https://helvia.github.io/platform-api-docs/specs/helvia-rag-pipelines.openapi.json) | Local export |
| semantic-doc-segmenter | [semantic-doc-segmenter.openapi.json](https://helvia.github.io/platform-api-docs/specs/semantic-doc-segmenter.openapi.json) | Local export |

## How Specs Are Generated

Specs are fetched, curated, and published using `scripts/fetch-and-curate-specs.py` in the main platform repo:

1. **Fetch**: For services with a live production URL (e.g. hbf-core), the spec is fetched directly from the running service. For others, a local export fallback is used.
2. **Curate**: Rules in `docs/api-docs-config.yaml` control which tags/paths are included, tag ordering, and info field overrides.
3. **Publish**: Curated specs are written to `specs/` in this repo. Push to `main` and GitHub Pages updates automatically.

## Updating Specs

```bash
# In the platform repo:
make docs-api                    # Fetch, curate, and write specs
cd platform-api-docs
git add specs/ && git commit -m "Update specs" && git push
```

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
specs/                      ← Curated OpenAPI JSON specs (hosted via GitHub Pages)
api-reference/              ← Reference markdown for manual GitBook pages (copy-paste source)
index.html                  ← Quick-browse page with Swagger UI
```
