# PowerCONCRETE Product Page — Implementation Plan

**Target:** replace the 4-line product panel on mrythm.com with a standalone, shareable,
ultra-modern product page that does the job the white paper currently does.

**Status:** built — live locally at `http://localhost:4173/powerconcrete/`, awaiting review
**Author:** Claude (for Ashok)
**Date:** 2026-08-15

---

## 1. Objective

Today, `mrythm.com` → *product* node opens a sliding panel with ~60 words and a link out to
`powerconcrete.app`. That is a placeholder, not a product page.

We replace it with a dedicated page at **`mrythm.com/powerconcrete/`** that:

- Leads with **WHAT we do** and **WHAT PROBLEM we solve** — not *how* the solver works.
- Is understandable in ~90 seconds by a **precast plant owner / QC manager / mix designer**,
  and by **construction + cement company** people.
- Gives technical readers enough signal that there is real engineering underneath.
- Reads credibly to an investor, without being an investor deck.
- **Replaces sending the white paper around.** This page *is* the artifact we send.

### Audience priority
1. Precast concrete manufacturer (owner, plant manager, QC/lab manager) — must land 100%.
2. Construction / cement company personnel — must be clearly understandable.
3. Technical / engineering readers — must sense the rigour.
4. Investors — should get the shape of the business without being pitched.

### Non-goals
- No solver math, no constraint formulation, no LP/NLP jargon in the main flow.
- Not a feature list. Not a pricing page. Not a login.

---

## 2. Source material

| Source | Use |
|---|---|
| `powerCONCRETE/docs/1b.PowerCONCRETE-Industrial-White-Paper.docx` | Narrative spine, all numbers, all claims |
| `powerCONCRETE/docs/PowerCONCRETE-Constraint-Driven-Optimization-for-Precast-Mix-Design-1.pptx` | 30 rendered visuals + 3 real app screenshots |

The white paper already tells a story in a strong order:
**hidden cost → why it happens → the insight → what we do → what you get → what it's worth → how it deploys.**
The page follows that spine. Every number on the page traces to the white paper.

### Visuals selected (from the deck)

| Deck image | Content | Page use |
|---|---|---|
| `image10.png` | Precast plant hero render w/ wordmark | Hero background |
| `image14.png` | Drift → buffering → compounding → $160k–$480k | Act I — the cost |
| `image30.png` | No envelope → no trade-off → conservative heuristics | Act I — why it happens |
| `image17.png` | Constrained mix space, feasible region, lowest-cost point | Act II — the key insight (hero visual) |
| `image2.png` | Build space → enforce constraints → optimize cost | Act II — what we do |
| *(rebuilt as SVG)* | Precast manufacturing process, PowerCONCRETE's slot | Act III — where it fits |
| `image9.png` | Optimized mix dashboard (cost, mass, w/cm, carbon, PSD) | Act III — what you get #1 |
| `image1.png` | Scenario A vs B delta comparison | Act III — what you get #2 |
| `image4.png` | Cost waterfall + EPD report + constraint slack | Act III — what you get #3 |
| `image23.png` | Cost vs carbon frontier, overdesign vs optimal | Act IV — cost & carbon align |
| `image21.png` | Deterministic / constraint-first / transparent | Act IV — why deterministic |
| `image15.png` | "heuristic practice → deterministic system" | Closing |

All images are 1–4 MB PNGs. They get resized + converted to WebP (with JPEG fallback path
kept simple: WebP only — universally supported since 2020) targeting **< 300 KB each**,
**< 3 MB total page weight**.

---

## 3. Information architecture

Eleven blocks. Ruthlessly short copy — the visuals carry the weight.

```
0  STICKY BAR      mrythm · PowerCONCRETE          [ Launch PowerCONCRETE app ]   ← always visible
1  HERO            What it is, in one sentence. Primary CTA at the top.
2  THE COST        "$160k–$480k a year. Nobody budgeted for it."
3  WHY             Why every good plant does this. (no blame — this is rational behaviour)
4  THE INSIGHT     Your mix is not a recipe. It is a point in a space nobody has mapped.
5  WHAT WE DO      Three moves: build the space, enforce the limits, find the cheapest point.
6  WHERE IT FITS   One box in the plant. Nothing else changes.
7  WHAT YOU GET    Three real screens from the product.
8  COST + CARBON   Cheaper and lower-carbon are the same direction. 12–19%.
9  WHAT IT'S WORTH The economics table → ≈$275,000/yr, animated.
10 WHY THIS, NOT   vs spreadsheets · vs batch automation · vs ML. Deterministic/constraint-first/transparent.
11 CLOSE           One line + CTA + who built it + contact.
```

