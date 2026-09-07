# 05 — Technical: rendering, delivery, Core Web Vitals

## Rendering: split the rule by audience

**Put the content in the raw HTML response.** But understand *why*, because the
reason has changed and the rule is now audience-dependent.

| Consumer | Executes JavaScript? | Consequence |
|---|---|---|
| Googlebot | Yes, but rendering is deferred and budget-limited | Client-rendered content *can* rank — slower to index, and at scale the deferral bites |
| Most AI crawlers | Largely **no** | Client-rendered content is effectively invisible to AI retrieval |

So client-side rendering is survivable for classic search and close to fatal for
AI visibility. Two observed data points make the trade concrete:

- A major payments platform's developer documentation is almost entirely
  client-rendered — a ~1.1MB response containing only a couple of hundred words
  of readable text and no `<h1>` — and it ranks strongly, because Google renders
  it. **But it also publishes a ~90KB `llms.txt`**, which is precisely the
  compensation this trade-off demands. That pairing is the lesson: if you render
  client-side, you owe AI consumers a readable path by some other route.
- A large open web reference serves its content fully server-rendered — several
  thousand words in the raw HTML — and needs no such workaround.

The reference implementation takes the simpler route. It is a JavaScript
application — a router, a WASM engine, client-side state — yet every URL returns
a complete, pre-rendered HTML document, well under 20KB, containing the H1, the
full body copy, the comparison table, the FAQ, the internal links and the
JSON-LD. The JavaScript then hydrates it into an interactive tool.

That combination — **static HTML for the reader, JS for the tool** — is the
whole technical story. It means:

- The page is fully readable with JS disabled (there is a `<noscript>` note
  explaining the tool needs JS, but the *content* is already there)
- Crawlers never depend on rendering, which is deferred and budget-limited
- AI retrieval systems, which largely do **not** execute JavaScript, can read it
- First paint is immediate

**This single decision serves both AI postures** (`06`), so commit to it before
you have decided anything else.

Verify: `curl -s https://example.com/page | grep "<h1>"`. If your content isn't
in that output it is effectively invisible to AI retrieval, and it reaches
search only via Google's deferred render queue — survivable, but slower to index
and increasingly costly as your surface grows.

Acceptable approaches: static generation (best), SSG with hydration, SSR, or
prerendering at the edge. Not acceptable for content: client-side-only rendering.

## Delivery

Observed stack, and a good default:

| Element | Choice |
|---|---|
| Host | CDN edge (Cloudflare Pages / Netlify / Vercel) |
| Protocol | HTTP/2 or HTTP/3 |
| Compression | Brotli |
| CSS | **One** file, tens of KB uncompressed |
| JS | **One** ES module, tens of KB, `type="module"` (deferred by default) |
| Framework | None |
| Favicon | Inline `data:` SVG — zero requests |
| Analytics | One lightweight async script |

Two static assets for an entire site. No framework runtime, no hydration cost on
content, no third-party tag soup. Most sites' performance problems are not
optimisation failures — they are the accumulated cost of dependencies nobody
audited.

**Calibration — do not oversell this.** Lean delivery is good engineering and it
helps conversion, but it is not what decides rankings. Two of the most dominant
programmatic surfaces observed for this playbook ship **~1.1MB and ~1.5MB** per
page, against the reference implementation's sub-20KB, and outrank almost
everyone. Authority and intent-match beat page weight, every time. Optimise
delivery because it is cheap and it serves users — not because you expect
rankings to move.

## Core Web Vitals

Thresholds (75th percentile of real users, per metric, per page group):

| Metric | Good | Needs work | Poor |
|---|---|---|---|
| **LCP** — Largest Contentful Paint | ≤ 2.5s | ≤ 4.0s | > 4.0s |
| **INP** — Interaction to Next Paint | ≤ 200ms | ≤ 500ms | > 500ms |
| **CLS** — Cumulative Layout Shift | ≤ 0.1 | ≤ 0.25 | > 0.25 |

INP replaced FID in March 2024 and is stricter — it measures *all* interactions
through to the next paint, not just the first input delay. A heavy client app
that felt fine under FID can fail INP.

What actually moves them:
- **LCP** — server response time, render-blocking resources, unoptimised hero
  images. Pre-rendered HTML on a CDN largely solves this by construction.
- **INP** — long JavaScript tasks blocking the main thread. Break up work; for
  genuinely heavy compute use a Web Worker so the main thread stays responsive.
- **CLS** — always set explicit `width`/`height` (or `aspect-ratio`) on images
  and embeds; reserve space for anything injected late. **Ad slots are the
  single largest cause of CLS**, which is a real and rarely-priced SEO cost of
  running ads.

Calibrate expectations: Core Web Vitals are a genuine but **light** ranking
input. They are a tiebreaker among comparable results, not a substitute for
relevance. Fix them because they affect conversion and bounce; don't expect
rankings to jump because you shaved 200ms.

Measure **field data** (Chrome UX Report, Search Console), not only lab data
(Lighthouse). Lab numbers routinely disagree with what real users experience.

## Security and correctness headers

Observed and worth copying:

```
x-content-type-options: nosniff
x-frame-options: DENY
strict-transport-security: max-age=31536000; includeSubDomains
```

Not ranking factors, but HTTPS is a (light) one, and these prevent
whole categories of problem.

## Status codes

- 200 for real pages
- **True 404** for nonexistent ones — never a 200 with "not found" text
- 301 for permanent moves; update internal links to point at the destination
  rather than relying on the hop
- Never chain redirects
- 410 when you deliberately removed something permanently

## Verification commands

```bash
# Content present without JS?
curl -s https://example.com/page | grep -E '<h1>|<title>'

# Compression, protocol, caching, status
curl -sIL -H "Accept-Encoding: br,gzip" https://example.com/page \
  | grep -Ei 'HTTP/|content-encoding|cache-control|x-'

# Soft-404 check — must return 404
curl -o /dev/null -w "%{http_code}\n" https://example.com/definitely-not-a-page

# Index directives across a sample of URLs
curl -s https://example.com/page | grep -E 'name="robots"|rel="canonical"'
```
