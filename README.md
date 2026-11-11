# Financial Models

Valuation models built in Excel.

## Visa Inc. (V): Discounted Cash Flow

**Status: work in progress (v0.2), on branch `visa-dcf-rebuild`.** Every item in the old
"Known issues" list below has been addressed -- see `CHANGELOG.md` for exactly what changed and
why -- but this hasn't been reviewed yet, so the status stays work-in-progress until it has.
Not merged to `main`.

File: `V_DCF_Model.xlsx`

### The question

**What does Visa's price already assume?** The Summary tab leads with a reverse DCF rather than
a single target price: holding this model's own assumptions fixed except one at a time, what
growth rate or what margin does the current $359.33 price actually require?

- Required terminal growth rate: **~3.7%**, vs. the 3.0% this model assumes.
- Required flat operating margin: **~73.4%**, vs. the 65.1% FY2026A actual this model carries
  forward -- Visa has never posted anything close to 73% in the five years of history here.

Both levers alone would need to move further than the historical record or this model's own
terminal-growth assumption supports. That gap -- not a single point estimate -- is the finding.

### Model outputs

Valuation date 9/30/2026 -- now the single date behind every input (price, Treasury yield, ERP,
cash, debt, shares). The old version mixed 6/30, 9/30 and 10/2/2026.

| Method | Implied share price | vs. market ($359.33) |
|---|---|---|
| **Perpetuity growth (g = 3.0%, WACC = 8.49%) -- base case** | **$317.63** | **-11.6%** |
| Exit multiple cross-check (15.4x, set by rule = the perpetuity-implied multiple) | $317.63 | -11.6% |
| *Reference only -- if today's actual multiple persisted to FY2031E unchanged:* | | |
| &nbsp;&nbsp;Mastercard's current EV/EBITDA (22.1x) | $427.58 | +19.0% |
| &nbsp;&nbsp;Visa's own current EV/EBITDA (23.2x) | $445.91 | +24.1% |
| &nbsp;&nbsp;Old model's exit multiple (18.0x, never sourced) | $360.39 | +0.3% |

The reference rows are a deliberate point, not a hedge: Visa and Mastercard both currently trade
well above the multiple this DCF's own growth and margin assumptions can justify by FY2031E.
Carrying today's multiple forward unchanged would contradict the model's own premise that growth
decelerates -- so the cross-check doesn't do that (see "Known issues" history in `CHANGELOG.md`
for the old, unsourced 18.0x this replaced).

Beta is the single input most of this conclusion rides on: a 5-year weekly beta of 0.80 (base
case) gives the -11.6% above; a 2-year weekly beta of 0.42 -- computed the same way, same source,
just a shorter window -- gives a WACC of 6.96% and an implied price of **$441.03**, i.e. the
*opposite* conclusion (undervalued, not overvalued). Both are shown on the Summary tab. I have
not resolved which window is "right," and I'm not pretending the choice doesn't matter.

### How it is built

Seven tabs: Summary, Assumptions, Historicals, Projections, DCF, WACC, Sensitivity.

- Historicals FY2021A-FY2026A (FY2026 is now an actual year: the 10-K was filed 2026-11-05),
  projections FY2027E-FY2031E.
- The DCF discounts FY2027E-FY2031E free cash flow using the mid-year convention.
- Revenue growth glide path: 11.0%, 10.5%, 9.5%, 8.75%, 8.0%.
- Flat ex-litigation operating margin of 65.1% (re-based to the FY2026A actual; the old version
  assumed 66.5%, about 140bp above what FY2026 actually came in at). Tax rate 16.30% (re-based
  to the FY2026A actual effective rate; the old version assumed 17.5%).
- WACC of 8.49%: risk-free rate 5.29% (10-year Treasury, 9/30/2026, FRED), equity risk premium
  4.14% (Damodaran, 2026-09-01 estimate), beta 0.80 (computed from Yahoo Finance weekly returns,
  5-year window -- see the beta sensitivity above), credit spread 0.625% (approximate -- see
  `CHANGELOG.md`).
- All three sensitivity tables are live formulas, not a frozen Excel Data Table: WACC vs.
  terminal growth, WACC vs. exit multiple, and a new growth(CAGR) vs. margin grid.

### Known issues (still open)

- **Credit spread is approximate** (0.625%, interpolated between two rating buckets) and Visa's
  Aa3/AA- rating itself could not be confirmed from a primary agency document. Debt is ~3% of
  the capital structure, so the effect on WACC is small either way.
- **Beta choice is the single biggest lever on the conclusion** -- see above. Not resolved here.
- **The growth-vs-margin sensitivity grid is a simplification**: it swaps the actual declining
  glide path for a flat CAGR so the two axes can be solved independently in closed form. Stated
  directly on the Sensitivity tab; the real base case uses the glide path, not a flat rate.
- Formatting inconsistencies and typos may remain.

### Bull / base / bear

| Case | Margin | Growth (CAGR) | Implied price |
|---|---|---|---|
| Bear | 62.1% | 7.5% | $293.04 |
| **Base (actual glide path)** | **65.1%** | glide path, not a flat rate | **$317.63** |
| Bull | 68.1% | 11.5% | $381.69 |

*Educational project. Not investment advice.*
