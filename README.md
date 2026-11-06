# LuminaTech Sales Briefing

[![Case Study](https://img.shields.io/badge/Case_Study-Read_the_page-111111?style=for-the-badge)](https://racheltran184.github.io/Python-Project-2/)
[![Python](https://img.shields.io/badge/Python-pandas_·_NumPy_·_statsmodels-3776AB?style=for-the-badge&logo=python&logoColor=white)](#how-to-run)

**A steady business, misread by its own data.** Five years of sales for LuminaTech, a commercial and industrial lighting supplier in Australia and New Zealand: 1.09 million ERP lines cleaned in Python, audited before any analysis, and turned into a 90-day plan and a three-year strategy for the leadership team.

---

## Overview

The leadership team wanted one read of five years of sales: *are we growing, where is profit made, and where should effort go next?* The cleaning pipeline (Task 1) and the original analysis (Task 2) answered those questions. A re-check (Task 3) then tested the yardstick behind the answers, found three blind spots in the extract, and changed several of the conclusions. The [case study](https://racheltran184.github.io/Python-Project-2/) presents the re-checked findings; every figure in it comes from Task 3.

| Item | Detail |
|---|---|
| **Company** | LuminaTech (anonymised): commercial and industrial lighting, sold across Australia and New Zealand |
| **Data** | 1,089,726 ERP sales lines, January 2020 to December 2024; NZD and USD converted to AUD at Reserve Bank of Australia monthly rates |
| **Scope** | 3,884 external customers, 13 sales districts, 10 company codes, around 25,000 products |
| **Methods** | Python (pandas, NumPy, SciPy, statsmodels), rule-based imputation, Welch t-tests, OLS regression with HC3 robust errors |
| **Origin** | Postgraduate business analytics group project; re-checked line by line for the case study |

### Headline numbers

| Figure | What it means |
|---:|---|
| **42.6%** | Real gross margin to external customers, not the 66% the raw file shows |
| **$248M** | Sales a year, steady from 2020 to 2024 |
| **70%** | Share of external sales still from Traditional lighting |
| **73%** | Share of external sales from the most frequent fifth of customers |
| **$8M** | Credited back to external customers each year |

---

## What changed when I checked the numbers

| Question | Original result (Task 2) | Re-checked (Task 3) | Verdict |
|---|---|---|---|
| Is the business shrinking? | 99% fall from the 2020 peak | Flat: $244M to $257M a year; July to December is missing for 7 of 10 company codes | Changed |
| Gross margin | 62% average | 42.6% to external customers, on lines with a real cost | Changed |
| Projects against normal orders (Welch t-test) | Projects +13.7 points | 43.8% against 44.1%: no premium | Changed |
| Metro against regional districts (Welch t-test) | 0.6 points apart, negligible | Regional 45.2%, metro 42.2% | Sharper |
| LED against Traditional (Welch t-test) | LED 2.8 points lower | LED 40.0%, Traditional 42.6% | Holds |
| What drives customer value? (OLS) | Order frequency, R² 0.81 | Spend rises about one-for-one with orders (R² 0.72 on orders alone) | Holds |
| What drives unit price? (OLS) | Volume is the strongest discount, R² 0.56 | Highest-volume tenth sells at $6.34 a unit, lowest-volume tenth at $280 | Holds |

### Three blind spots in the extract

1. **Half of every year is missing.** 98% of recorded sales fall in January to June, and 7 of the 10 company codes have no July to December lines at all. Annual totals are flat ($257M in 2020, then $249M, $244M, $244M and $246M). The "99% fall" compares May 2020 ($48M) with December 2024 ($0.4M), a month when only 3 company codes report; May 2024 sold $47M.
2. **Placeholder costs.** On 52% of lines the recorded cost equals the quantity, a $1.00 unit cost. Of the affected products sold on ten or more lines, 97% carry a real cost on their other lines, so these are placeholders. Left in, they lift the margin to 65.9%; taking them out gives 36.1%.
3. **Intercompany sales.** 22% of the file is intercompany and head-office sales at a 12.8% margin. On external customers alone, gross margin is **42.6%**, the yardstick for every finding below.

---

## Six findings

*External customers only; margins on lines with a real cost; "a year" is the 2020–2024 average.*

**1. Sales are steady, but the mix isn't moving.** Sales held between $244M and $257M a year, with about 3,450 active external customers each year. Traditional lighting is 70% of external sales, accessories 18% and LED 11–12%; LED sales fell from $23.8M in 2020 to $21.7M in 2024.

**2. Margin is made in the mix and the channel, not in projects.** Accessories earn 44.9%, Traditional 42.6% and LED 40.0%, so every dollar that moves from Traditional to LED today costs margin. EDI reorders earn 45.1%, the best channel, on only 4.2% of sales. Project orders earn 43.8% against 44.1% for normal orders.

**3. A fifth of customers bring 73% of sales.** Ranked by orders a year, the most frequent fifth of the 3,884 external customers place a median of 58 orders and spend $116K a year; the least frequent fifth order less than once a year and spend $372. Spend rises about one-for-one with order frequency, and the top 10% of customers bring 64% of sales. Ten per cent more orders from the 1,554 accounts in the top two fifths would add roughly $18M a year at today's order value of about $2,640 (an illustration, not a forecast).

**4. Volume is sold at commodity prices.** The highest-volume tenth of products carries 57% of external sales at a median $6.34 a unit; the lowest-volume tenth sells at a median $280. Each doubling of volume comes with a unit price about 26% lower.

**5. $8M a year flows back as credits.** Credits reverse 4.0% of gross external sales. Half is goods coming back ($4.0M a year); $2.75M a year is price-only credits, where no goods return. Melbourne credits 4.7% of its sales, against 3.6–4.2% in Sydney, Brisbane and Perth, and the Northern Region of New Zealand 8.8%.

**6. Regions are closer than they look.** The four biggest districts sit within two points: Sydney 43.0%, Perth 42.3%, Brisbane 42.2%, Melbourne 41.3%. Brisbane, once flagged for an urgent margin audit, sits in the middle. The four smaller Australian districts earn 45.2% against 42.2% for the five largest.

---

## The next 90 days

| # | Owner | Action | Why | Track |
|---|---|---|---|---|
| 01 | CFO & finance | Restate margin on real costs | Raw file shows 65.9%; costed lines to external customers show 42.6% | Share of sales lines with a valid cost |
| 02 | CFO & finance | Recover the missing half-years | 98% of recorded sales fall in January to June | Company codes reporting all 12 months |
| 03 | CFO & finance | Report customers apart from intercompany | 22% of the file at a 12.8% margin | External gross margin, monthly |
| 04 | Sales director | Protect the top fifth (777 accounts) | They bring 73% of external sales | Orders per top-fifth account, per quarter |
| 05 | Sales director | Move the next fifth up with reorder prompts and EDI | EDI earns 45.1% on 4.2% of sales; 10% more orders from the top two fifths adds about $18M a year | EDI share of external sales |
| 06 | Sales director | Put price-only credits behind approval | $2.75M a year credited with no goods returned | Price-only credits per month |
| 07 | Product & pricing | Set floors for the commodity core | Highest-volume tenth: 57% of sales at a median $6.34 a unit | Margin on the top-volume tenth |
| 08 | Product & pricing | Value-price the specialised tail | Lowest-volume tenth sells at a median $280 a unit | Average discount on low-volume lines |
| 09 | Product & pricing | Sell LED as a solution, not a box | LED earns 40.0%, accessories 44.9% | LED margin and accessory attach rate |
| 10 | Operations & data | Run the pipeline every month with data checks | One unchecked extract produced a 99% fall that never happened | Data checks passed each month |
| 11 | Operations & data | Find out why goods come back | Returns cost $4.0M a year; Melbourne 4.7%, Northern NZ 8.8% | Credit rate by district |
| 12 | Operations & data | Plan the peak once the data is confirmed | 98% of sales in six months needs different working capital | Fill rate in peak months |

## Three-year strategy

| Track | Now (0–6 months) | Next (6–18 months) | Later (18–36 months) |
|---|---|---|---|
| **Manage by true margin** | One trusted monthly margin pack | Cost to serve by customer and product | Price floors and value pricing, refreshed monthly |
| **Grow by frequency** | Protect the 777 accounts that bring 73% of sales | EDI and a reorder portal for the next tier | Grow share of wallet, starting with accessories |
| **Move the range before the rules do** | Map Traditional products against the 2027 fluorescent phase-out | LED retrofit packages for Traditional customers | Lift LED from 11% of external sales in 2024 |

**Business scorecard:** external gross margin on costed lines (42.6%) · orders per top-two-fifths account · LED share of external sales (11.3% in 2024) · credits as a share of gross sales (4.0%).
**Data scorecard:** sales lines with a valid cost (48%) · company codes reporting all twelve months (3 of 10) · intercompany separated in every customer report · monthly pipeline checks passed.

---

## How it's built

**Task 1, cleaning pipeline.** Two raw ERP exports become one table of 1,087,121 lines and 39 columns:

```python
df = (
    df
    .pipe(clean_dates, date_col='accounting_date')   # numbers to dates; drops lines with no usable date
    .pipe(clean_identifiers, standard_cols=integer_columns, decimal_fix_cols=decimal_fix_columns)
    .pipe(clean_strings, target_cols=string_columns)  # trim, upper-case, standard missing values
    .pipe(filter_sparse_rows, threshold=0.5)          # drop mostly empty lines
)
df_cleaned = run_imputation(df)                       # business rules first, statistical fallbacks last,
                                                      # then validation against Metadata.xlsx
df_final = apply_currency_conversion(df_cleaned, fx_path='final_exchange rate.xls')
```

- 1,089,726 raw lines; 2,562 dropped for no usable date (2,552 of them dated 29 February in a non-leap year, about $1M of sales a year) or for being mostly empty; 43 dropped for invalid codes.
- Missing values filled with product-hierarchy and code-prefix rules first, grouped and statistical fallbacks last; none remain.
- NZD and USD converted to AUD at RBA monthly rates; returns and credits kept; skewed values given an arcsinh transform for modelling.

**Task 2, original analysis.** Seasonality, product, district, currency and supply-chain views; Welch t-tests (with a Yeo-Johnson transform where skewed); OLS models for customer value (R² 0.81) and unit price (R² 0.56) with HC3 robust errors.

**Task 3, re-check before the board.** Tests completeness by month and company code, tests whether costs are real, separates intercompany and head office (districts 710, 720 and 545) from customers, then recomputes the six findings on external customers and lines with a real cost. It writes `briefing_figures.json`, the data behind every chart on the case study page.

## Repository

| File | What it is |
|---|---|
| `Task 1: Clean Data.ipynb` | Cleaning, imputation, validation and currency conversion; writes `dataset_cleaned.csv` |
| `Task 2: Data Analysis Workflow.ipynb` | Original group-project analysis |
| `Task 3: Re-check Before the Board.ipynb` | Audit of the extract and every figure in the case study |
| `briefing_figures.json` | Figures behind the case study charts, written by Task 3 |
| `index.html` | Case study page, served at [racheltran184.github.io/Python-Project-2](https://racheltran184.github.io/Python-Project-2/) |
| `Dataset1.csv.gz`, `Dataset2.csv.gz` | Raw ERP exports, gzip-compressed |
| `Metadata.xlsx` | Code lists used for validation |
| `final_exchange rate.xls` | RBA monthly exchange rates |

## How to run

1. Unzip `Dataset1.csv.gz` and `Dataset2.csv.gz` into the project folder, next to `Metadata.xlsx` and `final_exchange rate.xls`.
2. Install `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `statsmodels`, `openpyxl` and `xlrd` (for the `.xls` rates file).
3. Run **Task 1** to create `dataset_cleaned.csv` (about 1.09M rows × 39 columns, a few minutes on a laptop).
4. Run **Task 2** for the original analysis, then **Task 3** for the re-check and `briefing_figures.json` (about a minute; pandas and NumPy only).

## Data and privacy

Built on an anonymised ERP sales extract used in a postgraduate business analytics group project; "LuminaTech" is not the company's real name. Figures are in AUD. Patterns are associations in historical sales, not proof of cause, and the recommendations are illustrative: each would be piloted against a comparison group before any rollout.
