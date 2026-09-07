# SEO/GEO Playbook

A method for **architecting a website's page surface for search and AI
visibility** — and for auditing one that already exists.

Packaged as a Claude Code skill, so it loads automatically whenever SEO work
comes up.

## Install

**In Claude Code** — browse and install from the plugin UI:

```
/plugin marketplace add yash-banka/seo-playbook
```

```
/plugin install seo-playbook@seo-playbook
```

**Or one command in a terminal** — works with other agents too, not just Claude
Code:

```bash
npx skills add yash-banka/seo-playbook
```

Then type `/seo-playbook` in Claude Code to confirm it loaded. Or just describe
the task — it triggers on its own for SEO, GEO, schema, sitemap, indexing,
backlink, faceted-navigation and site-structure questions.

<details>
<summary><b>Manual install, project scope, updating, and reading it without installing</b></summary>

<br>

**Manual** — clone the skill directory straight into place:

```bash
git clone https://github.com/yash-banka/seo-playbook.git /tmp/seo-playbook \
  && cp -R /tmp/seo-playbook/skills/seo-playbook ~/.claude/skills/ \
  && rm -rf /tmp/seo-playbook
```

**One project only** — replace `~/.claude/skills/` above with `.claude/skills/`
in your repo root, and commit it so your team gets it too.

The directory must be named `seo-playbook` — that's how the skill is addressed.

**Update:**

```bash
npx skills update seo-playbook
```

**Or don't install it at all.** It's a playbook, not a program. Every file under
`skills/seo-playbook/reference/` reads fine straight from GitHub — start with
[`00-surface-design.md`](skills/seo-playbook/reference/00-surface-design.md).
Installing only adds the part where Claude loads it automatically while you work.

</details>

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

## Who this is for — and who it isn't

**Use it for** sites that must *manufacture* demand capture: tools, catalogues,
marketplaces, comparison products, documentation, anything with a matrix of
things people search for.

**Don't use it for** personal sites, portfolios, blogs, studios or agencies.
Those rank on brand, singular content and earned links — a generated page
surface would actively harm them. Three such sites studied for this playbook
rank at the top of genuinely competitive queries with *no structured data, no
canonical tags and no robots meta*; one ships a title tag reading, in full,
`About`. If that's your site, skip to
[`reference/09-authority.md`](skills/seo-playbook/reference/09-authority.md).

## Two modes

| Mode | When | Start at |
|---|---|---|
| **A — Architect** | Site or section doesn't exist yet | `reference/00-surface-design.md` → `worksheets/surface-plan.md` |
| **B — Audit** | Site exists, needs improving | `audit-checklist.md` |

Mode B has a ceiling: if the page surface is wrong, no amount of on-page work
fixes it.

## Contents

```
skills/seo-playbook/
  SKILL.md                    Entry point, routing, the eight rules
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
  templates/                  <head> block, robots.txt, page archetypes
  worksheets/surface-plan.md  ★ Fill-in worksheet for Mode A
  audit-checklist.md          Mode B run sheet, with verification commands
.claude-plugin/               Plugin + marketplace manifests
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

**And know when not to use it.** See *Who this is for* above — the wrong site
type is the most expensive mistake available here.

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
