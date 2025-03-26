# 🍕 Pizza Sales Analysis

## 📌 SQL Queries Used

```sql
-- Q1) Retrieve the total number of orders placed.
SELECT COUNT(*) AS order_placed 
FROM orders;
sql
Copy
Edit
-- Q2) Calculate the total revenue generated from pizza sales.
SELECT SUM(p.price * od.quantity) AS total_revenue
FROM pizzas AS p
JOIN order_details AS od ON p.pizza_id = od.pizza_id;
sql
Copy
Edit
-- Q3) Identify the highest-priced pizza.
SELECT pizza_id, price AS highest_priced_pizza
FROM pizzas
ORDER BY price DESC
LIMIT 1;
sql
Copy
Edit
-- Q4) Identify the most common pizza size ordered.
SELECT size, COUNT(*) AS total_pizza
FROM pizzas
GROUP BY size
ORDER BY total_pizza DESC;
sql
Copy
Edit
-- Q5) List the top 5 most ordered pizza types along with their quantities.
SELECT pt.name, pt.pizza_type_id, SUM(od.quantity) AS total_quantity
FROM pizzas AS p
JOIN order_details AS od ON p.pizza_id = od.pizza_id
JOIN pizza_types AS pt ON pt.pizza_type_id = p.pizza_type_id
GROUP BY pt.name, pt.pizza_type_id
ORDER BY total_quantity DESC
LIMIT 5;
sql
Copy
Edit
-- Q6) Find the total quantity of each pizza category ordered.
SELECT pt.category, SUM(od.quantity) AS total_ordered 
FROM pizzas AS p
JOIN order_details AS od ON p.pizza_id = od.pizza_id
JOIN pizza_types AS pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.category
ORDER BY total_ordered DESC;
sql
Copy
Edit
-- Q7) Determine the distribution of orders by hour of the day.
SELECT HOUR(order_time) AS order_hour, COUNT(*) AS total_orders
FROM orders
GROUP BY order_hour
ORDER BY order_hour;
sql
Copy
Edit
-- Q8) Find the category-wise distribution of pizzas.
SELECT category, COUNT(*) AS pizza_distribution 
FROM pizza_types 
GROUP BY category 
ORDER BY pizza_distribution DESC;
sql
Copy
Edit
-- Q9) Group the orders by date and calculate the average number of pizzas ordered per day.
SELECT o.order_date, AVG(od.quantity) AS avg_pizzas_per_day
FROM orders o
JOIN order_details od ON o.order_id = od.order_id
GROUP BY o.order_date
ORDER BY o.order_date;
sql
Copy
Edit
-- Q10) Determine the top 3 most ordered pizza types based on revenue.
SELECT pt.name AS Pizza_type, SUM(p.price * od.quantity) AS Total_Revenue 
FROM pizza_types pt
JOIN pizzas p ON pt.pizza_type_id = p.pizza_type_id 
JOIN order_details od ON od.pizza_id = p.pizza_id 
GROUP BY pt.name
ORDER BY Total_Revenue DESC 
LIMIT 3;
sql
Copy
Edit
-- Q11) Calculate the percentage contribution of each pizza type to total revenue.
SELECT 
    pt.category AS Pizza_Category,  
    SUM(p.price * od.quantity) AS Total_Revenue,  
    (SUM(p.price * od.quantity) * 100.0) / 
    (SELECT SUM(p.price * od.quantity)  
     FROM pizzas p  
     JOIN order_details od ON od.pizza_id = p.pizza_id)  
    AS Revenue_Percentage  
FROM pizza_types pt  
JOIN pizzas p ON pt.pizza_type_id = p.pizza_type_id  
JOIN order_details od ON od.pizza_id = p.pizza_id  
GROUP BY pt.category  
ORDER BY Total_Revenue DESC;
sql
Copy
Edit
-- Q12) Analyze the cumulative revenue generated over time.
SELECT o.order_date, 
       SUM(p.price * od.quantity) AS Total_Revenue,
       SUM(SUM(p.price * od.quantity)) OVER(ORDER BY o.order_date) AS Cumulative_Revenue 
FROM order_details AS od 
JOIN pizzas AS p ON p.pizza_id = od.pizza_id
JOIN orders AS o ON o.order_id = od.order_id
GROUP BY o.order_date 
ORDER BY o.order_date;
sql
Copy
Edit
-- Q13) Determine the top 3 most ordered pizza types based on revenue for each pizza category.
WITH PizzaRevenue AS (
    SELECT 
        pt.category,  
        pt.name AS Pizza_Type,  
        SUM(p.price * od.quantity) AS Total_Revenue,  
        RANK() OVER (PARTITION BY pt.category ORDER BY SUM(p.price * od.quantity) DESC) AS rnk  
    FROM pizza_types pt  
    JOIN pizzas p ON pt.pizza_type_id = p.pizza_type_id  
    JOIN order_details od ON od.pizza_id = p.pizza_id  
    GROUP BY pt.category, pt.name  
)  
SELECT category, Pizza_Type, Total_Revenue, rnk 
FROM PizzaRevenue  
WHERE rnk <= 3  
ORDER BY category, rnk;
📊 Key Insights
✔️ The most popular pizza size is determined.
✔️ The top-selling pizza types drive the most revenue.
✔️ The most profitable time slots for orders are identified.
✔️ The average order value (AOV) provides pricing strategy insights.
✔️ The revenue contribution of different pizza categories is analyzed.

📌 Conclusion
This SQL analysis provides crucial insights for business owners to optimize their menu, pricing, and marketing strategies.
