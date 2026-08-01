# Brightseed

Brightseed is a bioactives and AI company (founded 2017, San Francisco CA / Durham NC) whose Forager AI
platform maps compounds in plants to human biological pathways and health benefits, drawing on a
proprietary dataset of 21 million bioactive compounds across 23 health areas. Hummingbird layers an
agentic AI system on top of Forager to carry partners from discovery through development.

**Brightseed publishes no product or developer API.** Forager, Hummingbird, the Bioactive Profiler and
the Bioactive Ingredient Finder are sold as enterprise engagements arranged through a contact form —
there is no developer portal, no documentation host, no SDK, no `/.well-known/` discovery surface, no
MCP server and no A2A agent card. The single machine-readable surface the company operates is the
**WordPress REST API (`wp/v2`)** behind its corporate site at `www.brightseedbio.com`.

## What is in this repo

| Artifact | What it covers |
|---|---|
| `openapi/` | 11 OpenAPI 3.1.0 documents, 84 operations, derived from the route metadata the server itself publishes at `https://www.brightseedbio.com/wp-json/wp/v2` |
| `overlays/` | One OpenAPI Overlay 1.0.0 per spec carrying the API Evangelist annotations |
| `authentication/` | HTTP Basic via WordPress application passwords |
| `conventions/` | Pagination (`page`/`per_page`, `X-WP-Total`), sparse fields (`_fields`), embedding (`_embed`), error envelope, CORS — no idempotency contract, no rate-limit signalling |
| `errors/` | The WordPress error envelope `{code, message, data.status}` (not RFC 9457) |
| `data-model/` | Entity graph for posts, pages, media, terms, users and comments |
| `lifecycle/` | `wp/v2` namespace versioning; no status page, deprecation policy, SLA or changelog |
| `conformance/` | Standards asserted and denied, each with evidence |
| `well-known/` | Observed absence — every `/.well-known/` path returns 404 on both hosts |
| `security/` | TLS 1.3, HSTS preload, SPF, DMARC `p=reject`; no DNSSEC, no CAA, no security.txt, no VDP, no trust center |
| `agentic-access/` | Recommended `x-agentic-access` contracts for all 84 operations |
| `mcp/` | A **candidate** tool surface derived from the anonymous read operations — Brightseed operates no MCP server |
| `skills/` | Two packaged agent skills grounded in real `operationId`s |
| `llms/` | `llms.txt` generated from this catalog |

Anonymous read access was verified on 2026-07-31 for posts, pages, media, categories, tags, users,
comments, search, taxonomies, types and statuses. `settings`, `themes`, `plugins` and `menu-items`
return 401.

- https://www.brightseedbio.com/
- https://www.brightseedbio.com/forager-ai/
- https://forgeglobal.com/brightseed_stock/
