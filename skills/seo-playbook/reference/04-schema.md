# 04 — Structured data: one linked entity graph

## The principle

Emit **one** `<script type="application/ld+json">` per page containing an
`@graph` array, with entities cross-referenced by `@id`. Not several
disconnected blobs.

The `@id` linking is what turns separate declarations into a machine-readable
statement about *one* organisation publishing *one* site running *one*
application — instead of three unrelated facts.

## The stable base graph (identical on every page)

```json
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Organization",
      "@id": "https://example.com/#organization",
      "name": "Brand",
      "url": "https://example.com/",
      "logo": {"@type":"ImageObject","url":"https://example.com/logo.png","width":512,"height":512},
      "description": "…",
      "slogan": "…"
    },
    {
      "@type": "WebSite",
      "@id": "https://example.com/#website",
      "url": "https://example.com/",
      "publisher": {"@id": "https://example.com/#organization"},
      "inLanguage": "en",
      "potentialAction": {
        "@type": "SearchAction",
        "target": {"@type":"EntryPoint","urlTemplate":"https://example.com/search?q={search_term_string}"},
        "query-input": "required name=search_term_string"
      }
    },
    {
      "@type": "WebApplication",
      "@id": "https://example.com/#app",
      "applicationCategory": "MultimediaApplication",
      "operatingSystem": "Any browser",
      "publisher": {"@id": "https://example.com/#organization"},
      "isAccessibleForFree": true,
      "offers": {"@type":"Offer","price":"0","priceCurrency":"USD"}
    }
  ]
}
```

Notes:
- `publisher: {"@id": …}` is the linking mechanism — a reference, not a copy.
- **`SearchAction` is legacy.** It declared the sitelinks search box, which
  Google retired globally on 21 November 2024 and removed from Search Console
  and the Rich Results Test. Google says removing the markup is *not* required
  (unsupported data causes no harm) and that **the `WebSite` entity itself
  should stay**, because site-name display still uses a variation of it. So:
  keep `WebSite`, treat `SearchAction` as optional dead weight, and never
  present it as a live feature. The reference implementation still emits it —
  an example of markup outliving the feature it was written for. (`07`)
- `offers` with `price: "0"` plus `isAccessibleForFree` is how "free" becomes a
  machine-readable fact rather than a marketing word in a title.

## Per-page additions

**Money / action pages** — append `HowTo` and `FAQPage` and `BreadcrumbList`:

```json
{
  "@type": "HowTo",
  "name": "How to convert PNG to JPG",
  "totalTime": "PT1M",
  "supply": [{"@type":"HowToSupply","name":"A PNG file"}],
  "tool":   [{"@type":"HowToTool","name":"Brand"}],
  "step": [
    {"@type":"HowToStep","name":"Add your file","text":"…","url":"https://example.com/convert/png-to-jpg#add"},
    {"@type":"HowToStep","name":"Check the settings","text":"…","url":"https://example.com/convert/png-to-jpg#settings"},
    {"@type":"HowToStep","name":"Convert and download","text":"…","url":"https://example.com/convert/png-to-jpg#download"}
  ]
}
```

Each step carries a **fragment URL to the actual element on the page**. That is
the detail most implementations skip.

**Entity pages** — `FAQPage` + `BreadcrumbList`, and consider `DefinedTerm` or
`TechArticle` where the page is genuinely definitional.

**Every page** — `BreadcrumbList` matching the visible breadcrumb trail:

```json
{
  "@type": "BreadcrumbList",
  "itemListElement": [
    {"@type":"ListItem","position":1,"name":"Home","item":"https://example.com/"},
    {"@type":"ListItem","position":2,"name":"Conversions","item":"https://example.com/convert"},
    {"@type":"ListItem","position":3,"name":"PNG to JPG","item":"https://example.com/convert/png-to-jpg"}
  ]
}
```

## Hard rules

1. **Schema must describe what is visibly on the page.** FAQ schema requires the
   Q&A to be visible to the user. Marking up content that isn't there is a
   structured-data violation and risks a manual action.
2. **Never emit *competing* declarations of the same entity.** Note the precise
   rule: multiple JSON-LD blocks are not themselves a problem. A large B2B
   automation platform ships ten separate blocks on a single page and ranks
   dominantly — but it also ships **two `BreadcrumbList` blocks**, which *is*
   the bug, because two conflicting breadcrumb trails leave a parser to guess.

   The common failure is a second block re-declaring `WebSite` and
   `WebApplication` *without* `@id` anchoring, creating unlinked duplicates of
   entities the first block had carefully linked — usually a legacy block a
   template kept appending. Both validate in isolation, so nothing flags it.

   One `@graph` remains the cleanest approach and is what this playbook
   recommends. But if your framework emits several blocks, the thing to enforce
   is **no entity declared twice**, not block count. **Check the rendered
   output, not just the template.** (`08-pitfalls.md` pitfall 6)
3. **`FAQPage` and `HowTo` no longer produce rich results at all.** Restricted
   in 2023, then fully deprecated: How-to rich results were dropped from desktop
   and mobile, and FAQ rich results stopped appearing in Google Search in May
   2026, with reporting and Rich Results Test support removed over the following
   months. Google's guidance is that the markup is harmless to leave in place —
   it simply produces nothing visible in Search.

   **So emit them for a different reason, or not at all.** The remaining
   justification is entity understanding and AI extraction (see below), which is
   real but is *not* a SERP feature. Never sell a client or a stakeholder on
   "FAQ rich results" — that product no longer exists. (`07-sources.md`)
4. **Validate.** Google Rich Results Test and Schema Markup Validator, against
   the *rendered* page.
5. Keep it truthful. Ratings, prices, and availability that don't match the page
   are the fastest route to a structured-data manual action.

## Why this matters more for AI than for rich results

Rich-result eligibility keeps narrowing, so the classic justification is
weakening. The stronger reason now: an explicit entity graph is the cleanest
statement a page can make about what it is, who published it, and how it relates
to everything else. That is precisely what retrieval and grounding systems
consume. Schema has quietly shifted from a SERP-decoration tactic to an
AI-legibility one. (`06-ai-search.md`)
