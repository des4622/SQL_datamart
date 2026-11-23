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
