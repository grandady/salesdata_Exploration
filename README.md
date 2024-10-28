# RFM ANALYSIS AND DATA EXPLORATION
***
# Global Vehicle Traders Inc.


## 🧾 Table of Contents
- [Business Task](#business-task)
- [Methodology](#methodology)
- [Data Exploration](#data-exploration)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)
***


## Business Task
### Global Vehicle Traders Inc.
Global Vehicle Traders Inc. is a dynamic multi-vehicle trading company that excels in acquiring, selling, and occasionally restoring a diverse range of vehicles. The Marketing Department is keen to broaden its market reach and execute a significant sales campaign. The marketing manager seeks your expertise to analyze and address the following objectives:

1. Identify Top Customers: Determine the most valuable and frequent buyers to tailor marketing strategies effectively.

2. Determine the Two Most Frequently Purchased Product Pairings: Analyze sales data to uncover popular product combinations, aiding in promotional efforts.

3. Perform Customer Segmentation: Categorize customers into distinct groups based on behaviors and preferences, facilitating targeted marketing and personalized experiences.

Your insights will be instrumental in driving the success of this campaign.

## Data Source
Sales data: The primary data used for this analysis is the "salesdata.csv" file, containing detailed information about each sale made by the company

## Methodology

- Data Importation: Importing datasets into the SQL Server Database for comprehensive analysis.
Exploring Sales Data: Generate various analytics and insights based on customers' past purchase behavior.

- Analyzing Sales Revenue: Assessing revenue trends and patterns over time.

- Customer Segmentation using RFM Technique: Performing customer segmentation analysis to categorize customers based on Recency, Frequency, and Monetary values, enabling targeted marketing efforts.

- Advanced SQL Techniques: Utilizing a range of SQL techniques including:

    - Basic SQL Queries for foundational analysis

    - Sub Queries and Common Table Expressions (CTEs) to solve complex problems

    - SQL Aggregate Functions to compute summary statistics

    - SQL Window Functions for advanced calculations

    - SQL XML Path Functions for structured data handling.

- Data Visualization in Tableau: Creating intuitive visualizations and dashboards to represent findings and facilitate data-driven decision-making.

  ## Data Exploration
  
  ### Column Dictionary

1. Order Number: A unique identifier for each order placed by customers. Essential for tracking and managing orders.

2. Quantity Ordered: The number of units purchased in an order. Helps in inventory management and sales analysis.

3. Price Each: The price per unit of the product sold. Important for calculating total sales and revenue.

4. Orderline Number: Sequence number of items in a particular order, which helps in organizing multi-item orders.

5. Sales Order Date: The date when the order was placed. Useful for tracking sales trends and performance over time.

6. Status: Current status of the order (e.g., shipped, pending, canceled). Critical for order management and customer service.

7. Quarter ID: Identifies the fiscal quarter in which the order was placed. Helps in quarterly sales analysis and reporting.

8. Month ID: Identifies the month in which the order was placed. Used for monthly sales analysis and trends.

9. Year ID: Identifies the year in which the order was placed. Crucial for annual performance reviews and trends.

10. Product: Name of the product sold. The key to analyzing product performance and popularity.

11. MSRP: Manufacturer's Suggested Retail Price. Useful for price comparison and discount analysis.

12. Product Code: Unique identifier for each product. Essential for inventory tracking and product management.

13. Customer Name: Name of the customer who placed the order. Important for customer relationship management and personalization.

14. Phone: Customer’s phone number. Useful for communication and customer support.

15. Address Line 1: Primary address of the customer. Essential for shipping and delivery.

16. Address Line 2: Secondary address information. Additional detail for accurate shipping.

17. City: City where the customer is located. Useful for regional sales analysis and logistics.

18. State: State where the customer is located. Important for compliance and regional analysis.

19. Postal Code: Postal code for the customer’s address. Used for detailed geographic analysis and shipping logistics.

20. Country: Country of the customer. Helps in international sales analysis and compliance.

21. Territory: Sales territory assigned to the order. Useful for sales strategy and regional performance analysis.

22. Contact Last Name: Last name of the customer’s contact person. Important for personalized communication.

23. Contact First Name: First name of the customer’s contact person. Complements personalized communication efforts.

24. Deal Size: Size or value of the deal. Useful for revenue analysis and sales strategy.

### Data Exploration

**Step 1: Yearly Sales Trend Analysis**
Objective: Analyze sales revenue trends over the years to identify growth patterns and any potential seasonality in sales.

````sql
SELECT YEAR(orderdate) AS Year, 
       SUM(sales) AS TotalSales
FROM [dbo].[sales_data_sample]
GROUP BY YEAR(orderdate)
ORDER BY Year;

````

**Answer:**



**2.Monthly Sales Trend**
Objective: Examine monthly sales data to detect monthly patterns and trends.

````sql
SELECT YEAR(orderdate) AS Year, 
       MONTH(orderdate) AS Month, 
       SUM(sales) AS TotalSales
FROM [dbo].[sales_data_sample]
GROUP BY YEAR(orderdate), MONTH(orderdate)
ORDER BY Year, Month;
````

**Answer:**


**3. Which month had the highest sales within a given year? What was the total revenue for that month?**

````sql
SELECT MONTH_ID, SUM(sales) AS Revenue, COUNT(ORDERNUMBER) AS Frequency
FROM [PortfolioDB].[dbo].[sales_data_sample]
WHERE YEAR_ID = 2004  -- Change the year to see the rest
GROUP BY MONTH_ID
ORDER BY Revenue DESC;

````

**Answer:**


**4. What products sell most in November?**

````sql
SELECT MONTH_ID, PRODUCTLINE, SUM(sales) AS Revenue, COUNT(ORDERNUMBER) AS OrderCount
FROM [dbo].[sales_data_sample]
WHERE YEAR_ID = 2004 AND MONTH_ID = 11  --change year to see the rest
GROUP BY MONTH_ID, PRODUCTLINE
ORDER BY Revenue DESC;

````

**Answer:**


**5. Grouping by PRODUCTLINE and sorting by revenue**

````sql
SELECT PRODUCTLINE, SUM(sales) AS Revenue
FROM [dbo].[sales_data_sample]
GROUP BY PRODUCTLINE
ORDER BY Revenue DESC;

````

**6. Grouping by CITY and sorting by revenue**

````sql
SELECT city, SUM(sales) AS Revenue
FROM [PortfolioDB].[dbo].[sales_data_sample]
GROUP BY city
ORDER BY Revenue DESC;

````

**7. Grouping by DEAlSIZE and sorting by revenue**

````sql
SELECT DEALSIZE, SUM(sales) AS Revenue
FROM [PortfolioDB].[dbo].[sales_data_sample]
GROUP BY DEALSIZE
ORDER BY Revenue DESC;

````
**8. Grouping by TERRITORY and sorting by revenue**

````sql
SELECT TERRITORY, SUM(sales) AS Revenue
FROM [PortfolioDB].[dbo].[sales_data_sample]
GROUP BY TERRITORY
ORDER BY Revenue DESC;

````

**9. Grouping by STATUS and sorting by revenue**

````sql
SELECT STATUS, SUM(sales) AS Revenue
FROM [PortfolioDB].[dbo].[sales_data_sample]
GROUP BY STATUS
ORDER BY Revenue DESC;

````
**10. Top Customers**

````sql
SELECT customername, SUM(sales) AS TotalSales, COUNT(ORDERLINENUMBER) AS PurchaseFrequency
FROM [dbo].[sales_data_sample]
GROUP BY customername
ORDER BY TotalSales DESC, PurchaseFrequency DESC;

````

**11. Customer Segmentation**

````sql
DROP TABLE IF EXISTS #rfm;

WITH rfm AS (
    SELECT CUSTOMERNAME,
           SUM(sales) AS MonetaryValue,
           AVG(sales) AS AvgMonetaryValue,
           COUNT(ORDERNUMBER) AS Frequency,
           MAX(ORDERDATE) AS last_order_date,
           (SELECT MAX(ORDERDATE) FROM [dbo].[sales_data_sample]) AS max_order_date,
           DATEDIFF(DD, MAX(ORDERDATE), (SELECT MAX(ORDERDATE) FROM [dbo].[sales_data_sample])) AS Recency
    FROM [PortfolioDB].[dbo].[sales_data_sample]
    GROUP BY CUSTOMERNAME
),
rfm_calc AS (
    SELECT r.*,
           NTILE(4) OVER (ORDER BY Recency DESC) AS rfm_recency,
           NTILE(4) OVER (ORDER BY Frequency) AS rfm_frequency,
           NTILE(4) OVER (ORDER BY MonetaryValue) AS rfm_monetary
    FROM rfm r
)
SELECT c.*, 
       rfm_recency + rfm_frequency + rfm_monetary AS rfm_cell,
       CAST(rfm_recency AS VARCHAR) + CAST(rfm_frequency AS VARCHAR) + CAST(rfm_monetary AS VARCHAR) AS rfm_cell_string
INTO #rfm
FROM rfm_calc c;

SELECT CUSTOMERNAME, 
       rfm_recency, 
       rfm_frequency, 
       rfm_monetary,
       CASE 
           WHEN rfm_cell_string IN ('111', '112', '121', '122', '123', '132', '211', '212', '114', '141') THEN 'lost_customers'
           WHEN rfm_cell_string IN ('133', '134', '143', '244', '334', '343', '344', '144') THEN 'slipping away, cannot lose'
           WHEN rfm_cell_string IN ('311', '411', '331') THEN 'new customers'
           WHEN rfm_cell_string IN ('222', '223', '233', '322') THEN 'potential churners'
           WHEN rfm_cell_string IN ('323', '333', '321', '422', '332', '432') THEN 'active'
           WHEN rfm_cell_string IN ('433', '434', '443', '444') THEN 'loyal'
       END AS rfm_segment
FROM #rfm;

````

** 12. The Two Most Frequently Purchased Product Pairings**

````sql
SELECT DISTINCT OrderNumber, 
    STUFF((
        SELECT ',' + PRODUCTCODE
        FROM [dbo].[sales_data_sample] p
        WHERE ORDERNUMBER IN (
            SELECT ORDERNUMBER
            FROM (
                SELECT ORDERNUMBER, COUNT(*) rn
                FROM [PortfolioDB].[dbo].[sales_data_sample]
                WHERE STATUS = 'Shipped'
                GROUP BY ORDERNUMBER
            ) m
            WHERE rn = 2
        )
        AND p.ORDERNUMBER = s.ORDERNUMBER
        FOR XML PATH('')
    ), 1, 1, '') AS ProductCodes
FROM [dbo].[sales_data_sample] s
ORDER BY ProductCodes DESC;

````

## Recommendations
## Limitations
## References






  
