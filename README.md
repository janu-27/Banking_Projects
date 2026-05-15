# Banking Loan Analytics Dashboard using Python & Power BI

# Project Objective

The goal of this project is to analyze:
- Loan approvals
- Customer financial behavior
- Credit risk
- Income patterns
- Asset analysis

using:
- Python for data cleaning & analysis
- Power BI for dashboard visualization

---

# Complete Workflow

```text
Dataset → Python Cleaning → Feature Engineering → EDA → Export CSV → Power BI Dashboard
```

---

# STEP 1 — DATASET COLLECTION

## Dataset Used

```text
loan_approval_dataset.csv
```

---

# Dataset Columns

| Column | Description |
|---|---|
| loan_id | Unique loan ID |
| no_of_dependents | Number of dependents |
| education | Graduate / Not Graduate |
| self_employed | Yes / No |
| income_annum | Annual income |
| loan_amount | Loan amount requested |
| loan_term | Loan duration |
| cibil_score | Credit score |
| residential_assets_value | Residential assets |
| commercial_assets_value | Commercial assets |
| luxury_assets_value | Luxury assets |
| bank_asset_value | Bank assets |
| loan_status | Approved / Rejected |

---

# STEP 2 — PYTHON ENVIRONMENT SETUP

## Install Libraries

```python
pip install pandas numpy matplotlib seaborn
```

---

# STEP 3 — IMPORT LIBRARIES

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

---

# STEP 4 — LOAD DATASET

```python
df = pd.read_csv('loan_approval_dataset.csv')
```

---

# STEP 5 — UNDERSTAND DATA

## View First Rows

```python
print(df.head())
```

## Dataset Information

```python
print(df.info())
```

## Statistical Summary

```python
print(df.describe())
```

---

# STEP 6 — DATA CLEANING

## Remove Extra Spaces from Columns

```python
df.columns = df.columns.str.strip()
```

## Check Missing Values

```python
print(df.isnull().sum())
```

## Verify Data Types

```python
print(df.dtypes)
```

---

# STEP 7 — FEATURE ENGINEERING

## Create Total Assets Column

```python
df['total_assets'] = (
    df['residential_assets_value']
    + df['commercial_assets_value']
    + df['luxury_assets_value']
    + df['bank_asset_value']
)
```

## Create Loan-Income Ratio

```python
df['loan_income_ratio'] = (
    df['loan_amount'] / df['income_annum']
)
```

## Create Risk Category

```python
def risk_category(score):
    if score >= 750:
        return 'Low Risk'
    elif score >= 550:
        return 'Medium Risk'
    else:
        return 'High Risk'


df['risk_category'] = df['cibil_score'].apply(risk_category)
```

---

# STEP 8 — EXPLORATORY DATA ANALYSIS (EDA)

## Loan Status Count

```python
sns.countplot(x='loan_status', data=df)
plt.title('Loan Approval Count')
plt.show()
```

## Income Distribution

```python
sns.histplot(df['income_annum'], bins=20)
plt.title('Income Distribution')
plt.show()
```

## CIBIL Score Analysis

```python
sns.boxplot(x='loan_status', y='cibil_score', data=df)
plt.title('CIBIL Score Analysis')
plt.show()
```

## Correlation Heatmap

```python
numeric_df = df.select_dtypes(include='number')

sns.heatmap(numeric_df.corr(), annot=True)
plt.title('Correlation Heatmap')
plt.show()
```

---

# STEP 9 — EXPORT CLEANED DATA

```python
df.to_csv('cleaned_loan_data.csv', index=False)
```

This exported cleaned dataset is used in Power BI.

---

# STEP 10 — OPEN POWER BI

Open:

```text
Power BI Desktop
```

---

# STEP 11 — IMPORT CSV INTO POWER BI

## Process

```text
Home → Get Data → Text/CSV
```

Select:

```text
cleaned_loan_data.csv
```

Click:

```text
Load
```

---

# STEP 12 — CREATE DAX MEASURES

## TOTAL LOANS

```DAX
Total Loans =
COUNT(cleaned_loan_data[loan_id])
```

## APPROVED LOANS

```DAX
Approved Loans =
CALCULATE(
    COUNT(cleaned_loan_data[loan_id]),
    cleaned_loan_data[loan_status] = " Approved"
)
```

## REJECTED LOANS

```DAX
Rejected Loans =
CALCULATE(
    COUNT(cleaned_loan_data[loan_id]),
    cleaned_loan_data[loan_status] = " Rejected"
)
```

## APPROVAL RATE

```DAX
Approval Rate =
DIVIDE(
    [Approved Loans],
    [Total Loans]
) * 100
```

## AVG INCOME

```DAX
Avg Income =
AVERAGE(cleaned_loan_data[income_annum])
```

## AVG LOAN AMOUNT

