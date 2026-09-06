# Audit checklist (Mode B)

Run against an existing site. Set `BASE` and work top-down — findings are
ordered by impact, so stop and fix rather than completing the list first.

```bash
BASE=https://example.com
```

**Mode B has a ceiling.** On-page fixes cannot rescue a wrong page surface. If
section 2 fails, the fix is Mode A (`worksheets/surface-plan.md`) for that
section, not more meta-description edits. Say so plainly in the findings.

---

## 1. Crawl reality — what actually exists

```bash
curl -s $BASE/robots.txt
curl -s $BASE/sitemap.xml | grep -c '<loc>'
curl -s $BASE/sitemap.xml | grep -o '<loc>[^<]*' | sed 's/<loc>//' \
  | sed "s|$BASE/||" | awk -F/ '{print $1}' | sort | uniq -c | sort -rn
```

- [ ] robots.txt present, sitemap referenced with an absolute URL
- [ ] No `Disallow` on anything that should be indexed
- [ ] CSS/JS not blocked
- [ ] AI-crawler directives are **deliberate** (`reference/06`) — a default
      nobody chose is a finding
- [ ] URL count and section distribution match what the business thinks it has

## 2. Surface sanity — the highest-leverage question

- [ ] Does each indexed page have a reason only it can exist for?
- [ ] Is any section a cartesian product with no demand cut? (`reference/00`)
- [ ] Are there pages with real search demand that **don't exist**? (the most
      valuable finding an audit can produce)
- [ ] Do the sitemap sections match the intent tiers? (`reference/01`)

## 3. Index control

```bash
# noindexed URLs sitting in the sitemap — contradictory signals
curl -s $BASE/sitemap.xml | grep -o '<loc>[^<]*' | sed 's/<loc>//' \
  | while read u; do curl -s "$u" | grep -q 'content="noindex' && echo "NOINDEX IN SITEMAP: $u"; done

# soft-404 check
curl -o /dev/null -w '%{http_code}\n' $BASE/definitely-not-a-real-page
```

- [ ] Sitemap contains only indexable, canonical, 200 URLs
- [ ] Nonsense URLs return a true **404**, not 200
- [ ] Low-value pages are `noindex, follow` — not indexed, not disallowed
- [ ] Nothing is both `noindex` **and** robots.txt-disallowed (`reference/07`)
- [ ] `<lastmod>` is truthful, or absent

## 4. Internal links — where bugs hide

```bash
# every sitemap URL returns 200
curl -s $BASE/sitemap.xml | grep -o '<loc>[^<]*' | sed 's/<loc>//' \
  | while read u; do printf '%s %s\n' "$(curl -o /dev/null -sw '%{http_code}' "$u")" "$u"; done | grep -v '^200'

# sitewide nav/footer links — the classic broken-hub bug
curl -s $BASE/ | grep -o 'href="/[a-z-]*"' | sort -u \
  | sed 's|href="||;s|"||' \
  | while read p; do printf '%s %s\n' "$(curl -o /dev/null -sw '%{http_code}' "$BASE$p")" "$p"; done
```

- [ ] **Every hub resolves** — check header and footer links explicitly
      (`reference/08` pitfall 5)
- [ ] Every page ≤ 1 click from a hub
- [ ] Leaves link up / sideways / backwards
- [ ] Descriptive anchor text
- [ ] Links are real `<a href>` in served HTML
- [ ] No redirect chains

## 5. Rendering

```bash
curl -s $BASE/some-page | grep -E '<h1>|<title>'
```

- [ ] H1, body copy, links and schema all present **without JS**
- [ ] `<noscript>` explains anything genuinely JS-dependent

## 6. On-page

```bash
# duplicate titles across a sample
curl -s $BASE/sitemap.xml | grep -o '<loc>[^<]*' | sed 's/<loc>//' | head -50 \
  | while read u; do curl -s "$u" | grep -o '<title>[^<]*'; done | sort | uniq -d
```

- [ ] Titles unique, keyword-first, ~60 chars
- [ ] Descriptions unique, objection-killing, ~155 chars
- [ ] One H1 per page, and it differs from the title tag
- [ ] H2s are question-shaped
- [ ] Self-referencing canonical on every indexable page
- [ ] OG + Twitter tags complete

## 7. Structured data

```bash
curl -s $BASE/some-page | grep -c 'application/ld+json'   # expect 1
```

- [ ] **Exactly one** JSON-LD block (`reference/08` pitfall 6)
- [ ] Entities linked by `@id`, no competing duplicates
- [ ] Markup describes only **visible** content
- [ ] Validates in the Rich Results Test / Schema Markup Validator
- [ ] Nobody is being promised FAQ/HowTo rich results or a sitelinks search box
      — all retired (`reference/07`)

## 8. Content quality

- [ ] Sibling pages differ in **substance**, not just nouns (`reference/02`)
- [ ] Pages say when *not* to do the thing
- [ ] Failure modes / "what goes wrong" present
- [ ] Comparison data is real, not asserted
- [ ] **Test:** move a page's distinctive paragraph to a sibling — does it become
      false? If not, it's padding.

## 9. Performance

```bash
curl -sIL -H "Accept-Encoding: br,gzip" $BASE/some-page \
  | grep -Ei 'HTTP/|content-encoding|cache-control|x-content-type|x-frame'
```

- [ ] Brotli/gzip, HTTP/2+, CDN
- [ ] Field CWV from Search Console / CrUX — **not just Lighthouse**
- [ ] LCP ≤ 2.5s · INP ≤ 200ms · CLS ≤ 0.1 at p75 (`reference/05`)
- [ ] Images have explicit dimensions; late-injected content has reserved space
- [ ] Third-party scripts audited — each one justified

## 10. Authority (`reference/09`)

The section most audits skip, and often the real reason a good site doesn't rank.

- [ ] Referring **domains** (not backlinks) — trend over time
- [ ] Is there anything on this site genuinely worth citing? If not, that is the
      finding
- [ ] Any link-scheme exposure — bought links, excessive exchanges, widget
      links, unqualified advertorials (`reference/07` quotes the policy)
- [ ] Are target queries realistic for this domain's authority, or is it
      contesting terms held by 5-year-old incumbents?
- [ ] Impressions trend in Search Console — do new pages get *any* traction
      within ~2 months? (Ranking is bimodal; flat at 3 months means change it,
      not wait)

## 11. Pairs and facets

- [ ] For pair matrices: is the relationship **directional or symmetric**? Both
      directions indexed for a symmetric relationship = self-inflicted duplicate
      content (`reference/01`)
- [ ] Facets: is there a depth threshold, or is the combinatorial space crawlable?
- [ ] Sort/view/page-size parameters canonicalised away
- [ ] Same parameter twice in a URL — blocked?

## 12. Maintenance

- [ ] Someone owns regeneration when underlying data changes
- [ ] Pages with zero impressions get pruned or noindexed
- [ ] Analytics consistent with the site's privacy claims

---

## Findings format

| # | Severity | Finding | Evidence | Fix | Ref |
|---|---|---|---|---|---|
| 1 | Critical / Serious / Minor | | command output or URL | | `reference/0X` |

Rank by impact, not by checklist order. Always include the evidence — a
reproducible command or a URL — so the finding can be verified independently.
