---
name: brightseed-browse-site-content
description: Browse and read Brightseed newsroom posts, knowledge-base articles and site pages through the public WordPress REST API on www.brightseedbio.com.
api: openapi/brightseed-posts-api-openapi.yml
operations:
  - listWpV2Posts
  - getWpV2PostsById
  - listWpV2Pages
  - getWpV2PagesById
  - listWpV2Categories
  - listWpV2Tags
  - listWpV2Media
---

# Browse Brightseed site content

Brightseed publishes **no product API** for Forager or Hummingbird. The only machine-readable surface
is the corporate site's WordPress REST API at `https://www.brightseedbio.com/wp-json`. Use it to read
Brightseed's own published material — newsroom announcements, knowledge-base articles, product and
platform pages — and nothing else.

## Auth

Read operations are **anonymous**: send no credentials. Writes require HTTP Basic with a WordPress
application password you will not have; do not attempt them.

## Steps

1. **List recent posts** — `listWpV2Posts` on `GET /wp/v2/posts`.
   - Use `per_page` (max 100) and `page` to walk the collection. `X-WP-Total` and `X-WP-TotalPages`
     response headers give the size; the `Link` header carries `rel="next"`.
   - Narrow with `search`, `after`, `before`, `categories`, `tags`, `orderby=date&order=desc`.
   - Keep payloads small with `_fields=id,slug,date,title,link,excerpt`.
2. **Read one post** — `getWpV2PostsById` on `GET /wp/v2/posts/{id}`. Add `_embed` to pull the author,
   featured media and terms in one call instead of following `_links` yourself.
3. **List site pages** — `listWpV2Pages` / `getWpV2PagesById`. Pages carry the durable marketing
   content: `/forager-ai/`, `/market-ready-ingredients/`, `/bio-gut-fiber/`, `/patents/`, `/team/`.
4. **Resolve taxonomy** — `listWpV2Categories` and `listWpV2Tags` turn the numeric `categories[]` and
   `tags[]` on a post into names.
5. **Resolve images** — `listWpV2Media`, or the embedded `wp:featuredmedia` when you used `_embed`.

## Rules

- `content.rendered` and `excerpt.rendered` are **HTML**. Strip or render it; never present raw markup.
- There is **no idempotency contract** and **no rate-limit header** on this API. Be conservative:
  serial requests, `per_page<=100`, and stop when `page` exceeds `X-WP-TotalPages` (WordPress returns
  `rest_post_invalid_page_number` past the end).
- Errors come back as `{"code": "...", "message": "...", "data": {"status": 4xx}}` — read `code`, not
  the prose. See `errors/brightseed-problem-types.yml`.
- `context=edit` is refused anonymously (`rest_forbidden`). Stay on the default `context=view`.
- Do not treat this content API as a source of bioactive/compound data. Forager's 21M-compound dataset
  is not exposed here or anywhere else publicly.
