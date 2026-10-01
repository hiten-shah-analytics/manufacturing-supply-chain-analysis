# Manufacturing Supply Chain Analysis: Purchase → Inventory → Sales

**Tracing how problems in one department show up as costs in another, using six months of operational data from a wear-parts foundry.**

`Python (pandas)` · `SQL (SQLite)` · `Data quality auditing` · `Power BI / matplotlib`

---

## The problem

A foundry that makes wear parts for cement, mining, power and sugar customers keeps three separate records: purchase orders, a store inventory ledger, and customer sales orders. Each department looks at its own sheet. The question was whether joining the three would reveal problems that no single department can see on its own.

**Data:** 131 purchase orders, 1,201 store ledger transactions and 407 sales orders, January–June 2026.

## Key findings

### 1. One vendor's delays became 53 late customer orders
**Purchase → Store → Sales**

Four purchase orders from a single vendor arrived 22–27 days late in February–March, against promised lead times of 9–13 days. Steel Scrap stock fell from 2,585 kg to 324 kg, and **53 customer orders worth ₹59 lakh were dispatched late** across 12 customers, averaging 21.9 days late. Every affected product is made from the delayed materials.

The root cause is concentration: **MS Plates come from this vendor on 17 of 17 purchase orders**, with no second source. Steel Scrap, which is split 50/50 between two vendors, recovered faster.

![Steel Scrap stock vs delayed orders](images/finding1_supply_break.png)

### 2. Purchasing didn't follow the sugar season down
**Sales → Store → Purchase**

Sugar-mill orders fell **93%** from January to June as the crushing season ended, and consumption of the two sugar-line raw materials fell with them. Purchasing kept buying: **17 more purchase orders (25,178 kg) were placed after demand collapsed.** Pig Iron stock rose 774% and Ferro Manganese 1,004%, leaving **₹9 lakh of working capital** in stock with 6–8 months of cover.

![Sugar demand vs raw material stock](images/finding2_seasonal_mismatch.png)

### 3. Input cost rises reached the price list two months late
Ferro Chrome, the main alloying input for crusher liners, rose 37% between January and April. The crusher liner selling price didn't move until April, so the margin in between was absorbed.

## Data quality audit

Nine checks were run before any analysis. Every issue was logged with its evidence, and records were **corrected rather than deleted** wherever the data allowed it.

| Issue | Impact if left uncorrected | Action |
|---|---|---|
| 6 sales orders entered twice under new IDs | Revenue overstated by ₹8.9 lakh (2.1%) | Removed the duplicates |
| 4 customers entered under two spellings | 20 names for 16 real customers; one customer ranked #5 instead of #2 | Standardised names, verified by matching industry and region |
| 4 ledger rows with wrong quantities (sign reversals and unsaved values) | January Steel Scrap consumption understated by 1,153 kg | Recovered true values from the running-balance column (99.66% of rows reconcile) |
| 161 orders marked "Fulfilled" but dispatched 1–3 days late | On-time delivery reads **86%**; the real figure is **41%** | Measured on-time delivery from dates, not the status field |

![On-time delivery gap](images/on_time_delivery_gap.png)

Three things that looked like errors were confirmed as correct: 7 purchase orders without delivery dates were still in transit at the cut-off, 29 sales orders without dispatch dates were all Pending or Cancelled, and 6 store entries after 30 June were production completions from a slightly later extract.

## Recommendation

**Qualify a second MS Plates supplier within 60 days and set a minimum stock trigger on Steel Scrap and MS Plates.**

1. Approach the existing second Steel Scrap vendor first. It already delivers reliably, with an average slip of 0.4 days.
2. Split volume about 70/30 once qualified, so the alternative is active rather than theoretical.
3. Set a reorder trigger at one month of consumption (about 4,000 kg Steel Scrap, 2,500 kg MS Plates) and review it weekly. Stock fell to 324 kg in March with no warning raised.

Also: report on-time delivery from actual dispatch dates rather than the status field.

Full write-up: [reports/findings_summary.md](reports/findings_summary.md)

## Approach

| Step | Notebook | What it does |
|---|---|---|
| 1 | [`01_data_quality_audit.ipynb`](notebooks/01_data_quality_audit.ipynb) | Profiles the three sheets, runs nine quality checks, builds an issue log, exports cleaned tables |
| 2 | [`02_cross_department_sql_analysis.ipynb`](notebooks/02_cross_department_sql_analysis.ipynb) | Loads the cleaned tables into SQLite and joins purchase → inventory → sales using CTEs, window functions and conditional aggregation; produces the charts |

## Repository structure

```
├── data/
│   ├── raw/case_study_datasets.xlsx      # Purchase, Store_Inventory, Sales sheets
│   └── processed/                        # Cleaned CSVs, issue log, SQL results, chart data
├── notebooks/
│   ├── 01_data_quality_audit.ipynb
│   └── 02_cross_department_sql_analysis.ipynb
├── images/                               # Charts used in this README
├── reports/findings_summary.md           # Business-facing write-up
└── requirements.txt
```

## How to run

```bash
pip install -r requirements.txt
cd notebooks
jupyter notebook
```

Run notebook 01 first. It writes the cleaned files that notebook 02 reads.

## About the data

This project began as a take-home case study for a Data Analyst role. The company, vendor, customer and sales-rep names have been replaced, and all monetary values rescaled by a constant factor. Quantities, dates and data quality issues are unchanged, so every percentage and pattern in the analysis is preserved.

---

**Hiten Shah** · Data Analyst with 4+ years in procurement and supply chain
[LinkedIn](https://www.linkedin.com/in/hiten-shah-analyst963) · [Portfolio](https://hiten-shah-analytics.github.io/hiten-shah-portfolio)
