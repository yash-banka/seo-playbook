# SEO/GEO Playbook

A method for **architecting a website's page surface for search and AI
visibility** — and for auditing one that already exists.

Packaged as a [Claude Code](https://claude.com/claude-code) skill, so it loads
automatically whenever SEO work comes up.

## The premise

Most people treat SEO as something applied to a site after it exists. That is
backwards, and it caps the ceiling.

**The page surface of a site *is* its SEO strategy**, and it is an architecture
decision made before the first line of code.

This playbook was reverse-engineered from a production site that competes
successfully in a heavily commoditised category. Functionally that site is *one
tool* — a single JS bundle, a single stylesheet, a router mapping a URL slug to
a preset. Product necessity would justify about three pages. It ships
**several hundred**.

Those extra URLs are not features. They are deliberately manufactured **search
surface**: the same tool, wrapped hundreds of times over in prose written for
one specific query each. That single decision is the whole strategy.

The playbook generalises the method, then cross-checks every claim against
primary documentation from Google and the standards bodies.

## Two modes

| Mode | When | Start at |
|---|---|---|
| **A — Architect** | Site or section doesn't exist yet | `reference/00-surface-design.md` → `worksheets/surface-plan.md` |
| **B — Audit** | Site exists, needs improving | `audit-checklist.md` |

Mode B has a ceiling: if the page surface is wrong, no amount of on-page work
fixes it.

## Contents

```
SKILL.md                      Entry point, routing, the seven rules
reference/
  00-surface-design.md        ★ Demand → page surface. The core method.
  01-architecture.md          Tiered URLs, index tiering, sitemaps
  02-content.md               Differentiation standard, the thin-content line
  03-on-page.md               Titles, metadata, headings, link mesh
  04-schema.md                JSON-LD @graph, entity linking
  05-technical.md             Rendering, delivery, Core Web Vitals
  06-ai-search.md             AI visibility vs AI refusal — an explicit fork
  07-sources.md               Primary-source citations for every claim
  08-pitfalls.md              Failure modes and how to detect them
  09-authority.md             Earning the right to rank at all
templates/                    <head> block, robots.txt, page archetypes
worksheets/surface-plan.md    ★ Fill-in worksheet for Mode A
audit-checklist.md            Mode B run sheet, with verification commands
```

## The core ideas

**Manufacture surface from demand, not from permutation.** A cartesian product
of your capabilities is not a strategy. Enumerate the space, then cut it against
real demand evidence.

**Tier the index, not just the sitemap.** Every page gets one of three fates:
indexed, `noindex, follow`, or don't build. The middle tier is what makes a
large surface safe — the product stays complete and link equity still flows, but
thin pages never enter the index. Most programmatic sites fail precisely here.

**Each page must carry an argument only it can make.** Measured on the reference
site, sibling pages share **84%** of their vocabulary. That works only because
the differing fraction is the substantive answer to that page's specific query.
The playbook states plainly that this is close to the line.

**Server-render the content.** If the answer isn't in the raw HTML, it does not
reliably exist — for crawlers, or for AI retrieval that doesn't execute JS.

**Decide the AI posture on purpose.** Allowing or blocking AI crawlers is a real
strategic choice with costs both ways. Default-by-accident is the only wrong
answer.

**A manufactured surface is authority avoidance.** Only ~1.74% of newly
published pages reach the top 10 within a year, and ~72.9% of top-10 results are
more than three years old. You cannot outrank a five-year-old incumbent on a head
term — but you can trivially outrank *nothing* on a query no one bothered to
write a page for. That is why this works for a new domain, and why it stops
working once your targets have real competition.

**And know when not to use it.** Personal sites, portfolios and studios rank on
brand, singular content and earned links. A generated surface would harm them.

## Sourcing

Claims are cited to primary documentation — Google Search Central, Google's spam
policies, the Search Quality Rater Guidelines, schema.org, web.dev — plus
practitioner research that publishes original data with stated methodology.
SEO marketing blogs and methodology-free listicles are deliberately excluded.

Where the playbook is uncertain, it says so rather than borrowing confidence
from that material.

> **Claims verified September 2026.** Search platforms move fast — parts of this
> will go stale. `reference/07-sources.md` carries a re-verification checklist
> ordered by what changes fastest (AI-crawler tokens first, then which
> structured-data types still produce rich results, then Core Web Vitals
> thresholds, then spam-policy wording). Re-check anything load-bearing before
> relying on it.

## Installation

```bash
git clone <this-repo> ~/.claude/skills/seo-playbook
```

Claude Code discovers it automatically. Invoke explicitly with
`/seo-playbook`, or just describe the task — the skill triggers on SEO, GEO,
schema, sitemap, indexing and site-structure questions.
