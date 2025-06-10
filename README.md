# WalmartSalesAnalysis

# Walmart Sales Data Analysis (MySQL + Python)

This project presents an end-to-end data analysis workflow on Walmart sales data. It covers the entire pipeline from data acquisition to business problem-solving using **Python**, **SQL**, and **relational databases (MySQL/PostgreSQL)**. The goal is to extract actionable insights for business decision-making.

---

## 📌 Key Highlights

- ✅ Real-world business questions answered using complex SQL queries.
- 📊 Advanced MySQL queries using `RANK()`, `JOIN`, `CASE`, and `WINDOW FUNCTIONS`.
- 🔄 Integrated data loading from Python into MySQL using `SQLAlchemy`.
- 🧹 Cleaned and transformed data for analysis-ready format.
- 🗃️ Database used: MySQL (also compatible with PostgreSQL).

---

## 🧰 Tools & Technologies

- **Python** (Pandas, SQLAlchemy)
- **SQL** (MySQL + PostgreSQL support)
- **Jupyter Notebook**
- **Kaggle API**
- **Visual Studio Code**

---

## 🔍 Business Problems Solved (SQL)

| No. | Business Question | Description |
|-----|-------------------|-------------|
| Q1  | Payment Insights  | Find different payment methods, total transactions, and quantity sold |
| Q2  | Ratings Analysis  | Identify highest-rated category in each branch |
| Q3  | Peak Days         | Busiest day for each branch |
| Q4  | Quantity by Payment | Total quantity sold per payment method |
| Q5  | City-Level Ratings | Avg, min, max ratings of product categories by city |
| Q6  | Profit Analysis   | Total profit for each category |
| Q7  | Preferred Payment | Most common payment method per branch |
| Q8  | Time-Based Sales  | Sales split by Morning, Afternoon, and Evening shifts |
| Q9  | Revenue Drop      | Top 5 branches with biggest revenue decrease YoY |

---

## 📄 Files Included

| File/Folder | Description |
|-------------|-------------|
| `README.md` | Project documentation |
| `queries/walmart_queries.sql` | Full set of MySQL business queries |
| `notebooks/` | Optional: Jupyter notebook for EDA and data prep |
| `scripts/` | Optional: Python scripts for loading data into MySQL |


---

## 🧠 Sample SQL Query (Q2: Highest-Rated Category in Each Branch)

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



📦 Dataset
Source: Kaggle - Walmart Sales Data