### Copy principles
- Second person. "Your plant", "your lab", "your mix".
- No sentence longer than ~22 words in the main flow.
- Every claim that carries a number is labelled as representative where the white paper does.
- Minimal machinery vocabulary. "Solver" appears three times, always as the thing that
  *refuses* to break a constraint — never as a technical boast.

---

## 4. Design direction — "engineering paper, ultra modern"

The deck's own style brief says: *modern; engineering/physics/software; deterministic,
constrained, transparent; light theme; grid lines in background; flat, minimalist, not 3D;
no glow, no floating elements.* The page honours that, and inherits mrythm's identity.

**System**
- Type: **Syne** (display, from mrythm.com) + **JetBrains Mono** (eyebrows, data, labels).
  Body at a large, confident size; display at heavy negative tracking.
- Palette:
  - paper `#f7f6f2` / ink `#111111` / muted `#6f6e6a` / hairline `#dcdad0`
  - accent **`#e85d22`** (mrythm orange — carried over exactly)
  - deep `#0e1113` for the hero and the closing band
- Motifs carried from mrythm.com:
  - **4px orange anchor squares** at rule intersections
  - hairline **grid / graph-paper** background at 3% opacity
  - **radar ring** echo behind the insight block
- Layout: 12-column, generous whitespace, asymmetric section headers, full-bleed visuals.
- **Band alternation** so scrolling has rhythm: accent hero → paper → white → accent insight
  → paper → … → accent close.

**Light and dark themes**

Older readers often can't read light-on-dark, so the accent bands (hero, key insight, cost +
carbon, close, footer) come in two skins and the reader picks:

| | Light theme | Dark theme |
|---|---|---|
| accent band | warm paper `#eeebe2` | near-black `#0e1113` |
| text on it | ink `#111` | white |
| hero photo | 50% opacity, reads as a faint architectural texture | full strength |

The rest of the page — the bulk of the reading — is light in **both**, because all twelve
figures have light backgrounds. A full inversion would put twelve glaring white rectangles on
a black page, which is worse for exactly the readers this is meant to help.

Mechanics:
- Every dark value is a CSS custom property (`--deep`, `--deep-fg`, `--deep-muted`,
  `--deep-line`, `--deep-dim`, `--deep-rgb`, `--hero-photo`). Nothing is hardcoded.
- **Default follows the reader's OS setting** via `prefers-color-scheme`, so most people land
  on light without touching anything.
- A labelled toggle (☀ Light / 🌙 Dark — icon *and* word, not a bare icon) sits top-right of
  the hero and in the sticky bar. The choice persists in `localStorage` and is applied by a
  tiny inline script in `<head>`, so there is no flash on load.
- Body text was also enlarged slightly and `--muted` darkened to `#5d5c58` (≈5.9:1 on paper)
  for the same readability reason.

**Motion** (restrained — the deck brief says no glow, no floating)
- `IntersectionObserver` reveal: 12px rise + fade, staggered, 500ms, one-shot.
- Hero: hairline grid draws in on load; wordmark letter-spacing settles.
- Block 9: number counts up to $275,000 once, when scrolled into view.
- Sticky bar: appears after hero, progress hairline across the top.
- Everything respects `prefers-reduced-motion`.

**The process diagram is drawn, not pasted**

The precast-process figure was the one weak visual — a low-resolution screenshot lifted from
the Word document, with several labels barely legible. It has been rebuilt as an **inline SVG**:

- Crisp at any zoom, any screen, any print. No raster to go soft.
- Laid out in **two rows of six and five**, with a wrap connector from *Curing* down to
  *Stripping*. A single row of eleven boxes forced the labels down to ~11.6px at column width;
  two rows put them at **19px** — larger than the body text.
- Themed from the same CSS variables as everything else, so it tracks light/dark and any future
  palette change automatically.
- Below 1060px it scrolls horizontally inside its own frame at full size rather than shrinking
  the type, with a visible scroll hint.
- The dashed boundary is now labelled *"plant process — treated as fixed"*, and PowerCONCRETE
  sits outside it in accent orange — which is precisely the argument of that section.
- Also fixes the source document's "Proccess" / "Demoiding" typos, and carries a real `<title>`
  and `<desc>` for screen readers.

**Screenshot handling**
- The three product screens are dense. Each sits in a browser-chrome-less frame with a
  hairline border and a caption, and is **click-to-zoom** (lightbox, esc/click to close)
  so a plant manager can actually read the numbers.

**Responsive**
- ≥1200px: full experience, side-by-side figure/text.
- 768–1199: single column, visuals full width.
- <768: stacked, hero CTA moves inline, sticky bar collapses to just the launch button.

**Accessibility**
- Real semantic landmarks, `alt` on every figure, ≥4.5:1 contrast on all body text,
  visible focus rings, keyboard-operable lightbox.

---

## 5. Files

