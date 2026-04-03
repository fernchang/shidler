from datetime import date
 
# ── Memo Metadata ────────────────────────────────────────────────────────────
TITLE        = "ExxonMobil Corporation (XOM) — Financial Ratio Analysis Memo"
CREATED_BY   = "Fern Chang"
UPDATED_BY   = "Fern Chang"
DATE_CREATED = "April 2, 2026"
DATE_UPDATED = date.today().strftime("%B %d, %Y")
VERSION      = "0.1"
LLM_USED     = "Claude (Anthropic)"  # set to "" to omit this line
 
# ── Section Content ───────────────────────────────────────────────────────────
EXECUTIVE_SUMMARY = """\
This memo outlines a comprehensive financial ratio analysis of Exxon Mobil Corporation (NYSE: XOM),
one of the world's largest publicly traded oil and gas companies. The analysis covers fiscal year
2024, during which ExxonMobil reported earnings of $33.7 billion and cash flow from operations of
$55.0 billion — its third-best year in a decade. The objective is to assess XOM's profitability,
liquidity, leverage, and efficiency across key financial dimensions. Key takeaways will inform
investment and valuation decisions by benchmarking XOM against historical performance and industry
peers. This memo describes the analytical approach and sets the stage for a full quantitative model
to follow.\
"""
 
COMPANY_BACKGROUND = """\
Exxon Mobil Corporation is a vertically integrated petroleum refining and energy company
headquartered in Spring, Texas, operating across upstream exploration and production, downstream
refining, and chemical products segments. The fiscal year under review is **FY2024** (ending
December 31, 2024).
 
ExxonMobil achieved record production in the Permian Basin and Guyana, record sales volumes of
high-value products, and distributed $36.0 billion to shareholders — more than all but five
companies in the S&P 500. Revenue of $349.6 billion grew 3.3% year-over-year but missed analyst
estimates, and forward revenue is expected to decline modestly over the next three years.
 
A ratio analysis is warranted to look beneath these headline figures — to evaluate whether XOM's
profitability is structurally improving, how efficiently it deploys capital, and whether its balance
sheet can sustain its shareholder return commitments. Results will inform portfolio positioning and
relative value assessments versus integrated energy peers.\
"""
 
APPROACH = """\
The analysis will draw primarily on XOM's FY2024 10-K filing (submitted to the SEC on February 19,
2025) and quarterly earnings press releases. Five ratio categories will be examined:
 
**Profitability** — net margin, return on equity (ROE), return on capital employed (ROCE), and
earnings per share trends. XOM reported a ROCE of 12.7% for 2024, leading the industry for the year.
 
**Liquidity** — current ratio and quick ratio to assess short-term obligations coverage.
 
**Leverage & Solvency** — debt-to-equity, interest coverage, and debt-to-EBITDA to evaluate
balance sheet risk.
 
**Efficiency** — asset turnover and capital expenditure efficiency. Cumulative structural cost
savings reached $12.1 billion since 2019, including $2.4 billion added in 2024.
 
**Valuation** — P/E, EV/EBITDA, and price-to-free-cash-flow multiples relative to peers.
 
Results will be organized in a structured model with a summary dashboard, time-series comparisons
(2020–2024), and peer benchmarking.\
"""
 
LIMITATIONS = """\
Several constraints apply to this analysis. First, oil and gas financials are highly sensitive to
commodity price assumptions — ratios in any single year reflect macro conditions as much as
management quality. Second, XOM's 2024 results include contributions from the Pioneer Natural
Resources acquisition (closed mid-2024), which complicates direct year-over-year comparisons.
Third, segment-level ratio analysis is limited by how costs and capital are allocated across
upstream, downstream, and chemicals in public disclosures.
 
Immediate next steps are to build the quantitative ratio model in Excel using 10-K data, run peer
comparisons against Chevron (CVX), Shell (SHEL), and BP, and produce a one-page summary dashboard.
This will feed into a final investment memo and, if warranted, a discounted cash flow valuation.\
"""
 
REFERENCES = """\
ExxonMobil. (2025, January 31). *ExxonMobil announces 2024 results* [Press release].
  https://corporate.exxonmobil.com/news/news-releases/2025/0131_exxonmobil-announces-2024-results
 
Exxon Mobil Corporation. (2025, February 19). *Annual report on Form 10-K for fiscal year ending
  December 31, 2024* (Accession No. 0000034088-25-000010). U.S. Securities and Exchange Commission.
  https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK=0000034088&type=10-K
 
Yahoo Finance / Simply Wall St. (2025, February 1). *Exxon Mobil full year 2024 earnings:
  Revenues disappoint*.
  https://finance.yahoo.com/news/exxon-mobil-full-2024-earnings-125038353.html\
"""
 
 
# ── Template ──────────────────────────────────────────────────────────────────
def generate_memo() -> str:
    llm_line = f"**LLM Used:** {LLM_USED}\n" if LLM_USED else ""
 
    return f"""\
# {TITLE}
 
**Created by:** {CREATED_BY}
**Updated by:** {UPDATED_BY}
**Date Created:** {DATE_CREATED}
**Date Updated:** {DATE_UPDATED}
**Version:** {VERSION}
{llm_line}
---
 
## Executive Summary
 
{EXECUTIVE_SUMMARY}
 
---
 
## Company Background & Objectives
 
{COMPANY_BACKGROUND}
 
---
 
## Approach
 
{APPROACH}
 
---
 
## Limitations & Next Steps
 
{LIMITATIONS}
 
---
 
## References
 
{REFERENCES}
"""
 
 
# ── Run ───────────────────────────────────────────────────────────────────────
if __name__ == "__main__":
    output_path = "xom_memo.md"
    with open(output_path, "w", encoding="utf-8") as f:
        f.write(generate_memo())
    print(f"Memo saved to: {output_path}")
