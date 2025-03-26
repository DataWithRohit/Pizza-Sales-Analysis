# 🍕 Pizza Sales Analysis (SQL)

## 📌 Project Overview  
This project analyzes **pizza sales data** using SQL to derive meaningful business insights. The analysis includes revenue calculation, order trends, pizza popularity, and category-wise performance.

## 📂 Database Schema  
The project consists of the following tables:  
- **`orders`** - Stores order ID, date, and time.  
- **`order_details`** - Contains details of pizzas ordered, including quantity.  
- **`pizzas`** - Holds pizza size, price, and `pizza_type_id`.  
- **`pizza_types`** - Describes each pizza, including category and name.

---

## 📊 SQL Queries & Insights  

### 🛠️ Database Setup  
```sql
CREATE DATABASE pizza_db;
USE pizza_db;

-- 1️⃣ Retrieve the total number of orders placed
SELECT COUNT(*) AS order_placed 
FROM orders;

-- 2️⃣ Calculate the total revenue generated from pizza sales
SELECT SUM(p.price * od.quantity) AS total_revenue
FROM pizzas AS p
JOIN order_details AS od ON p.pizza_id = od.pizza_id;

-- 3️⃣ Identify the highest-priced pizza
SELECT pizza_id, price AS highest_priced_pizza
FROM pizzas
ORDER BY price DESC
LIMIT 1;

-- 4️⃣ Identify the most common pizza size ordered
SELECT size, COUNT(*) AS total_pizza
FROM pizzas
GROUP BY size
ORDER BY total_pizza DESC;

-- 5️⃣ List the top 5 most ordered pizza types along with their quantities
SELECT pt.name, pt.pizza_type_id, SUM(od.quantity) AS total_quantity
FROM pizzas AS p
JOIN order_details AS od ON p.pizza_id = od.pizza_id
JOIN pizza_types AS pt ON pt.pizza_type_id = p.pizza_type_id
GROUP BY pt.name, pt.pizza_type_id
ORDER BY total_quantity DESC
LIMIT 5;

-- 6️⃣ Find the total quantity of each pizza category ordered
SELECT pt.category, SUM(od.quantity) AS total_ordered
FROM pizzas AS p
JOIN order_details AS od ON p.pizza_id = od.pizza_id
JOIN pizza_types AS pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.category
ORDER BY total_ordered DESC;

-- 7️⃣ Determine the distribution of orders by hour of the day
SELECT HOUR(order_time) AS order_hour, COUNT(*) AS total_orders
FROM orders
GROUP BY order_hour
ORDER BY order_hour;

-- 8️⃣ Find the category-wise distribution of pizzas
SELECT category, COUNT(*) AS pizza_distribution
FROM pizza_types
GROUP BY category
ORDER BY pizza_distribution DESC;

-- 9️⃣ Group the orders by date and calculate the average number of pizzas ordered per day
SELECT o.order_date, AVG(od.quantity) AS avg_pizzas_per_day
FROM orders o
JOIN order_details od ON o.order_id = od.order_id
GROUP BY o.order_date
ORDER BY o.order_date;

-- 🔟 Determine the top 3 most ordered pizza types based on revenue
SELECT pt.name AS pizza_type, SUM(p.price * od.quantity) AS total_revenue
FROM pizza_types pt
JOIN pizzas p ON pt.pizza_type_id = p.pizza_type_id
JOIN order_details od ON od.pizza_id = p.pizza_id
GROUP BY pt.name
ORDER BY total_revenue DESC
LIMIT 3;

-- 1️⃣1️⃣ Calculate the percentage contribution of each pizza type to total revenue
SELECT 
    pt.category AS pizza_category,  
    SUM(p.price * od.quantity) AS total_revenue,  
    (SUM(p.price * od.quantity) * 100.0) / 
    (SELECT SUM(p.price * od.quantity)  
     FROM pizzas p  
     JOIN order_details od ON od.pizza_id = p.pizza_id)  
    AS revenue_percentage  
FROM pizza_types pt  
JOIN pizzas p ON pt.pizza_type_id = p.pizza_type_id  
JOIN order_details od ON od.pizza_id = p.pizza_id  
GROUP BY pt.category  
ORDER BY total_revenue DESC;

-- 1️⃣2️⃣ Analyze the cumulative revenue generated over time
SELECT o.order_date, 
       SUM(p.price * od.quantity) AS total_revenue,
       SUM(SUM(p.price * od.quantity)) OVER(ORDER BY o.order_date) AS cumulative_revenue
FROM order_details AS od
JOIN pizzas AS p ON p.pizza_id = od.pizza_id
JOIN orders AS o ON o.order_id = od.order_id
GROUP BY o.order_date
ORDER BY o.order_date;

-- 1️⃣3️⃣ Determine the top 3 most ordered pizza types based on revenue for each pizza category
WITH PizzaRevenue AS (
    SELECT 
        pt.category,  
        pt.name AS pizza_type,  
        SUM(p.price * od.quantity) AS total_revenue,  
        RANK() OVER (PARTITION BY pt.category ORDER BY SUM(p.price * od.quantity) DESC) AS rnk  
    FROM pizza_types pt  
    JOIN pizzas p ON pt.pizza_type_id = p.pizza_type_id  
    JOIN order_details od ON od.pizza_id = p.pizza_id  
    GROUP BY pt.category, pt.name  
)  
SELECT category, pizza_type, total_revenue, rnk
FROM PizzaRevenue  
WHERE rnk <= 3  
ORDER BY category, rnk;

--- 
📊 Key Insights
✔ The most popular pizza size is determined.
✔ The top-selling pizza types drive the most revenue.
✔ The most profitable time slots for orders are identified.
✔ The average order value (AOV) provides pricing strategy insights.
✔ The revenue contribution of different pizza categories is analyzed.

---
📢 Conclusion
This SQL analysis provides crucial insights for business owners to optimize their menu, pricing, and marketing strategies to maximize revenue and customer satisfaction.
