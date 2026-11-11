# Changelog -- V_DCF_Model.xlsx

## 2026-11-11 -- rebuild on branch `visa-dcf-rebuild` (not yet merged to `main`)

Audited and rebuilt the model against the issues listed in the README's "Known issues." Not
yet merged -- status stays "work in progress" until this is reviewed.

**Rolled forward to FY2026A actuals.** Visa's FY2026 10-K was filed 2026-11-05 (SEC EDGAR
accession 0001403161-26-000134), six days before this rebuild. Replaced the old FY2026E
estimate (9-months-actual + a guessed Q4 growth rate) with the real, audited FY2026A figures.
This also fixes the balance-sheet-date mismatch: cash, debt and diluted shares are now all as
of 9/30/2026, the same date as the DCF's valuation date, the market price, the 10-year
Treasury yield and the ERP estimate -- previously these were scattered across 6/30/2026,
9/30/2026 and 10/2/2026. Historicals now run FY2021A-FY2026A (6 columns); projections are
FY2027E-FY2031E (5 columns, one more than before).

**Sourced every hardcoded input**, verified against primary documents, not third-party
aggregators:
- All FY2026A income-statement, cash-flow and balance-sheet figures: Visa's FY2026 10-K,
  pulled directly from sec.gov (R-page XBRL tables, not the rendered HTML).
- Diluted share count: reconciled a discrepancy between the old 1,898mm and a mis-derived
  1,958mm (the latter double-counted Visa's per-class EPS rows, which are each already a
  self-contained if-converted calculation, not additive). Correct figure is 1,882mm -- Class A
  diluted weighted-average shares, Q4 FY2026, Visa's Oct 27 2026 earnings release
  (8-K accession 0001403161-26-000126), cross-checked against the 10-K's EPS footnote
  (R95.htm, "Schedule of Weighted Average Number of Shares as Converted").
- 10-year Treasury yield (5.29%, 9/30/2026): FRED series DGS10.
- Visa and Mastercard closing prices (9/30/2026): Yahoo Finance.
- Equity risk premium (4.14%): Damodaran's monthly implied-ERP spreadsheet, 2026-09-01 row,
  downloaded directly from pages.stern.nyu.edu.
- Beta (0.80, base case): computed from scratch (Yahoo Finance weekly returns, V vs. S&P 500,
  5-year window ending 9/30/2026) rather than cited from a third party. A 2-year window gives
  0.42 -- shown as a labeled sensitivity on the Summary tab, not silently picked between. A
  WRDS/CRSP cross-check was run privately and kept out of this repo (publication rights for
  WRDS-derived data are unresolved -- see `vma-pairs/docs/DATA_PERMISSIONS.md` for the same
  issue in a sibling repo); it does not cover a comparable window (CRSP's market-index table in
  this environment stops at 2024-12-31 regardless of using the newer CIZ security-level format),
  so it is weak corroboration at best and is not presented as independent confirmation.
- Credit spread (0.625%): flagged as approximate. Visa's Aa3/Moody's / AA-/S&P rating could not
  be confirmed from a primary agency document or from the text retrieved from Visa's 10-K or
  its Feb-2026 bond prospectus; Damodaran's spread table has no exact Aa3/AA- bucket, so this is
  the midpoint of the two adjacent buckets (Aa2/AA 0.55%, A1/A+ 0.70%). Debt is ~3% of the
  capital structure, so this barely moves WACC either way.
- Effective tax rate (16.30%, replacing an unsourced 17.5% guess): Visa's 10-K, Note 19 tax-rate
  reconciliation (R106.htm).

**Fixed the exit-multiple problem, before computing any price from it.** The old 18.0x exit
multiple was never sourced. Pulled real comps as of the same 9/30/2026 valuation date: Mastercard
trades at ~22.1x LTM EV/EBITDA (from its Q3 FY2026 10-Q, filed 10/29/2026) and Visa itself trades
at ~23.2x. Both are well above the 18.0x the old model used, and both are *higher* multiples than
the problem actually called for: carrying either forward into FY2031E unchanged would contradict
this model's own assumption that growth decelerates to 8% by FY2031E and 3% in perpetuity. Fixed
the rule first: the exit multiple used in the cross-check is now set equal to the perpetuity
method's own implied multiple (so the two methods agree by construction, as they should once
growth has actually decelerated to the terminal rate) rather than picked by hand. Today's actual
multiples (MA's, V's own, and the old 18.0x) are shown as an explicit, captioned "if today's
multiple persisted" reference range -- illustrative, not used in the headline price.

**Re-based two forward assumptions that FY2026A actuals showed were off**, instead of leaving
them as stale guesses: the old flat 66.5% ex-litigation operating-margin assumption vs. FY2026A's
actual 65.1%, and the old flat 17.5% tax-rate guess vs. FY2026A's actual 16.30%. Both now carry
the FY2026A actual forward flat, with the gap stated explicitly in a cell comment.

**Removed the stale `Assumptions!N8` note** ("consensus 10.8%, slowing from 15%") by removing the
whole FY2026-estimation block it was attached to (rows 18-24) -- that block only ever existed to
estimate FY2026E from 9 months of actuals, which is moot now that FY2026 is an actual year.

**Documented the two remaining undocumented judgment calls**: share-based comp is not added back
in the FCF build (treated as a real, dilutive economic cost -- the more conservative of two
defensible treatments), and the litigation provision is treated as recurring, not one-off,
because Visa has recorded one in every year of the 5-year historical window with no visible
trend toward zero.

**Added, as the new first tab, a Summary tab** leading with a reverse DCF (what growth or margin
does the current $359.33 price actually imply, solved in closed form against this model's own
assumptions), a stated base-case conclusion (model implies ~$317.63, -11.6% vs. market), the beta
sensitivity above, and a bull/base/bear range.

**Added a third sensitivity table** (growth CAGR vs. flat operating margin) on the Sensitivity
tab, with the simplification it relies on (a flat CAGR standing in for the actual declining
glide path) stated directly on the sheet, not just in this changelog.

**Verified, did not change:** the two existing WACC/G and WACC/multiple sensitivity tables were
already live `SUMPRODUCT` formulas, not a frozen Excel Data Table, confirming the profile
README's claim -- their cell ranges were shifted to the new FY2031E terminal column but the
formula logic is untouched.
