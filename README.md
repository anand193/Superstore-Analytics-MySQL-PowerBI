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

### 2. Count the number of orders shipped by each Ship Mode.
```sql
select `Ship Mode`,count(*) as order_shipped
from superstore 
group by `Ship Mode`
order by order_shipped desc;
```

### 3. Find the percentage contribution of each Category to the overall Sales
```sql
select Category,round(sum(Sales) / (select round(sum(Sales),2) 
from superstore),2)*100 as sales_dist
from superstore
group by Category;
```

### 4. For each Category, calculate the percentage contribution of each Sub-Category to the Category’s total Sales
```sql
with cte2 as (
with cte as (
select distinct Category,`Sub-Category`,
       round(sum(Sales)over(partition by Category,`Sub-Category`),2) as total_sales
from superstore) select *,
                  sum(total_sales)over(partition by Category) as total from cte) 
                  select Category,`Sub-Category`,concat(round((total_sales/total)*100,2),' %') as percentage from cte2;
```


#### 5. For each Customer, find their first order date, last order date, and the total number of orders they placed
```sql
select `Customer Name`,min(`Order Date`) first_order,
        max(`Order Date`) last_order, count(`Order Date`) total_order_placed
from superstore
group by `Customer Name`;
```


### 6. Rank Customers within each Region by their total Sales, and return the top 3 Customers per Region
```sql
select * from (
select Region,`Customer Name`,
       sum(Sales) as total_sales,
       dense_rank()over(partition by Region order by sum(Sales) desc) as sales_rank
       from superstore
       group by Region,`Customer Name`
) ranked_group 
where sales_rank<=3
order by Region,sales_rank;
```


### 7. Find the month-wise total sales for the year 2017
```sql
select month(`Order Date`) as monthly,round(sum(Sales),2) as total_sales
from superstore
where year(`Order Date`) = 2017
group by monthly
order by monthly;
```

### 8. List all customers who have placed more than 10 orders
```sql
select `Customer ID`,`Customer Name`,count(*) as total_orders
from superstore
group by `Customer ID`,`Customer Name`
having total_orders >10;
```

### 9. Find the total sales and profit for each Region
```sql
select Region, round(sum(Sales),2) as total_sales,round(sum(Profit),2) as total_profit
from superstore
group by Region
order by total_sales desc,total_profit desc;
```

### 10. Retrieve all orders that were shipped late (Ship Date > Required Date)
```sql
select * 
from superstore
where `Ship Date` > 
      case 
          when `Ship Mode` = 'Same Day' then date_add(`Order Date`,interval 0 Day)
          when `Ship Mode` = 'First Class' then date_add(`Order Date`,interval 1 Day)
          when `Ship Mode` = 'Second Class' then date_add(`Order Date`,interval 3 Day)
          when `Ship Mode` = 'Standard Class' then date_add(`Order Date`,interval 5 Day)
	  end;
```
(…and many more queries for detailed insights)

📈 Key Insights from SQL Analysis

Total revenue generated during the analysis period was $2272449'

Top-selling category: Technology,Furniture, followed by Office Supplies.

Most orders were shipped using Standard Class, indicating it is the preferred shipping option among customers

Chairs dominate Furniture sales, while Binders are the top-selling Office Supplies product. This highlights which sub-categories drive revenue within each category

The highest sales occurred in November (117,383.38), likely due to holiday season demand, while the lowest sales were in February (20,262.32). Overall, sales show a steady upward trend from mid-year to year-end.

Category contribution to revenue: Furniture 32%, Technology 37%, Office Supplies 31%.

The West region generated the highest sales and profit, followed by East, while South had the lowest sales despite a slightly higher profit than Central. This suggests that West and East regions are the most profitable markets.

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

linkedin.com/in/anandmehto
