# The True Cost of Car Ownership

**Part 1 of The Compounding Series.** An editorial data viz about what the dealership leaves off the brochure — the interest, the depreciation, and the retirement that didn't happen.

→ **Live:** https://amandarae220.github.io/true-cost-of-car-ownership/
→ **Part 2:** [Calculator2.0](https://amandarae220.github.io/Calculator2.0/) — the same force, pointed the other way.

---

## What this is

A self-contained, single-page data viz built to answer one question: what does buying the average new car *actually* cost over a working life? Not the sticker. Not the monthly payment. The whole thing — interest, mandatory full-coverage insurance, depreciation, and the retirement contribution that didn't happen.

The piece runs on a recurring **BROCHURE ↔ RECEIPT** device: on each beat, the dealership's version of the math sits on the left, and the part they leave off sits on the right. The reader meets Jordan — a fictional 30-year-old buying their first new car on a Tuesday — and follows the actual US averages (KBB, Experian, NPR, Bankrate, BTS) through six years of payments, then the alternate Tuesday that would have built ~$1.76M instead.

Interactive moments anchor the argument at the points where it matters most:

- A **trade-in cadence** slider showing lifetime depreciation across multiple new-car cycles.
- A **counterfactual fork** letting the reader plug in their own used-car price and expected return rate.
- A signature **mirror curve** visualization with two scenarios: 6 years of redirected contributions (~$674k) and a lifetime of them (~$1.76M).
- A **US-average calculator** at the end so the reader can plug in their own Tuesday and watch the depreciation, the interest, and the alternate Tuesday redraw themselves.
- An **FAQ accordion** covering the rationalizations (*"a new car is more reliable, I'll keep it forever, I deserve this"*) and the nuance (*"isn't all debt bad? — no, here's when it isn't"*).

## Branches

This repo carries four complete variants of the piece, one per branch. Switch with `git checkout <branch>` and reload `index.html` to see each.

| Branch | What it is |
|---|---|
| **`lite-light`** *(default — what Pages serves)* | Five-act narrative version: The Pitch → Meet Jordan → The Tuesday That Didn't Happen → About That Asterisk → Your Tuesday (calculator), plus an FAQ accordion for the rationalizations and debt nuance. |
| `main` | The full seven-act version. The rationalizations and the "is all debt bad" nuance live inline as full scrollable acts rather than FAQ accordions. |
| `lite-moderate` | One BROCHURE/RECEIPT beat + cycle widget + mirror finale + calculator. Tighter than lite-light. |
| `lite-aggressive` | Hero + signature mirror visual + calculator + outro. ~two scrolls. |

---

## Technical highlights

This is the part recruiters and senior devs usually don't read on a single-HTML viz — so here's what was actually non-trivial about building it.

### The signature mirror visualization is hand-rolled HTML5 Canvas, not a charting library

The signature *Compound interest, shown both ways* curve is rendered in raw `<canvas>` rather than Chart.js, for several specific reasons:

1. **Per-canvas scenarios.** The same drawing function handles two different curve models. The **`twoPhase` scenario** (Act I primer) plots an annuity that contributes $946/mo for 69 months, *then stops*, and lets the balance compound at 7% through year 35. The **`continuous` scenario** (Act III finale) plots the same money flowing continuously for the full 35 years. The two scenarios share rendering code but diverge mathematically — a flexibility a generic charting library doesn't give you cleanly.
2. **Editorial annotations baked into the canvas.** Year-axis ticks, dollar-value data point labels, an endpoint callout (`$1.76M / at age 65`), a highlighted *contribution-phase segment* with a connector line, a "$0" anchor at the zero axis — all hand-positioned. Chart.js would technically do this with plugins, but the layout precision would be fighting the library.
3. **Devicepixelratio-aware drawing.** Canvas pixels are scaled to match the device's pixel ratio for crisp lines on retina screens. The hero visual matters; pixelation would undermine it.
4. **Scroll-triggered fill animation.** When the Act III mirror chart enters the viewport, the lower navy "loss" curve draws itself in stroke-by-stroke over ~1.4s with ease-out cubic. Implemented with `IntersectionObserver` + `requestAnimationFrame`, lossFraction interpolated over a `MIRROR_INSTANCES` config object so any future canvas can opt in by setting `animateTo`.

### Interactive widgets are live-recalculating, not pre-rendered

Three sliders distributed across the piece each run live annuity math on every `input` event:
- The **trade-in cadence widget** rebuilds a lifetime cycle bar + three dependent stats (cars bought, lifetime depreciation, lifetime interest) on every drag, with the cycle bar regenerated as `flex`-distributed segments tinted from full-saturation navy to ghost.
- The **counterfactual sliders** (used-car price + return rate) compound the redirected down payment and monthly annuity through 35 years and update the headline `~$1.76M` finale in real time.
- The **end-of-piece calculator** runs a 7-year amortization, a 12-bucket per-year depreciation model, and a 35-year monthly investment simulation on every slider movement — driving three Chart.js charts and four summary cards.

### Production polish details

- **Skeleton loading state** with a CSS shimmer keyframe on the insight panels (instead of a literal "Loading..." string). Respects `prefers-reduced-motion`.
- **`aria-busy` / `aria-live`** on the live-updating insight panels so screen readers announce the populated math without re-announcing every input change.
- **Strict allowlist `.gitignore`** at the repo root (which lives in `$HOME`) — only `index.html`, `README.md`, and `.gitignore` itself can be committed. Designed so `git add .` from this directory is safe.

---

## Accessibility

WCAG 2.1 AA compliance was a design constraint, not an afterthought.

- **Keyboard navigation.** The FAQ uses native `<details>` / `<summary>` — fully keyboard-operable without a single line of JS. Every slider has an `aria-label`, focusable via tab, adjustable via arrow keys.
- **Color contrast.** The cream paper background (`#f4efe5`) was chosen partly because it gives every accent — deep ink navy (`#2a3a5a`), steel teal (`#3a7388`), slate violet (`#5a4d8a`), mustard ochre (`#9c7820`) — comfortable contrast against the text and surface colors. All pairings pass WCAG AA for normal body text against the background.
- **`prefers-reduced-motion`** is respected — the skeleton shimmer animation degrades to a static low-opacity placeholder for readers with motion sensitivity.
- **Charts** have descriptive `aria-label` attributes describing the curve's shape and final value, so screen readers can convey the visual argument without seeing the canvas.
- **Live regions** on the calculator insight panels use `aria-live="polite"` so updates are announced without interrupting the reader.

## Mobile / responsive

This is a scrollytelling piece, which usually falls apart on phones. It doesn't.

- **Three layout breakpoints**, mapped to layout intent rather than specific devices: `1000px` (sidebar calculator stacks), `700px` (scenario summary cards stack), and `520px` (mirror axis labels and series-nav reflow, slider touch targets grow to 18px, receipt lines allow text wrapping).
- **Fluid type scaling** via `clamp()` on every headline, big-number callout, and chart label — no breakpoints needed for headline sizing.
- **Mirror canvas** sizes via `clamp(280px, 38vw, 420px)` and redraws on `resize` with device-pixel-ratio scaling, so the visual stays crisp from 320px to 4K.
- **BROCHURE / RECEIPT** stacks brochure-above-receipt below 760px; the dotted "perforated edge" between the two halves rotates from vertical to horizontal.

## Stack

- Single self-contained HTML file — no build step, no framework, no install
- Vanilla JS for all interactivity
- Chart.js v4 for the calculator's three charts (depreciation, cost breakdown, investment portfolio)
- Native HTML5 Canvas for the signature mirror curve
- Native `<details>` / `<summary>` accordions for the FAQ
- IBM Plex Mono / IBM Plex Serif / Oswald via Google Fonts

## Tone & references

The voice aims for *Nudge* meets *Freakonomics* — investigative in substance, kitchen-table in tone. Plain language, numbers translated into tactile comparisons, no jargon without a translation. Inspired by ChooseFI's "The True Cost of Car Ownership" podcast episode, with structural cues from The Pudding, the NYT Upshot, and Reuters Graphics.

## Sources

- Avg new-car transaction price: KBB / Cox Automotive ATP, Dec 2025
- New car loans over $1,000/mo (~19%): [NPR, Oct 2025](https://www.npr.org/2025/10/29/nx-s1-5556935/cost-of-living-cars)
- Avg APR & loan term: Experian State of the Automotive Finance Market Q4 2025
- Full-coverage auto insurance: Bankrate 2025 Auto Insurance Study
- US household transportation spending: Bureau of Transportation Statistics, 2024
- Historical S&P 500 real (inflation-adjusted) return: ~7%

---

*Built for portfolio. Use freely.*
