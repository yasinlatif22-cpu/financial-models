# Financial Models

Valuation models built in Excel.

## Visa Inc. (V): Discounted Cash Flow

**Status: work in progress (v0.1).** Assumptions and source documentation are being revised. No investment conclusion yet. Published as-is so the work and the open issues are visible. Every item under "Known issues" below is still open; none has been fixed.

File: `V_DCF_Model.xlsx`

### Model outputs

Valuation date 9/30/2026. Market price $360.66 (as of 10/2/2026).

| Terminal value method | Implied share price | vs. market |
|---|---|---|
| Perpetuity growth (g = 3.0%, WACC = 8.94%) | $278.00 | -22.9% |
| Exit multiple (18.0x FY2030E EBITDA) | $342.61 | -5.0% |

### How it is built

Six tabs: Assumptions, Historicals, Projections, DCF, WACC, Sensitivity.

- Historicals FY2021 to FY2025, projections FY2026E to FY2030E. FY2026E revenue combines nine months of reported revenue with a Q4 growth assumption.
- The DCF discounts FY2027E to FY2030E free cash flow using the mid-year convention.
- Revenue growth: 14.4% in FY2026E, then 11.0%, 10.5%, 9.25% and 8.0%.
- Operating margin of 63.1% after a recurring litigation provision of 3.4% of revenue. Tax rate 17.5%.
- WACC of 8.94%: risk-free rate 5.28% (10-year Treasury, 10/2/2026), equity risk premium 5.0%, beta 0.76, credit spread 0.75%.
- Both sensitivity tables are live formulas (WACC vs. terminal growth, WACC vs. exit multiple).

### Known issues (to fix)

- **Sources are not yet documented in the workbook** for beta, equity risk premium, credit spread, share price, and the 10-Q figures.
- **The 18.0x exit multiple is unsupported.** The perpetuity assumptions imply about 14.1x, which is why the two methods disagree by about $65 per share.
- **A stale note** in `Assumptions!N8` refers to 10.8% consensus growth. The model uses 14.4%.
- **Balance sheet date mismatch:** cash and debt are as of 6/30/2026, the valuation date is 9/30/2026.
- **Undocumented judgment calls:** share-based compensation is not added back, and litigation is modeled as a recurring cost.
- **No stated investment conclusion yet.**
- Formatting inconsistencies and typos.

### Planned

- Summary tab with a stated conclusion
- Reverse DCF (what the market price implies)
- Bull / base / bear scenarios
- Revenue growth vs. operating margin sensitivity
- Source notes for every hardcoded input

*Educational project. Not investment advice.*
