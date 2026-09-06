---
name: seo-playbook
description: >-
  Architect a website's page surface for search and AI visibility, audit an
  existing site's SEO, or judge whether a site should do programmatic SEO at
  all. Use WHENEVER the user mentions SEO, GEO, AEO, search rankings, "why
  isn't this page indexed", "why isn't my site ranking", "we get no organic
  traffic", meta descriptions, title tags, canonical tags, robots.txt,
  sitemaps, schema markup / JSON-LD / structured data, internal linking, crawl
  budget, Core Web Vitals, programmatic or templated pages, landing-page
  generation at scale, keyword targeting, AI search visibility (ChatGPT /
  Perplexity / AI Overviews), llms.txt, blocking AI or SEO-crawler bots.
  ALSO use for: backlinks, link building, domain authority / domain rating,
  how a new site earns the right to rank, and how long ranking takes; faceted
  navigation, filters, e-commerce category and pagination indexing; and
  whenever the user is planning a new site or product site and asks how to
  structure it, how many pages it should have, or what URLs it should expose.
  ALSO use to advise against a generated page surface for a personal site,
  portfolio, blog, studio or agency, where it would do harm. Covers greenfield
  architecture, retrofit audits, and authority building.
user-invocable: true
---

# SEO/GEO Playbook

## The premise this playbook is built on

Most people treat SEO as something applied to a site after it exists. That is
backwards and it caps your ceiling.

**The page surface of a site *is* its SEO strategy**, and it is an architecture
decision made before the first line of code.

The reference implementation behind this playbook is a browser-based media
conversion tool. Functionally it is *one* engine — a single JS bundle, a single
stylesheet, a router that maps a URL slug to a preset. Product necessity would
justify roughly three pages. It ships **several hundred**.

Nearly all of those URLs are not product. They are deliberately manufactured
**search surface**. Each one is the same tool wrapped in an explanation written
for one specific query. That single decision is why it competes in a brutally
commoditised category.

So the first question for any new site is never "how do I optimise these pages".
It is: **"what is the full space of things people search for that this product
can answer, and how much of that space deserves a URL?"**

---

## Routing: which mode are you in?

**First: does this playbook even apply?** It is written for sites that must
*manufacture* demand capture. Personal sites, portfolios and studios generally
should not — they rank on brand, singular content and earned links, and a
generated surface would actively harm them. Read the "When this playbook does
not apply" section of `reference/00-surface-design.md` before proceeding if the
site is of that kind.

**Mode A — Architect** (the site does not exist yet, or a new section is being
planned). This is the primary mode and the higher-leverage one.

→ Read `reference/00-surface-design.md`, then work through
  `worksheets/surface-plan.md`. Do this *before* any code, routing, or CMS
  decisions. Then read `01`, and pull `03`/`04`/`05` as you build.

**Mode B — Audit** (a site exists and needs improving).

→ Run `audit-checklist.md`. Pull the relevant `reference/` file for each
  finding. Note that Mode B has a ceiling: if the site's page surface is wrong,
  no amount of on-page work fixes it — escalate to Mode A for that section.

Either mode: `reference/06-ai-search.md` forces an explicit decision about AI
crawlers that most sites make by accident.

---

## Reference map

| File | Covers |
|---|---|
| `reference/00-surface-design.md` | **★ Demand → page surface. The core method.** |
| `reference/01-architecture.md` | Tiered URL model, index tiering, sitemaps |
| `reference/02-content.md` | Differentiation standard, the thin-content line |
| `reference/03-on-page.md` | Title/meta/H1 formulas, internal-link mesh |
| `reference/04-schema.md` | JSON-LD `@graph`, entity linking |
| `reference/05-technical.md` | Rendering, delivery, Core Web Vitals, crawl |
| `reference/06-ai-search.md` | Posture A vs B: AI visibility or AI refusal |
| `reference/07-sources.md` | Primary-source citations for every claim |
| `reference/08-pitfalls.md` | Failure modes and how to detect them |
| `reference/09-authority.md` | **Earning the right to rank at all** |
| `templates/` | `<head>` block, robots.txt, page archetypes |
| `worksheets/surface-plan.md` | **★ Fill-in worksheet for Mode A** |
| `audit-checklist.md` | Mode B run sheet |

---

## The eight rules (load-bearing summary)

1. **Manufacture surface from demand, not from permutation.** A cartesian
   product of your capabilities is not a strategy. Enumerate the space, then cut
   it against real demand. (`00`)

2. **Tier your index, not just your sitemap.** Every page you build gets one of
   three fates: indexed, `noindex, follow`, or don't build. The middle tier is
   what keeps a large surface safe — the product stays complete and link equity
   still flows, but thin pages never enter the index. Most programmatic sites
   fail precisely here. (`01`)

3. **Each page must carry an argument only it can make.** Siblings share a
   scaffold; the differentiating fraction must be the substantive answer to that
   specific query, not spun filler. Say when *not* to use the thing. (`02`)

4. **Put the content in the raw HTML.** Google renders JavaScript, but slowly
   and on a budget; most AI crawlers don't render at all. So client-side
   rendering is survivable for search and close to fatal for AI visibility — and
   if you render client-side anyway, you owe AI consumers an `llms.txt`. (`05`)

5. **One entity graph per page, linked by `@id`.** Multiple JSON-LD blocks are
   fine; what is never fine is the *same entity declared twice*, unlinked and
   competing. (`04`)

6. **Link up, sideways, and backwards.** Every leaf links to its hub, its
   siblings, and its inverse. Keep every page within one click of a hub. (`03`)

7. **Decide the AI posture on purpose.** Allowing or blocking AI crawlers is a
   strategic choice with real trade-offs both ways. Default-by-accident is the
   only wrong answer. (`06`)

8. **Know that surface is authority avoidance.** Only ~1.74% of new pages reach
   the top 10 within a year, and ~72.9% of top-10 results are over three years
   old. A manufactured surface wins by competing where nobody showed up — which
   is why it works for a new domain, and why it stops working the moment your
   targets have real competition. (`09`)

---

## Working notes

- **Cite primary sources.** When you make a claim about what search engines
  reward or penalise, source it from `reference/07-sources.md` rather than
  asserting it. If a claim isn't in there and matters, go verify it against
  Google Search Central or the Quality Rater Guidelines before stating it.
- **Never promise rankings.** Recommend mechanisms and explain the reasoning;
  outcomes depend on competition and authority you don't control.
- **Flag the risk boundary honestly.** A large manufactured surface sits nearer
  the scaled-content-abuse line than a small one. `02` and `08` describe where
  that line is. Don't pretend there is more margin than there is.
