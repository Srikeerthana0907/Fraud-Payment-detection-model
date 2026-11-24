# Fraud Payment Detection Model

A machine learning and data analysis project built to detect fraudulent activity across **6.36 million+ financial transactions**. This repository covers end-to-end exploratory data analysis, feature engineering, class imbalance handling, and predictive modeling for online payment risk detection.

---

## Overview
Digital payment platforms process millions of transactions daily across multiple channels (`PAYMENT`, `TRANSFER`, `CASH_OUT`, `CASH_IN`, `DEBIT`). Identifying fraudulent transfers accurately—without disrupting legitimate customer activity—requires analyzing account balance behaviors, transaction frequencies, and anomalous transfer patterns across heavily imbalanced transaction logs.

---

## Dataset Summary
* **Source:** Online Payment Fraud Detection Dataset (`6,362,620` records, `10` core features)
* **Schema Overview:**
  * `step`: Unit of time where 1 step equals 1 hour of real-world time.
  * `type`: Transaction channel (`CASH_IN`, `CASH_OUT`, `DEBIT`, `PAYMENT`, `TRANSFER`).
  * `amount`: Transaction value in local currency.
  * `nameOrig` / `nameDest`: Originator (`C...`) and recipient (`C...` / `M...`) account identifiers.
  * `oldbalanceOrg` / `newbalanceOrig`: Originator account balance before and after the transaction.
  * `oldbalanceDest` / `newbalanceDest`: Recipient account balance before and after the transaction.
  * `isFraud`: Target label (`1` = Fraudulent, `0` = Legitimate).

---

## Methodology & Workflow

1. **Data Preprocessing & Validation**
   * Loaded the `6.36M`-record dataset via `kagglehub` and verified zero missing values across all attributes.
   * Removed `isFlaggedFraud` to prevent target leakage and evaluated numerical bounds across balance and amount fields.
2. **Exploratory Data Analysis (EDA)**
   * **Class Imbalance:** Measured the distribution between legitimate (`0`) and fraudulent (`1`) transactions to inform model evaluation strategies.
   * **Transaction Channels:** Analyzed volume by payment type, confirming that fraudulent activity concentrates within `CASH_OUT` and `TRANSFER` transactions.
   * **Amount & Balance Distributions:** Compared transaction amounts on a log scale and examined Spearman rank correlations across account balances.
3. **Feature Engineering & Modeling**
   * Derived balance discrepancy indicators (`oldbalanceOrg - amount` vs. `newbalanceOrig`) to capture sudden account depletion patterns.
   * Trained and evaluated classification models optimized for imbalanced fraud detection using **Recall** and **Precision-Recall AUC (PR-AUC)**.

---

## Key Insights
* Fraudulent transactions occur almost exclusively in `TRANSFER` and `CASH_OUT` channels.
* Discrepancies between expected post-transaction balances and recorded balances serve as strong predictive signals for account takeover behavior.
* Due to severe class imbalance (`<0.2%` fraud rate), Precision-Recall metrics provide a far more reliable measure of model effectiveness than raw accuracy.

---

## Tech Stack
* **Language:** Python 3
* **Data Analysis & Visualization:** Pandas, NumPy, SciPy, Matplotlib, Seaborn, Tabulate
* **Machine Learning:** Scikit-Learn, TensorFlow
* **Data Retrieval:** KaggleHub

---
