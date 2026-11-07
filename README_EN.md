# Big Data — Databricks Labs and a Predictive Lakehouse with SIEDCO Data

*Spanish version: [README.md](README.md)*

Three labs from the *Big Data* course, MSc in Data Science, Universidad Santo Tomás (2026), covering the full cycle in **Databricks**: Delta Lake storage, programmatic ingestion from an API, and a **Bronze → Silver → Gold Lakehouse** with **Spark ML** models on Bogotá crime data.

**Team:** Adrian Felipe Grateron · Jhonatan Smith Doncel · Jorge Emmanuel Marín

| # | Lab | Topics |
|---|---|---|
| 1 | Exploration and Delta storage | Crypto dataset profiling, Delta tables, Spark SQL |
| 2 | CoinGecko API pipeline | RAW JSON layer, CSV vs. Parquet, Unity Catalog Delta table, ACID operations (INSERT, UPDATE, DELETE, MERGE, time travel) |
| 3 | SIEDCO Lakehouse | Medallion architecture and classification of months with high crime variation |

## Lab 3 highlights

- **Question:** can we identify conditions associated with a high variation in next-month cases, by locality × crime type?
- **Data:** SIEDCO (Bogotá Security Secretariat), 2018–2026: 368,651 records → 18,746 monthly rows (182 series) → 17,229 modeling rows.
- **Features:** 1/3/7-month changes, 3/7/14-month moving averages and volatility, night-time and firearm context.
- **Target:** log change above the 75th percentile, threshold learned on training data only.
- **Temporal validation:** train Aug 2018 – Nov 2024 (13,594), test Dec 2024 – Jul 2026 (3,453).

| Test metric | Logistic Regression | Random Forest |
|---|---|---|
| Accuracy | 56.6 % | 67.6 % |
| Precision | 33.8 % | 43.4 % |
| Recall | 51.7 % | 38.3 % |
| F1 | 40.9 % | 40.7 % |
| ROC AUC | 57.8 % | 60.1 % |
| PR AUC | 39.5 % | 43.5 % |

A majority-class baseline reaches 71 % accuracy with 0 % recall, which is why several metrics are reported. The signal is moderate; administrative counts reflect reporting dynamics, and feature importance does not imply causality.

**Tech:** Databricks · PySpark · Delta Lake · Spark SQL · Spark ML · **License:** CC BY 4.0
