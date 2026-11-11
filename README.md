# Financial Models

Valuation models built in Excel.

## Visa Inc. (V): Discounted Cash Flow

**Status: complete (v1.0).** Every item in the old "Known issues" list has been addressed, the
rebuild went through a review pass that caught and fixed real bugs (see `CHANGELOG.md`), and the
headline numbers were recomputed outside Excel from the workbook's own inputs. Open limitations
are listed under "Known issues (still open)".

File: `V_DCF_Model.xlsx`

### The question

**What does Visa's price already assume?** The Summary tab leads with a reverse DCF rather than
a single target price: holding this model's own assumptions fixed except one at a time, what
growth rate or what margin does the current $359.33 price actually require?

- Required terminal growth rate: **~3.7%**, vs. the 3.0% this model assumes.
- Required flat *reported* operating margin (i.e. after the litigation-provision deduction,
  the apples-to-apples comparison to what Visa actually reports): **~69.0%**, vs. the 61.2%
  FY2026A actual. That is above every reported margin in the six years of history here (the highest was 65.7%, in FY2024).

Both levers alone would need to move further than the historical record or this model's own
terminal-growth assumption supports. That gap -- not a single point estimate -- is the finding.

### Model outputs

Valuation date 9/30/2026 -- the single date behind every input (price, Treasury yield, ERP,
cash, debt, shares). The old version mixed 6/30, 9/30 and 10/2/2026.

| Method | Implied share price | vs. market ($359.33) |
|---|---|---|
| **Perpetuity growth (g = 3.0%, WACC = 8.49%) -- base case** | **$319.36** | **-11.1%** |
| Exit multiple at 15.4x (set equal to the perpetuity-implied multiple, so identical by construction, not an independent check) | $319.36 | -11.1% |
| *Reference only -- if today's actual multiple persisted to FY2031E unchanged:* | | |
| &nbsp;&nbsp;Mastercard's current EV/EBITDA (22.1x) | $429.88 | +19.6% |
| &nbsp;&nbsp;Visa's own current EV/EBITDA (23.1x) | $446.47 | +24.2% |
| &nbsp;&nbsp;Old model's exit multiple (18.0x, never sourced) | $362.32 | +0.8% |

The reference rows are a deliberate point, not a hedge: Visa and Mastercard both currently trade
well above the multiple this DCF's own growth and margin assumptions can justify by FY2031E.
Carrying today's multiple forward unchanged would contradict the model's own premise that growth
decelerates -- so the cross-check doesn't do that (see `CHANGELOG.md` for the old, unsourced
18.0x this replaced).

Beta is the single input most of this conclusion rides on: a 5-year weekly beta of 0.80 (base
case) gives the -11.1% above; a 2-year weekly beta of 0.42 -- computed the same way, same
source, just a shorter window -- gives a WACC of 6.96% and an implied price of **$443.43**, i.e.
the *opposite* conclusion (undervalued, not overvalued). I have not resolved which window is
"right," or verified *why* beta has fallen this much -- both are stated as open questions on the
Summary tab, not papered over.

### How it is built

Seven tabs: Summary, Assumptions, Historicals, Projections, DCF, WACC, Sensitivity.

- Historicals FY2021A-FY2026A (FY2026 is now an actual year: the 10-K was filed 2026-11-05),
  projections FY2027E-FY2031E.
- The DCF discounts FY2027E-FY2031E free cash flow using the mid-year convention.
- Revenue growth glide path: 11.0%, 10.5%, 9.5%, 8.75%, 8.0%.
- Operating margin: flat 65.1% ex-litigation (re-based to the FY2026A actual; the old version
  assumed 66.5%), *minus* a flat 3.6% litigation-provision assumption, for an effective
  *reported* margin of ~61.5% in the FCF build -- confirmed by re-deriving it from the formulas,
  not assumed: litigation is and was always deducted in the actual DCF chain (`Projections!EBIT`
  references the post-litigation margin). What was wrong, and has been fixed, is that two
  *secondary* outputs -- the growth-vs-margin sensitivity grid and the reverse-DCF margin
  solve -- skipped that deduction and used the pre-litigation 65.1% figure directly as the EBIT
  driver. See `CHANGELOG.md`.
- Tax rate 16.30% (re-based to the FY2026A actual effective rate; the old version assumed 17.5%).
- Diluted shares: 1,872mm, period-end as-converted (9/30/2026) plus a hand-computed
  treasury-stock-method addition for options/RSUs/performance shares -- not Visa's own quarterly
  weighted-average diluted count. See `CHANGELOG.md` for the exact build.
- WACC of 8.49%: risk-free rate 5.29% (10-year Treasury, 9/30/2026, FRED), equity risk premium
  4.14% (Damodaran, 2026-09-01 estimate), beta 0.80 (Yahoo Finance weekly returns, 5-year window
  -- see the beta sensitivity above), credit spread 0.625% (approximate -- see `CHANGELOG.md`).
- All three sensitivity tables are live formulas, not a frozen Excel Data Table: WACC vs.
  terminal growth, WACC vs. exit multiple, and a growth(CAGR) vs. margin grid.
- Bull and bear cases run on the *same* glide-path engine as the base case (duplicated
  Projections columns with shifted growth/margin inputs, not a simplified formula) -- they
  differ from the base case only in assumptions, not mechanics.

### Known issues (still open)

- **Credit spread is approximate** (0.625%, interpolated between two rating buckets) and Visa's
  Aa3/AA- rating itself could not be confirmed from a primary agency document. Debt is ~3% of
  the capital structure, so the effect on WACC is small either way.
- **Beta choice is the single biggest lever on the conclusion** -- see above. Not resolved here.
- **Terminal value is about 79% of enterprise value**, so the result is very sensitive to WACC and terminal growth (see the sensitivity tables).
- **Forward margin:** the base case's effective reported margin is ~61.5% (65.1% ex-litigation less a 3.6% litigation assumption, the FY2023-FY2026 average), about 0.3 points above FY2026A's actual 61.2%.
- **The growth-vs-margin sensitivity grid is a simplification**: it swaps the actual declining
  glide path for a flat CAGR so the two axes can be solved independently in closed form (the
  bull/base/bear cases below do not have this limitation). Stated directly on the Sensitivity
  tab.
- Formatting inconsistencies and typos may remain.

### Bull / base / bear

Same engine as the base case (see "How it is built"), scaled from it -- not a shortcut formula.

| Case | Implied price |
|---|---|
| Bear (growth and margin both -150bp vs. base, every forecast year) | $292.09 |
| **Base (actual glide path)** | **$319.36** |
| Bull (growth and margin both +150bp vs. base, every forecast year) | $348.71 |

*Educational project. Not investment advice.*
