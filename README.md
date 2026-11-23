# GI Portfolio Profitability & Rating Adequacy Data Mart

## 🎯 Project Objective
To build an end-to-end analytical data mart for a General Insurance portfolio, transforming raw transactional data (policies, customers, claims) into actuarial-ready metrics — Earned Premium, Incurred Claims, Loss Ratio, and Rating Adequacy (Lift) — using high-performance SQL.

*This proof-of-concept demonstrates data engineering and portfolio analytics capabilities relevant to a Pricing / Portfolio Analytics function.*

## 🛠️ Methodology & Tech Stack
- **Engine:** DuckDB (in-process OLAP database) for fast analytical querying.
- **Modeling:** Dimensional design — `Dim_Customer`, `Dim_Policy`, `Fact_Claims`.
- **SQL Techniques:** CTEs, multi-table JOINs, Window Functions (`ROW_NUMBER`, `SUM() OVER`, `LAG`), conditional aggregation (`CASE WHEN`), and null handling (`COALESCE`).
- **Outputs:** Clean CSV data marts ready for downstream BI visualization.

## 📂 Repository Contents
```text
gi-portfolio-sql-analytics/
├── README.md
├── day1_data_mart.sql          # DuckDB setup + analytical data mart query
├── day2_rating_adequacy.sql    # Loss Ratio, Lift & segment profitability
├── day2_monthly_trends.sql     # MoM claim trend analysis (LAG window fn)
├── build_data_mart.py          # Python driver (synthetic data + DuckDB load)
└── assets/
    ├── day1_data_mart_sample.png
    ├── day2_rating_adequacy.png
    └── day2_monthly_trends.png
```

## 📊 Key Analytical Outputs

### 1. Analytical Data Mart (Day 1)
A policy-level view joining premium, exposure, and claim data, enriched with derived metrics and risk segmentation:
* `EarnedPremium = BasePremium × Exposure`
* `IncurredClaims = PaidAmount + ReservedAmount` (with `COALESCE` for non-claiming policies)
* `LossRatio` computed per policy via `CASE WHEN`
* Regional portfolio totals via `SUM() OVER(PARTITION BY Region)` (preserving row-level granularity)
* Largest single claim per policy isolated via `ROW_NUMBER() OVER(... ORDER BY PaidAmount DESC)`

### 2. Rating Adequacy & Profitability (Day 2)
Segment-level analysis by **Region × Age Band**, comparing Actual vs. Expected claims to compute **Lift** (Actual ÷ Expected). Segments where the Loss Ratio exceeds the target are flagged as *Underperforming*, indicating potential under-pricing.

### 3. Monthly Claim Trends (Day 2)
Month-over-Month claim growth calculated using the `LAG()` window function, enabling early detection of emerging risk trends before they impact the annual loss ratio.

---

## 💡 Business Value

* **Data integrity:** Exposure-weighted premium and claim aggregation ensures metrics are actuarially sound (not simple row averages).
* **Portfolio steering:** Lift analysis directly identifies which segments require pricing intervention or underwriting tightening.
* **Scalability:** The DuckDB + SQL approach is reproducible against production data warehouses and feeds cleanly into Power BI / Tableau.
