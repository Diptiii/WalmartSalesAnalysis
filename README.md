# Walmart Sales Analysis

## Walmart Sales Data Analysis (MySQL + Python)

This project presents an end-to-end data analysis workflow on Walmart sales data. It covers the entire pipeline from data acquisition to business problem-solving using **Python**, **SQL**, and **relational databases (MySQL)**. The goal is to extract actionable insights for business decision-making.

---

## Key Highlights

* Real-world business questions answered using complex SQL queries.
* Advanced MySQL queries using `RANK()`, `JOIN`, `CASE`, and window functions.
* Integrated data loading from Python into MySQL using `SQLAlchemy`.
* Cleaned and transformed data into an analysis-ready format.
* Databases used: MySQL (PostgreSQL compatible).

---

## Tools & Technologies

* **Python** (Pandas, SQLAlchemy)
* **SQL** (MySQL + PostgreSQL support)
* **Jupyter Notebook**
* **Kaggle API**
* **Visual Studio Code**

---

## Business Problems Solved (SQL)

| No. | Business Question        | Description                                             |
| --- | ------------------------ | ------------------------------------------------------- |
| Q1  | Payment Insights         | Find payment methods, total transactions, quantity sold |
| Q2  | Ratings Analysis         | Identify highest-rated category in each branch          |
| Q3  | Peak Days                | Identify the busiest day for each branch                |
| Q4  | Quantity by Payment      | Total quantity sold by payment method                   |
| Q5  | City-Level Ratings       | Avg, min, max ratings by city and category              |
| Q6  | Profit Analysis          | Total profit for each category                          |
| Q7  | Preferred Payment Method | Most common payment method per branch                   |
| Q8  | Time-Based Sales         | Sales split by shift (Morning, Afternoon, Evening)      |
| Q9  | Revenue Drop             | Top 5 branches with biggest revenue decrease YoY        |

---

## Files Included

| File/Folder                   | Description                                     |
| ----------------------------- | ----------------------------------------------- |
| `README.md`                   | Project documentation                           |
| `queries/walmart_queries.sql` | Full set of MySQL business queries              |
| `notebooks/`                  | (Optional) Jupyter notebook for EDA & data prep |
| `scripts/`                    | (Optional) Python scripts for data loading      |

---

## Sample SQL Query (Q2: Highest-Rated Category per Branch)

```sql
SELECT branch, category, avg_rating
FROM (
    SELECT 
        branch,
        category,
        AVG(rating) AS avg_rating,
        RANK() OVER(PARTITION BY branch ORDER BY AVG(rating) DESC) AS rank
    FROM walmart
    GROUP BY branch, category
) AS ranked
WHERE rank = 1;
```

---

## Business Questions & SQL Insights

### Q1: What are the different payment methods and their transactions and item counts?

```sql
SELECT payment_method, COUNT(*) AS no_payments, SUM(quantity) AS quantity
FROM walmart
GROUP BY payment_method;
```

**Insight**: Understands customer preference in payment channels.

### Q2: Highest-rated category in each branch

```sql
SELECT branch, category, AVG(rating) AS avg_rating
FROM walmart
GROUP BY branch, category
ORDER BY branch, avg_rating DESC;
```

**Insight**: Promotes top-rated categories per branch.

### Q3: Busiest day for each branch

```sql
SELECT branch, day_name, no_transactions
FROM (
    SELECT branch, DAYNAME(STR_TO_DATE(date, '%d/%m/%Y')) AS day_name,
           COUNT(*) AS no_transactions,
           RANK() OVER(PARTITION BY branch ORDER BY COUNT(*) DESC) AS rk
    FROM walmart
    GROUP BY branch, day_name
) AS ranked
WHERE rk = 1;
```

**Insight**: Assists in staff and inventory planning.

### Q4: Total quantity sold per payment method

```sql
SELECT payment_method, SUM(quantity) AS no_qty_sold
FROM walmart
GROUP BY payment_method;
```

**Insight**: Monitors payment-driven item flow.

### Q5: Average, min, and max ratings by city and category

```sql
SELECT city, category,
       MIN(rating) AS min_rating,
       MAX(rating) AS max_rating,
       AVG(rating) AS avg_rating
FROM walmart
GROUP BY city, category;
```

**Insight**: Assesses satisfaction levels geographically.

### Q6: Total profit per category

```sql
SELECT category,
       SUM(unit_price * quantity * profit_margin) AS total_profit
FROM walmart
GROUP BY category
ORDER BY total_profit DESC;
```

**Insight**: Reveals high-profit categories for focus.

### Q7: Most common payment method per branch

```sql
WITH cte AS (
   SELECT branch, payment_method, COUNT(*) AS total_trans,
          RANK() OVER(PARTITION BY branch ORDER BY COUNT(*) DESC) AS rank
   FROM walmart
   GROUP BY branch, payment_method
)
SELECT branch, payment_method AS preferred_payment_method
FROM cte
WHERE rank = 1;
```

**Insight**: Helps branches optimize payment options.

### Q8: Sales distribution by shift

```sql
SELECT branch,
       CASE
           WHEN HOUR(TIME(time)) < 12 THEN 'Morning'
           WHEN HOUR(TIME(time)) BETWEEN 12 AND 17 THEN 'Afternoon'
           ELSE 'Evening'
       END AS shift,
       COUNT(*) AS num_invoices
FROM walmart
GROUP BY branch, shift;
```

**Insight**: Supports shift-based resource allocation.

### Q9: Branches with largest YoY revenue drop

```sql
WITH revenue_2022 AS (
    SELECT branch, SUM(total) AS revenue
    FROM walmart
    WHERE YEAR(STR_TO_DATE(date, '%d/%m/%Y')) = 2022
    GROUP BY branch
),
revenue_2023 AS (
    SELECT branch, SUM(total) AS revenue
    FROM walmart
    WHERE YEAR(STR_TO_DATE(date, '%d/%m/%Y')) = 2023
    GROUP BY branch
)
SELECT r2022.branch, r2022.revenue AS last_year_revenue,
       r2023.revenue AS current_year_revenue,
       ROUND(((r2022.revenue - r2023.revenue) / r2022.revenue) * 100, 2) AS revenue_decrease_ratio
FROM revenue_2022 r2022
JOIN revenue_2023 r2023 ON r2022.branch = r2023.branch
WHERE r2022.revenue > r2023.revenue
ORDER BY revenue_decrease_ratio DESC
LIMIT 5;
```

**Insight**: Helps identify underperforming branches for action.

---

## Project Outcome

This project showcases how SQL and Python can be applied for meaningful business insights and decision-making. It prepares stakeholders to:

* Understand customer behavior
* Optimize sales operations
* Plan staffing
* Identify growth or problem areas

---

## Future Scope

* Real-time sales dashboards (Power BI / Tableau)
* Predictive sales forecasting with ML models
* Store clustering and segmentation using unsupervised learning
