# Customer Churn Analysis — Power BI Dashboard

## 📌 Overview
An end-to-end customer churn analysis project built in **Power BI**, using a dataset of ~1,600 telecom/subscription customers. The dashboard identifies *who* is churning, *why*, and *how much revenue* is at risk — across three focused report pages.

## 🎯 Objective
To help a subscription-based business understand churn drivers and prioritize retention efforts by answering:
- What is our overall churn rate, and how has it changed by tenure?
- Which contract types, plans, and payment methods have the highest churn?
- What behavioral signals (usage, login recency, support tickets) predict churn?
- How much revenue is at risk from churned customers, and who were our highest-value losses?

## 🛠️ Tools Used
- **Microsoft Excel** — data cleaning, calculated columns, formula-based feature engineering
- **Power BI Desktop** — data modeling (DAX), visualization, interactive dashboard design

## 🗂️ Dataset
- ~1,600 customer records with 17+ attributes: demographics (Gender, SeniorCitizen), account info (ContractType, SubscriptionPlan, PaymentMethod, TenureMonths), usage (UsageHoursPerMonth, LastLoginDaysAgo, SupportTicketsRaised), billing (MonthlyCharges, TotalCharges), and churn outcome (Churn, ChurnReason).

### Data Cleaning & Feature Engineering
- Filled missing `ChurnReason` values with `"Not Applicable"` for non-churned customers
- Added calculated columns:
  - `TenureGroup` — bucketed tenure (0-12, 13-24, 25-48, 49-72 months)
  - `RecencyFlag` — Active / At Risk / Dormant, based on `LastLoginDaysAgo`
  - `SeniorCitizenFlag` — readable Yes/No version of the 0/1 flag
  - `AvgMonthlyUsageCost` — MonthlyCharges ÷ UsageHoursPerMonth
  - `EstimatedCLV` — TenureMonths × MonthlyCharges (customer lifetime value estimate)
  - `TotalChargesGap` — sanity check comparing calculated vs. recorded TotalCharges

## 📊 Dashboard Structure (3 Pages)

### Page 1 — Executive Overview
High-level KPIs and top-line churn trends: total customers, churn rate, revenue at risk, average tenure, churn split, churn rate by contract type, and churn rate by tenure group. Includes slicers (ContractType, SubscriptionPlan, Gender) for interactive filtering.

### Page 2 — Customer Behavior & Risk Signals
Explores behavioral drivers of churn: usage vs. login recency scatter plot, support ticket volume (churned vs. retained), churn rate by recency flag and payment method, churn reason breakdown with counts, and billing method vs. churn.

### Page 3 — Financial Impact
Quantifies the cost of churn: total revenue at risk by contract type and subscription plan (treemap), average monthly charges by recency flag, and a table of the highest-value churned customers ranked by estimated CLV.

## 📈 Key Insights
- **Month-to-Month contracts churn at ~60%**, dramatically higher than One Year (26%) or Two Year (22%) — contract length is the single strongest churn predictor in this dataset.
- **Churn rate is highest in the first 12 months** of tenure and declines steadily as tenure increases, pointing to onboarding/early-engagement as a critical retention window.
- **Dormant and At-Risk customers (by login recency) churn at 54% and 51%** respectively, vs. only 24% for Active users — login recency is a strong early-warning signal.
- **Total revenue at risk is concentrated in Month-to-Month contracts** (~0.72M), far exceeding One Year and Two Year combined.

## 🔧 Key DAX Measures
```DAX
Churn Rate % = DIVIDE([Churned Customers], [Total Customers])
Total Revenue at Risk = CALCULATE(SUM(customer_churn[EstimatedCLV]), customer_churn[Churn] = "Yes")
Avg Support Tickets (Churned) = CALCULATE(AVERAGE(customer_churn[SupportTicketsRaised]), customer_churn[Churn] = "Yes")
Avg Support Tickets (Retained) = CALCULATE(AVERAGE(customer_churn[SupportTicketsRaised]), customer_churn[Churn] = "No")
```

## 🚀 Future Improvements
- Add a Top-N filter to the churned-customer CLV table to strictly cap it at the top 20
- Build a simple churn-prediction model (logistic regression) using the behavioral features as a follow-up analysis
- Add a "What-if" parameter to simulate retention campaign impact on revenue at risk

## 📁 Files
- `customer_churn.xlsx` — source dataset with engineered features
- `churn_dashboard.pbix` — Power BI report file

---
*Built as a portfolio project to demonstrate data cleaning, DAX modeling, and dashboard design in Power BI.*
