# 00 — Surface design: deriving a site's pages from demand

> The core method. Read this before writing any code for a new site.

## The inversion

Conventional order: build the product → get pages → optimise those pages.

Correct order: **map demand → decide the page surface → build the product behind it.**

The reference implementation is one media-conversion engine. One bundle, one
stylesheet, one router mapping a slug to a preset. Product necessity justifies
about three pages. It ships several hundred.

The extra URLs are not features. They are search surface, manufactured on
purpose. Each is the same engine wrapped in prose written for exactly one query.
The engine is the *product*; the surface is the *strategy*. They are separate
design problems and the surface one is usually neglected.

**Consequence:** "how many pages should this site have?" is never answered by
the product. It is answered by the demand map. A tool with one function can
legitimately warrant hundreds of URLs; a ten-feature SaaS might warrant twelve.

## Why this works (the mechanism, not the magic)

A search engine matches a query to the page that most specifically answers it.
A single generic page ("convert your files") competes badly for a specific query
("heic to jpg") against a page whose title, H1, body, schema and URL are all
about exactly that. Specificity is the entire advantage.

Manufacturing surface is therefore just: **paying the cost of specificity, at
scale, for every query worth winning.** The cost is real — each page needs a
genuine reason to exist (see `02-content.md`). Sites that skip that cost produce
doorway pages and get classified as such.

---

## The method

### Step 1 — Enumerate the capability space

List what the product can do, as structured data rather than prose. Look for the
**dimensions** — the variables that, when combined, describe every job the
product performs.

Common dimension shapes:

| Shape | Dimensions | Yields |
|---|---|---|
| **Pair matrix** | source × target | `/{a}-to-{b}` |
| **Verb × object** | action × thing | `/{action}-{thing}` |
| **Entity** | one thing | `/{class}/{thing}` |
| **Attribute × entity** | modifier × thing | `/{modifier}-{thing}` |
| **Comparison** | thing vs thing | `/{a}-vs-{b}` |
| **Segment × use case** | audience × job | `/{audience}/{job}` |
| **Geo × service** | place × offering | `/{place}/{service}` |

Most products carry two or three of these simultaneously. The reference
implementation runs three at once: a pair matrix (conversions), a verb × object
set (tools), and an entity set (formats).

**Do not cut anything yet.** Enumerate fully, including combinations that seem
worthless. The cutting happens in step 3 with evidence, not intuition.

### Step 2 — Translate capabilities into queries

This is the step people skip, and skipping it is why most programmatic SEO
reads as robotic.

A capability is not a query. `decode(HEIC) → encode(JPEG)` is a capability.
*"how do i open heic on windows"*, *"iphone photos won't open"*, and
*"heic to jpg"* are queries — three different framings of the same capability,
with different intent and different natural page shapes.

For each capability cell, write down **how a human actually asks for it**,
including:
- the transactional phrasing (`heic to jpg`)
- the problem phrasing (*"why can't I open this file"*)
- the comparison phrasing (*"heic vs jpg"*)
- the "should I" phrasing (*"is webp better than png"*)

Different phrasings may deserve different page types, or may collapse into one
page that addresses several. The reference implementation's most distinctive
pages come from problem phrasings, not transactional ones — pages headed with
questions like *"can anyone tell where this photo was taken?"* target intent
no competitor has bothered to name, and so face almost no competition. **The unclaimed queries are
usually problem-shaped, not keyword-shaped.**

### Step 3 — Attach demand evidence

For each candidate, get a signal. In descending order of reliability:

1. Your own site-search logs and Search Console query data (best — real, yours)
2. Keyword tools with volume estimates
3. Autocomplete / "people also ask" / related searches
4. Competitor URL structures — what has someone already found worth building
5. Community sources — forum and Q&A phrasing for the problem framings
6. Reasoned judgement (worst; label it as such)

Precision doesn't matter much. You need order-of-magnitude and a
**zero-vs-nonzero** call, because that call decides the index tier.

