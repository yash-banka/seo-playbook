# 06 — AI search: an explicit posture, not an accident

Generative engines (AI Overviews, ChatGPT, Perplexity, Copilot) are both a
**traffic source** and a **content consumer**. Those two facts pull in opposite
directions, and the resulting decision is strategic. Most sites make it by
accident, via a default robots.txt they never read.

**Make it deliberately. Neither posture is the default here.**

---

## The two postures

### Posture A — Maximise AI visibility

*Goal: be cited and surfaced inside AI answers.*

- Allow AI crawlers (GPTBot, ClaudeBot, PerplexityBot, Google-Extended, CCBot…)
- Content fully in raw HTML (`05`) — most AI crawlers do **not** execute JS
- Publish `/llms.txt` — a convention for pointing LLMs at your key content in
  Markdown. **Status as observed: genuinely adopted in one niche, absent
  elsewhere.** Two major developer platforms studied for this playbook serve
  substantial ones (roughly 90KB and 13KB; one is titled, in effect, a root map
  for AI agents), while a large open web reference, an international
  money-transfer service and a major personal-finance publisher serve none.

  So: real traction among developer-facing sites, no traction among publishers.
  Still not a formal standard, still cheap to publish. **It matters most when
  your content is client-rendered** — see the payments-documentation case in
  `05-technical.md`, where a large `llms.txt` is the compensation for HTML that
  AI crawlers cannot read.
- Write extractably: question-shaped headings, a direct answer in the first
  sentence beneath each, self-contained paragraphs that survive being quoted
  without surrounding context
- Strong entity grounding via `@graph` schema (`04`)
- Facts, numbers, dates and named specifics — these get cited; adjectives don't
- Build presence on sources these systems lean on (well-maintained docs,
  reputable third-party mentions)

**Costs:** your content trains and grounds models you don't control; zero-click
answers can satisfy the user without a visit; attribution is inconsistent.

### Posture B — Search-only (what the reference implementation does)

*Goal: full search visibility, no AI training.*

Its `robots.txt` allows normal search crawling while disallowing GPTBot,
ClaudeBot, Google-Extended, CCBot, Bytespider, Amazonbot,
Applebot-Extended and meta-externalagent, and declares Cloudflare's
**Content Signals**:

```
User-agent: *
Content-Signal: search=yes, ai-train=no, use=reference
Allow: /
```

Meaning: index me for search; do not train on me; reference-only use.

- Critically, **`Google-Extended` is disallowed but `Googlebot` is not** — so
  normal search indexing is unaffected. Google states Google-Extended controls
  Gemini/Vertex grounding and training without affecting Search ranking or
  inclusion.
- Content Signals is a **licence expression, not an enforcement mechanism.** It
  states terms; compliance is voluntary and legally untested. Well-behaved
  crawlers honour robots.txt; others ignore it entirely.

**Costs:** you forfeit AI referral traffic and citation presence, in a channel
that is growing.

---

---

## The third crawler category: SEO-intelligence bots

The posture debate is usually framed as search vs AI. There is a third class
that most sites never consider: **backlink and competitive-intelligence
crawlers** — the bots behind commercial SEO toolsets, which crawl the web to
build link graphs and keyword databases.

They are not search engines and not AI trainers. They exist so that *anyone*,
including your competitors, can inspect your backlink profile, your ranking
keywords, and the shape of your page surface.

An international money-transfer service studied for this playbook disallows a
long list of these by name, alongside its ordinary rules.

**The trade:**

| Blocking them | Allowing them |
|---|---|
| Competitors can't audit your link profile or surface | Your own team can use those tools on your site |
| Your strategy is harder to copy | You appear in others' research, case studies and comparisons |
| No effect on Google, Bing, or rankings | Some link-based reputation tooling sees you |

**Worth knowing:** blocking them is invisible to search — these bots have no
relationship with ranking. The cost is mostly to your own tooling and to being
studied by others. For a site whose page surface *is* its strategy — exactly the
kind this playbook teaches you to build — hiding that surface from competitor
tooling is a defensible move, and one that costs almost nothing.

Note also a narrower variant: a major payments platform blocks the Internet
Archive's crawler from its documentation while allowing search engines. The
reasoning is sound — archived, stale API documentation is actively harmful to
users who find it.

---

## Choosing

| If… | Lean |
|---|---|
| Content *is* the product (research, editorial, data) | **B** |
| Content is marketing for a product sold elsewhere | **A** |
| Category is commoditised, discovery is the bottleneck | **A** |
| Content is proprietary or expensively produced | **B** |
| Queries are transactional — user must arrive to act | Either; A costs little |
| Queries are informational — an AI answer fully substitutes | **B** protects, **A** trades traffic for reach |

**A useful middle path:** allow AI crawlers but block them from your highest-value
sections; or allow retrieval/grounding while disallowing training, which the
Content Signals vocabulary is designed to express.

Note the reference implementation's own logic: its pages are *transactional* —
an AI can describe how to convert a file, but the user must still arrive to
actually convert one. That makes forfeiting AI traffic cheap for it, and the
same reasoning may not hold for an informational site.

---

## What serves both postures

Do these regardless of the choice — they are pure upside:

- Content in raw HTML
- Clean semantic structure and question-shaped headings
- Entity-linked `@graph` schema
- Factual specificity — numbers, dates, named things
- Fast, accessible pages
- A direct answer near the top of each page

Posture affects **who may consume** the content. It does not change what good
structure looks like.

## Measuring AI visibility (Posture A)

- Referral traffic from `chatgpt.com`, `perplexity.ai`, `copilot.microsoft.com`
  in analytics — segment it explicitly
- Server logs for AI crawler user agents: are they fetching, and what?
- Periodically ask the assistants your target questions and record whether you
  are cited
- Expect softer measurement than classic SEO. Tooling is immature; be sceptical
  of vendors claiming precise "AI rank tracking".

## Caution

This field moves fast and is full of confident claims with no evidence behind
them. Stick to mechanisms you can verify: crawler access, raw-HTML content,
structure, schema, factual specificity. Treat anything labelled a "GEO ranking
factor" as unproven unless it comes with methodology. (`07-sources.md`)
