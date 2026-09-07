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
- **Pairs: ask whether the relationship is directional or symmetric.** This is
  the mistake most matrix sites make in one direction or the other.

  | Relationship | Is A→B a different job from B→A? | Treatment |
  |---|---|---|
  | Conversion (`png-to-jpg`) | **Yes** — different output, different query | Build both, self-canonicalise both, cross-link |
  | Integration (`connect A and B`) | **No** — one job | Build one, canonicalise the other to it |
  | Comparison (`A vs B`) | **Usually no** | Pick one order as canonical |
  | Directional transfer (`A to B rate`) | **Yes** — the query is directional | Build both |

  A large B2B automation platform demonstrates the symmetric case precisely: it
  serves both orderings of its integration pairs, and the reverse URL carries a
  canonical pointing at the primary one. Connecting two apps is a single job, so
  it gets a single indexable page — while both URLs stay reachable for users and
  internal links.

  Get this wrong in the symmetric direction and you double your surface with
  genuine duplicates. Get it wrong in the directional direction and you throw
  away half your qualified traffic.

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

## Faceted navigation — index control by *depth*

Facets (colour × size × brand × price × sort × page) generate a combinatorial
URL space that is effectively infinite. This is the hardest index-control
problem in SEO, and the technique that works is not "noindex the facets" — it is
a **depth threshold**.

The insight: facet *demand* collapses fast with depth.

| Facets combined | Example query | Demand | Treatment |
|---|---|---|---|
| 0 | "sofas" | High | Index |
| 1 | "blue sofas" | Real | Index |
| 2 | "blue leather sofas" | Thin but real | Index selectively |
| 3+ | "blue leather 3-seat under-£800 sofas sorted by price" | Effectively zero | Block |

A UK department store implements exactly this in `robots.txt`, using the fact
that its facet URLs encode each selected value with a repeated separator. It
allows the shallow canonical facet form, then disallows any URL whose separator
repeats past a threshold — five or more combined facets are simply uncrawlable.
It additionally blocks URLs where the *same* parameter appears twice, which is
the classic infinite-loop signature.

A large home-goods retailer applies the identical idea to **pagination**: the
first few pages of a listing are explicitly allowed, everything deeper is
disallowed by wildcard. Page 40 of a category has no search demand and no unique
content; it exists for users, not for crawlers.

**The general pattern:**

```
Allow:    /browse/*/{one-facet-form}
Allow:    /browse/*/{two-facet-form}
Disallow: /browse/*/{three-or-more-facet-form}
Disallow: */{param}=*/{param}=*          # same parameter twice
Disallow: /browse/*/*/*/*/*/*/*/*/*/     # excessive path depth
```

**Which control to use where** — these are not interchangeable:

| Situation | Control | Why |
|---|---|---|
| Facet has real demand | Index, self-canonical | It is a destination |
| Facet is a thin variant of an indexed page | `canonical` to the parent | Consolidates signals |
| Facet combination has no demand but users need it | `noindex, follow` | Works, stays out of the index |
| Combinatorial explosion, never useful | **`robots.txt` disallow** | Stops the crawl before it starts |

Note the last row is the one case where robots.txt disallow is right rather than
`noindex`. The distinction: `noindex` requires the crawler to *fetch* the page
to see the directive, which is fine for hundreds of URLs and hopeless for
millions. When the space is combinatorially infinite, you must prevent the crawl,
not the indexing — accepting that a disallowed URL can still be indexed
link-only, which is a price worth paying at that scale.

Sorting and view parameters (`?sort=`, `?view=`, `?page_size=`) should never be
indexable: canonical them to the unparameterised URL.

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
