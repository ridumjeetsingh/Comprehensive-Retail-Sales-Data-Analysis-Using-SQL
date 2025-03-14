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

## Technologies Used

- SQL for data cleaning, exploration, and analysis

- PostgreSQL (Recommended) for executing queries efficiently


## Conclusion

This project demonstrates the effective use of SQL in analyzing retail sales data. By following structured data cleaning and insightful queries, businesses can enhance their sales strategies and improve customer engagement.

