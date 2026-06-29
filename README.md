# The True Cost of Car Ownership

**Part 1 of The Compounding Series.** An editorial data viz about what the dealership leaves off the brochure — the interest, the depreciation, and the retirement that didn't happen.

→ **Live:** https://amandarae220.github.io/true-cost-of-car-ownership/
→ **Part 2:** [Calculator2.0](https://amandarae220.github.io/Calculator2.0/) — the same force, pointed the other way.

---

## What this is

A self-contained, single-page data viz built to answer one question: what does buying the average new car *actually* cost over a working life? Not the sticker. Not the monthly payment. The whole thing — interest, mandatory full-coverage insurance, depreciation, and the retirement contribution that didn't happen.

The piece runs on a recurring **BROCHURE ↔ RECEIPT** device: on each beat, the dealership's version of the math sits on the left, and the part they leave off sits on the right. The reader meets Jordan — a fictional 30-year-old buying their first new car on a Tuesday — and follows the actual US averages (KBB, Experian, NPR, Bankrate, BTS) through six years of payments, then the alternate Tuesday that would have built ~$2.16M instead.

Interactive moments anchor the argument at the points where it matters most:

- A **trade-in cadence** slider showing lifetime depreciation across multiple new-car cycles.
- A **counterfactual fork** letting the reader plug in their own used-car price and expected return rate.
- A signature **mirror curve** visualization rendered in native HTML5 canvas, showing two scenarios side by side: 6 years of redirected contributions (~$1.1M) and a lifetime of them ($2.16M).
- A **US-average calculator** at the end so the reader can plug in their own Tuesday and watch the math redraw itself.
- An **FAQ accordion** covering the rationalizations (*"a new car is more reliable, I'll keep it forever, I deserve this"*) and the nuance (*"isn't all debt bad? — no, here's when it isn't"*).

## Branches

This repo carries four complete variants of the piece, one per branch. Switch with `git checkout <branch>` and reload `index.html` to see each.

| Branch | What it is |
|---|---|
| **`lite-light`** *(default, what Pages serves)* | The shipped version. Acts I, II, V + asterisk callback + calculator + FAQ accordion (Acts III & VI content folded into accordions). |
| `main` | The full seven-act narrative version. Acts III (rationalizations) and VI (debt nuance) live inline as full scrollable acts. |
| `lite-moderate` | One BROCHURE/RECEIPT beat + cycle widget + mirror finale + calculator. Tighter than lite-light. |
| `lite-aggressive` | Hero + signature mirror visual + calculator + outro. ~two scrolls. |

## Stack

- Single self-contained HTML file — no build step, no framework, no install
- Vanilla JS for all interactivity (sliders, scroll-triggered animations, the mirror curve)
- Chart.js v4 for the calculator's depreciation / cost / portfolio charts
- Native HTML5 Canvas for the signature **mirror curve** visualization with per-canvas scenarios (`twoPhase`, `continuous`) and key data-point annotations
- Native `<details>` / `<summary>` accordions for the FAQ — zero JS, keyboard accessible
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
