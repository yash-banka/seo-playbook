# 09 — Authority: earning the right to rank

Everything else in this playbook is on-site. This file covers the thing you
cannot build with architecture: whether Google trusts your domain enough to rank
the pages you have carefully designed.

**Skipping this is how people follow the rest of the playbook perfectly and rank
for nothing.**

## The numbers, first

From a study of ~1 million URLs against ~1.3 million US keywords
(`07-sources.md`):

| Finding | Figure |
|---|---|
| Newly published pages that reach the top 10 **within a year** | **1.74%** |
| Of pages that *do* reach the top 10, share that got there within a month | **40.82%** |
| Average age of the **#1** ranking page | **5 years** |
| Top-10 pages **older than 3 years** | **72.9%** |

Read those together and two things follow.

**First, ranking is bimodal.** A page either finds its footing quickly or
essentially never does. "Give it 18 months" is not a strategy — if a page hasn't
moved in a few months, the problem is the page, the query, or the domain, not
the wait.

**Second, incumbency is enormous.** Nearly three-quarters of top-10 results are
more than three years old. On any query worth real money, you are arguing with a
page that has been accumulating links and engagement since before you started.

## The strategic consequence: surface is authority avoidance

This is the idea that connects this file to `00-surface-design.md`, and it is the
most important sentence in the playbook:

> **A manufactured page surface is a strategy for winning without authority.**

You cannot outrank a five-year-old page with a hundred referring domains on a
head term. You can trivially outrank *nothing* on a query so specific that no
one has bothered to write a page for it.

That is what the reference implementation does. It does not fight for the
category head term. It builds hundreds of pages for queries where the
competition is weak or absent, and collects the aggregate. The 1.74% figure is
not an argument against the strategy — **it is the argument for it.** Long-tail
specificity is how a new domain gets traffic before it has earned authority.

Which also tells you where the strategy stops working: **the moment your target
queries have real competition, surface alone will not win them.** At that point
you need the rest of this file.

## What actually earns authority

Ranked by what holds up:

1. **Something worth citing.** Original data you collected, a free tool that
   solves a real problem, a definitive reference nobody else wrote. This is the
   only link source that compounds without ongoing effort. The reference
   implementation's own strongest asset is not its prose — it is that the tool
   is genuinely free, genuinely private, and has no upload limit, which is
   inherently linkable.
2. **Being the obvious source in a niche.** Topical depth across a coherent
   subject earns more than scattered coverage. Entity pages (`01`) exist partly
   for this.
3. **Digital PR — being newsworthy to people who publish.** Original research,
   a strong opinion, a useful free thing. Slow, unpredictable, legitimate.
4. **Genuine relationships.** Communities, open source, conference talks,
   answering questions where your audience already is.
5. **Being unusually good at one thing.** See the personal-site case in `00`: an
   independent developer's blog outranks well-funded competitors on hard queries
   because each article is dramatically better than the alternatives, and people
   link to it unprompted.

## What is against policy

Google's spam policies prohibit these explicitly (`07-sources.md`):

- "Buying or selling links for ranking purposes… Exchanging money for links, or
  posts that contain links"
- "Excessive link exchanges… or partner pages exclusively for the sake of
  cross-linking"
- "Using automated programs or services to create links to your site"
- "Advertorials or native advertising where payment is received for articles
  that include links that pass ranking credit"
- "Low-quality directory or bookmark site links"
- Links embedded in widgets distributed across sites
- Requiring a link in a Terms of Service or contract without allowing the owner
  to qualify it

**The legitimate escape hatch:** paid and sponsored links are fine when
qualified — `rel="nofollow"` or `rel="sponsored"`. The violation is passing
ranking credit for payment, not the payment.

Note that most commercial "link building" sits somewhere on this list. If a
service sells you links, you are buying a policy violation with your domain as
collateral.

## The cold-start sequence

For a genuinely new domain:

1. **Pick queries where authority barely matters.** Long-tail, specific,
   low-competition. This is `00-surface-design.md` step 3, and at cold start it
   is the *only* viable target class.
2. **Ship one thing genuinely worth linking to** — a free tool, a dataset, a
   definitive guide. One is enough to start.
3. **Get the technical fundamentals right** so nothing wastes what little
   crawl attention you get (`01`, `05`).
4. **Be present where your audience already is.** Not for links directly — for
   the awareness that eventually produces them.
5. **Measure at the right horizon.** Check whether pages are *indexed* within
   weeks, and whether they earn impressions within a couple of months. Given the
   bimodal data, a page flat at three months usually needs changing, not waiting.
6. **Only then contest competitive terms**, once the domain has a track record.

## Measuring it honestly

- **Impressions before positions.** Search Console impressions move first and
  tell you whether Google considers you relevant at all.
- **Referring *domains*, not backlinks.** A hundred links from one site is one
  vote. The studied correlation is with referring domains.
- **Beware third-party authority scores.** Domain Rating, Domain Authority and
  similar are *vendor models*, not Google metrics. Useful for relative
  comparison, meaningless as targets. Optimising a vendor's score is not
  optimising anything real.
- **Segment new pages from old.** Aggregate traffic hides whether new surface is
  working.

## The honest summary

Architecture determines whether you *can* rank; authority determines whether you
*do*. A manufactured surface buys you a real, legitimate way to win traffic
before you have authority — by competing where nobody else showed up. It does
not exempt you from needing authority eventually, and no amount of on-page
craft substitutes for being worth citing.
