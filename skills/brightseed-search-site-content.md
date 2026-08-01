---
name: brightseed-search-site-content
description: Search Brightseed's published site content (posts and pages) by keyword through the public WordPress REST search endpoint, then hydrate the hits.
api: openapi/brightseed-search-api-openapi.yml
operations:
  - listWpV2Search
  - getWpV2PostsById
  - getWpV2PagesById
  - listWpV2Posts
---

# Search Brightseed site content

Answer a question about what Brightseed has publicly said — a clinical result, a partnership, an
ingredient, a Forager capability — by searching its own site content API.

## Auth

Anonymous. No credentials.

## Steps

1. **Search** — `listWpV2Search` on `GET /wp/v2/search`.
   - `search=<keywords>` is required to be useful; `per_page` and `page` paginate.
   - `type=post` restricts to the post-type family; `subtype=post` or `subtype=page` narrows further.
   - Each hit returns `{id, title, url, type, subtype, _links}` — a **pointer**, not the body.
2. **Hydrate** — take each hit's `id` and `subtype` and call `getWpV2PostsById`
   (`GET /wp/v2/posts/{id}`) or `getWpV2PagesById` (`GET /wp/v2/pages/{id}`) with
   `_embed&_fields=id,slug,date,link,title,content,excerpt` to get the full body.
3. **Fall back** — if `/wp/v2/search` is too coarse, `listWpV2Posts` accepts the same `search`
   parameter directly against posts and returns full objects in one round trip, with
   `orderby=relevance` available when `search` is set.
4. **Cite** — always return the `link` field (the canonical www.brightseedbio.com URL) alongside any
   quoted text, plus the post `date`.

## Rules

- Search matches title and content, not metadata; it will not find a compound name that only appears
  in a PDF or an image.
- Results are ranked by WordPress relevance, which is weak. Retrieve a few pages and re-rank yourself
  rather than trusting the first hit.
- Rendered fields are HTML; strip tags before quoting.
- No rate-limit headers are published — keep requests serial.
- The newsroom is the authoritative record of Brightseed claims; the `resources.brightseedbio.com`
  knowledge base is a separate host and is **not** covered by this API.
