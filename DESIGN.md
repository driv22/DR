# DESIGN.md — driv.space

The visual system for Dan Rivero's site. Written from the built world. If you change the site, change this file.

Companion: `PRODUCT.md` holds the facts and the voice. This file holds the look.

---

## 1. The world

**The site is a personnel change record printed on green-bar continuous-feed listing paper.**

Not "inspired by." It behaves like one. The masthead is a `FROM → TO` change block with an `IN EFFECT` stamp. The career is a register of `CHG-01`…`CHG-07` rows read newest-first. Credentials are a table with issue dates and status. The 404 is `NO SUCH SHEET · not on file`.

**Why this world.** Green-bar paper is the shared artifact of both worlds Dan has lived in: the datacenter and the trading floor. Its alternating printed bands are *structurally* useful on a page that is mostly records. It is a light ground, which is correct for the real use scene (a laptop in a bright office). And it avoids the three saturated AI clusters: warm cream + serif + terracotta; near-black + neon accent; broadsheet hairlines + tracked mono.

**The thesis the world serves.** A runbook is what you write so somebody else can do the work without you. That is the whole career, and it is why the promotion to people leadership makes sense. A change record is the artifact of making other people capable — which is the argument the site is making.

**Mode: Persuade.** The visitor should decide Dan is worth contacting.

---

## 2. Tokens

Defined on `:root`, overridden on `html[data-theme='night']`. Theme is persisted in `localStorage` under `driv-theme` and applied by an inline pre-paint script so there is no flash.

### Color

| Token | Day | Night | Use |
|---|---|---|---|
| `--paper` | `#f7f8f5` | `#101310` | Page ground |
| `--band` | `#dde8d6` | `rgba(163,208,152,0.085)` | Printed band, odd rows |
| `--band-soft` | `#ebf1e7` | `rgba(163,208,152,0.042)` | Printed band, even rows |
| `--ink` | `#12150f` | `#e9ede6` | Body and display type |
| `--ink-2` | `#4a5147` | `#a3ad9d` | Secondary prose, field labels |
| `--rule` | `#12150f` | `#e9ede6` | Structural rules |
| `--rule-soft` | `#c2c9ba` | `rgba(233,237,230,0.19)` | Row separators |
| `--stamp` | `#a63417` | `#ff9b73` | **The one accent.** Stamp-pad vermilion |
| `--stamp-on` | `#ffffff` | `#1c0f07` | Text on a stamp fill |
| `--perf` | `#d2d9c9` | `rgba(233,237,230,0.10)` | Tractor-feed holes |
| `--inverted-bg` | `#12150f` | `#1c2119` | Council band |
| `--inverted-ink` | `#f2f4ef` | `#eef2e9` | Type on the council band |
| `--inverted-stamp` | `#ff9b73` | `#ff9b73` | Accent on dark grounds |

**Contrast.** Every pair was computed. The lowest ratio on the page is 5.31:1 (`--stamp` on `--band`). Everything passes WCAG AA; most body pairs clear AAA.

**The accent is a single ink at two values.** `#a63417` on light grounds, `#ff9b73` on dark. It is not a second color; it is the same stamp pad reading correctly on a dark sheet.

**The accent has one meaning: *in effect*.** It marks the `IN EFFECT` stamp, the left edge of the current register row, active credential status, the live section in the strip, and the primary action. Nothing else may take it. A forward-looking aside gets a neutral ink rule, not vermilion — proposals are not in effect.

### Type

Two families. No third.

- **Archivo** (variable, `wdth 62..125`, `wght 400..800`) — everything with a voice. A form/signage grotesque, so it belongs to the world rather than decorating it.
- **Courier Prime** (400/700 + italic) — **record fields only**: IDs, dates, counts, status, field labels, buttons. Mono here is data and measurement, never a costume for "technical."

| Step | Value |
|---|---|
| `--step-xs` | `0.6875rem` |
| `--step-s` | `0.8125rem` |
| `--step-m` | `clamp(1rem, 0.95rem + 0.25vw, 1.125rem)` |
| `--step-l` | `clamp(1.35rem, 1.15rem + 1vw, 1.85rem)` |
| `--step-xl` | `clamp(1.9rem, 1.4rem + 2.4vw, 3.25rem)` |
| `--step-2xl` | `clamp(2.9rem, 1.6rem + 6vw, 6rem)` |

Display type is set at `font-stretch: 112%`, `letter-spacing: -0.045em`, `line-height: 0.86`. Prose runs `1.6` at a `68ch` max measure; register bodies are capped at `54ch`.

### Layout

`--shell: 78rem`, `--gutter: clamp(1.25rem, 4vw, 3.5rem)`. Breakpoints: 1180 (perforation off), 980, 960, 860, 720, 620, 520, 380.

---

## 3. Components

