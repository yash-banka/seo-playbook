# 01 — Architecture: tiers, index control, sitemaps

## The tiered surface

Sort every page into a tier by **intent**, and let the tier drive its template,
its internal-link role, and its sitemap priority.

The reference implementation's three tiers:

| Tier | Pattern | Share of surface | Priority | Intent | Job |
|---|---|---|---|---|---|
| **Money** | `/convert/{a}-to-{b}` | ~¾ — by far the largest | 0.8 | Transactional | Convert the visitor |
| **Tool** | `/tools/{verb-noun}` | small | 0.7 | Mid-tail head terms | Capture broader queries |
| **Entity** | `/formats/{class}/{x}` | small | 0.6 | Informational | Build topical authority |

Note what is counter-intuitive: **the long-tail money pages carry the highest
priority**, above the head-term tool pages. The long tail is treated as the
primary asset, not the leftovers. That is correct when each long-tail page has
low competition and high intent — collectively they beat the head term you
probably can't win anyway.

Entity pages exist to demonstrate subject knowledge across the whole domain and
to funnel equity down into money pages. They rarely convert directly. Judge them
on the authority and internal links they contribute, not their own conversion.

### Sitemap priority — a caveat on the table above

**Google states plainly that it ignores `<priority>` and `<changefreq>`.**
(`07-sources.md`) So the priority column above documents the *reference
implementation's own intent*; it does not influence Google.

Keep setting it if you like the self-documentation, but understand that a tier's
**real** expression is its template, its content depth, and its internal links.
If your tiering exists only in the sitemap, it does not exist.

---

## Index tiering — the load-bearing technique

**Every page gets one of three fates.** This is the discipline that separates a
large defensible surface from a spam classification.

### Fate 1 — Indexed
Real demand. In the sitemap. Indexable. Gets genuinely bespoke content.

### Fate 2 — `noindex, follow`
Built, working, internally linked — **and deliberately kept out of the index.**

```html
<meta name="robots" content="noindex, follow">
```

Also: excluded from the sitemap (a sitemap is a list of pages you *want*
indexed; listing a noindexed URL is a contradictory signal).

This is how the product stays complete while the index stays clean. In the
reference implementation, obscure conversion targets resolve, work perfectly,
and are linked from sibling navigation — but carry `noindex, follow` and are
absent from the sitemap. A user who needs that conversion gets it. The index
never sees it.

Why `follow` and not `noindex, nofollow`: you still want link equity to pass
*through* these pages to the indexed ones they link to. `nofollow` would strand
it.

**When to use it**
- Product-complete but zero search demand
- Faceted / filtered / sorted views
- Paginated pages beyond the first, where the hub already exposes the items
- Internal search results
- Thin-by-nature utility pages (thank-you, confirmation, preview)
- Anything you'd be embarrassed to see ranked

**Caveat worth knowing:** Google has stated that a long-lived `noindex` page
eventually gets treated as though its links were `nofollow`, because it
recrawls such pages less often. Don't route *critical* equity paths exclusively
through noindexed pages — hubs must be indexable.

### Fate 3 — Don't build
No demand, no product need. The URL must return a **true HTTP 404** with an
error page. Never a 200 with "not found" text — that is a soft 404, and at scale
it teaches crawlers that your generated URL space is unbounded, wasting crawl
budget on pages you never wanted.

Verify: `curl -o /dev/null -w "%{http_code}" https://example.com/nonsense`

---

## Canonicals

- **Self-referencing canonical on every indexable page.** Cheap, and it defends
  against parameter and duplicate variants you didn't anticipate.
- Canonical to the **clean** URL — no tracking parameters, no session IDs.
- Absolute URLs. One canonical per page. Never chain them.
- A canonical is a *hint*, not a directive. Reinforce it with consistent
  internal linking to the canonical form. Don't canonicalise pages that aren't
  genuine duplicates — for near-duplicates that must both exist, differentiate
  them or noindex one instead.
- Directional pairs (A→B and B→A) are **not** duplicates. Both self-canonicalise
  and cross-link.

## Sitemaps

- List **only** indexable, canonical, 200-status URLs. No noindexed, no
  redirected, no 404, no non-canonical.
- `<lastmod>` must be truthful. A file where every URL claims today's date
  carries no information and is discounted. If you can't track real modification
  dates, omit the element.
- Split at 50,000 URLs / 50MB uncompressed, with a sitemap index.
- Splitting by page type (`sitemap-convert.xml`, `sitemap-tools.xml`) is worth
  doing beyond a few hundred URLs: indexation reporting becomes segmentable, so
  you can see *which tier* isn't getting indexed.
- Reference from `robots.txt` with an absolute URL.

## robots.txt

- Robots.txt controls **crawling**, never indexing. A disallowed URL can still
  be indexed from external links, showing a bare title with no snippet. To keep
  something out of the index use `noindex` — which requires the page to be
  *crawlable*. **Never disallow a page you have noindexed**; the crawler then
  can't see the directive.
- Never block CSS or JS needed to render the page.
- Blocking a genuinely non-content directory (a vendored engine, build assets)
  is legitimate crawl-budget hygiene.
- AI crawler directives: see `06-ai-search.md`.

## Crawl budget

Only a real concern above roughly 10k URLs, but the architecture decisions are
free to make early:

- Keep every page ≤ 3 clicks from the homepage — hubs do this structurally.
- Don't generate infinite URL spaces (unbounded filters, calendars, session
  params). This is the most common crawl-budget catastrophe.
- Return true 404s so the crawler learns the boundaries of the space.
- Fast responses raise crawl rate; slow servers lower it.
- Prune or noindex pages that have never earned an impression. A surface that
  only grows becomes a liability.
