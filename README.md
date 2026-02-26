# Helvia.ai Platform API Documentation

Welcome to the Helvia.ai AI Agents Platform API documentation.

The platform enables AI Builders to create Conversational AI Agent solutions with LLMs and manage their full lifecycle: design, deployment, monitoring, improvement, and debugging.

## Quick Links

- **[API Reference](api-reference/overview.md)** — Interactive API documentation for all platform services
- **[Authentication](api-reference/authentication.md)** — How to obtain and use API tokens

## Updating Specs

1. In the main platform repo, run `make docs-api` to re-export all OpenAPI specs
2. Specs are automatically copied to `specs/` if this repo is cloned alongside the platform
3. Commit and push — GitHub Pages and GitBook update automatically

Or use the `/publish-api-docs` Claude Code skill which handles the full workflow.

## Local Browsing

Open `index.html` in a browser to browse all API specs with Swagger UI.
