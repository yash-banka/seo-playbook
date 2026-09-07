# 03 — On-page: titles, metadata, headings, internal links

## Title tag

**Formula:** `{Keyword, natural order} – {Differentiator} | {Brand}`

Example: `HEIC to JPG Converter – Free, No Upload | Brand`

Rules:
- **Keyword first.** It is the strongest on-page signal and the part that
  survives truncation.
- **The differentiator earns the click.** "Free, No Upload" pre-empts the two
  objections every visitor in that category has. Your differentiator should
  answer the reader's biggest hesitation, not describe your feature set.
- **Brand last**, after a pipe. Drop it entirely if the title is long.
- ~60 characters / ~580px before truncation. Front-load accordingly.
- Unique on every page. Duplicate titles across a generated surface are a
  reliable symptom that the pages are duplicates too.
- Don't stuff. `PNG to JPG Converter Convert PNG JPG Online Free` reads as spam
  to both classifiers and humans.

Google frequently rewrites titles. Write the honest, specific one; rewriting is
usually triggered by stuffing, boilerplate, or a mismatch with page content.

## Meta description

Not a ranking factor. It is **ad copy for the SERP** and it drives click-through,
which matters.

Observed pattern: `Convert PNG to JPG free, in your browser. No upload, no
account, no size limit. Your image is converted on your own device and never
sent anywhere.`

- Repeat the exact query phrasing (it gets bolded in results)
- **Stack the objection-killers**: free / no account / no limit / private
- State the mechanism that makes the promise credible ("on your own device")
- ~155 characters
- Unique per page

The reference implementation reuses one description shape across its whole
surface, with the format names substituted. That is acceptable here *because* the
differentiators are identical and true everywhere. If your pages differ in what
they offer, the descriptions must differ too.

## Headings

- **One `<h1>` per page, and it should not equal the title tag.** Title is for
  the SERP and is keyword-shaped; H1 is for the reader and is natural.
  Observed: title `PNG to JPG Converter – Free, No Upload | Brand`,
  H1 `PNG to JPG converter`.
- H2s should read as **the questions the reader actually has** — "Should you
  convert PNG to JPG?", "JPG cannot hold transparency", "What usually goes
  wrong". These map to long-tail queries and to AI extraction, both of which
  reward question-shaped structure.
- Never skip levels. Never style with heading tags.
- The most distinctive pages use a **question as the H1** — "Can anyone tell
  where this photo was taken?", "What is actually inside this file?" — targeting
  problem-shaped intent with almost no competition. Worth doing wherever a real user question exists that
  nobody has claimed.

## The internal link mesh

Every leaf page carries four link classes:

1. **Backwards — the inverse.** `PNG to JPG` links `Convert JPG to PNG instead`.
   Captures visitors who arrived at the wrong direction, and builds a
   bidirectional mesh.
2. **Sideways — siblings.** ~12 links to pages sharing a dimension value ("Other
   ways to convert PNG"). Note the reference implementation deliberately links
   siblings that are themselves `noindex, follow` — pushing equity and users
   toward the complete product, not only the indexed subset.
3. **Up — the entity hub.** "About PNG files" → the format's entity page. This
   connects transactional pages to the informational layer and is what makes the
   entity tier pay for itself.
4. **Sitewide — header and footer.** Hub links on every page.

Rules:
- **Descriptive anchor text.** "Convert JPG to PNG", never "click here".
- Keep the count sane per page. ~12 siblings is generous and readable; hundreds
  of leaf links dilutes each one.
- Hub pages are the exception — the reference implementation's conversion hub
  carries close to a thousand links, which is correct for a hub: it exists to
  distribute crawl access.
- **Every page within one click of a hub, three of the homepage.**
- Links must be real `<a href>` in the served HTML. Click handlers and
  JS-constructed hrefs are not reliably followed.

## Head boilerplate

Every page needs, and `templates/head-block.html` provides:

- `<meta charset>` and viewport
- Unique title + description
- Self-referencing canonical
- Open Graph: type, site_name, title, description, url, image (1200×630),
  image:alt
- Twitter: `summary_large_image`, title, description, image
- One JSON-LD `@graph` (`04-schema.md`)
- `theme-color`, `color-scheme`
- Favicon — an inline `data:` SVG costs zero requests

## Accessibility that doubles as SEO

The reference implementation does these, and they are the same work:

- Skip-to-content link
- `<main>`, `<nav aria-label="…">`, `<section>`, real `<table>` markup with
  `<th scope="row">`
- Descriptive `alt` on meaningful images, empty `alt=""` on decorative ones
- `<noscript>` fallback explaining what the page needs

Semantic structure is how both assistive technology and extraction systems
determine what a page is about.
