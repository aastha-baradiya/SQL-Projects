# SQL-Projects
-- Create Table Vital_Pharma
Create table Vital_Pharma_2
(
Transaction_ID VARCHAR(225),
Customer_Id INT,
Date DATE,
Product VARCHAR(225),
Country VARCHAR(225),
Region VARCHAR(225),
Zip_Code INT,
CITY VARCHAR(225),
Sales_Rep VARCHAR(225),
Channel VARCHAR(225), 
Units_Sold INT,
Unit_Price FLOAT,
Returns INT,
Batch_ID VARCHAR(225),
Total_Revenue FLOAT,
Net_Units_Sold INT
);

SELECT * FROM Vital_Pharma_2;

/*
Easy
Total Revenue:
Find the total revenue generated from all sales.

Top Products by Units Sold:
List the top 3 products based on total units sold.

Distinct Sales Channels:
List all unique sales channels.

Sales by Region:
Show the total revenue for each region.

Date Range:
Find the earliest and latest transaction dates in the table.
*/

--Q.1 Find the total revenue generated from all sales.
SELECT SUM(Total_Revenue) as Total_Revenue FROM Vital_Pharma_2;

--Q.2 List the top 3 products based on total Total_Revenue.
SELECT Product, SUM(Total_Revenue) FROM Vital_Pharma_2 GROUP BY 1 ORDER BY 2 DESC LIMIT 3;

--Q.3 List all unique sales channels
SELECT DISTINCT Channel FROM Vital_Pharma_2;

--Q.4 Show the total revenue for each region.
SELECT Region, SUM(Total_Revenue) AS TOTAL_REVENUE_BY_REGION FROM Vital_Pharma_2 GROUP BY 1 ORDER BY 2 DESC ;

--Q.5 Find the earliest and latest transaction dates in the table.
SELECT MIN(Date) AS EARLIEST_DATE, MAX(Date) AS LATEST_DATE FROM Vital_Pharma_2;

/*
Medium
Monthly Sales Trend:
Calculate total revenue for each month.

Top Sales Rep:
Find the sales representative with the highest total revenue.

Product Performance:
For each product, show total units sold and total returns.

Average Price per Region:
Find the average unit price of products sold in each region.

High-Return Transactions:
List transactions where the number of returns is more than 10% of units sold.
*/

--Q.6 Calculate total revenue for each month.
SELECT  DATE_TRUNC('month', Date) AS Month, SUM(Total_Revenue) AS TOTAL_REVENUE FROM Vital_Pharma_2 GROUP BY 1 ORDER BY 1;

--Q.7 Find the sales representative with the highest total revenue.
SELECT Sales_Rep, SUM(Total_Revenue) FROM Vital_Pharma_2 GROUP BY 1 ORDER BY 2 DESC LIMIT 1;

--Q.8 For each product, show total units sold and total returns.
SELECT Product, SUM(Units_Sold) AS TOTAL_UNITS_SOLD , SUM (Returns) AS TOTAL_RETURNS FROM Vital_Pharma_2 GROUP BY 1 ORDER BY 2;

--Q.9 Find the average unit price of products sold in each region.
SELECT Region, AVG(Unit_Price) FROM Vital_Pharma_2 GROUP BY 1 ORDER BY 2 DESC;

--Q.10 List transactions where the number of returns is more than 10% of units sold.
SELECT * FROM Vital_Pharma_2 WHERE Returns>(0.1*Units_Sold);

/*
Advanced
Repeat Customers:
Identify customers who have made purchases in at least 2 different months.

Channel Comparison:
Compare average revenue per transaction between Online and Distributor channels.

City & Product Combo:
Find the city-product combinations with the highest net units sold.

Sales Rep Efficiency:
For each sales rep, calculate the average net units sold per transaction, and rank them.

Profit Loss Analysis:
Assume every returned unit incurs a 30% loss on its unit price. For each product, estimate total loss due to returns.
*/

--Q.11 Identify customers who have made purchases in at least 2 different months.
SELECT Customer_Id FROM
(SELECT Customer_Id, COUNT (DISTINCT DATE_TRUNC('month', Date)) AS Month FROM Vital_Pharma_2
GROUP BY 1) WHERE Month >=2;

--Q.12 Compare average revenue per transaction between Online and Distributor channels.
SELECT Channel, AVG(Total_Revenue) AS AVG_TOTAL_REVENUE FROM Vital_Pharma_2
WHERE Channel IN ('Online', 'Distributor') GROUP BY 1;

--Q.13 Find the city-product combinations with the highest net units sold.
SELECT CITY, Product, SUM(Net_Units_Sold) FROM Vital_Pharma_2
GROUP BY 1,2 ORDER BY 3 DESC LIMIT 1;

--Q.14 For each sales rep, calculate the average net units sold per transaction, and rank them.
SELECT Sales_Rep, AVG (Net_Units_Sold),
RANK() OVER(ORDER BY AVG (Net_Units_Sold) DESC) AS RANK
FROM Vital_Pharma_2 GROUP BY 1;

--Q.15 Assume every returned unit incurs a 30% loss on its unit price. For each product, estimate total loss due to returns.
SELECT
    Product,
    SUM(Returns * Unit_Price * 0.3) AS total_loss_due_to_returns
FROM Vital_Pharma_2
GROUP BY Product
ORDER BY total_loss_due_to_returns DESC;



