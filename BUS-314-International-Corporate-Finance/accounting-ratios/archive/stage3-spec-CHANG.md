# ExxonMobil Corporation (XOM) – Technical Specification

**Created by:** changfern
**Updated by:** changfern
**Date Created:** April 24, 2026
**Date Updated:** April 24, 2026
**Version:** 1.0
**LLM Used:** Claude Sonnet 4.6 (Anthropic)

**Role:** Financial Analyst / FP&A Analyst
**Audience:** CFO or Director of FP&A

**Purpose:** Provide a professional, quantitative specification documenting the Excel model's analytical structure for computing and interpreting accounting and performance ratios from ExxonMobil Corporation's FY2024 financial statements. This post-build spec captures what was built, key analytical decisions, and how the model should be refined.

*University of Hawaiʻi at Mānoa · BUS-314 International Corporate Finance · April 24, 2026*

---

## 1. Problem Statement

ExxonMobil Corporation (NYSE: XOM) is a publicly traded integrated oil and gas company and one of the world's largest energy producers. This specification outlines the analytical framework for computing 28+ accounting and performance ratios from the company's FY2024 financial statements (fiscal year ended December 31, 2024), enabling senior management to assess financial health, operational efficiency, capital structure, and value creation.

| Field | Detail |
|---|---|
| Company | ExxonMobil Corporation |
| Ticker / Exchange | XOM — New York Stock Exchange |
| Industry | Energy – Integrated Oil & Gas |
| Current Fiscal Year | FY2024 (Dec 31, 2024) |
| Prior Fiscal Year | FY2023 (Dec 31, 2023) |
| Analytical Objective | Compute 28+ ratios; assess financial health, efficiency, leverage, and value creation |
| Decision Context | CFO briefing, board presentation, investor relations |
| Data Source | ExxonMobil 10-K FY2024, SEC EDGAR |
| Reporting Currency | Millions of USD |

---

## 2. Inputs (Known Variables)

### 2.1 Balance Sheet Items

All items sourced from the ExxonMobil FY2024 10-K Balance Sheet. Named ranges follow the convention `BAL_<item>_<year>`.

| Variable | Named Range (Current) | FY2024 ($M) | Named Range (Prior) | FY2023 ($M) |
|---|---|---|---|---|
| Cash & marketable securities | `BAL_cash_marketable_securities_2024` | 23,029 | — | 31,539 |
| Receivables | `BAL_receivables_2024` | 43,681 | `BAL_receivables_2023` | 38,015 |
| Inventories | `BAL_inventories_2024` | 23,524 | `BAL_inventories_2023` | 25,120 |
| Other current assets | `BAL_other_current_assets_2024` | 1,756 | — | 1,906 |
| Total current assets | `BAL_assets_current_2024` | 91,990 | — | 96,580 |
| Net PP&E | `BAL_fixed_assets_net_2024` | 294,318 | — | 214,940 |
| Intangible assets (goodwill) | `BAL_intangibles_2024` | 6,900 | — | 4,800 |
| Other assets | `BAL_other_assets_2024` | 60,267 | — | 59,997 |
| Total assets | `BAL_assets_total_2024` | 453,475 | `BAL_assets_total_2023` | 376,317 |
| Debt due for repayment | `BAL_debt_short_term_2024` | 4,955 | — | 4,090 |
| Accounts payable | `BAL_accounts_payable_2024` | 52,000 | `BAL_accounts_payable_2023` | 50,000 |
| Other current liabilities | `BAL_other_current_liabilities_2024` | 13,352 | `BAL_other_current_liabilities_2023` | 11,226 |
| Total current liabilities | `BAL_liabilities_current_2024` | 70,307 | — | 65,316 |
| Long-term debt | `BAL_debt_long_term_2024` | 36,755 | `BAL_debt_long_term_2023` | 37,483 |
| Other long-term liabilities | `BAL_other_lt_liabilities_2024` | 75,807 | — | 60,980 |
| Total liabilities | `BAL_liabilities_total_2024` | 182,869 | — | 163,779 |
| Common stock & paid-in capital | `BAL_common_stock_2024` | 46,238 | — | 17,781 |
| Retained earnings | `BAL_retained_earnings_2024` | 224,368 | — | 194,757 |
| Total shareholders' equity | `BAL_equity_shareholders_2024` | 270,606 | `BAL_equity_shareholders_2023` | 212,538 |

