# Fraud Payment Detection Model

An end-to-end data processing and machine learning pipeline designed to ingest, profile, and detect fraudulent activity across **6.36 million+ financial transactions**. Built with a focus on **Data Engineering best practices**—including scalable data ingestion, data quality validation, schema profiling, and handling extreme class imbalance in banking datasets.

---

## Business Context (Banking & Financial Risk)
Financial institutions process millions of daily digital transactions (`PAYMENT`, `TRANSFER`, `CASH_OUT`, `CASH_IN`, `DEBIT`). Identifying fraudulent transfers in real time without blocking legitimate customer activity requires:
1. **High-throughput data ingestion** capable of handling multi-million-row datasets (~485+ MB in memory).
2. **Strict data quality & schema enforcement** to ensure account balance integrity (`oldbalanceOrg`, `newbalanceOrig`, `oldbalanceDest`, `newbalanceDest`).
3. **Domain-driven feature engineering** to capture anomalous balance depletion patterns typical of account takeovers.

---

## Dataset & Data Profiling
* **Source:** Online Payment Fraud Detection Dataset (`6,362,620` records, `10` core attributes)
* **Volume & Memory Footprint:** `~485.4 MB` raw in-memory DataFrame
* **Key Schema Attributes:**
  * `step`: Time-series unit mapping to 1 hour of real-world transaction time.
  * `type`: Transaction channel (`CASH_IN`, `CASH_OUT`, `DEBIT`, `PAYMENT`, `TRANSFER`).
  * `amount`: Transaction value in local currency.
  * `nameOrig` / `nameDest`: Customer (`C...`) and Merchant (`M...`) entity identifiers.
  * `oldbalanceOrg` / `newbalanceOrig`: Originator account balance pre- and post-transaction.
  * `oldbalanceDest` / `newbalanceDest`: Recipient account balance pre- and post-transaction.
  * `isFraud`: Target binary flag (`1` = Fraudulent, `0` = Legitimate).

---

## Data Engineering & Pipeline Workflow

1. **Automated Data Ingestion:**
   * Programmatic dataset retrieval via `kagglehub` with local caching for reproducible pipeline runs.
2. **Data Cleaning & Schema Hygiene:**
   * Audited all `6.36M` rows for null/missing values (`df.isnull()`)—confirming **100% completeness** across all 10 columns.
   * Dropped redundant/leaky system flags (`isFlaggedFraud`) to prevent target leakage and reduce memory overhead.
   * Validated numerical boundaries (`min`/`max` profiling) across balance and transaction amount fields.
3. **Exploratory Data Analysis (EDA) & Statistical Profiling:**
   * **Class Imbalance Analysis:** Quantified severe skew between legitimate (`0`) and fraudulent (`1`) transactions to guide downstream resampling/threshold tuning.
   * **Channel Distribution:** Profiled transaction volume across payment types (`CASH_OUT` and `TRANSFER` identified as primary fraud vectors).
   * **Log-Scale Amount Distribution:** Analyzed transaction value distributions across fraud vs. non-fraud cohorts.
   * **Spearman Rank Correlation:** Evaluated non-linear feature relationships across account balances and transaction amounts.
4. **Predictive Modeling:**
   * Engineered balance discrepancy features and trained classification models to flag high-risk transactions with high recall.

---

## Tech Stack
* **Languages:** Python 3
* **Data Processing & Profiling:** Pandas, NumPy, SciPy, Tabulate
* **Data Visualization:** Matplotlib, Seaborn
* **Machine Learning / Deep Learning:** TensorFlow, Scikit-Learn
* **Data Ingestion:** KaggleHub API

---

## Key Takeaways for Banking Data Systems
* **Data Integrity Checks:** Detecting discrepancies between `oldbalanceOrg - amount` and `newbalanceOrig` acts as both a data quality rule and a high-signal fraud indicator.
* **Memory & Type Optimization:** Downcasting `float64`/`int64` columns and categorical encoding (`type`) significantly reduces memory usage when scaling to production banking workloads.
* **Metric Selection:** Because fraud represents `<0.2%` of total volume, standard accuracy is misleading; pipelines must optimize for **Precision-Recall AUC (PR-AUC)** and **Recall** to minimize financial loss.

---

## How to Run
1. Clone the repository:
   ```bash
   git clone [https://github.com/](https://github.com/)<your-username>/Fraud-payment-detection-model.git
   cd Fraud-payment-detection-model
