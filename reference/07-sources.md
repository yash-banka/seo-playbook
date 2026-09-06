# 07 — Sources

Every non-obvious claim in this playbook traces to a source here. Tier 1 is
primary documentation from search engines and standards bodies. Tier 2 is
practitioner research that publishes **original data with stated methodology**.
Marketing blogs and methodology-free listicles are deliberately excluded.

Verified as of **September 2026**. Search platforms change fast — re-verify
anything load-bearing before relying on it, especially the AI-crawler section.

---

## Tier 1 — Primary

### Google spam policies — *the* governing document for manufactured surfaces
https://developers.google.com/search/docs/essentials/spam-policies

- **Scaled content abuse:** "when many pages are generated for the primary
  purpose of manipulating search rankings and not helping users."
- "This abusive practice is typically focused on creating large amounts of
  unoriginal content that provides little to no value to users, **no matter how
  it's created**."
- **Doorway pages:** pages "created to rank for specific, similar search
  queries" that "lead users to intermediate pages that aren't as useful as the
  final destination."
- **Site reputation abuse:** third-party content published "mainly because of
  that host's already-established ranking signals."

**Implication for this playbook:** generation is *not* the violation —
valuelessness is. The stated test is user value, and the doorway definition
turns on whether the page is a *waystation* or a *destination*. A generated page
where the user completes their task is a destination. This is the single most
important source here; `00`, `02` and `08` are built on it.

### Blocking indexing — the `noindex` rule
https://developers.google.com/search/docs/crawling-indexing/block-indexing

- "For the `noindex` rule to be effective, the page or resource **must not** be
  blocked by a robots.txt file, and it has to be otherwise accessible to the
  crawler."
- noindex may be combined with other rules.

**Implication:** the `noindex, follow` tier in `01` requires those pages to stay
crawlable. Never disallow a page you have noindexed.

### Sitemaps
https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap

- "Google ignores `<priority>` and `<changefreq>` values."
- `<lastmod>` is used **only** when "consistently and verifiably accurate", and
  should reflect significant content changes — not trivial edits.
- Limits: 50,000 URLs / 50MB uncompressed per file; use a sitemap index beyond.
- Include only canonical URLs you want in results; absolute URLs.

### Core Web Vitals
https://web.dev/articles/vitals

| Metric | Good | Needs improvement | Poor |
|---|---|---|---|
| LCP | ≤ 2.5s | 2.5–4s | > 4s |
| INP | ≤ 200ms | 200–500ms | > 500ms |
| CLS | ≤ 0.1 | 0.1–0.25 | > 0.25 |

- Assess at the **75th percentile** of page loads, segmented mobile/desktop.
- INP became a stable Core Web Vital in 2024, replacing FID.

### Google-Extended (AI training control)
https://developers.google.com/search/docs/crawling-indexing/google-common-crawlers

- "Google-Extended is a standalone product token that web publishers can use to
  manage whether content Google crawls from their sites may be used for training
  future generations of Gemini models."
- "**Google-Extended does not impact a site's inclusion in Google Search nor is
  it used as a ranking signal in Google Search.**"

**Implication:** Posture B in `06` is real and safe — you can refuse AI training
without harming search visibility. This is the fact that makes the posture
decision a genuine choice rather than a trade-off against rankings.

### FAQ and HowTo rich results — deprecated
https://developers.google.com/search/blog/2023/08/howto-faq-changes
https://developers.google.com/search/blog/2025/06/simplifying-search-results

- 2023: visibility restricted; FAQ limited to authoritative government and
  health sites; HowTo dropped on mobile.
- Subsequently deprecated outright: How-to rich results removed from desktop and
  mobile; **FAQ rich results stopped appearing in Search in May 2026**, with the
  search appearance, rich-result report and Rich Results Test support removed in
  June 2026 and Search Console API support in August 2026.
- Existing markup is harmless and need not be removed — it simply renders
  nothing in Search.

**Implication:** `04` recommends these only for entity/AI legibility. Never
promise the rich result.

### Sitelinks search box — retired
https://developers.google.com/search/blog/2024/10/sitelinks-search-box

- Removed globally from 21 November 2024, all languages and countries; dropped
  from Search Console reports and the Rich Results Test.
- Does not affect rankings or other sitelinks.
- Removing the markup is **not required**; and **`WebSite` markup should be
  kept**, since site names use a variation of it.

### Other Tier 1 references
- Google Search Essentials — https://developers.google.com/search/docs/essentials
- Structured data general guidelines —
  https://developers.google.com/search/docs/appearance/structured-data/sd-policies
- Search gallery (what still produces rich results) —
  https://developers.google.com/search/docs/appearance/structured-data/search-gallery
- Search Quality Rater Guidelines (E-E-A-T; how human raters judge quality) —
  https://guidelines.raterhub.com/searchqualityevaluatorguidelines.pdf
- schema.org vocabulary — https://schema.org/
- Bing Webmaster Guidelines —
  https://www.bing.com/webmasters/help/webmasters-guidelines-30fba23a

---

## Tier 2 — Original research with methodology

### Ahrefs — most pages get no search traffic
https://ahrefs.com/blog/search-traffic-study/

- **96.55%** of pages studied get **zero** traffic from Google; a further 1.94%
  get 1–10 monthly visits. Only ~3.45% get meaningful organic traffic.
- Sample: ~14 billion pages from Ahrefs' Content Explorer (a subset of their
  ~340bn index, acknowledged to skew toward higher-quality content).
- Traffic estimated against a ~651m keyword database; the authors note niche
  queries may go undetected, so the figure may slightly overstate the zero
  bucket.

**Implication — this is the empirical case for index tiering.** If ~96% of pages
earn nothing, then building a large *indexed* surface without demand evidence is
statistically a losing bet, and the `noindex, follow` tier is how you keep the
product complete without paying that cost. It also argues for the `00` demand
cut: pages built on hope are the 96%.

---

## Explicitly excluded

SEO agency blog posts, "N ranking factors for 2026" listicles, and GEO/AEO
vendor content that asserts ranking factors without methodology. A search for
current guidance returns overwhelmingly this tier; none of it is used here.

**Where this playbook is uncertain, it says so** rather than borrowing false
confidence from that material. Known-speculative items:
- `llms.txt` (`06`) — an emerging convention, not an adopted standard
- Cloudflare Content Signals (`06`) — a licence expression, voluntary and
  legally untested
- All "GEO ranking factor" claims — unproven; optimise mechanisms you can
  verify instead

## Re-verification checklist
When revisiting this playbook, re-check in this order — these change fastest:
1. AI crawler tokens and Content Signals status (`06`)
2. Which structured-data types still produce rich results (`04`)
3. Core Web Vitals metrics and thresholds (`05`)
4. Spam policy wording on scaled content (`02`, `08`)
