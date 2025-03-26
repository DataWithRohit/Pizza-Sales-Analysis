# 🍕 Pizza Sales Analysis - SQL Project  

## 📌 Project Overview  
This project analyzes pizza sales data using SQL. The dataset includes order details, customer information, and pizza types. Our goal is to extract insights such as revenue, best-selling pizzas, customer behavior, and sales trends.

## 🗂️ Dataset Description  
The dataset consists of the following tables:  
- **orders**: Contains order details with timestamps.  
- **order_details**: Stores pizza quantity and order_id mappings.  
- **pizzas**: Lists different pizzas with their price and size.  

---

## 🛠️ SQL Queries and Solutions  

```sql 
-- 1️⃣ Group the orders by date and calculate the average number of pizzas ordered per day.
SELECT o.order_date, AVG(od.quantity) AS Avg_Pizzas_Ordered  
FROM orders o  
JOIN order_details od ON o.order_id = od.order_id  
GROUP BY o.order_date;

-- 2️⃣ Find the total revenue generated from pizza sales.
SELECT SUM(od.quantity * p.price) AS Total_Revenue  
FROM order_details od  
JOIN pizzas p ON od.pizza_id = p.pizza_id;

-- 3️⃣ Identify the top 5 most ordered pizza types.
SELECT p.pizza_type, SUM(od.quantity) AS Total_Orders  
FROM order_details od  
JOIN pizzas p ON od.pizza_id = p.pizza_id  
GROUP BY p.pizza_type  
ORDER BY Total_Orders DESC  
LIMIT 5;

-- 4️⃣ Find the day with the highest pizza sales.
SELECT o.order_date, SUM(od.quantity) AS Total_Pizzas_Sold  
FROM orders o  
JOIN order_details od ON o.order_id = od.order_id  
GROUP BY o.order_date  
ORDER BY Total_Pizzas_Sold DESC  
LIMIT 1;

-- 5️⃣ Determine the most popular pizza size.
SELECT p.size, COUNT(od.order_id) AS Order_Count  
FROM order_details od  
JOIN pizzas p ON od.pizza_id = p.pizza_id  
GROUP BY p.size  
ORDER BY Order_Count DESC  
LIMIT 1;

-- 6️⃣ Calculate the percentage contribution of each pizza type to total revenue.
SELECT p.pizza_type,  
       SUM(od.quantity * p.price) AS Revenue,  
       (SUM(od.quantity * p.price) * 100) / (SELECT SUM(od.quantity * p.price) FROM order_details od JOIN pizzas p ON od.pizza_id = p.pizza_id) AS Revenue_Percentage  
FROM order_details od  
JOIN pizzas p ON od.pizza_id = p.pizza_id  
GROUP BY p.pizza_type;

-- 7️⃣ Find the customers who placed the highest number of orders.
SELECT o.customer_id, COUNT(o.order_id) AS Total_Orders  
FROM orders o  
GROUP BY o.customer_id  
ORDER BY Total_Orders DESC  
LIMIT 5;

-- 8️⃣ Get the average order value.
SELECT AVG(Total_Amount) AS Average_Order_Value  
FROM (SELECT o.order_id, SUM(od.quantity * p.price) AS Total_Amount  
      FROM orders o  
      JOIN order_details od ON o.order_id = od.order_id  
      JOIN pizzas p ON od.pizza_id = p.pizza_id  
      GROUP BY o.order_id) AS Order_Totals;

-- 9️⃣ Identify the least popular pizza type.
SELECT p.pizza_type, SUM(od.quantity) AS Total_Orders  
FROM order_details od  
JOIN pizzas p ON od.pizza_id = p.pizza_id  
GROUP BY p.pizza_type  
ORDER BY Total_Orders ASC  
LIMIT 1;

-- 🔟 Find the total number of orders placed each hour of the day.
SELECT EXTRACT(HOUR FROM order_time) AS Hour, COUNT(order_id) AS Total_Orders  
FROM orders  
GROUP BY EXTRACT(HOUR FROM order_time)  
ORDER BY Hour;

-- 1️⃣1️⃣ Find the most common order day of the week.
SELECT TO_CHAR(order_date, 'Day') AS Order_Day, COUNT(order_id) AS Total_Orders  
FROM orders  
GROUP BY TO_CHAR(order_date, 'Day')  
ORDER BY Total_Orders DESC  
LIMIT 1;

-- 1️⃣2️⃣ Find the total number of unique customers who have placed an order.
SELECT COUNT(DISTINCT customer_id) AS Unique_Customers  
FROM orders;

-- 1️⃣3️⃣ Find the month with the highest sales revenue.
SELECT TO_CHAR(order_date, 'Month') AS Order_Month, SUM(od.quantity * p.price) AS Total_Revenue  
FROM orders o  
JOIN order_details od ON o.order_id = od.order_id  
JOIN pizzas p ON od.pizza_id = p.pizza_id  
GROUP BY TO_CHAR(order_date, 'Month')  
ORDER BY Total_Revenue DESC  
LIMIT 1;
 
```

## 📊 Key Insights
✅ The highest-selling pizza is the one with the most total orders.  
✅ The most common order day helps understand peak sales times.  
✅ The most popular pizza size affects pricing strategy.  
✅ The average order value provides insights into customer spending behavior.



## 📌 Conclusion
This SQL-based Pizza Sales Analysis helps in understanding sales trends, customer preferences, and revenue patterns. These insights can be used to improve marketing strategies and optimize sales.


## 📂 Project Repository
📌 GitHub Link:(GitHub)[https://github.com/DataWithRohit/Pizza-Sales-Analysis]


## 📢 Feel free to fork, use, or suggest improvements! 🚀