```
mrythm.com/
├── index.html                      ← MODIFIED (product node now navigates to the page)
├── _config.yml                     ← NEW  (keeps docs/ out of the published site)
├── robots.txt                      ← NEW
├── sitemap.xml                     ← NEW
├── .gitignore                      ← MODIFIED (ignore .claude/)
├── powerconcrete/
│   ├── index.html                  ← NEW  (the whole page; self-contained CSS+JS)
│   └── i/
│       ├── og.jpg                  (1200×630 social share card)
│       ├── hero-plant.webp
│       ├── cost-chain.webp
│       ├── why-heuristics.webp     (process-fit.webp deleted — now inline SVG)
│       ├── feasible-space.webp
│       ├── what-it-does.webp
│       ├── process-fit.webp
│       ├── screen-optimized-mix.webp
│       ├── screen-scenario-compare.webp
│       ├── screen-cost-carbon.webp
│       ├── cost-vs-carbon.webp
│       ├── deterministic.webp
│       └── close.webp
└── docs/
    └── mrythm-product-pC-impl-plan.md   ← this file (repo only — never published)
```

Single self-contained HTML file (matching how `index.html` is built today) — no build step,
no dependencies, drops straight onto GitHub Pages. Total image payload ≈ 840 KB.

### Change to `index.html`
The *product* node, the mobile *product* nav item, and the `PowerCONCRETE` label in the
left-hand footer grid all now navigate to `/powerconcrete/`. The old `#panel-product` block is
removed. A small deep-link handler was added so `/?panel=contact` opens the contact panel —
that is what the product page's "Talk to us" button uses. Everything else is untouched.

### Keeping this plan out of the published site
This document is meant for the team on GitHub, **not** for visitors. Three layers:

1. **No link to it exists** anywhere on the site — verified.
2. **`_config.yml`** lists `docs/` under `exclude`, so GitHub Pages' Jekyll build does not copy
   it into the published site at all. There is no `mrythm.com/docs/...` URL to find.
3. **`robots.txt`** disallows `/docs/` as a belt-and-braces measure (also covers the
   `ashokrs.github.io/mrythm.com/` mirror).

The file stays in the repository and is readable on github.com by anyone with repo access.

---

## 6. Execution steps

1. ✅ Extract white paper text + deck slide text/images.
2. ✅ Review all 30 deck visuals; select the 12 above.
3. ✅ Optimize + convert selected images → `powerconcrete/i/*.webp` (all ≤ 120 KB).
   The precast-process figure was cropped to the flow diagram only — this also drops the
   "Proccess" typo that is in the source document.
4. ✅ Write `powerconcrete/index.html` — structure, copy, CSS, JS.
5. ✅ Wire `index.html` product node → `/powerconcrete/`; add `/?panel=contact` deep link.
6. ✅ Keep `docs/` out of the published site (`_config.yml`, `robots.txt`).
7. ✅ Serve locally on `http://localhost:4173`; reviewed at 1400px and 375px.
   Verified: no horizontal overflow, lightbox opens/closes, counter lands on $275,000,
   homepage → product navigation, `/?panel=contact` deep link.
8. ✅ Add light/dark themes with a labelled toggle; default to the reader's OS setting.
   Verified both themes at 1400px and 375px, plus the sticky bar at 375px.
9. ✅ Rebuild the precast-process figure as an inline SVG (see §4); delete `process-fit.webp`.
10. Iterate on copy/design with Ashok.
11. Hand over — Ashok commits to GitHub himself.

### Run it locally

```bash
cd /Users/ashok/Code/apps/mrythm.com && python3 -m http.server 4173
```

Then open `http://localhost:4173/powerconcrete/`.

---

## 7. Copy deck (source of truth for the page)

> Draft. Refine in place during step 7.

**HERO**
> eyebrow: `MRYTHM / PRODUCT`
> PowerCONCRETE™
> **Precast plants pay for concrete they don't need.**
> PowerCONCRETE finds the cheapest mix that still meets every one of your requirements —
> strength, finish, durability, workability, color, carbon — and proves why it's safe.
> `[ Launch PowerCONCRETE app ↗ ]` `[ See how ↓ ]`

**2 · THE COST**
> `THE HIDDEN CEMENT TAX`
> **$160,000 – $480,000 a year. Nobody budgeted for it.**
> Cement chemistry shifts between mill runs. Quarries evolve, so gradation drifts. SCMs vary.
> Your lab absorbs all of it the only way it safely can: a few extra pounds of cement here, a
> wider margin there. It works. It also never comes back off.
> For a plant at 20,000–40,000 yd³/yr, 40–60 lb/yd³ of accumulated caution is $160k–$480k a
> year — spent every year, embedded in recipes nobody has re-derived from first principles.

