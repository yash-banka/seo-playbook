# 08 — Pitfalls

Failure modes, ordered by how much damage they do.

Pitfalls 5 and 6 deserve special attention: they are the kind that survive on
otherwise excellent, carefully-built sites, because nothing surfaces them.
**Competence elsewhere does not imply competence here.** Check for them
explicitly rather than assuming.

---

## Catastrophic

### 1. Indexing the whole cartesian product
Building every possible combination and indexing all of it. This is the defining
failure of programmatic SEO and it invites a scaled-content-abuse
classification (`07`). Ahrefs' data says ~96.55% of pages get zero traffic
anyway — so most of what you index is dead weight that only dilutes quality
signals.

**Fix:** the three-fate model in `01`. Indexed / `noindex, follow` / don't build.

### 2. Pages with nothing specific to say
If the only difference is a swapped noun, they are doorway pages by Google's own
definition. **Fix:** `02` — derive differentiation from structured facts, and
let "this pair has no derived sections" automatically demote the page to
`noindex, follow`.

### 3. Content not in the raw HTML
Client-side-only rendering. Crawlers deprioritise rendering; most AI retrieval
does not execute JS at all. **Fix:** `05` — pre-render. Verify with `curl`.

### 4. Infinite URL spaces
Unbounded filter combinations, calendars, session parameters. Burns crawl budget
without limit and can bury the pages that matter. **Fix:** bound the space,
`noindex` facets, return true 404s outside the intended set.

---

## Serious

### 5. A hub that 404s
A section hub linked from the sitewide header or footer — so from *every* page —
that returns **404**. The section's leaf pages end up with no hub of their own.

A sitewide broken link wastes crawl budget on every page and orphans a whole
section from hub-based discovery. This one survives for years on good sites,
because nobody clicks their own footer.

**Fix:** build hubs *before* leaves. Then crawl your own site and assert every
internal link returns 200:
```bash
curl -s https://example.com/sitemap.xml | grep -o '<loc>[^<]*' | sed 's/<loc>//' \
  | while read u; do printf '%s %s\n' "$(curl -o /dev/null -sw '%{http_code}' "$u")" "$u"; done \
  | grep -v '^200'
```

### 6. Duplicate / competing JSON-LD
A page emitting **two** `<script type="application/ld+json">` blocks, where the
second re-declares entities such as `WebSite` and `WebApplication` **without**
`@id` anchoring — creating unlinked duplicates of the very entities the first
block linked into a coherent graph. (If both carry the same `id` attribute,
that is invalid HTML too.)

The usual cause is a legacy block that a template kept appending after someone
wrote a proper graph. The first block is textbook; the second silently
undermines it, and nobody notices because both validate in isolation.

**Fix:** one graph per page; validate the **rendered** output, not the template:
```bash
curl -s https://example.com/page | grep -c 'application/ld\+json'   # expect 1
```

### 7. Soft 404s
Returning 200 with "not found" content. At scale this teaches crawlers your URL
space is unbounded. **Fix:** true 404 status. Verify with `curl -w "%{http_code}"`.

### 8. `noindex` on a robots.txt-disallowed page
The crawler can't fetch the page, so it never sees the directive — and the URL
can still be indexed from external links, with no snippet. A very common
own-goal. **Fix:** to remove something from the index, it must stay crawlable.

### 9. Duplicate titles and descriptions across generated pages
A reliable symptom that the *pages* are duplicates. **Fix:** if you can't write
a distinct title, the page probably shouldn't be indexed.

### 10. Schema describing invisible content
FAQ markup for Q&A not on the page violates the structured-data guidelines and
risks a manual action. **Fix:** markup mirrors visible content, always.

---

## Costly but recoverable

### 11. Selling deprecated rich results
`FAQPage` and `HowTo` no longer produce rich results, and the sitelinks search
box is retired (`07`). Emitting them is fine; *promising* them is
misinformation. **Fix:** justify schema on entity/AI legibility, not SERP
features.

### 12. Ads destroying CLS
Ad slots are the largest single cause of layout shift. The reference
implementation takes donations and runs no ads — stated explicitly on the page —
which removes the problem entirely. If you must run ads, reserve their space
with fixed dimensions.

### 13. Redirect chains and stale internal links
Pointing internal links at URLs that 301 elsewhere. **Fix:** link the
destination directly; never chain.

### 14. Untruthful `lastmod`
Every URL claiming today's date. Google uses `lastmod` only when "consistently
and verifiably accurate" (`07`) — so a dishonest one gets the whole file
discounted. **Fix:** real modification dates, or omit the element.

### 15. Analytics that contradict the pitch
A privacy-first product loading trackers that undercut the claim. The reference
implementation runs a single analytics script and documents in an HTML comment
what it does and does not capture. Consistency between claim and implementation
is an E-E-A-T signal, and inconsistency is a credibility hole a competitor will
point at.

### 16. Surface rot
Generated pages go stale silently. Underlying facts change; nobody notices
because no human reads page 300. **Fix:** assign an owner and a regeneration
trigger, and prune or noindex pages that have never earned an impression.

---

## The pre-launch assertions

Run these before shipping any generated surface:

```bash
BASE=https://example.com

# every sitemap URL returns 200
curl -s $BASE/sitemap.xml | grep -o '<loc>[^<]*' | sed 's/<loc>//' \
  | while read u; do printf '%s %s\n' "$(curl -o /dev/null -sw '%{http_code}' "$u")" "$u"; done | grep -v '^200'

# no sitemap URL is noindexed
curl -s $BASE/sitemap.xml | grep -o '<loc>[^<]*' | sed 's/<loc>//' \
  | while read u; do curl -s "$u" | grep -q 'name="robots" content="noindex' && echo "NOINDEXED IN SITEMAP: $u"; done

# nonsense URL really 404s
curl -o /dev/null -w '%{http_code}\n' $BASE/definitely-not-a-real-page

# no entity declared twice (multiple blocks are fine; duplicates are not)
curl -s $BASE/some-page | grep -o '"@type":"[A-Za-z]*"' | sort | uniq -d

# content present without JS
curl -s $BASE/some-page | grep -E '<h1>|<title>'

# titles unique across the surface
curl -s $BASE/sitemap.xml | grep -o '<loc>[^<]*' | sed 's/<loc>//' | head -50 \
  | while read u; do curl -s "$u" | grep -o '<title>[^<]*'; done | sort | uniq -d
```

Any output from the first, second, or last command is a bug.
