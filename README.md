# Comprehensive-Retail-Sales-Data-Analysis-Using-SQL

## Project Overview

This project uses SQL to analyze retail sales data, focusing on data cleaning, exploration, and insights generation. Key steps include handling null values, renaming columns, and ensuring data consistency. The analysis covers total sales, customer demographics, category performance, high-value transactions, and identifying top customers. Advanced SQL techniques like window functions and CTEs are used for optimized insights, including monthly trends and shift-based sales patterns. The project is designed to provide actionable business insights for improving sales strategies.

## Dataset Description

The dataset contains the following columns:

- transactions_id: Unique identifier for each transaction

- sale_date: Date of the sale

- sale_time: Time of the sale

- customer_id: Unique identifier for each customer

- gender: Gender of the customer

- age: Age of the customer

- category: Product category (e.g., Clothing, Beauty, etc.)

- quantity: Number of items purchased

- price_per_unit: Price of each unit

- cogs: Cost of goods sold

- total_sale: Total amount spent by the customer

## Key Steps Performed

1. Data Cleaning

- Renamed quantiy column to quantity

- Identified and removed rows with null values in critical columns

2. Data Exploration

- Identified total sales volume

- Counted unique customers

- Listed distinct product categories

3. Business Insights and Analysis

Sales Insights:

- Identified total sales for specific dates (e.g., 2022-11-05)

- Filtered sales for categories like 'Clothing' with quantities greater than 3 in November 2022

Category Analysis:

- Calculated total sales and total orders by category

- Found the average age of customers purchasing from the 'Beauty' category

High-Value Transactions:

- Retrieved transactions where the total sale was greater than $1000

Gender-Based Insights:

- Counted total transactions per gender by category

Monthly Trends:

- Identified the best-selling month in each year using RANK() for optimized results

Top Customers:

- Retrieved the top 5 customers based on total sales

Customer Segmentation:

- Counted unique customers purchasing from each category

Shift Analysis:

- Created morning, afternoon, and evening shifts to analyze transaction trends

