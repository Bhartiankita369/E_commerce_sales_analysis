# E_commerce_sales_analysis
---Revenue by country

```SQL
SELECT c.country, SUM(o.total_amount) AS total_revenue
FROM "Sales Analysis".fact_orders o
JOIN "Sales Analysis".dim_customers c ON o.customer_id = c.customer_id
GROUP BY c.country
ORDER BY total_revenue DESC;
```

---Top 10 Products by Revenue
```SQL
SELECT p.description, SUM(o.total_amount) AS revenue
FROM  "Sales Analysis".fact_orders o
JOIN "Sales Analysis".product_dimension p ON o.stock_code = p.stock_code
GROUP BY p.description
ORDER BY revenue DESC
LIMIT 10;
```

---Monthly Sales Trend
```SQL
SELECT 
	To_char(invoice_date, 'yyyy-mm') AS month, 
SUM(o.total_amount) AS monthly_revenue
FROM "Sales Analysis".fact_orders o
GROUP BY To_char(invoice_date,'yyyy-mm')
ORDER BY month;
```

---Customer Lifetime Value (CLV)
```SQL
SELECT o.customer_id, SUM(o.total_amount) AS lifetime_value
FROM "Sales Analysis".fact_orders o
GROUP BY o.customer_id
ORDER BY lifetime_value DESC;
```

---Cohort Analysis (First Purchase Month vs. Retention)
```SQL
WITH first_purchase AS (
  SELECT customer_id, MIN(To_char(invoice_date, 'yyyy-mm')) AS cohort_month
  FROM "Sales Analysis".fact_orders
  GROUP BY customer_id
)
SELECT f.cohort_month, To_char(invoice_date, 'yyyy-mm') AS active_month,
       COUNT(DISTINCT o.customer_id) AS active_customers
FROM "Sales Analysis".fact_orders o
JOIN first_purchase f ON o.customer_id = f.customer_id
GROUP BY f.cohort_month, active_month
ORDER BY f.cohort_month, active_month;


---Average Order Value (AOV) by Country
```SQL
SELECT c.country, 
       ROUND(SUM(o.total_amount) / COUNT(DISTINCT o.invoice_no), 2) AS avg_order_value
FROM "Sales Analysis".fact_orders o
JOIN "Sales Analysis".dim_customers c ON o.customer_id = c.customer_id
GROUP BY c.country
ORDER BY avg_order_value DESC;





