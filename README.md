# Bank-Churn-Analysis
Identified key drivers of bank customer churn across 10,127 records using R. Proportion tests, ANOVA, chi-square, and regression reveal that female customers and lower-income segments carry the highest attrition risk.
# Bank Churn Analysis

Statistical analysis of customer attrition at a bank using 10,127 customer records. Built in R with proportion tests, t-tests, chi-square tests, one-way ANOVA, Tukey HSD, and linear regression.

**Key finding:** The bank's attrition rate (16.1%) is significantly below the 20% industry benchmark. Female customers, lower-income segments, and low credit utilization customers show the highest churn risk.

---

## Business Problem

Customer churn is one of the highest-cost problems in retail banking — acquiring a new customer costs 5–7× more than retaining an existing one. This analysis identifies which customer segments are most likely to churn and what financial behaviours precede attrition, giving the bank a basis for targeted retention campaigns.

---

## Key Findings

**Attrition rate:** 16.1% of customers churned — statistically significantly below the 20% industry standard (one-sample proportion test, p < 2.2e-16). The bank is performing well on retention overall.

**Gender gap:** Female customers churn at a significantly higher rate than male customers (two-sample proportion test, p = 0.0002). The difference is real, not sampling noise.

**Age:** Attrited customers average 47.5 years vs. 46.2 for existing customers. Older customers trend slightly higher in churn risk.

**Credit utilization:** Attrited customers have lower average utilization ratios than existing customers — counterintuitively, disengaged customers aren't maxing out their cards before leaving, they're simply using them less (independent t-test, p < 0.05).

**Income and churn:** Income category and attrition are not independent (chi-square, p = 0.025). Lower-income customers face more financial pressure; higher-income customers may be seeking more competitive products elsewhere.

**Income and utilization (ANOVA):** Mean utilization ratio differs significantly across all six income categories (F = 255.6, p < 2e-16). Tukey HSD confirms lower-income groups carry higher utilization — consistent with financial constraint behaviour.

**Credit limit predicts utilization:** Simple linear regression confirms Credit_Limit as a significant negative predictor of utilization ratio (slope = −0.0000147, p < 2e-16). A $10,000 increase in credit limit corresponds to a ~14.7pp decrease in utilization. R² = 0.233 — moderate fit, suggesting other variables are at play. Heteroskedasticity detected (Breusch-Pagan test, p < 0.05); a more complex model is warranted.

---

## Methods

| Method | Variable(s) | Purpose |
|---|---|---|
| One-sample proportion test | Attrition_Flag | Test if attrition rate < 20% industry benchmark |
| Two-sample proportion test | Attrition_Flag × Gender | Compare attrition rates by gender |
| Independent t-test | Avg_Utilization_Ratio × Attrition_Flag | Compare utilization between churned vs. retained |
| Chi-square test | Attrition_Flag × Income_Category | Test independence of income and churn |
| One-way ANOVA | Avg_Utilization_Ratio × Income_Category | Compare utilization across 6 income groups |
| Tukey HSD | Post-hoc ANOVA | Identify which income pairs differ significantly |
| Simple linear regression | Avg_Utilization_Ratio ~ Credit_Limit | Predict utilization from credit limit |

---

## Dataset

**Source:** [BankChurners Dataset](https://www.kaggle.com/datasets/sakshigoyal7/credit-card-customers) — Kaggle  
**Size:** 10,127 customers, 20 variables (3 dropped: CLIENTNUM, 2 Naive Bayes classifier columns)  
**Missing values:** None (NA). Note: `Income_Category` contains an "Unknown" category representing undisclosed income.

Key variables:

| Variable | Type | Description |
|---|---|---|
| `Attrition_Flag` | Binary categorical | Attrited vs. Existing Customer |
| `Customer_Age` | Numerical | Age of customer |
| `Gender` | Binary categorical | M / F |
| `Income_Category` | Ordinal categorical | Six income bands + Unknown |
| `Credit_Limit` | Numerical | Customer credit limit |
| `Avg_Utilization_Ratio` | Continuous | Credit utilization ratio (0–1) |
| `Total_Trans_Amt` | Numerical | Total transaction amount |
| `Months_Inactive_12_mon` | Numerical | Months inactive in last 12 months |

---

## Setup

```r
# Clone the repo
git clone https://github.com/your-username/bank-churn-analysis
cd bank-churn-analysis

# Download BankChurners.csv from Kaggle and place in data/
# https://www.kaggle.com/datasets/sakshigoyal7/credit-card-customers

# Open and knit the report
# Requires: lmtest package
install.packages("lmtest")
```

Then open `bank_churn_analysis.Rmd` in RStudio and click **Knit** to generate the HTML report.

**R version:** ≥ 4.0  
**Required packages:** `lmtest` (all other functions use base R)

---

## Repo Structure

```
bank-churn-analysis/
├── bank_churn_analysis.Rmd    # Full analysis — open in RStudio and knit
├── data/
│   └── BankChurners.csv       # Download from Kaggle (link above)
├── README.md
```

---

## Limitations & Next Steps

The linear regression model shows heteroskedasticity and non-normal residuals, meaning OLS standard errors are unreliable. A logistic regression model predicting `Attrition_Flag` directly from all available variables would be a stronger next step and would quantify each variable's independent contribution to churn probability.

The "Unknown" income category (undisclosed) was retained in the analysis but treated as a separate group. Imputing or excluding these records may change the chi-square and ANOVA results.

---

## Author

**Bismark Addo Amoako**  