### Step 4 — Cut and tier

Every enumerated cell now gets exactly one of three fates:

| Fate | When | Treatment |
|---|---|---|
| **Indexed** | Real, recurring demand | In sitemap, indexable, gets bespoke content |
| **`noindex, follow`** | Product needs it; ~no search demand | Built, linked, works, **excluded from sitemap**, not indexed |
| **Don't build** | Neither demand nor product need | Omitted; the URL 404s properly |

The middle tier is the load-bearing decision and the one almost everyone misses.
See `01-architecture.md`. It lets the product be complete without the index
being diluted.

Two cuts also happen here:

- **Validity cut.** Drop combinations that are nonsense. The reference
  implementation's matrix is not cartesian: audio sources map only to audio
  targets, video sources map to video *and* audio (extract the soundtrack) and to
  animated images, but audio never maps to video. A cartesian product would have
  produced hundreds of absurd pages. **Domain logic prunes before demand does.**
- **Cannibalisation cut.** If two cells would produce near-identical pages
  competing for the same query, merge them and pick one canonical URL.

### Step 5 — Derive URLs

Only now do URL patterns get chosen, and they follow the dimensions directly:

- Keep the keyword in the slug, in natural query order: `/convert/heic-to-jpg`,
  not `/c?f=heic&t=jpg` and not `/convert/jpg-from-heic`.
- One canonical URL per concept. Directional pairs are **two** concepts (A→B and
  B→A are different jobs and different queries) — build both and cross-link them.
- Namespace by page type (`/convert/…`, `/tools/…`, `/formats/…`). This makes
  hubs obvious, makes reporting segmentable, and makes index tiering
  administrable by section.
- Lowercase, hyphenated, no dates, no IDs, stable forever.

### Step 6 — Derive hubs and the link mesh

The surface only works if it is crawlable and if equity reaches the leaves.

- Every page type gets a **hub** that links every page of that type. Hubs make
  the whole surface reachable in one hop and give the section a rankable page for
  its head term.
- Every leaf links **up** (its hub / its entity page), **sideways** (siblings
  sharing a dimension value), and **backwards** (its inverse, where one exists).
- **Build the hub before the leaves.** A missing hub orphans an entire section,
  and a hub that 404s while being linked sitewide wastes crawl on every page.
  This is a common and long-lived bug — see `08-pitfalls.md` pitfall 5.

Details in `03-on-page.md`.

---

## Worked example (different domain, same method)

**Product:** a cost-of-living comparison calculator. Functionally one page: pick
two cities, see the comparison.

**Step 1 — dimensions.** Cities (say 500), a pair matrix (city × city), plus
entity pages per city, plus attribute × entity (`cost of living in {city} for
{students|families|remote workers}`).

**Step 2 — query framings.** Transactional: *"london vs berlin cost of living"*.
Problem: *"can i afford to move to lisbon on my salary"*. Comparison:
*"is zurich more expensive than singapore"*. Should-I: *"is berlin cheaper than
london for students"*.

**Step 3 — demand.** 500 × 499 = 249,500 *ordered* pairs — but this comparison
is **symmetric**: *"london vs berlin"* and *"berlin vs london"* are the same job
and want the same page. So the real space is ~124,750 *unordered* pairs, and one
canonical order per pair (see the pairs table in `01-architecture.md`). Halving
the space before you start is the cheapest cut available.

Of those, nearly all have zero demand — nobody searches *"boise vs bratislava"*.
Perhaps 2,000 pairs have real, recurring volume, clustered on economically- and
migration-linked cities.

**Step 4 — cut and tier.**
- Indexed: ~2,000 demand-backed pairs + 500 city entity pages + ~1,500
  attribute × city pages that show demand.
- `noindex, follow`: the calculator still answers *any* pair a user selects, so
  those URLs resolve and work — but they are excluded from the sitemap and not
  indexed. The reverse ordering of each pair canonicalises to its primary.
