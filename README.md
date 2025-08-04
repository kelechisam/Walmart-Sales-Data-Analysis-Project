# Walmart Sales Data Analysis Project with SQL

# Project Overview
This project analyzes Walmart sales data to derive insights into product performance, customer behavior, and branch efficiency. The analysis uses SQL queries to process and aggregate data, providing actionable insights for business decision-making. The dataset includes sales transactions with details such as product lines, customer types, branch locations, and payment methods.

# Objectives
Product Analysis: Identify top-selling product lines, profitability, and VAT distribution by product category.
Customer Insights: Analyze customer type distribution, gender-based purchasing patterns, and payment method preferences.
Branch Performance: Evaluate revenue, transaction volume, and other metrics by branch and city.

# Dataset Description
The dataset contains sales transactions with the following key columns:

branch: Branch identifier
city: City of the branch
product_line: Product category
customer_type: Member or Normal
gender: Customer gender
payment: Payment method (e.g., Cash, Credit card, Ewallet)
total: Total transaction amount
quantity: Number of units sold
gross_income: Profit from the transaction
rating: Customer rating
date: Transaction date
time: Transaction time


# Additional derived columns were added:

time_of_day: Morning, Afternoon, or Evening
day_name: Day of the week
month_name: Month of the transaction

#Prerequisites

Database: SQL Server or any compatible SQL database
Tools: SQL client (e.g., SQL Server Management Studio, DBeaver)
Dataset: Walmart sales data (assumed to be loaded into a table named sales)

# Data Preparation

The following SQL scripts prepare the dataset by adding derived columns for time-based analysis.

-- Add time_of_day column
ALTER TABLE sales ADD time_of_day VARCHAR(30);
UPDATE sales SET time_of_day = CASE
    WHEN time BETWEEN '00:00:00' AND '11:59:59' THEN 'morning'
    WHEN time BETWEEN '12:00:00' AND '15:59:59' THEN 'afternoon'
    ELSE 'evening' END;

-- Add day_name column
ALTER TABLE sales ADD day_name VARCHAR(10);
UPDATE sales SET day_name = DATENAME(WEEKDAY, date);

-- Add month_name column
ALTER TABLE sales ADD month_name VARCHAR(15);
UPDATE sales SET month_name = DATENAME(MONTH, date);

# Analysis Queries

1. Product Analysis
Most Selling Product Lines
This query identifies the top-performing product lines by revenue and quantity sold.

SELECT 
    product_line,
    COUNT(*) AS total_transaction,
    SUM(quantity) AS total_quantity_sold,
    SUM(total) AS total_revenue,
    AVG(unit_price) AS avg_unit_price,
    SUM(gross_income) AS total_profit,
    AVG(rating) AS avg_rating
FROM sales
GROUP BY product_line
ORDER BY total_revenue DESC;

# Product Line Profitability
Profitability is assessed by summing the gross_income for each product line.
SELECT 
    product_line,
    SUM(gross_income) AS total_profit,
    SUM(total) AS total_revenue,
    (SUM(gross_income) / SUM(total) * 100) AS profit_margin_percentage
FROM sales
GROUP BY product_line
ORDER BY total_profit DESC;

# VAT Analysis by Product Category
This query calculates the VAT contribution by product line, assuming VAT is included in the total column and using a standard VAT rate (e.g., 5%).
SELECT 
    product_line,
    SUM(total * 0.05) AS total_vat,
    SUM(total) AS total_revenue,
    (SUM(total * 0.05) / SUM(total) * 100) AS vat_percentage
FROM sales
GROUP BY product_line
ORDER BY total_vat DESC;

2. Customer Insights
Customer Type Distribution
This query analyzes transaction volume and revenue by customer type (Member vs. Normal).
SELECT 
    Customer_type,
    COUNT(*) AS total_transaction,
    SUM(quantity) AS total_quantity_sold,
    SUM(total) AS total_revenue,
    AVG(unit_price) AS avg_unit_price,
    SUM(gross_income) AS total_profit,
    AVG(rating) AS avg_rating
FROM sales
GROUP BY Customer_type
ORDER BY total_revenue DESC;

Gender-Based Purchasing Patterns
This query examines purchasing behavior by gender.
SELECT 
    gender,
    product_line,
    COUNT(*) AS total_transaction,
    SUM(quantity) AS total_quantity_sold,
    SUM(total) AS total_revenue,
    AVG(unit_price) AS avg_unit_price,
    SUM(gross_income) AS total_profit
FROM sales
GROUP BY gender, product_line
ORDER BY total_revenue DESC;

# Payment Method Preferences
This query analyzes the distribution of payment methods.
SELECT 
    payment,
    COUNT(*) AS total_transaction,
    SUM(total) AS total_revenue,
    AVG(total) AS avg_transaction_value
FROM sales
GROUP BY payment
ORDER BY total_revenue DESC;

3. Branch Performance
Revenue by City/Branch
This query evaluates branch performance by city and branch.
SELECT 
    branch,
    city,
    COUNT(*) AS transaction_count,
    SUM(total) AS revenue,
    SUM(quantity) AS total_units_sold,
    SUM(gross_income) AS total_profit,
    AVG(rating) AS avg_rating
FROM sales
GROUP BY branch, city
ORDER BY revenue DESC;

# Branch Comparison Metrics
This query provides a detailed comparison of branches by multiple metrics.
SELECT 
    branch,
    city,
    COUNT(DISTINCT invoice_id) AS unique_transactions,
    SUM(total) AS total_revenue,
    SUM(quantity) AS total_units_sold,
    AVG(total) AS avg_transaction_value,
    SUM(gross_income) AS total_profit,
    AVG(rating) AS avg_rating
FROM sales
GROUP BY branch, city
ORDER BY total_revenue DESC;

Monthly Sales by Branch
This query tracks monthly sales trends for each branch.
SELECT 
    branch,
    city,
    month_name,
    COUNT(*) AS transaction_count,
    SUM(total) AS revenue,
    SUM(quantity) AS total_units_sold,
    SUM(gross_income) AS total_profit
FROM sales
GROUP BY branch, city, month_name
ORDER BY branch, month_name;

# Results and Insights

# Product Analysis:
Top-selling product lines can be identified by total revenue and quantity sold.
Profitability varies across product lines, with some categories offering higher profit margins.
VAT contributions highlight product lines with significant tax implications.


# Customer Insights:
Members vs. Normal customers show different purchasing behaviors, with Members potentially driving higher revenue.
Gender-based analysis reveals preferences for specific product lines.
Payment method preferences indicate the popularity of cash, credit card, or Ewallet transactions.


# Branch Performance:
Revenue and transaction volume vary significantly by branch and city.
Monthly trends help identify seasonal patterns or operational issues.
Average ratings provide insights into customer satisfaction by branch.
















