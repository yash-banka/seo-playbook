# 02 — Content: the differentiation standard

The hardest part of a manufactured surface is that every page must deserve to
exist. This file defines the standard, with measurements.

## The measurement

Body-text vocabulary overlap between pages of the reference implementation:

| Comparison | Overlap |
|---|---|
| Same family (`png-to-jpg` vs `png-to-webp`) | **84%** |
| Cross family (`png-to-jpg` vs `mp4-to-mp3`) | **44%** |

Roughly 600 words per page.

**Read this honestly: 84% is close to the line.** Sibling pages share most of
their vocabulary. What makes them defensible is not the size of the difference
but its *nature* — the differing fraction is the substantive answer to that
page's specific query, not reworded filler.

The `png-to-jpg` page argues:
- when you should *not* do this conversion (screenshots, logos, line art)
- that the target format cannot carry transparency, so transparent areas must
  become a real colour, chosen before conversion, irreversibly
- that repeatedly re-saving the lossy format compounds damage

None of that appears on `png-to-webp`, because none of it is true there — the
target keeps transparency. The pages differ **because the underlying facts
differ**, and that is the only durable source of differentiation.

**The test:** could this page's distinctive paragraph be moved to a sibling
without becoming false? If yes, it isn't differentiation — it's padding.

## What makes a generated page defensible

1. **A fact only this page can state.** Grounded in the actual properties of the
   thing. Not an adjective swap.
2. **Advice against the action.** "Don't do this when…" is nearly impossible to
   fake, immediately useful, and a strong quality signal. Entity pages in the
   reference implementation carry an explicit *"where this format falls short"*
   section listing real weaknesses.
3. **The failure mode.** "What usually goes wrong" — the thing an expert knows
   and a template doesn't.
4. **A structured comparison.** A small attribute table (lossless? transparency?
   animation?) generated from a real attribute database is legitimately useful
   and scannable, and it is *data*, so it is true by construction.
5. **The specific answer to the specific question.** Not the generic answer with
   a noun substituted.

## What makes it a doorway page

- Same text with the keyword swapped
- Spun synonyms ("convert" / "transform" / "change")
- Filler padding to a word count
- No advice that could ever discourage the action
- Claims that are false for this particular combination because the template
  didn't know the difference
- Pages for combinations nobody searches for **and** the product can't do well

Google's spam policies name **scaled content abuse** and **doorway pages**
explicitly (`07-sources.md`). The distinguishing question is not "was this
generated?" but **"does each page provide value to the user?"** Generation is not
the violation; valuelessness is.

## Practical content model

Store facts as **structured data**, and let the template compose prose from
them. The differentiation then comes from the data, and is true by construction:

```
formats:
  png:
    lossless: true
    alpha: true
    animation: false
    good_for: [screenshots, logos, line art, anything with hard edges]
    weak_at: [photographs — large files]
    failure_mode: "Photos saved as PNG are several times larger than JPG."
  jpg:
    lossless: false
    alpha: false
    failure_mode: "Re-saving repeatedly re-encodes damage and compounds it."
```

Then the page's argument is *derived*: `alpha: true → alpha: false` triggers the
transparency-loss section. `lossless: true → lossless: false` triggers the
quality-trade section. A pair where neither applies gets neither section — and
if a pair ends up with *no* derived sections, that is your signal it should be
`noindex, follow`, because you have nothing specific to say about it.

**This is the key implementation insight:** let the data decide which pages have
something to say, and let that decide the index tier.

## Length

600 words is enough when every word is specific. Length is not a ranking factor;
completeness relative to the query is. A 300-word page that fully answers a
narrow question beats a 2,000-word page padded to hit a target. Write until the
question is answered, then stop.

## Page-level structure that works

1. **H1** — natural language, not the stuffed title tag
2. **One-line answer** immediately, before any preamble
3. **The tool / action itself** — high on the page, not below an essay
4. **"Should you do this?"** — the honest framing, including when not to
5. **The specific consequence** — what changes, what is lost, what is gained
6. **Comparison table** — from structured data
7. **What goes wrong** — the expert knowledge
8. **How to, numbered** — matches `HowTo` schema
9. **Related links** — inverse, siblings, entity hub
10. **FAQ** — matches `FAQPage` schema, real questions only

Put the answer and the tool above the explanation. A visitor who came to do a
job should be able to do it without scrolling; the prose serves the crawler and
the visitor who wants to understand. Never make the visitor scroll past an essay
to reach the thing they came for — that trade costs engagement, and engagement
is worth more than the word count.

## Maintenance

Generated surfaces rot silently. Assign an owner and a regeneration trigger.
When the underlying data changes, every derived page must change. If nothing
owns this, the surface becomes a liability of stale, wrong pages — which is
worse than never having built it.