```DAX
Avg Loan Amount =
AVERAGE(cleaned_loan_data[loan_amount])
```

## AVG CIBIL SCORE

```DAX
Avg CIBIL Score =
AVERAGE(cleaned_loan_data[cibil_score])
```

---

# STEP 13 — CREATE PAGE 1

# EXECUTIVE OVERVIEW

## Add KPI Cards

Create Cards for:
- Total Loans
- Approved Loans
- Rejected Loans
- Approval Rate
- Avg Income
- Avg Loan Amount
- Avg CIBIL Score

---

## Create Pie Chart

### Fields

Legend:

```text
loan_status
```

Values:

```text
loan_id
```

Aggregation:

```text
Count
```

Title:

```text
Loan Approval Status
```

---

## Create Risk Category Bar Chart

Y-axis:

```text
risk_category
```

X-axis:

```text
loan_id
```

Aggregation:

```text
Count
```

Title:

```text
Risk Category Distribution
```

---

## Create Income Distribution

X-axis:

```text
income_annum
```

Y-axis:

```text
loan_id
```

Aggregation:

```text
Count
```

Title:

```text
Income Distribution
```

---

# STEP 14 — CREATE PAGE 2

# CUSTOMER RISK ANALYSIS

## Create Avg CIBIL Chart

X-axis:

```text
loan_status
```

Y-axis:

```text
cibil_score
```

Aggregation:

```text
Average
```

Title:

```text
Average CIBIL Score by Loan Status
```

---

## Create Scatter Plot

X-axis:

```text
income_annum
```

Y-axis:

```text
loan_amount
```

Legend:

```text
risk_category
```

Details:

```text
loan_id
```

Title:

```text
Income vs Loan Amount
```

---

# STEP 15 — CREATE PAGE 3

# FINANCIAL ASSET ANALYSIS

## Create Total Assets Column

```DAX
total_assets =
cleaned_loan_data[residential_assets_value]
+ cleaned_loan_data[commercial_assets_value]
+ cleaned_loan_data[luxury_assets_value]
+ cleaned_loan_data[bank_asset_value]
```

---

## Create Total Assets Card

Field:

```text
total_assets
```

Aggregation:

```text
Sum
```

---

## Create Asset Comparison Chart

Use:
- residential_assets_value
- commercial_assets_value
- luxury_assets_value
- bank_asset_value

Chart Type:

```text
Stacked Column Chart
```

---

## Create Loan vs Assets Scatter

X-axis:

```text
total_assets
```

Y-axis:

```text
loan_amount
```

Legend:

```text
loan_status
```

Details:

```text
loan_id
```

Title:

```text
Loan Amount vs Assets
```

---

# STEP 16 — ADD SLICERS

Add slicers for:
- loan_status
- education
- self_employed
- risk_category

---

# STEP 17 — FORMAT DASHBOARD

## Enable

- Titles
- Borders
- Shadows
- Data labels

---

## Use Colors

| Category | Color |
|---|---|
| Approved | Green |
| Rejected | Red |
| Low Risk | Blue |
| Medium Risk | Orange |
| High Risk | Dark Red |

---

# STEP 18 — FINAL DASHBOARD STRUCTURE

## PAGE 1

```text
Executive Overview
```

Contains:
- KPI Cards
- Pie Chart
- Risk Analysis
- Income Distribution

---

## PAGE 2

```text
Customer Risk Analysis
```

Contains:
- CIBIL Analysis
- Scatter Plot
- Risk Insights

---

## PAGE 3

```text
Financial Asset Analysis
```

Contains:
- Total Assets
- Asset Comparison
- Loan vs Assets

---

# Dashboard Name

```text
BANKING LOAN ANALYTICS DASHBOARD
```

Subtitle:

```text
Loan Approval • Risk Analysis • Financial Insights
```

---

# FINAL PROJECT OUTCOME

You successfully built:
- Banking analytics project
- Python data analysis project
- Power BI business dashboard
- Financial risk analysis system

---

# SKILLS GAINED

## Python
- Pandas
- EDA
- Feature Engineering
- Data Cleaning

## Power BI
- DAX
- KPI Reporting
- Interactive Dashboards
- Business Analytics

## Banking Domain
- Loan approvals
- Risk analysis
- Credit score analysis
- Asset evaluation

---

# RESUME DESCRIPTION

Developed a Banking Loan Analytics Dashboard using Python and Power BI. Performed data cleaning, feature engineering, exploratory data analysis, and created interactive dashboards for loan approval analysis, customer risk profiling, and financial asset insights.

---

# GITHUB PROJECT STRUCTURE

```text
Banking-Loan-Analytics-Dashboard/
│
├── dataset/
├── python/
├── powerbi/
├── outputs/
├── screenshots/
└── README.md
```
