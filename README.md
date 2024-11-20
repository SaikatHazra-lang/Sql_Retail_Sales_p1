# Sql_Retail_Sales_p1
 ```sql
Select * from Retail_Sales
```
--2. Data Exploration & Cleaning

-- **Record Count**: Determine the total number of records in the dataset.
 ```sql
     select count(*) from Retail_Sales
 ```
-- **Customer Count: Find out how many unique customers are in the dataset **
 ```sql
.    select count(distinct customer_id) from Retail_Sales
 ```
-- **Category Count**: **Identify all unique product categories in the dataset.**
 ```sql
     select count(distinct category) from Retail_Sales
 ```
-- **Null Value Check: Check for any null values in the dataset and delete records with missing data.**
 ```sql
     Select * from Retail_Sales
	 where 
	 transactions_id is null
	 or
	 sale_date is null
	 or
	 sale_time is null
	 or
	 gender is null 
	 or
	 category is null
	 or
	 quantiy is null 
	 or
	 cogs is null
	 or
	 total_sale is null
```
-- Data Cleaning
   ```sql
delete from Retail_Sales
where
 transactions_id is null
	 or
	 sale_date is null
	 or
	 sale_time is null
	 or
	 gender is null 
	 or
	 category is null
	 or
	 quantiy is null 
	 or
	 cogs is null
	 or
	 total_sale is null 
  ```
--**Data Exploration**
--**How many sales we have?**
select count(*) from Retail_Sales
--**How many customers we have ?**
 select count(distinct customer_id) from Retail_Sales

 --**Data Analysis and Business key problems & answers.**


 --1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
 ```sql
 select * from 
  Retail_Sales
  where sale_date = '2022-11-05'
```
--2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:

 ```sql
 select * from 
  Retail_Sales
  where category='Clothing' and quantiy>=4 and Datepart(Year,sale_date) = 2022 and Datepart(MONTH,sale_date)=11
```
--3. **Write a SQL query to calculate the total sales (total_sale) for each category.**

 ```sql
  select category,sum(total_Sale) as net_sales from 
  Retail_Sales
  group by category
```

--4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**
 ```sql
  select round(avg(age),2)as avg_age from 
  Retail_Sales
  where category='Beauty'
  ```
--5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**
 ```sql
  SELECT * FROM retail_sales
  WHERE total_sale > 1000
  ```
--6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**
 ```sql
  select category,gender,count(transactions_id) as Total_transaction from 
  Retail_Sales
  group by category,gender
  ```
--7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year.**
 ```sql
 with cte as(
 SELECT 
     Datepart(Year,sale_date) as years,Datepart(MONTH,sale_date) as Months,avg(total_Sale) as avg_Sales,
     rank() over(partition by Datepart(Year,sale_date) order by avg(total_Sale) asc ) rn
 FROM retail_sales
     group by Datepart(Year,sale_date),Datepart(MONTH,sale_date)
 )
 select years,months,avg_Sales from cte
 where rn=1
  ```
--8.**Write a SQL query to find the top 5 customers based on the highest total sales**
 ```sql
 Select top 5
 customer_id, sum(total_Sale) as net_Sales
 from
 Retail_Sales
 group by customer_id
 order by net_Sales desc
  ```
 --9. **Write a SQL query to find the number of unique customers who purchased items from each category.**
 ```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category
  ```
--10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**
 ```sql
with cte as(select *,
 case 
     when Datepart(hour,sale_time)<12 then 'Morning'
     when Datepart(hour,sale_time) between 12 and 17 then 'Afternoon'
 else 'Evening'
     end as 'Shift'
from retail_sales)
select shift ,count(*) as Total_orders
from cte
group by Shift
  ```
--End of Project


