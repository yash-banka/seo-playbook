# Surface plan worksheet (Mode A)

Fill this in **before writing any code**. Method: `reference/00-surface-design.md`.

---

## 0. Product

- **What it does (one sentence):**
- **Pages product necessity alone would require:** ______
  *(Usually small. If large, question whether it's really one product.)*

---

## 0.5 Does this playbook apply?

☐ This site must **manufacture** demand capture (tool, catalogue, marketplace,
  matrix product) → continue

☐ This is a **personal site, portfolio, studio or agency** → **stop.** These
  rank on brand, singular content and earned links. Read "When this playbook
  does not apply" in `reference/00-surface-design.md` and go to
  `reference/09-authority.md` instead. A generated surface would harm you.

---

## 1. Dimensions

| # | Shape | Dimension A | Dimension B | Symmetric? | Raw cells |
|---|---|---|---|---|---|
| 1 | pair matrix / verb×object / entity / attribute×entity / comparison / geo×service | | | ☐ | |
| 2 | | | | ☐ | |
| 3 | | | | ☐ | |

**Total raw cells:** ______  ← enumerate fully; do not cut yet.

**Symmetry check** (`reference/01`): for each pair matrix, is A→B a *different
job* from B→A?
- **Different job** (conversion, directional transfer) → build both, both
  self-canonical, cross-link
- **Same job** (integration, comparison) → **halve the space now**; pick one
  canonical ordering rule (alphabetical is deterministic and generator-friendly)
  and canonical the reverse to it

Getting this wrong doubles your surface with genuine duplicates.

---

## 2. Query framings

For each dimension, how do humans actually ask? Fill all four rows — the
problem row is where the uncontested opportunities are.

| Framing | Example phrasing | Deserves its own page type? |
|---|---|---|
| Transactional | | |
| Problem ("why won't…", "can I…", "is this…") | | |
| Comparison ("X vs Y") | | |
| Should-I ("is X better than Y for Z") | | |

---

## 3. Demand evidence

| Source used | Reliability | Covers |
|---|---|---|
| Search Console / site-search logs | highest | |
| Keyword tool volumes | high | |
| Autocomplete / PAA / related | medium | |
| Competitor URL structures | medium | |
| Community & forum phrasing | medium (best for problem framings) | |
| Reasoned judgement | lowest — **label it** | |

---

## 4. Cuts

**Validity cut** (domain logic — applied *before* demand):
- Rule: ____________________  → removes ______ cells
- Rule: ____________________  → removes ______ cells

**Cannibalisation cut** (cells that would produce competing near-identical pages):
- Merge ____________ into ____________, canonical = ____________

---

## 5. The three fates

| Fate | Criterion | Count | Sitemap? | Indexable? | Built? |
|---|---|---|---|---|---|
| **Indexed** | demand > 0 **and** has ≥1 page-specific section | | yes | yes | yes |
| **`noindex, follow`** | product needs it, ~no demand | | **no** | **no** | yes |
| **Don't build** | neither | | no | n/a | no → true 404 |

**Sanity check:** if "Indexed" ≈ total raw cells, you have not cut. Go back.

**Reality check** (`07`): ~96.55% of pages get zero Google traffic. Every page
in the Indexed column is a bet. What is the evidence for each?

---

## 6. URL patterns

| Page type | Pattern | Example | Tier |
|---|---|---|---|
| Money | `/{section}/{a}-to-{b}` | | |
| Entity | `/{class}/{thing}` | | |
| Tool | `/{section}/{verb-noun}` | | |
| Problem | `/{section}/{question-slug}` | | |
| Hub | `/{section}` | | |

Checks: keyword in slug ☐ · natural query word order ☐ · lowercase-hyphenated ☐
· no dates/IDs ☐ · namespaced by type ☐ · stable forever ☐

---

## 7. Hubs and link mesh

| Page type | Hub URL | Hub exists & returns 200? |
|---|---|---|
| | | ☐ |
| | | ☐ |

Per leaf page:
- **Up →** ____________________
- **Sideways →** ____________ (~12, siblings sharing a dimension value)
- **Backwards →** ____________ (inverse, if one exists)

☐ Every page ≤ 1 click from a hub, ≤ 3 from the homepage

---

## 8. Differentiation source

☐ Facts stored as **structured data**, not prose templates
☐ Each page's distinctive sections are **derived** from those facts
☐ **Rule wired up:** a cell with zero derived sections is automatically
  demoted to `noindex, follow`

Attribute schema sketch:
```yaml
{thing}:
  {attribute}: {value}
  failure_mode: "…"
  weak_at: […]
```

Derivation rules (attribute delta → section):
- `____________ → ____________` triggers section: ____________
- `____________ → ____________` triggers section: ____________

---

## 9. AI posture (`reference/06`)

☐ **A** — maximise AI visibility  ☐ **B** — search-only

Reasoning: ______________________________________________

☐ Content is in raw HTML — *or* client-rendered **plus** an `llms.txt`, since
  most AI crawlers don't execute JS (`reference/05`)

☐ SEO/competitive-intelligence crawlers: allow ☐ / block ☐ — no ranking effect
  either way; blocking hides your surface from competitor tooling

## 9.5 Authority reality check (`reference/09`)

Only ~1.74% of new pages reach the top 10 within a year; ~72.9% of top-10 results
are over 3 years old. A manufactured surface wins by competing **where nobody
showed up**.

☐ Target queries are genuinely low-competition (not head terms held by
  established incumbents)
☐ There is at least one thing on this site worth linking to
☐ Domain age / existing authority: ____________
☐ Realistic first-traction horizon: ____________ *(check indexation in weeks,
  impressions in ~2 months; flat at 3 months means change it, not wait)*

---

## 10. Maintenance

- **Owner:** ____________
- **Regeneration trigger:** ____________
- **Prune review cadence:** ____________ *(noindex or remove pages that have
  never earned an impression)*

---

## Sign-off

☐ Dimensions enumerated as data ☐ Query framings incl. problem-shaped
☐ Demand evidence attached & labelled ☐ Validity cut ☐ Cannibalisation cut
☐ Every cell assigned a fate ☐ Each indexed page has a reason only it can exist for
☐ URL patterns fixed ☐ Hubs specified and resolving ☐ Link mesh specified
☐ Differentiation derived from data ☐ AI posture chosen ☐ Owner assigned