### 2.2 Income Statement Items

All items sourced from the ExxonMobil FY2024 10-K Income Statement (single-year). Named ranges follow the convention `INC_<item>`.

| Variable | Named Range | FY2024 ($M) | % of Sales |
|---|---|---|---|
| Net sales | `INC_sales` | 349,585 | 100.0% |
| Cost of goods sold | `INC_cost_goods_sold` | 276,274 | 79.0% |
| SG&A expenses | `INC_sga` | 73,311 | 21.0% |
| Depreciation | `INC_depreciation` | 23,442 | 6.7% |
| EBIT | `INC_ebit` | 49,869 | 14.3% |
| Other income | `INC_other_income` | 0 | — |
| Interest expense | `INC_interest_expense` | 996 | 0.3% |
| Taxable income | `INC_taxable_income` | 48,873 | 14.0% |
| Taxes | `INC_taxes` | 15,193 | 4.3% |
| Net income | `INC_net` | 33,680 | 9.6% |
| Dividends | `INC_dividends` | 16,704 | 4.8% |
| Addition to retained earnings | `INC_retained_earnings_addition` | 16,976 | 4.9% |

### 2.3 Cash Flow Statement Items

Indirect-method cash flow for FY2024. Named ranges follow the convention `CASH_<item>`.

| Variable | Named Range | FY2024 ($M) | Source / Notes |
|---|---|---|---|
| Net income | `INC_net` | 33,680 | Income Statement |
| Depreciation add-back | `INC_depreciation` | 23,442 | Income Statement |
| Δ Accounts receivable | `CASH_delta_receivables` | (5,666) | Balance Sheet change |
| Δ Inventories | `CASH_delta_inventories` | 1,596 | Balance Sheet change |
| Δ Other current assets | `CASH_delta_other_current_assets` | 150 | Balance Sheet change |
| Δ Accounts payable | `CASH_delta_accounts_payable` | 2,000 | Balance Sheet change |
| Δ Other current liabilities | `CASH_delta_other_current_liabilities` | (180) | Balance Sheet change |
| Total change in working capital | `CASH_delta_working_capital` | (2,100) | Sum of Δ items |
| Cash provided by operations | `CASH_operating` | 55,022 | Net income + D&A + ΔWC |
| Capital expenditures | `CASH_capex` | (24,306) | Analyst input |
| Sales of long-term assets | `CASH_asset_sales` | 4,987 | Analyst input |
| Other investing activities | `CASH_other_investing` | (619) | Analyst input |
| Cash from investments | `CASH_investing` | (19,938) | Sum of investing items |
| Δ Short-term debt | `CASH_delta_short_term_debt` | 899 | Analyst input |
| Δ Long-term debt | `CASH_delta_long_term_debt` | (5,912) | Analyst input |
| Dividends paid | `CASH_dividends_paid` | (16,704) | Income Statement |
| Stock repurchases (net) | `CASH_stock_repurchases` | (19,629) | Analyst input |
| Other financing | `CASH_other_financing` | (2,248) | Analyst input |
| Cash from financing | `CASH_financing` | (43,594) | Sum of financing items |
| Net change in cash | `CASH_net_change` | (8,510) | Operating + Investing + Financing |

### 2.4 Market & Analyst Inputs

| Variable | Named Range | Value | Notes |
|---|---|---|---|
| Share price | `share_price` | $111.73 | End-of-FY2024 closing price |
| Shares outstanding | `shares_outstanding` | 4,305 M | As reported |
| Cost of capital (WACC) | `cost_capital` | 8.2% | Estimated; students may refine with CAPM |
| Tax rate | `tax_rate` | 28.3% | Effective rate; statutory alternative is 21% |

