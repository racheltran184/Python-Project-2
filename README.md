
# LuminaTech Sales Analytics Platform

## 📊 Executive Summary

**LuminaTech faces a critical inflection point.** Our analysis of approximately **1.09 million transactions** reveals a business with historically strong profitability (baseline margin around **62%**) but facing significant strategic challenges:

- **Legacy Dependency**: **70.5%** of profit from Traditional lighting products
- **Geographic Concentration**: Four regions drive **80%+** of profitable volume
- **Supply Chain Fragility**: Limited redundancy in key fulfilment corridors

**However, clear opportunities exist:**

- **Project Orders** deliver a **13.7% higher margin** than standard business
- **Specialised Applications** command **very large price premiums** (up to **~178%** uplift vs baseline)
- **Transaction Frequency** is the #1 driver of customer value
- **LED Technology** has strong growth potential at premium margins, despite currently lower profit share

---

## 🏗️ Platform Architecture

This end-to-end analytics platform transforms raw ERP data into strategic business intelligence through two integrated components:

### 1. Data Processing Pipeline

Transforms fragmented, raw CSV exports into a single, high-integrity analytical dataset.

**Key Capabilities:**

- **Currency Normalisation**  
  Converts multi-currency transactions to **AUD** using **Reserve Bank of Australia monthly FX rates**, creating `value_sales_aud` and `value_cost_aud`.
- **Intelligent Imputation**  
  Uses business logic and hierarchy rules (e.g. deriving `item_type` from `item_group_code`, mapping currency/tax from `country_code`) to fill missing values.
- **Outlier Management**  
  Preserves “whale transactions” (very large project orders and returns) but applies **arcsinh transformation** to key financial variables to reduce skew while retaining zero and negative values.
- **Quality Validation**  
  Validates codes against a `Metadata.xlsx` **“Golden Source”** and removes or fixes invalid entries so all identifiers (customers, items, warehouses, chains) are consistent.

**Input → Output:**

- **Input:**  
  `Dataset1.csv`, `Dataset2.csv` (raw ERP exports)  
  `Metadata.xlsx` (code definitions)  
  `final_exchange rate.xls` (RBA FX rates)
- **Output:**  
  `dataset_cleaned.csv` (**1,087,121 transactions × 39 features**)
- **Processing Time:**  
  Approximately **2–3 minutes** for full transformation on a standard laptop environment.

The core cleaning flow is implemented as a modular, reusable pipeline:

```python
df_cleaned = (
    load_raw_data()
    .pipe(clean_dates)          # Fix date formats
    .pipe(clean_identifiers)    # Standardise IDs and codes
    .pipe(clean_strings)        # Normalise text, trim, uppercase
    .pipe(impute_missing_data)  # Business-rule & grouped imputation
    .pipe(convert_currency)     # Normalise to AUD (monthly FX)
    .pipe(transform_features)   # Arcsinh-transform skewed financials
)
````

---

### 2. Sales Analytics Framework

The analytical layer builds on `dataset_cleaned.csv` and delivers insights through three tiers:

| Tier                         | Focus                | Key Deliverables                                                                                                 |
| ---------------------------- | -------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Exploratory**              | Trends & patterns    | Financial seasonality, margin stability, product and geographic performance, currency impact, supply chain flows |
| **Comparative**              | Statistical testing  | Metro vs regional, project vs standard orders, LED vs Traditional product economics                              |
| **Predictive / Explanatory** | Causal understanding | OLS models for customer value drivers and product pricing determinants                                           |

Conceptually, the analysis is structured as:

```python
1. EXPLORATORY INSIGHTS (Section 2)
   ├─ Financial seasonality & margin stability
   ├─ Product category performance (Traditional vs LED vs Accessories)
   ├─ Geographic performance by customer district
   ├─ Currency and FX impact
   └─ Supply chain route analysis (warehouse → destination)

2. COMPARATIVE ANALYSIS (Section 3)
   ├─ Metro vs regional profitability (Welch’s t-tests)
   ├─ Project vs standard order margin comparison
   └─ LED vs Traditional unit economics

3. EXPLANATORY INFERENCE (Section 4)
   ├─ Customer value drivers (OLS regression, R² ≈ 0.81)
   └─ Product pricing determinants (OLS regression, R² ≈ 0.56)
