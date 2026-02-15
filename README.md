# telco-customer-churn-analysis
# 📊 Telco Customer Churn Analysis

![SQL](https://img.shields.io/badge/SQL-PostgreSQL-blue)
![Portfolio](https://img.shields.io/badge/Portfolio-Data%20Analyst-brightgreen)

## 📌 Project Overview
Analyzed customer churn for a telecommunications company using PostgreSQL. 
Identified key factors driving customer churn and provided actionable insights.

## 🛠️ Skills Demonstrated
- **SQL:** CTEs, Aggregations, CASE statements, JOINs, Subqueries
- **Data Cleaning:** Handling NULL values, data type conversions
- **Exploratory Data Analysis:** Pattern identification, trend analysis
- **PostgreSQL:** Table creation, CSV import, query optimization

## 🔍 Key Questions Answered
1. What is the overall customer churn rate?
2. How does contract type affect churn?
3. Do higher monthly charges lead to more churn?
4. Which internet service type has the highest churn?
5. Do senior citizens churn more?

## 📈 Key Findings

| Finding | Insight | Business Impact |
|--------|---------|-----------------|
| **Month-to-month contracts** have 42% churn rate vs 11% for 1-year and 6% for 2-year | Customers need incentives to commit longer | Offer discounts for annual contracts |
| **Fiber optic customers** churn at 41% vs 25% for DSL | Higher speed = higher expectations | Improve fiber optic reliability |
| **Average monthly charges** for churned customers: $74 vs $61 for loyal customers | Price sensitivity | Review pricing strategy |

## 🖥️ SQL Queries

```sql
-- Total customers and churn rate
SELECT 
    COUNT(*) AS total_customers,
    SUM(CASE WHEN churn = 'Yes' THEN 1 ELSE 0 END) AS churned_customers,
    ROUND(100.0 * SUM(CASE WHEN churn = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) AS churn_rate_percent
FROM customer_churn;

-- Compare average spending between churned and stayed customers
SELECT 
    churn,
    COUNT(*) AS customer_count,
    ROUND(AVG(monthlycharges), 2) AS avg_monthly_charges,
    ROUND(AVG(tenure), 1) AS avg_tenure_months
FROM customer_churn
GROUP BY churn;

-- Contract analysis - this is GOLD for portfolio!
SELECT 
    contract,
    COUNT(*) AS total_customers,
    SUM(CASE WHEN churn = 'Yes' THEN 1 ELSE 0 END) AS churned,
    ROUND(100.0 * SUM(CASE WHEN churn = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) AS churn_rate
FROM customer_churn
GROUP BY contract
ORDER BY churn_rate DESC;

-- Does internet service type affect churn?
SELECT 
    internetservice,
    COUNT(*) AS customers,
    ROUND(AVG(monthlycharges), 2) AS avg_charges,
    ROUND(100.0 * SUM(CASE WHEN churn = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) AS churn_rate
FROM customer_churn
WHERE internetservice != 'No'  -- Exclude those without internet
GROUP BY internetservice
ORDER BY churn_rate DESC;

-- Do senior citizens churn more?
SELECT 
    CASE WHEN seniorcitizen = 1 THEN 'Senior' ELSE 'Non-Senior' END AS customer_type,
    COUNT(*) AS total,
    ROUND(100.0 * SUM(CASE WHEN churn = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) AS churn_rate
FROM customer_churn
GROUP BY seniorcitizen;