---

## 3. Assumptions & Constraints

- All figures reported in millions of USD unless otherwise noted.
- Tax rate set at 28.3% (effective rate from FY2024 financials); may be adjusted to the statutory 21% rate with documented justification.
- Cost of capital estimated at 8.2% (WACC proxy). Students may refine using CAPM and class methodology.
- Share price of $111.73 reflects the end-of-fiscal-year (Dec 31, 2024) closing price for XOM.
- Start-of-year values use the FY2023 Balance Sheet (prior fiscal year) as the denominator base.
- Average values use the simple average of FY2023 and FY2024 Balance Sheet figures.
- Depreciation figure taken from the Income Statement (consistent with model structure).
- After-tax operating income approximates unlevered operating performance: Net Income + (1 − Tax Rate) × Interest Expense.
- Balance sheet integrity is verified by the check: Total Assets − (Total Liabilities + Shareholders' Equity) = 0.
- Cash flow working capital changes are derived directly from the year-over-year Balance Sheet deltas.
- No off-balance-sheet items, contingent liabilities, or minority interests are incorporated.
- All interest rates quoted on a simple annual basis.

---

## 4. Calculation Flow

The following pseudocode uses named ranges throughout. Ratios are computed sequentially; intermediate derived values must exist before ratio steps execute.

### Step 1 — Analyst Inputs (Blue Cells)

- Set `share_price` = 111.73
- Set `shares_outstanding` = 4305
- Set `cost_capital` = 0.082
- Set `tax_rate` = 0.283

### Step 2 — Start-of-Year Anchors

- `startYear_equity` = `BAL_equity_shareholders_2023`
- `startYear_inventory` = `BAL_inventories_2023`
- `startYear_receivables` = `BAL_receivables_2023`
- `startYear_total_assets` = `BAL_assets_total_2023`
- `startYear_total_capitalization` = `BAL_debt_long_term_2023` + `BAL_equity_shareholders_2023`

### Step 3 — Current-Year Derived Values

- `market_capitalization` = `share_price` × `shares_outstanding`
- `currentYear_after_tax_operating_income` = `INC_net` + (1 − `tax_rate`) × `INC_interest_expense`
- `currentYear_daily_sales_average` = `INC_sales` / 365
- `currentYear_cost_goods_sold_daily` = `INC_cost_goods_sold` / 365
- `currentYear_working_capital_net` = `BAL_assets_current_2024` − `BAL_liabilities_current_2024`
- `currentYear_total_capitalization` = `BAL_debt_long_term_2024` + `BAL_equity_shareholders_2024`

### Step 4 — Average (Mixed-Year) Values

- `avg_equity` = AVERAGE(`startYear_equity`, `BAL_equity_shareholders_2024`)
- `avg_total_assets` = AVERAGE(`startYear_total_assets`, `BAL_assets_total_2024`)
- `avg_total_capitalization` = AVERAGE(`startYear_total_capitalization`, `currentYear_total_capitalization`)

### Step 5 — Performance Ratios

- Market Value Added (MVA) = `market_capitalization` − `BAL_equity_shareholders_2024`
- Market-to-Book = `market_capitalization` / `BAL_equity_shareholders_2024`
- Economic Value Added (EVA) = `currentYear_after_tax_operating_income` − (`cost_capital` × `startYear_total_capitalization`)

### Step 6 — Profitability Ratios

- ROA = `currentYear_after_tax_operating_income` / `startYear_total_assets`
- ROC = `currentYear_after_tax_operating_income` / `startYear_total_capitalization`
- ROE = `INC_net` / `startYear_equity`
- ROA [AVG] = `currentYear_after_tax_operating_income` / `avg_total_assets`
- ROC [AVG] = `currentYear_after_tax_operating_income` / `avg_total_capitalization`
- ROE [AVG] = `INC_net` / `avg_equity`

### Step 7 — Efficiency Ratios

- Asset Turnover (`RATIO_asset_turnover`) = `INC_sales` / `startYear_total_assets`
- Receivables Turnover = `INC_sales` / `startYear_receivables`
- Average Collection Period = `startYear_receivables` / `currentYear_daily_sales_average`
- Inventory Turnover = `INC_cost_goods_sold` / `startYear_inventory`
- Days in Inventory = `startYear_inventory` / `currentYear_cost_goods_sold_daily`
- Profit Margin = `INC_net` / `INC_sales`
- Operating Profit Margin (`RATIO_operating_profit_margin`) = `currentYear_after_tax_operating_income` / `INC_sales`

### Step 8 — Leverage Ratios

- Long-term Debt Ratio = `BAL_debt_long_term_2024` / (`BAL_debt_long_term_2024` + `BAL_equity_shareholders_2024`)
- Long-term Debt-Equity Ratio = `BAL_debt_long_term_2024` / `BAL_equity_shareholders_2024`
- Total Debt Ratio = `BAL_liabilities_total_2024` / `BAL_assets_total_2024`
- Times Interest Earned = `INC_ebit` / `INC_interest_expense`
- Cash Coverage Ratio = (`INC_ebit` + `INC_depreciation`) / `INC_interest_expense`
- Debt Burden (`RATIO_debt_burden`) = `INC_net` / `currentYear_after_tax_operating_income`
- Leverage Ratio (`RATIO_leverage`) = `BAL_assets_total_2024` / `BAL_equity_shareholders_2024`

### Step 9 — Liquidity Ratios

- Net Working Capital to Assets = `currentYear_working_capital_net` / `BAL_assets_total_2024`
- Current Ratio = `BAL_assets_current_2024` / `BAL_liabilities_current_2024`
- Quick Ratio = (`BAL_cash_marketable_securities_2024` + `BAL_receivables_2024`) / `BAL_liabilities_current_2024`
- Cash Ratio = `BAL_cash_marketable_securities_2024` / `BAL_liabilities_current_2024`

### Step 10 — Du Pont Decomposition

- Du Pont ROA = `RATIO_asset_turnover` × `RATIO_operating_profit_margin`
- Du Pont ROE = `RATIO_leverage` × `RATIO_asset_turnover` × `RATIO_operating_profit_margin` × `RATIO_debt_burden`

> **Du Pont Identity Check:** Du Pont ROA should match the direct ROA from Step 6 within rounding tolerance. Du Pont ROE will differ from the direct ROE because the Du Pont version uses after-tax operating income while the direct version uses Net Income as the numerator — this is expected and reflects the decomposition methodology.

---

## 5. Outputs

| Output | Description | Format | Purpose |
|---|---|---|---|
| Ratio summary table | All 28+ ratios organized by category (Performance, Profitability, Efficiency, Leverage, Liquidity) | Table (Ratios sheet) | Core analytical output for CFO review |
| Intermediate values table | Named-range derived values used as ratio inputs | Table (Ratios sheet) | Auditability and formula transparency |
| Du Pont decomposition | ROA and ROE breakdown into component drivers | Table (Ratios sheet) | Identifies primary drivers of return |
| Cash flow statement | Indirect-method cash flow reconciling to cash change | Table (Cash Flow sheet) | Verifies operating/investing/financing breakdown |
| Balance sheet check | TA − TL&E = 0 verification | Single cell (Balance Sheet) | Model integrity check |
| Formula documentation | Named-range formula for each ratio shown in column D | Column (Ratios sheet) | Reproducibility and audit trail |
| Color coding legend | Blue = assumptions; Yellow = data inputs; Light yellow = student formula outputs | Color key (Ratios sheet) | User guidance |
| Executive summary | Key findings and strategic recommendations | 1–2 paragraphs | Stage 4 input for senior management memo |

---

## 6. Complete Ratio Reference

The table below documents every ratio formula, named-range notation, and expected FY2024 output. Light-yellow output cells in the workbook must contain a formula — not a hard-coded value.

### 6.1 Performance

| Ratio | Formula (Named Ranges) | FY2024 Output |
|---|---|---|
| Market Value Added (MVA) | `market_capitalization` − `BAL_equity_shareholders_2024` | $210,392M |
| Market-to-Book | `market_capitalization` / `BAL_equity_shareholders_2024` | 1.777× |
| Economic Value Added (EVA) | `currentYear_after_tax_operating_income` − (`cost_capital` × `startYear_total_capitalization`) | $13,892M |

### 6.2 Profitability

| Ratio | Formula (Named Ranges) | FY2024 Output |
|---|---|---|
| ROA (start-of-year) | `currentYear_after_tax_operating_income` / `startYear_total_assets` | 9.14% |
| ROC (start-of-year) | `currentYear_after_tax_operating_income` / `startYear_total_capitalization` | 13.76% |
| ROE (start-of-year) | `INC_net` / `startYear_equity` | 15.85% |
| ROA [AVG] | `currentYear_after_tax_operating_income` / `avg_total_assets` | 8.29% |
| ROC [AVG] | `currentYear_after_tax_operating_income` / `avg_total_capitalization` | 12.34% |
| ROE [AVG] | `INC_net` / `avg_equity` | 13.94% |

### 6.3 Efficiency

| Ratio | Formula (Named Ranges) | FY2024 Output |
|---|---|---|
| Asset Turnover * | `INC_sales` / `startYear_total_assets` | 0.929× |
| Receivables Turnover | `INC_sales` / `startYear_receivables` | 9.196× |
| Avg Collection Period (days) | `startYear_receivables` / `currentYear_daily_sales_average` | 39.7 days |
| Inventory Turnover | `INC_cost_goods_sold` / `startYear_inventory` | 11.00× |
| Days in Inventory | `startYear_inventory` / `currentYear_cost_goods_sold_daily` | 33.2 days |
| Profit Margin | `INC_net` / `INC_sales` | 9.63% |
| Operating Profit Margin * | `currentYear_after_tax_operating_income` / `INC_sales` | 9.84% |

\* Named range assigned: `RATIO_asset_turnover`, `RATIO_operating_profit_margin` — used in Du Pont decomposition.

### 6.4 Leverage

| Ratio | Formula (Named Ranges) | FY2024 Output |
|---|---|---|
| Long-term Debt Ratio | `BAL_debt_long_term_2024` / (`BAL_debt_long_term_2024` + `BAL_equity_shareholders_2024`) | 11.96% |
| Long-term Debt-Equity | `BAL_debt_long_term_2024` / `BAL_equity_shareholders_2024` | 0.136× |
| Total Debt Ratio | `BAL_liabilities_total_2024` / `BAL_assets_total_2024` | 40.33% |
| Times Interest Earned | `INC_ebit` / `INC_interest_expense` | 50.07× |
| Cash Coverage Ratio | (`INC_ebit` + `INC_depreciation`) / `INC_interest_expense` | 73.61× |
| Debt Burden * | `INC_net` / `currentYear_after_tax_operating_income` | 0.979 |
| Leverage Ratio * | `BAL_assets_total_2024` / `BAL_equity_shareholders_2024` | 1.676× |

\* Named range assigned: `RATIO_debt_burden`, `RATIO_leverage` — used in Du Pont decomposition.

### 6.5 Liquidity

| Ratio | Formula (Named Ranges) | FY2024 Output |
|---|---|---|
| NWC to Assets | `currentYear_working_capital_net` / `BAL_assets_total_2024` | 4.78% |
| Current Ratio | `BAL_assets_current_2024` / `BAL_liabilities_current_2024` | 1.308× |
| Quick Ratio | (`BAL_cash_marketable_securities_2024` + `BAL_receivables_2024`) / `BAL_liabilities_current_2024` | 0.949× |
| Cash Ratio | `BAL_cash_marketable_securities_2024` / `BAL_liabilities_current_2024` | 0.328× |

### 6.6 Du Pont Decomposition

| Ratio | Formula (Named Ranges) | FY2024 Output |
|---|---|---|
| Du Pont ROA | `RATIO_asset_turnover` × `RATIO_operating_profit_margin` | 9.14% |
| Du Pont ROE | `RATIO_leverage` × `RATIO_asset_turnover` × `RATIO_operating_profit_margin` × `RATIO_debt_burden` | 15.00% |

---

## 7. Model Review — What Worked & What to Improve

### What Worked Well

- Named range architecture: every formula on the Ratios sheet references named ranges rather than cell addresses, making formulas readable and resilient to layout changes.
- Balance sheet integrity check (TA − TL&E = 0) confirmed for both FY2024 and FY2023, validating data entry accuracy.
- Cash flow reconciliation: net change in cash (−$8,510M) matches the year-over-year change in `BAL_cash_marketable_securities` ($23,029 − $31,539), confirming indirect-method linkages.
- Du Pont ROA (9.14%) matched the direct ROA from the Profitability section within rounding, confirming internal consistency.
- Color coding (blue / yellow / light-yellow) provides clear visual separation between analyst assumptions, data inputs, and formula outputs.

### What to Improve

- The Quick Ratio formula references a placeholder named range (`BAL_receivables_YEAR`) rather than the explicit `BAL_receivables_2024` — this should be corrected to eliminate ambiguity.
- Start-of-year vs. average ROA/ROC/ROE are presented as separate rows without visual grouping; a subheader or indented style would improve scannability.
- The SG&A expense line item does not have its own named range (`INC_sga`) — it is currently derived from net sales minus COGS. An explicit named range would improve traceability.
- No data validation or input protection is applied to blue assumption cells; protecting those cells from accidental overwrite would improve model robustness.

### What Would Make the Model More Auditable

- Add a dedicated Documentation sheet listing every named range, its formula, and its source cell reference.
- Apply worksheet protection to data-input cells (yellow) and formula cells (light-yellow) separately so a new analyst cannot accidentally overwrite either.
- Add a version/change-log row at the top of the Notes sheet to track model revisions.

### Additional Analysis Worth Including

- Industry peer comparison: benchmark XOM's ratios against CVX, BP, and SHEL to contextualize performance.
- Multi-year trend (FY2022–FY2024): add a third year to identify whether efficiency and profitability are improving or deteriorating.
- Free cash flow yield: (Cash from Operations − CapEx) / Market Capitalization, providing a market-relative liquidity metric.
- Interest coverage sensitivity: model how ROC and EVA change under alternative WACC assumptions (±1%).

---

## 8. Limitations & Next Steps

This model does not incorporate industry peer comparisons, multi-year trend analysis, off-balance-sheet items, minority interests, or deferred tax adjustments. Ratio outputs reflect a single fiscal year snapshot (FY2024) and should be interpreted alongside industry benchmarks and trend data for meaningful strategic conclusions.

The next phase (Stage 4) involves constructing a structured AI prompt using this specification as the input blueprint, then generating a final analysis interpreting the 28+ ratio outputs for senior management in the form of an executive memo with strategic recommendations.

| Stage 3 Spec Element | What It Enables in Stage 4 |
|---|---|
| Named ranges with precise definitions | AI uses standardized variable names — no improvisation or ambiguity |
| Step-by-step calculation flow (Section 4) | AI generates correct, auditable formulas in the right sequence |
| Model review and improvement notes (Section 7) | AI builds an improved version, not just a replica of the prototype |
| Explicit output requirements (Section 5) | AI produces exact tables, charts, and sections needed for the memo |
| Ratio reference with expected outputs (Section 6) | AI can validate its own formula outputs before writing the analysis |

---

*University of Hawaiʻi at Mānoa · BUS-314 International Corporate Finance*
