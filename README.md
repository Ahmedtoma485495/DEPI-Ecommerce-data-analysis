# E-Commerce Marketplace Analytics (2024–2026) 🛒📊

An end-to-end data analytics project exploring multi-region e-commerce marketplace performance using Python, SQL, and Power BI — moving from raw database queries to interactive dashboard visualization and executive reporting.

---

## 👥 Team & Ownership

This project was built collaboratively by a 6-person team as part of the Digital Egypt Pioneers Initiative (DEPI). This repository highlights my specific contributions to the data engineering and analytics pipeline.

| Member | Domain & Primary Contributions |
| :--- | :--- |
| **Ahmed Mohamed** | **Customer Valuation & Top Spender Profiling (My Role)**<br>Segmented top customer cohorts, purchase distribution metrics, and customer recency trends using advanced SQL subqueries. |
| **Rojeh Tamer** | Data Model Architecture, DAX Calculations & Power BI Dashboard |
| **Hady Abohany** | Revenue & Profitability Analysis & Presentation Lead |
| **Yousef Sayed** | Customer Lifecycle & Activation Analysis |
| **Ibrahim Desouky** | Product Portfolio & Device Channel Performance |
| **Ahmed Shaban** | Return Rate Drivers & Operational Risk Analysis |

---

## 📁 Repository Structure

```text
├── data/
│   ├── ecommerce_data.sql                    # SQL schema setup & analytical queries
│   └── ecommerce_data.xlsx                   # Complete raw marketplace dataset
├── notebooks/
│   └── ecommerce_project2_notebook.ipynb     # Exploratory Data Analysis (EDA) & Python workflows
├── dashboard/
│   └── E_Commerce_Dashboard.pbix             # Dynamic Power BI dashboard workbook
└── README.md                                 # Project documentation
```

---

## 📊 Dataset Scope & Data Model

The analysis covers 32 months of operational data across 6 regions (Cairo, Giza, Alexandria, Jeddah, Riyadh, Abu Dhabi) and 2 device channels (Web / App).

```text
   ┌─────────────────┐             ┌─────────────────┐
   │    p2_users     │             │    p2_orders    │
   ├─────────────────┤             ├─────────────────┤
   │ PK  user_id     │1───────────*│ PK  order_id    │
   │     region      │             │ FK  user_id     │
   │     channel     │             │     order_dt    │
   │     signup_dt   │             │     gmv         │
   └─────────────────┘             │     discount    │
                                   │     cogs        │
                                   │     shipping    │
                                   │     region      │
                                   │     device      │
                                   └────────┬────────┘
                                            │ 1
                                            │
                                            │ *
   ┌─────────────────┐             ┌────────┴────────┐
   │   p2_returns    │             │ p2_order_items  │
   ├─────────────────┤             ├─────────────────┤
   │ PK,FK order_id  │*───────────1│ PK,FK order_id  │
   │       return_dt │             │ PK    sku       │
   │       reason    │             │       qty       │
   └─────────────────┘             │       price     │
                                   └─────────────────┘
```

| Table | Records | Key Fields | Purpose |
| :--- | :--- | :--- | :--- |
| **p2_users** | 3,000 | user_id, region, channel, signup_dt | Customer directory containing registration dates, channel source, and home region. |
| **p2_orders** | 15,000 | order_id, user_id, order_dt, gmv, discount... | Main order header data tracking financials, device type, and shipping destination. |
| **p2_order_items**| 38,655 | order_id, sku, qty, price | Line-item details capturing item quantities, price points, and SKU categories. |
| **p2_returns** | 1,531 | order_id, return_dt, reason | Returned order logs recording timestamps and categorized return reasons. |

---

## 🗄️ SQL Analytics (Customer Valuation Focus)

As the lead for Customer Valuation, I designed SQL workflows utilizing **Subqueries and Case Statements** to extract cohort behaviors and customer loyalty metrics:

### 1. Customer Retention (New vs. Repeat Customer Orders)
```sql
SELECT 
    strftime('%Y-%m', o.order_dt) AS order_month,
    SUM(CASE WHEN o.order_dt = f.first_date THEN 1 ELSE 0 END) AS new_orders,
    SUM(CASE WHEN o.order_dt != f.first_date THEN 1 ELSE 0 END) AS repeat_orders
FROM p2_orders o
JOIN (
    SELECT user_id, MIN(order_dt) AS first_date
    FROM p2_orders
    GROUP BY user_id
) f ON o.user_id = f.user_id
GROUP BY order_month
ORDER BY order_month;
```

### 2. Inactive Customers (Churn Analysis via Recency)
```sql
SELECT 
    strftime('%Y-%m', last_date) AS last_seen_month,
    COUNT(user_id) AS inactive_users_count
FROM (
    SELECT user_id, MAX(order_dt) AS last_date
    FROM p2_orders
    GROUP BY user_id
) AS last_purchase_table
GROUP BY last_seen_month
ORDER BY last_seen_month DESC;