```

---

## 🔍 Key Analytical Findings

### Financial Health Assessment

From 2020–2024, LuminaTech has undergone a severe contraction. Monthly sales have fallen from a **2020 peak of ≈ $48.1M** to around **$0.4M**, representing a **~99% decline in revenue scale**. At peak, margins reached around **68.8%**, while the most recent period sits closer to **41.8%**, significantly below the historical/baseline benchmark of **~62%**.

Seasonality is still visible: **May/June** remain the strongest trading months and **December** consistently underperforms. However, the absolute level of activity is now so low that these patterns matter more as directional indicators than as core revenue drivers. The revenue profile has become highly concentrated in a small number of “whale” months tied to very large project orders, with long stretches of minimal activity in between.

---

### Product Portfolio Analysis

The product portfolio is **heavily tilted towards legacy Traditional lighting**, which currently contributes **~70.5% of total margin**. **Accessories** contribute ~**18.5%**, and **LED products ~11.1%** of margin.

| Product Category | Margin Contribution | Strategic Position                   | Recommended Action                             |
| ---------------- | ------------------- | ------------------------------------ | ---------------------------------------------- |
| **Traditional**  | 70.5%               | Cash Cow (declining/regulatory risk) | Harvest profit, manage run-off, plan phase-out |
| **Accessories**  | 18.5%               | Star (stable enabler)                | Bundle with LED, support upsell                |
| **LED**          | 11.1%               | Question Mark (growth option)        | Accelerate investment and differentiation      |

**Pricing power analysis** shows that products serving **specialised applications** (e.g. industrial, medical, marine) and certain advanced technologies are able to command **very large price premiums**, with some technologies (e.g. specific INZ codes) achieving **up to ~178% uplift** versus baseline price levels. Conversely, high-volume, standardised items experience a **strong commodity effect**, with unit prices eroding by roughly **~59%** relative to low-volume, specialised items due to aggressive discounting.

This combination of heavy Traditional dependency and premium potential in specialised/LED segments highlights a clear strategic imperative: **use the remaining Traditional “cash cow” period to fund a disciplined pivot into higher-value LED and specialised offerings.**

---

### Geographic Performance Matrix

Regional analysis classifies customer districts into **Stars**, **Volume traps**, and **Concerns**:

| Region    | Category       | Revenue (≈) | Margin % | Strategic Priority           |
| --------- | -------------- | ----------- | -------- | ---------------------------- |
| Melbourne | ⭐ Star         | $205M       | 61.65%   | Replicate & expand           |
| Sydney    | 📦 Volume Trap | $218M       | 59.46%   | Tighten pricing, improve mix |
| Perth     | ⭐ Star         | $161M       | 63.72%   | Protect & reward             |
| Brisbane  | ⚠️ Concern     | $183M       | 53.27%   | **Immediate margin audit**   |

A small set of regions accounts for **80%+** of profitable volume. **Melbourne and Perth** exhibit the desired profile of healthy volume with strong margins and should serve as the playbook to copy. **Sydney** generates high revenue but suffers from softer margins, while **Brisbane** is the most concerning case: it combines high volume with a **~7.2 percentage-point margin deficit** versus the company average, indicating problematic discounting, cost leakage, or product mix.

---

### Customer Value Drivers

The **Customer Value Model (OLS, R² ≈ 0.81)** quantifies what makes a customer valuable:

| Driver                          | Impact Direction | Business Implication                                           |
| ------------------------------- | ---------------- | -------------------------------------------------------------- |
| **Transaction frequency**       | Strong positive  | Increasing purchase frequency is the most powerful value lever |
| **Chain membership (e.g. CET)** | Strong positive  | Some chains are worth **~5.4×** the baseline customer value    |
| **Credit/return activity**      | Strong negative  | High returns destroy value and must be actively managed        |
| **Project order share**         | Medium positive  | Higher project mix supports better margins and CLV             |

The key conclusion is that **behaviour matters more than single-order size**. Encouraging a customer to buy **more often** is substantially more valuable than merely pushing up basket size, and customers with heavy credit/return activity are net value destroyers once operational and margin impacts are considered.

---

### Pricing Power & Product Attributes

The **Product Pricing Model (OLS, R² ≈ 0.56)** focuses on **unit price drivers**. It shows that:

* **Technology and application** (e.g. LED, medical, marine, industrial) are consistent sources of **pricing power**.
* **High-volume, standardised SKUs** tend to be **commoditised**, with prices sharply discounted relative to niche, specialised products.
* There is a clear **volume–price trade-off**: products that carry the heaviest volume often do so at the cost of unit margin.

Strategically, this reinforces the need to **protect and grow specialised, lower-volume premium lines**, while imposing tighter governance and value-added differentiation on high-volume commoditised products.

---

### Supply Chain Efficiency & Risk

The supply chain network exhibits **strong local fulfilment but limited redundancy**:

| Warehouse | Primary Destination | % of Volume | Risk Level |
| --------- | ------------------- | ----------- | ---------- |
| Perth     | Perth customers     | 89%         | 🔴 High    |
| Melbourne | Melbourne customers | 84%         | 🟡 Medium  |
| Brisbane  | Brisbane customers  | 79%         | 🟡 Medium  |

Most warehouses overwhelmingly serve their local region. This is operationally efficient under normal conditions but creates **single-point-of-failure risk** if a key site experiences disruption. **Padstow** effectively acts as a hub for Sydney and some interstate flows, suggesting an opportunity to formalise hub-and-spoke design and introduce **greater redundancy** in the most critical corridors.

---

## ⚙️ Execution Guide

### Prerequisites

Install the core libraries:

* `pandas`, `numpy` – data processing
* `matplotlib`, `seaborn` – visualisation
* `statsmodels`, `scipy` – statistical testing and regression

### How to Run

1. **Setup**

   * Place raw CSVs, `Metadata.xlsx`, and `final_exchange rate.xls` in the project root.

2. **Clean Data**

   * Run the cleaning notebook/script (e.g. `Task 1/ Clean Data.ipynb`).
   * Output: `dataset_cleaned.csv` (≈ 1.09M rows × 39 columns).

3. **Analyse**

   * Run the analysis notebooks (e.g. `Task 2/ Data Analysis Workflow.ipynb`).
   * Outputs:

     * Statistical summaries and hypothesis test results
     * OLS regression tables
     * High-resolution charts for presentations and reports

---

## 🧭 Strategic Takeaways

* The business is **no longer operating at scale**, but residual strengths in project work, specialised applications, and select regions can form the basis of a focused turnaround.
* **Traditional lighting** still funds the business but represents a **declining and risky cash cow**; it must be used to finance an accelerated shift into **LED and premium specialised segments**.
* **Customer and regional strategies** should focus on:

  * Increasing **frequency** with high-potential customers,
  * Replicating the **Melbourne/Perth model**,
  * Fixing or exiting structurally weak profiles such as **Brisbane**.
* **Supply chain design** needs deliberate redundancy in the most critical flows to reduce fragility and support any future growth phase.

