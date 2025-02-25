# MY-Retail-Sales-Project

-- SQL Retail Sales Analysis - P1
create database sql_project_p2;

-- Create Table 
drop table if exists ratail_sales;
create table retail_sales
          (
		    transactions_id INT primary key,
            sale_date DATE ,
            sale_time TIME,
            customer_id INT,
            gender varchar(15),
            age INT,
            category varchar(15),
            quantity INT,
            price_per_unit float,
           cogs float,
           total_sale float
          );
-- count the total number of data row 
select 
count(*) as Rows from retail_sales;

-- DATA CLEANING METHOD
-- **Record Count**: Determine the total number of records in the dataset.
-- - **Customer Count**: Find out how many unique customers are in the dataset.
-- - **Category Count**: Identify all unique product categories in the dataset.
-- - **Null Value Check**: Check for any null values in the dataset and delete records with missing data.
-- check in the data set have the any NULL value

select * from retail_sales 
where  transactions_id is NULL
              or
            sale_date is NULL
              or
            sale_time is NULL
              or
            customer_id is NULL
              or
            gender is NULL
              or
            age is NULL
              or
            category is NULL
              or
            quantity is NULL
              or
            price_per_unit is NULL
              or
           cogs is NULL
              or
           total_sale is NULL ;

--  DELETE THE ALL ROW ,WHOSE HAVE THE NULL VALUE
DELETE FROM retail_sales

where  transactions_id is NULL
              or
            sale_date is NULL
              or
            sale_time is NULL
              or
            customer_id is NULL
              or
            gender is NULL
              or
            age is NULL
              or
            category is NULL
              or
            quantity is NULL
              or
            price_per_unit is NULL
              or
           cogs is NULL
              or
           total_sale is NULL ;

-- DATA EXPLORATION METHOD

--  how many sales we have?
select count(*) as total_sales from retail_sales 
-- 1987 person data have in the dataset.

--  How many (customer) and (unique customer) we have
select count(customer_id) as total_cid from retail_sales

-- how many distinct all column data here method.
select count(distinct customer_id) as unique_customer from retail_sales
select distinct category from retail_sales
select distinct age  from retail_sales

-- Data Analysis & Business Key Problem & Answers

-- 1. Write a SQL query to retrieve all columns for sales made on '2022-11-05':
```sql
 select * from retail_sales
 where sale_date = '2022-11-05'
```
-- 2. Write a SQL query to retrieve all transactions where the category is 'Clothing' 
-- and the quantity sold is more than 4 in the month of Nov-2022:
```sql
 select * from retail_sales
 where category = 'Clothing' 
 and 
 to_char(sale_date,'YYYY-MM') = '2022-11'
 and 
 quantity >= 4
```
-- 3.Write a SQL query to calculate the total sales (total_sale) for each category.:
```sql
 select category,
 sum(total_sale) as total_sales, 
 count(*) as total_order
 from retail_sales
 -- 1 means category 
 group by 1
```
-- 4.Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:
-- Round use for decimal value. 

```sql
select 
Round(avg(age),2) as CustomerAvg_age 
from retail_sales
where category = 'Beauty'
```
-- 5.Write a SQL query to find all transactions where the total_sale is greater than 1000.:
```sql
select * from retail_sales
where total_sale > 1000
```
-- 6.Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.:
```sql
 select category,gender,
 count(transactions_id) as numberof_transaction 
 from retail_sales
 group by 1,2
```
-- 7.Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:
```sql
select avg(total_sale) as avg_sales,
-- My Method.
-- to_char(sale_date,'MM')
-- to_char(sale_date,'YYYY')

    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month
from retail_sales
group by 1,2,3
order by 1 desc
--  limit 2 isliye ki,big amount wale pahle aa jayege.
--  accoring to your tum bhi limit lga skte ho.
limit 2
-- USe in the Rank method
--  big solution of 7 num. Questioon
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
```
-- 8.Write a SQL query to find the top 5 customers based on the highest total sales.:
```sql

select customer_id , sum(total_sale) as total_sales from retail_sales
group by 1
order by 2 desc
limit 5
```
-- 9.Write a SQL query to find the number of unique customers who purchased items from each category.:
```sql

select count(distinct customer_id) as number_pur, category from retail_sales
group by 2
```
-- 10.Write a SQL query to create each shift and number of orders
-- (Example Morning <12, Afternoon Between 12 & 17, Evening >17):
```sql
with hourly_sales
as(
select *,
     case 
	   when extract(hour from sale_time) < 12  then 'morning'
	   when extract(hour from sale_time) between 12 and 17 then 'Afternoon'
	   else 'evening'
	end as shift
from retail_sales
)
select 
shift,
count(*) as total_orders
from hourly_sales
group by shift
```
-- END of Project.
## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `SQL - Retail Sales Analysis_utf ` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `MY_Ratail&Sales Project` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.


his project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

Thank you for your support, and I look forward to connecting with you!
