# E_commerce_sales_analysis

```SQL
SELECT c.country, SUM(o.total_amount) AS total_revenue
FROM "Sales Analysis".fact_orders o
JOIN "Sales Analysis".dim_customers c ON o.customer_id = c.customer_id
GROUP BY c.country
ORDER BY total_revenue DESC;
```
