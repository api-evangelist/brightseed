# Brightseed

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
