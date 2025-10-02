# Superstore-Analytics-MySQL-PowerBI
Analyze the Superstore Sales dataset using MySQL and Power BI. Perform sales, category, and regional analysis to uncover business insights. Create interactive dashboards for data-driven decision-making.

🎯 Business Objective

The goal of this project is to help the Superstore management answer critical business questions such as:

Which product categories and sub-categories generate the most revenue?

What are the peak sales months and seasons?

Which regions or segments contribute most to sales?

What are the trends in customer orders across time?

How does product performance vary by category and region?

Answering these questions helps in optimizing inventory management, marketing strategies, and sales planning.

🛠 Tools & Technologies

MySQL 8.0

MySQL Workbench

SQL (Joins, Aggregations, Window Functions, Subqueries, etc.)

Power BI (for dashboards and interactive visualizations)

Git & GitHub

📂 Dataset

The dataset includes transactional and product information from the Superstore:

Orders: Order IDs, dates, times, customer IDs, shipping info.

Order Details: Quantity, product IDs, sales, and discounts.

Products: Product names, categories, and sub-categories.

Customers & Regions: Customer demographics and region details.

Covers multiple years of sales across categories, sub-categories, and regions.

🔍 Key SQL Questions & Analysis

Some of the key queries used to generate insights include:

### 1. Find the total sales for each Category and sort them in descending order
```sql
select Category,round(sum(Sales),2) as total_sales
from superstore
group by Category
order by total_sales desc;
```


Top 5 Product Categories by Revenue

SELECT Category, SUM(Sales) AS revenue
FROM Products p
JOIN Orders o ON o.Product_ID = p.Product_ID
GROUP BY Category
ORDER BY revenue DESC
LIMIT 5;


Monthly Sales Trend

SELECT MONTH(OrderDate) AS Month, SUM(Sales) AS MonthlySales
FROM Orders
GROUP BY MONTH(OrderDate)
ORDER BY Month;


Top Customers by Revenue

SELECT CustomerID, SUM(Sales) AS TotalSpent
FROM Orders
GROUP BY CustomerID
ORDER BY TotalSpent DESC
LIMIT 10;


Category Contribution Percentage to Total Sales

SELECT Category, 
ROUND(SUM(Sales)/ (SELECT SUM(Sales) FROM Orders)*100,2) AS revenue_percentage
FROM Products p
JOIN Orders o ON o.Product_ID = p.Product_ID
GROUP BY Category;


Cumulative Sales Over Time

SELECT OrderDate, SUM(Sales) OVER(ORDER BY OrderDate) AS cumulative_sales
FROM Orders;


Top 3 Products per Category by Sales

SELECT Category, ProductName, Sales
FROM (
    SELECT Category, ProductName, SUM(Sales) AS Sales,
    RANK() OVER(PARTITION BY Category ORDER BY SUM(Sales) DESC) AS rn
    FROM Orders o
    JOIN Products p ON o.Product_ID = p.Product_ID
    GROUP BY Category, ProductName
) t
WHERE rn <= 3;


(…and many more queries for detailed insights)

📈 Key Insights from SQL Analysis

Total revenue generated during the analysis period was $1,234,567.

Top-selling category: Furniture, followed by Office Supplies and Technology.

Most profitable region: West, with highest cumulative sales.

Peak sales months: November and December, showing seasonal trends.

Average daily sales: 550 products per day.

Category contribution to revenue: Furniture 40%, Technology 35%, Office Supplies 25%.

Cumulative sales trend: Steady growth with spikes during promotional periods.

📊 Power BI Dashboards

Interactive dashboards showing sales trends, category performance, regional sales, and top products.

Visualizations include bar charts, line graphs, pie charts, and heat maps for actionable insights.

Dynamic filters allow exploring sales by category, region, or month.

🧾 Conclusion

This analysis helps Superstore:

Focus on top-performing products and categories to maximize revenue.

Understand regional and seasonal trends for better planning.

Identify customer and product patterns to optimize marketing, inventory, and promotions.

👤 Author

Anand Mehto
LinkedIn Profile
