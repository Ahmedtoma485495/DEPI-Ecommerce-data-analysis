# E-Commerce: Orders, Returns, and Profitability Analysis 🛒📊

## 📝 Project Overview
This project serves as an end-to-end business analytics solution for an e-commerce marketplace operating across multiple regions and devices. The main goal is to extract actionable insights regarding revenue generation, customer lifecycle, platform performance, and product return rates to help stakeholders make data-driven decisions.

## 🎯 Business Objectives
The analysis answers key strategic questions for the COO, categorized into three main areas:
1. **Revenue & Margins:** Tracking month-over-month revenue before and after discounts, and identifying the most profitable product categories.
2. **Customer Lifecycle & Value:** Identifying the top 10 VIP customers, analyzing New vs. Returning customer ratios, and evaluating customer recency (churn analysis).
3. **Platform & Returns:** Comparing user spending between Mobile App and Web platforms, and investigating region-specific return rates.

## 🛠️ Tools & Technologies Used
* **SQL:** For robust data extraction, querying, subqueries, and data aggregation.
* **Python (Pandas & Matplotlib):** For data manipulation, cleaning, and creating insightful visualizations.
* **Power BI:** For building interactive dashboards to monitor business KPIs.

## 💡 Key Analytical Queries
* **Customer Retention (New vs. Returning):** Utilized advanced SQL subqueries to determine the first purchase date of each user and track their recurring orders.
* **Churn Analysis (Recency):** Grouped customers by their latest purchase month to identify inactive user segments and potential churn risks.
* **Top Spenders:** Aggregated total gross merchandise value (GMV) per user to highlight the most valuable customers for targeted loyalty campaigns.

## 🚀 How to Run the Project
1. Clone this repository.
2. Ensure you have the dataset (`p2_orders`, `p2_order_items`, etc.) loaded into your SQL/SQLite environment.
3. Run the SQL queries provided in the scripts folder or via the Jupyter/Colab notebook to view the insights.
