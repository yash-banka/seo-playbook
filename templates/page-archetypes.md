# Page archetypes

Four skeletons. Pair with `reference/02-content.md` (what makes each section
defensible) and `reference/03-on-page.md` (titles, headings, links).

---

## A. Money page — `/{action}/{a}-to-{b}`

```
<h1>{A} to {B} converter</h1>                    ← natural language, ≠ title tag
<p>{One-line answer with the exact query phrasing.}</p>

[ THE TOOL ITSELF ]                              ← above the prose, always

<h2>Should you {action} {A} to {B}?</h2>         ← honest framing incl. "don't"
<h2>{Pair-specific consequence}</h2>             ← derived from data, e.g.
                                                   "{B} cannot hold transparency"
<h2>{A} and {B} side by side</h2>
  <table> generated from the attribute database </table>
<h2>What usually goes wrong</h2>                 ← expert knowledge, per side
<h2>How to {action} {A} to {B}</h2>
  <ol> numbered steps, ids matching HowTo schema </ol>

<p><a href="/{action}/{b}-to-{a}">{B} to {A} instead</a></p>     ← backwards
<nav aria-label="Other ways to {action} {A}">                    ← sideways
  ~12 sibling links, including noindexed ones
</nav>
<p><a href="/{class}/{a}">About {A} files</a></p>                ← up

<h2>Questions</h2>                               ← must match FAQPage schema
  <h3>…</h3><p>…</p>   × 4–6, real questions only
```

**Index tier:** indexed if the pair has demand *and* at least one derived
pair-specific section. Otherwise `noindex, follow`.

---

## B. Entity page — `/{class}/{thing}`

```
<h1>The {THING} format</h1>
<p>{One-line definition.}</p>

<h2>What {THING} is</h2>            ← origin, who made it, when, why
<h2>What {THING} is good at</h2>    ← <ul> concrete strengths
<h2>Where {THING} falls short</h2>  ← ★ real weaknesses. E-E-A-T signal.
<h2>Converting {THING}</h2>         ← links to every money page from this entity

<h2>Questions</h2>                  ← "what is a .x file", "how do I open .x"
```

The "falls short" section is the highest-value part: nearly impossible to fake,
immediately useful, and it targets *"is X any good"* / *"X vs Y"* intent.

**Index tier:** almost always indexed — entity queries have durable demand.

---

## C. Hub page — `/{section}`

```
<h1>{Section} — {head term}</h1>
<p>{What this section covers.}</p>

<h2>{Grouping}</h2>                 ← group by dimension value, not one flat list
  <ul> every page of this type, descriptive anchors </ul>
```

- **Build the hub before the leaves** (`08` pitfall 5).
- Links every page of its type — high link counts are correct here.
- Targets the section's head term.
- Must be indexable and must resolve. Assert it in CI.

---

## D. Problem page — `/{tool}/{problem-name}`

The highest-leverage archetype and the most neglected.

```
<h1>What is actually inside this file?</h1>   ← the user's real question as H1
<p>{Direct answer in one sentence.}</p>

[ THE TOOL ]

<h2>What this checks</h2>
<h2>What it can't tell you</h2>     ← limits stated plainly
<h2>What to do if it finds something</h2>
```

Derived from the *problem* framings in `00` step 2, not from keyword lists.
Because these queries are phrased as worries rather than keywords, competitors
rarely target them — low competition, high intent. If a real user question
exists that nobody has claimed, claim it.