- **`.perf`** — two fixed SVGs down the page edges, filled with a 30px `<pattern>` of `r=3.75` circles. The sprocket holes on fanfold paper. Hidden below 1180px and in print. The pattern is filled via `fill: var(--perf)` on `.perf-dot`, **not** `currentColor` — `currentColor` inside a `<defs>` resolves against the defs element, not the referencing element.
- **`.strip`** — a `<header>`, fixed. Carries the brand, a `SECTION:` field that re-reads on scroll, section nav with `aria-current`, and the day/night toggle. Opaque, never blurred.
- **`.stamp`** — double-ruled vermilion box, rotated `-4deg`. Carries `role="img"` and a label.
- **`.fields` / `.offrec` / `.council-rec`** — the record vocabulary. Courier label, Courier value, alternating bands. Any block of discrete facts uses one of these rather than inventing a new container.
- **`.reg`** — the career register. Three columns (`ref | title | body`) collapsing to one at 960px. The in-effect row carries `box-shadow: inset 4px 0 0 var(--stamp)`.
- **`.spec`** — record tables. Header row in Courier, banded body, card-collapse at 620px with the `thead` visually hidden.
- **`.proc` + `.note`** — numbered procedure steps with signed margin notes. The testimonials live here, as annotations in the document's margin.
- **`.inverted`** — the council band. A different sheet in the same set. The perforated margins stay paper through it, because the margin belongs to the form and the dark band is ink printed on it.
- **`.mission`** — a single banded strip closing the masthead. The one place the customer-facing purpose is stated, so it does not compete with the personal thesis above it.
- **`.orders`** — five values as a fixed five-across row of banded cells, numbered `01`–`05` in stamp ink. Deliberately *not* a card grid: no radius, no shadow, no icon, shared rules between cells so it reads as one printed block.
- **`.credo`** — the last line on the sheet, above the sign-off. One quote, one accent word.

### Imagery — the house rule

The record is **machine-printed**; its **attachments are hand-drawn**. That single rule is what lets photography into a world built from type and rules, and it is the test for any future image.

- **`.idphoto`** — the portrait, affixed in the masthead's change block. Because the source is a sketch, it is treated as ink rather than as a photograph: `grayscale(1) contrast(1.32) brightness(1.06)` plus `mix-blend-mode: multiply` over a pale `#f1f2ec` photo stock, thin `--rule` border, hard offset shadow. Multiply drops the sketch's white ground into the paper, so it prints *with* the sheet instead of sitting on top of it. The pale backing is theme-independent on purpose: a physical photo on a dark desk is still light, and inverting a face reads as a negative.
- **`.exhibit`** — Attachment A, the hand-drawn one-page summary. Full colour, because it is a drawing and not part of the printed form. Same paper stock and offset shadow as `.idphoto`; hover moves the shadow to stamp ink. Style the wrapper as `.exhibit > a`, never `.exhibit a`, or the caption link inherits the frame.

---

## 4. Motion

**One authored moment.** The `IN EFFECT` stamp lands: `@keyframes land`, 720ms, `cubic-bezier(0.16, 1, 0.3, 1)`, entering at `rotate(-16deg) scale(1.75)` and settling through an overshoot at `rotate(-2deg) scale(0.97)`. Wrapped in `@media (prefers-reduced-motion: no-preference)`.

That is the entire motion budget. There are **no** scroll-triggered fade-ups, no stagger, no parallax, no counters. The page is a document; documents do not animate as you read them.

---

## 5. Bans

These are not preferences.

- No eyebrows or kickers above headings.
- No icon + heading + text card grids.
- No hero-metric stat tiles. Numbers live inside record rows where they mean something.
- No gradient text, no glass, no blur, no glow, no decorative gradient fields.
- No section numbers as ornament (the `CHG-0x` and `SEC 0x` refs are record IDs, not decoration).
- No mono as a costume — Courier is for data only.
- No Inter, Plus Jakarta Sans, Space Grotesk, or any other default-tell display face.
- No second accent color.
- No stock photography, no illustration, no generated imagery. The only two images on the page are Dan's own: a sketched portrait and the hand-drawn one-page summary he commissioned. Both are treated per the imagery house rule in §3 — a personnel record carries a photo on file, but it carries it as *ink*, not as a glossy headshot dropped into a document.

---

## 6. Accessibility

- Landmarks: `header`, `nav[aria-label="Sections"]`, `nav[aria-label="Contents"]`, `main`, `footer`.
- Heading order is unbroken `h1 → h2 → h3`. Every section has a heading, including the council.
- Skip link is the first tab stop.
- Focus: `3px solid var(--stamp)` at `3px` offset, everywhere.
- Toggles carry `aria-pressed`; the mobile disclosure carries `aria-expanded` + `aria-controls` and closes on Escape.
- No `aria-live` on the scroll-driven section field — it would chatter. `aria-current` on the nav conveys position instead.
- All interactive targets clear 44px of vertical hit area.
- Reduced motion removes the stamp animation and smooth scrolling.

---

## 7. Constraints

- **One file, no build step.** `index.html` contains its own CSS and JS. Two Google Fonts families are the only external requests.
- **Static, GitHub Pages, custom domain `driv.space`** (see `CNAME`).
- **`404.html` duplicates the tokens deliberately.** There is no shared stylesheet. If you change a token, change it in both files.
- A print stylesheet drops the strip, sheet, skip link, and perforation.

---

## 8. If you extend this

Ask: *what is this, on the form?* A new block of facts is a field list. A new sequence of events is a register. A new position is a spec table. A new claim is a margin note, signed. If it is none of those, it probably does not belong on this page.