```SQL

-- SQL Retail Sales Analysis ---

DROP TABLE IF EXISTS retail_sales;
create table retail_sales
(
	transactions_id INT PRIMARY KEY,
	sale_date DATE,
	sale_time TIME,
	customer_id INT,
	gender VARCHAR(15),
	age	INT,
	category VARCHAR(15),
	quantiy INT,
	price_per_unit FLOAT,
	cogs FLOAT,
	total_sale FLOAT
);

SELECT * FROM retail_sales;

-- changing the on column name ---
ALTER TABLE retail_sales 
RENAME COLUMN quantiy TO quantity;


---///DATA CLEANING///---

-- Identifying the null value in the data ---

SELECT 
* FROM retail_sales
WHERE transactions_id IS NULL;

SELECT 
* FROM retail_sales
WHERE sale_date IS NULL;

-- We can take step by step or we can directly include all the column together in one query --

SELECT 
* FROM retail_sales
WHERE transactions_id IS NULL
	OR
	sale_date IS NULL
	OR
	sale_time IS NULL
	OR
	customer_id IS NULL
	OR
	gender IS NULL
	OR 
	category IS NULL
	OR 
	quantity IS NULL
	OR 
	cogs IS NULL
	OR
	total_sale IS NULL;

--- Now finding a null value in some of the columns rows, now we have two options 
---1. To set null into 0.
---2. Delete the data where we are getting null value.

-- we are adatping 2nd options here !

DELETE FROM retail_sales
WHERE transactions_id IS NULL
	OR
	sale_date IS NULL
	OR
	sale_time IS NULL
	OR
	customer_id IS NULL
	OR
	gender IS NULL
	OR 
	category IS NULL
	OR 
	quantity IS NULL
	OR 
	cogs IS NULL
	OR
	total_sale IS NULL;

---/// DATA EXPLORATIONS ///---

-- How many sales we have ?

SELECT COUNT(*) as total_sales 
FROM retail_sales;

-- How many unique customers we have ?

SELECT COUNT(DISTINCT(customer_id)) as total_unique_customers 
FROM retail_sales;

-- How many category we have ?

SELECT DISTINCT(category) as total_category 
FROM retail_sales


---/// DATA ANALYSIS & BUSINESS KEY PROBLEMS & ANSWERS ///---

-- 1. Retrive all the columns from sales made on '2022-11-05' ?

SELECT * 
FROM retail_sales
WHERE sale_date = '2022-11-05';


-- 2. Retrive all the transactions where the category is 'clothing' and the quantity sold more than 3 in the month of Nov-2022 ?

SELECT * 
FROM retail_sales
WHERE category = 'Clothing'
AND quantity > 3
AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11';

-----/// Here TO_CHAR is use to compare the condition with strings and numbers.


-- 3. calculate the total sales (total_sale) and total orders for each category ?

SELECT category, SUM(total_sale) as Net_sale, COUNT(*) as total_orders 
FROM retail_sales
GROUP BY category;


-- 4. Retrive the AVG age of customers who purchased items from the 'Beauty'  category ?

SELECT 
ROUND(AVG(age),2) as avg_age
FROM retail_sales
WHERE category = 'Beauty';

--//// here we are using ROUND() to reduce the no. of decimals. 


-- 5. Retrive all the transactions where the total_sale is greater than 1000 ?


SELECT * 
FROM retail_sales
WHERE total_sale > 1000;


-- 6. Retrive the total number of transactions (transaction_id) made by each gender in each category ? 

	
SELECT  gender, category, COUNT(*) as total_transactions 
FROM retail_sales
GROUP BY gender, category 
ORDER BY category;


-- 7. calulate the avg sale for each months, finds out the best selling month in each year ?


--- There is total 3 way to do this.


	-- 1st way, where the code is less optimize and need manually to see the max_avg_sale month by month, and also use of order by. 

SELECT 
EXTRACT(YEAR FROM sale_date) as year, 				--EXTRACT() helps in comparession in dates
EXTRACT(MONTH FROM sale_date) as month,
AVG(total_sale) as avg_sale
FROM retail_sales
GROUP BY year, month
ORDER BY year, avg_sale DESC;


	-- 2nd way, where the code is more optimize, and easily identiy the max_avg_sale by the help of rank column by ranking, not using order by.  

SELECT 
EXTRACT(YEAR FROM sale_date) as year,
EXTRACT(MONTH FROM sale_date) as month,
AVG(total_sale) as avg_sale,
RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank   -- USING RANK() window fuction.
FROM retail_sales
GROUP BY year, month;


	-- 3rd way, where the code is best optimize, with the help of subquery and show the result infront of us.  

SELECT * FROM 

(
	SELECT 
	EXTRACT(YEAR FROM sale_date) as year,
	EXTRACT(MONTH FROM sale_date) as month,
	AVG(total_sale) as avg_sale,
	RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
	FROM retail_sales
	GROUP BY year, month
) as t1
WHERE rank = 1;


							-- FINAL SOLUTION after removing the rank column 		

SELECT year, month, avg_sale FROM 
(
	SELECT 
	EXTRACT(YEAR FROM sale_date) as year,
	EXTRACT(MONTH FROM sale_date) as month,
	AVG(total_sale) as avg_sale,
	RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) 	-- here we implemted the logic of max total avg sale 
	FROM retail_sales
	GROUP BY year, month
) as t1
WHERE rank = 1;


-- 8. Retrive the top 5 customer based on the highest total sales ?

SELECT 
	customer_id,
	SUM(total_sale) as total_sale
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sale DESC
LIMIT 5;



-- 9. Find the number of unique customers who purchased items from each category ?

SELECT 
	COUNT(DISTINCT customer_id) as count_unique_customers,
	category	
FROM retail_sales
GROUP BY category;


-- 10. Write sql query to create each shift (time) and number of transactions (Example Morning <12, Afternoon Between 12 & 17, Evening >17)

-- 1st approach create a shift timing condition with the help case statement it work like if-else-end, directly insert the logic in the statements
-- using CTE (COMMON TABLE EXPRESSION) help improve query readability, simplify complex queries, Temporary Named Result Set and Performance Optimization.

WITH hourly_sales as
(
	SELECT *,
		CASE   
			WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
			WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
			ELSE 'Evening'
			END as shift
	FROM retail_sales
)

SELECT 
COUNT(transactions_id) as total_transactions,
shift
FROM hourly_sales
GROUP BY shift;


```


## Technologies Used

- SQL for data cleaning, exploration, and analysis

- PostgreSQL (Recommended) for executing queries efficiently


## Conclusion

This project demonstrates the effective use of SQL in analyzing retail sales data. By following structured data cleaning and insightful queries, businesses can enhance their sales strategies and improve customer engagement.