**3 · WHY**
> `THIS IS NOT A MISTAKE`
> **Every good plant does this. It's the rational move.**
> Stripping failure is expensive. Surface defects get rejected. Curing cycles are locked to the
> schedule. When uncertainty goes up, adding margin is fast and recalculating is not.
> The problem isn't the caution. The problem is that **nobody has ever calculated where the
> safe edge actually is** — so caution is the only available instrument.

**4 · THE INSIGHT**
> `THE KEY INSIGHT`
> **Your mix isn't a recipe. It's one point in a space you've never mapped.**
> For any product you make, thousands of mixes satisfy every requirement you have. That set is
> real, it's bounded by physics and chemistry, and it can be computed.
> Your current mix sits somewhere inside it. Almost certainly not at the cheap end.

**5 · WHAT WE DO**
> `WHAT POWERCONCRETE DOES`
> **Three moves.**
> 1. **Builds the full feasible space** — every mix your materials can actually produce.
> 2. **Enforces your constraints as hard walls** — strength, w/cm, SCM limits, packing,
>    rheology, color, carbon cap. Not preferences. Walls.
> 3. **Finds the lowest-cost point inside** — and shows which limits are holding it there.
> Nothing is inferred from history. It is computed from your materials and your rules.

**6 · WHERE IT FITS**
> `DEPLOYMENT`
> **One box in your plant. Nothing else moves.**
> Your process, curing, equipment, labor, and QC procedures are inputs, not variables. The lab
> uses PowerCONCRETE during mix design; the mix then goes through your normal validation and
> your existing batching system.
> No new equipment. No retraining production. No change to quality control.

**7 · WHAT YOU GET**
> `THE OUTPUT`
> **An answer, and the reason for it.**
> - *Optimized mix* — cost/yd³, full batch, w/cm, air, carbon, packing checked against ASTM and Tarantula.
> - *Scenario comparison* — base vs cost-focus vs carbon-focus, with exact deltas.
> - *Cost waterfall, EPD report, constraint slack* — where the money went, what your carbon is,
>   and which constraints are binding.

**8 · COST + CARBON**
> `CARBON`
> **Cheaper and lower-carbon turn out to be the same direction.**
> Carbon-focus scenarios cut embodied carbon **12–19%** against base mixes while holding or
> reducing material cost. In mixes already engineered for low carbon, below **120 kg CO₂e/yd³**
> at essentially no premium.
> You don't have to choose. And when a GC asks for an EPD, you have one.
> *(Representative model results; plant-specific values depend on local materials, cement
> factors, SCM availability, and performance constraints.)*

**9 · WHAT IT'S WORTH**
> `ECONOMICS`
> 25,000 yd³/yr · 750 lb/yd³ baseline · $0.20/lb cement · −55 lb/yd³ · $11.00/yd³
> **≈ $275,000 recurring, every year.**
> No operational disruption. No new equipment. No staffing changes. The saving is structural —
> it comes from replacing conservative buffering with engineered constraint control.

**10 · WHY THIS, NOT SOMETHING ELSE**
> `POSITION`
> **Deterministic · Constraint-first · Transparent**
> - **vs spreadsheets** — a spreadsheet checks one candidate mix. It can't define an envelope
>   or solve for an optimum. It executes judgment; it doesn't expand it.
> - **vs batch automation** — dispatch, moisture, inventory, scheduling. Valuable, complementary,
>   and entirely downstream of the mix decision.
> - **vs machine learning** — prediction in exchange for enforcement. When stripping is on the
>   line, *why* a mix is safe matters as much as that it probably is.
> The solver cannot return a mix that violates one of your constraints. Safety is enforced,
> not weighted.

**11 · CLOSE**
> **PowerCONCRETE turns mix design from heuristic practice into a deterministic,
> economically optimized system.**
> `[ Launch PowerCONCRETE app ↗ ]` `[ Talk to us ]`
> Built by Mrythm — Suresh Sundaram (PhD, MIT; 24 yrs Aspen Technology) and
> Ashok Subramanian (AT&T Labs, Oracle, AspenTech, Dassault Systèmes).

---

## 8. Open questions for Ashok

Built under these assumptions — say the word and any of them changes in minutes.

1. **Trademark** — `™` appears on the hero wordmark and in the footer strapline only; plain
   `PowerCONCRETE` everywhere else.
2. **Contact CTA** — "Talk to us" goes to `../?panel=contact`, which opens the existing contact
   form on the homepage. Alternative: a `mailto:`.
3. **White paper download** — not offered. The page is the artifact you send instead.
4. **Product panel** — the homepage *product* node now navigates straight to the new page
   rather than opening a sliding panel. If you'd rather keep a panel with a "read more" link,
   that's a small change.
5. **Spelling** — US throughout (`color`, `labor`, `judgment`, `optimization`), matching the
   homepage and the white paper.
6. **App link** — `https://powerconcrete.app/`, opening in a new tab.
