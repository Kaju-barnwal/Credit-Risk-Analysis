# Credit Risk Analysis & Customer Risk Segmentation (Paisabazaar)

> End-to-end credit risk analysis on customer-level Paisabazaar data: **Python EDA** to understand what drives credit scores, followed by an **interactive Power BI dashboard** to identify and monitor high-risk customers.

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Pandas](https://img.shields.io/badge/Pandas-EDA-150458)
![Seaborn](https://img.shields.io/badge/Seaborn-Matplotlib-4c72b0)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811)

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Business Problem](#business-problem)
3. [Dataset](#dataset)
4. [Project Structure](#project-structure)
5. [Part 1: EDA in Python](#part-1-eda-in-python)
6. [Part 2: Power BI Dashboard](#part-2-power-bi-dashboard)
7. [Key Insights](#key-insights)
8. [Business Recommendations](#business-recommendations)
9. [Tech Stack](#tech-stack)
10. [How to Run](#how-to-run)
11. [Author](#author)

---

## Project Overview

Paisabazaar is a digital financial marketplace in India that connects customers with banks and NBFCs for loans and credit cards. Wrongly assessing a customer's credit behaviour leads to defaults and financial loss.

This project works in two stages:

| Stage | Goal | Tool |
|-------|------|------|
| **EDA** | Understand which financial and behavioural factors drive `Credit_Score` (Good / Standard / Poor) | Python (Pandas, Matplotlib, Seaborn) |
| **Dashboard** | Classify customers into High / Medium / Low risk, find risk thresholds, and monitor early-warning signals | Power BI |

---

## Business Problem

> **How can we identify high-risk customers and analyze the key drivers of credit risk using financial behaviour, repayment patterns, and customer demographics?**

Questions explored:
- Which customers fall into high, medium and low risk?
- How do utilization, income, EMI burden and savings ratio affect risk?
- How do delayed payments, number of loans and credit inquiries affect creditworthiness?
- At what **thresholds** does risk rise sharply?
- Which segments (income, age, occupation) are most vulnerable?
- What are the **early warning signals** of a customer turning high risk?

---

## Dataset

- **Name:** Paisabazaar Credit Risk Dataset
- **Size:** 100,000 rows × 28 columns (before cleaning)
- **Target variable:** `Credit_Score` (Good / Standard / Poor)
- **Source:** [Paisabazaar dataset (Google Drive)](https://drive.google.com/file/d/10RjIM_lbtIOPdE-8dPCGpVeOf4or-NON/view?usp=drive_link)

| Category | Key columns |
|----------|-------------|
| Demographic | `Age`, `Occupation`, `Annual_Income` |
| Financial | `Outstanding_Debt`, `Credit_Utilization_Ratio`, `Num_Bank_Accounts`, `Num_Credit_Card`, `Num_of_Loan`, `Total_EMI_per_month`, `Monthly_Balance` |
| Behavioural | `Delay_from_due_date`, `Num_of_Delayed_Payment`, `Payment_Behaviour`, `Payment_of_Min_Amount`, `Credit_Mix`, `Num_Credit_Inquiries` |

---

## Project Structure

```
├── notebooks/
│   └── Paisabazaar_EDA.ipynb            # Part 1: Python EDA
├── dashboard/
│   ├── Credit_Risk_Dashboard.pbix       # Part 2: Power BI file
│   └── screenshots/
│       ├── credit_risk_overview.png
│       ├── risk_driver_deep_dive.png
│       └── early_warning_customer_view.png
├── data/
│   └── cleaned_Paisabazaar_data.csv     # Output of the cleaning step
└── README.md
```

> Update the file names above to match your repository.

---

## Part 1: EDA in Python

### Data Cleaning
- Replaced disguised missing values (`'-'`, `'n/a'`, `'Unknown'`, etc.) with `NaN`
- Kept only customers aged **18+** (loan eligibility)
- Fixed wrong data types (`Age`, `SSN`, `Num_Bank_Accounts`, `Num_Credit_Card`, `Num_of_Loan`, `Num_of_Delayed_Payment`, `Num_Credit_Inquiries`)
- Cleaned the multi-valued `Type_of_Loan` column (split and counted individual loan types)
- Renamed `Monthly_Inhand_Salary` to `Monthly_Salary`

### Feature Engineering (KPIs)

| Group | Features |
|-------|----------|
| **Repayment risk** | `Delay_Severity`, `Chronic_Delay`, `Min_Payment_Flag`, `Payment_Risk` |
| **Financial stress** | `DTI` (Debt-to-Income), `EMI_Ratio`, `Disposable_Income` |
| **Credit behaviour** | `Inquiry_Level`, `Total_Accounts` |
| **Relationship metrics** | `Savings_Ratio`, `Debt_per_Loan`, `Has_Loan` |
| **Segmentation** | `Income_Group`, `Age_Group`, `Experience_Level`, `Credit_History_Years` |

### Analysis Approach (UBM)
- **U**nivariate: credit score, income and debt distributions
- **B**ivariate: DTI, EMI ratio, delays and segments vs credit score
- **M**ultivariate: correlation heatmap and pair plot

---

## Part 2: Power BI Dashboard

New variables created for the dashboard:
- `Risk_Level`: High / Medium / Low risk
- `Utilization_Bucket` and `Delay_Bucket`: bins to detect threshold effects
- `Risk_Reason`: primary factor behind a customer's risk label

### Dashboard Pages

| Page | Purpose |
|------|---------|
| **Credit Risk Overview** | KPI cards (High / Medium / Low risk %, average utilization) and top-level drivers: utilization, delayed payments, number of loans |
| **Risk Driver Deep Dive** | Threshold analysis using bins, plus segmentation by income group, age group and occupation |
| **Early Warning & Customer View** | Early-warning KPIs and a customer-level drill-down table with `Risk_Reason` filter |

### Screenshots

![Credit Risk Overview](dashboard/screenshots/credit_risk_overview.png)
![Risk Driver Deep Dive](dashboard/screenshots/risk_driver_deep_dive.png)
![Early Warning & Customer View](dashboard/screenshots/early_warning_customer_view.png)

**Dashboard link:** [View the Power BI dashboard (Google Drive)](https://drive.google.com/file/d/16Lp_gAgjoSJyUiaxL4H9bX1ObDX8cvh4/view?usp=drive_link)

---

## Key Insights

- **~38% of customers are High Risk** (High 37.8%, Medium 52.29%, Low 9.94%), a significant portfolio exposure.
- **Credit utilization above 35–40%** sharply increases the chance of high risk.
- Customers with **10+ delayed payments** are heavily concentrated in the high-risk group.
- Customers with **multiple loans** are over-leveraged and riskier.
- **Lower-income groups** are more vulnerable, with lower financial resilience.
- **Poor credit score customers** show the highest DTI, EMI ratio and outstanding debt, and the lowest disposable income.
- **Longer credit history** goes with better credit scores.
- Income alone is not enough: some high-income customers also carry high debt.
- Correlation highlights: DTI and Outstanding Debt (~0.71), Delay and Number of Delayed Payments (~0.54), Annual Income and DTI (~-0.48).

---

## Business Recommendations

1. **Screen using multiple signals**: DTI, delays, delayed payment count and disposable income, never income alone.
2. **Cap EMI burden**: for example, EMI should not exceed 30–40% of salary.
3. **Monitor early warnings**: minimum-payment-only behaviour, chronic delays, high inquiries, high utilization.
4. **Segment smartly**: better offers for low-risk groups, stricter checks for low income + low credit history.
5. **Build a composite risk score** (DTI + delay + debt + income) for approvals, interest rates and credit limits.
6. **Reduce default risk**: reject, or offer smaller loans / higher interest, for high DTI + high delay profiles.

---

## Tech Stack

- **Python:** Pandas, NumPy, Matplotlib, Seaborn
- **Notebook:** Google Colab / Jupyter
- **BI Tool:** Power BI (DAX, calculated columns, bucketing, slicers, drill-down)

---

## How to Run

**EDA notebook**
```bash
git clone https://github.com/Kaju-barnwal/EDA-on-Paisa-Bazaar.git
cd EDA-on-Paisa-Bazaar
pip install pandas numpy matplotlib seaborn
jupyter notebook
```
Update the dataset path in the notebook (the original uses a Google Drive path via Colab) and run all cells.

**Dashboard**
Open the `.pbix` file in Power BI Desktop, or use the dashboard link above.

---

## Author

**Kajal Kumari**
M.Sc. Statistics | Aspiring Data Scientist / Data Analyst
GitHub: [Kaju-barnwal](https://github.com/Kaju-barnwal)