- Don't build: nothing, since the calculator is generic; the cost of the middle
  tier here is near zero.

**Result:** ~4,000 indexed pages from a one-page product, and ~120,000 working
but unindexed URLs. Not 124,750 indexed pages — that would be a scaled-content
abuse case with no defence.

**Step 5 — URLs.** `/compare/london-vs-berlin`, `/cities/berlin`,
`/cost-of-living/berlin/students`. Fix one canonical ordering rule for the
symmetric pairs — alphabetical is the usual choice, because it is deterministic
and a generator can apply it without a lookup table.

**Step 6 — hubs.** `/compare` (all indexed pairs), `/cities` (all 500). Each pair
page links both city entity pages, the reverse pair, and sibling pairs sharing a
city.

The method transfers unchanged. Only the dimensions differ.

---

## When this playbook does not apply

**Read this before applying anything else.** The method here is for sites that
must *manufacture* demand capture. A large class of sites should not, and
applying it to them is actively harmful.

### The personal-site and studio case

Three sites studied for this playbook rank at or near the top of genuinely
competitive queries while doing **almost none** of what the rest of this
document recommends:

- A well-known independent developer's blog ranks first for hard, competitive
  front-end queries with **no structured data, no canonical tag, and no robots
  meta** — carried by one exceptionally good, deep, interactive article per
  topic.
- A widely-cited personal essay site serves plain, near-unstyled HTML, no
  schema, and a `robots` directive that has been dead since the directory it
  referenced shut down. It is cited everywhere regardless.
- A globally recognised design studio ships a title tag that reads, in full,
  `About`. Their brand does the work that a title tag would otherwise have to.

What these have in common: **their ranking comes from brand, singular content,
and earned links — not from surface area.** There is no matrix to build. A
portfolio has one "about", one "work", one "contact"; manufacturing variants of
those would produce exactly the doorway pages this playbook warns against.

For a personal site, portfolio, or studio, the work that actually moves search
is:
1. One genuinely excellent, hard-to-replicate piece per topic you want to own
2. A clear `Person` or `Organization` entity, consistent across the web
3. Earned links and citations (`09-authority.md`)
4. Basic technical hygiene — indexable, fast, one H1, honest titles
5. **Not** a generated page surface

Note the asymmetry, though: those sites can afford a title tag of `About`
*because* they already have the brand. A new site cannot. Copy their restraint
about surface; do not copy their indifference to fundamentals.

### The other preconditions

Beyond that, the strategy fails or backfires when:

- **Each page can't say anything specific.** If the only difference between two
  pages is a swapped noun, you are building doorway pages. Either find the
  specific substance or collapse them into one good page. (`02-content.md`)
- **The product can't actually do it.** A page for a job the product performs
  badly converts worse than no page and earns bad engagement signals.
- **Demand is genuinely concentrated.** Some categories have five queries worth
  winning. Then build five excellent pages. Surface volume is not a virtue.
- **You can't maintain it.** A generated surface goes stale in ways an editor
  won't notice. If nothing owns regeneration, it decays into a liability.
- **YMYL subject matter.** Medical, financial and legal topics are held to
  materially higher scrutiny; scaled generation there is high-risk. (`07`, `08`)

## Checklist before you build

- [ ] Dimensions enumerated as data, not prose
- [ ] Query framings written per cell — including problem-shaped ones
- [ ] Demand evidence attached, source and reliability labelled
- [ ] Validity cut applied (domain logic, before demand)
- [ ] Cannibalisation cut applied
- [ ] Every cell assigned: indexed / `noindex, follow` / don't build
- [ ] Each indexed page has a stated reason only it can exist for
- [ ] URL patterns keyword-bearing, stable, namespaced by type
- [ ] A hub exists for every page type, and it resolves
- [ ] Up / sideways / backwards links specified per leaf
- [ ] AI posture chosen deliberately (`06-ai-search.md`)
