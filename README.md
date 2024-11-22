# RFM ANALYSIS AND DATA EXPLORATION
***
# Global Vehicle Traders Inc.

![Global business logistics import export of containers cargo freight ship loading at port by crane container transport cargo plane truck to port background _ Premium AI-generated image](https://github.com/user-attachments/assets/9dd80c90-eb2d-4482-87c4-15e175dec564)




## 🧾 Table of Contents
- [Business Task](#business-task)
- [Methodology](#methodology)
- [Data Exploration](#data-exploration)
- [Visualization](#visualization)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)
***


## Business Task

### Global Vehicle Traders Inc.


Global Vehicle Traders Inc. is a dynamic multi-vehicle trading company that excels in acquiring, selling, and occasionally restoring a diverse range of vehicles. The Marketing Department seeks to broaden its market reach and execute a significant sales campaign. The marketing manager seeks your expertise to analyze and address the following objectives:

1. Identify Top Customers: Determine the most valuable and frequent buyers to tailor marketing strategies effectively.

2. Determine the Two Most Frequently Purchased Product Pairings: Analyze sales data to uncover popular product combinations, aiding in promotional efforts.

3. Perform Customer Segmentation: Categorize customers into distinct groups based on behaviors and preferences, facilitating targeted marketing and personalized experiences.

Your insights will be instrumental in driving the success of this campaign.

## Data Source
Sales data: The primary data used for this analysis is the "salesdata.csv" file, containing detailed information about each sale made by the company
[data source link](https://github.com/grandady/salesdata_Exploration/blob/main/sales_data_sample.csv)


## Methodology


- Data Importation: Importing datasets into the SQL Server Database for comprehensive analysis.

- Exploring Sales Data: Generate various analytics and insights based on customers' past purchase behavior.

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

  Upon careful examination, this dataset comprises 2,823 rows and 24 columns of records.
  
  ### Column Dictionary

1. Order Number: A unique identifier for each customer order. Essential for tracking and managing orders.

2. Quantity Ordered: The number of units purchased in an order. Helps in inventory management and sales analysis.

3. Price Each: The price per unit of the product sold. It is important for calculating total sales and revenue.

4. Orderline Number: Sequence number of items in a particular order, which helps organize multi-item orders.

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




### Exploration Data Analysis

[link to sql file](https://github.com/grandady/salesdata_Exploration/blob/main/RFM_Segmentation_Sales_Analysis_Main.sql)


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


![year](https://github.com/user-attachments/assets/9c6950b8-319c-48d8-afaa-878e6a0ec6c9)

- From 2003 to 2004, there was a steady increase in revenue, growing from approximately 3,516,980 in 2003 to around 4,724,163 in 2004. However, in 2005, revenue experienced a significant dip to roughly 1,791,487 due to incomplete data, as the records only include up to the fifth month of the year







**Step 2. Which month had the highest sales within a given year? What was the total revenue for that month?**

Objective: determine the peak sales period and quantify the revenue for that month. It helps identify when your business performed best within the year

````sql
SELECT MONTH_ID, SUM(sales) AS Revenue, COUNT(ORDERNUMBER) AS Frequency
FROM [dbo].[sales_data_sample]
WHERE YEAR_ID = 2004  -- Change the year to see the rest
GROUP BY MONTH_ID
ORDER BY Revenue DESC;

````

**Answer:**


![highest month](https://github.com/user-attachments/assets/ea0a027b-2902-4fd7-abb4-bd06387e773d)


-  In both 2003 and 2004, November achieved the highest sales, nearly doubling the revenue of the second-best month, October. Data from 2005 was excluded due to incomplete records, as the year only included data up to May.








**Step3. What products sell most in November?**

Objective: identify the top-selling products in November, which can help businesses optimize their inventory and marketing strategies for that month

````sql
SELECT MONTH_ID, PRODUCTLINE, SUM(sales) AS Revenue, COUNT(ORDERNUMBER) AS OrderCount
FROM [dbo].[sales_data_sample]
WHERE YEAR_ID = 2004 AND MONTH_ID = 11  --change year to see the rest
GROUP BY MONTH_ID, PRODUCTLINE
ORDER BY Revenue DESC;

````
**Answer:**

![most november sales](https://github.com/user-attachments/assets/e61c20f6-c539-4572-abdf-d185c3ab2695)

- In 2003 and 2004, classic cars recorded the highest sales, nearly doubling the revenue generated by vintage cars, which came in second. Data for 2005 has been excluded due to the year being incomplete.






**Step 4. Grouping by PRODUCTLINE and sorting by revenue**
Objective: identify which product lines are the most profitable. By grouping sales data by product line and then sorting by revenue

````sql
SELECT PRODUCTLINE, SUM(sales) AS Revenue
FROM [dbo].[sales_data_sample]
GROUP BY PRODUCTLINE
ORDER BY Revenue DESC;

````
**Answer:**

![Productline](https://github.com/user-attachments/assets/33534fc1-b48a-4ef2-8532-26b3f9cb5a28)


- Classic cars generated the highest revenue, amounting to approximately 3,919,616, which is double that of the second-highest revenue from vintage cars at around 1,903,151. The lowest revenue was from trains, totaling approximately 226,243.






**Step 5. Grouping by CITY and sorting by revenue**

Objective: identify which cities are the most profitable. By grouping sales data by city and then sorting by revenue

````sql
SELECT city, SUM(sales) AS Revenue
FROM [PortfolioDB].[dbo].[sales_data_sample]
GROUP BY city
ORDER BY Revenue DESC;

````

**Answer:**

_This screenshot is only for reference and doesn’t contain all entries due to the issue of space_


![city](https://github.com/user-attachments/assets/26c5b807-a581-409e-862a-6b403785a93f)


- There were 73 countries analyzed. The top three highest revenue-generating cities were Madrid, leading with approximately 1,082,551, followed by San Rafael with around 654,858, and a third city generating approximately 560,788. Charleroi recorded the lowest revenue at approximately 33,440.







**Step 6. Grouping by DEAlSIZE and sorting by revenue**

Objective: identify which Deal sizes are the most profitable. By grouping sales data by Deal size and then sorting by revenue

````sql
SELECT DEALSIZE, SUM(sales) AS Revenue
FROM [PortfolioDB].[dbo].[sales_data_sample]
GROUP BY DEALSIZE
ORDER BY Revenue DESC;

````
**Answer:**


![dealsize](https://github.com/user-attachments/assets/cf2006bf-ed41-4526-b0e9-991584e95b2d)


- The medium deal size generated a revenue of approximately 6,087,432, which is three times the revenue generated by the small deal size at around 2,643,077. The least revenue was generated by the large deal size, amounting to approximately 1,302,119.





**Step 7. Grouping by TERRITORY and sorting by revenue**

Objective: identify which Territories are the most profitable. By grouping sales data by Territory and then sorting by revenue

````sql
SELECT TERRITORY, SUM(sales) AS Revenue
FROM [dbo].[sales_data_sample]
GROUP BY TERRITORY
ORDER BY Revenue DESC;

````

**Answer:**


![territory](https://github.com/user-attachments/assets/257d0a20-bfc5-4fa2-b999-88aafe28e1cc)


- The EMEA region generated the highest revenue, amounting to approximately 4,979,272. The second highest revenue was generated by a territory with approximately 3,852,061, while Japan recorded the least revenue at approximately 455,173.






**Step 8. Grouping by STATUS and sorting by revenue**

Objective: identify which statutes are the most profitable. By grouping sales data by Status and then sorting by revenue

````sql
SELECT STATUS, SUM(sales) AS Revenue
FROM [PortfolioDB].[dbo].[sales_data_sample]
GROUP BY STATUS
ORDER BY Revenue DESC;

````

**Answer:**

![status](https://github.com/user-attachments/assets/46be9a01-bd78-4656-b67e-a84392d90321)


- The shipped status generated a remarkable revenue of approximately 9,291,501, significantly higher than the cancelled status, which was the second highest at around 194,487. The lowest revenue was from the disputed status, totaling approximately 72,213.





**Step 10. Top Customers**

Objective: to figure out which customers are contributing the most to the business's revenue, Knowing your top customers helps you focus on strengthening those relationships, providing them with tailored offers, and boosting loyalty.

````sql
SELECT customername, 
       SUM(sales) AS TotalSales, 
       COUNT(ordernumber) AS PurchaseFrequency
FROM [dbo].[sales_data_sample]
GROUP BY customername
ORDER BY TotalSales DESC, PurchaseFrequency DESC;

````
**Answer:**

_This screenshot is only for reference and doesn’t contain all entries due to the issue of space_

![top 10 customer](https://github.com/user-attachments/assets/769daddd-9a3c-4762-826c-6451c3d96e4b)


- Out of 92 customers in total, the top three who generated the highest revenue and made the most purchases were: Euro Shopping Channel with total sales of approximately 912,294 and a purchasing frequency of 259; Mini Gifts Distributors Ltd., generating around 654,858 in total sales with a purchasing frequency of 180; and Australian Collectors, Co., generating three times less revenue than the second top customer at approximately 200,995, with a purchasing frequency more than three times lower than that of Mini Gifts Distributors Ltd.







**Step 11. Customer Segmentation**

Objective: Segmenting customers based on shared characteristics such as buying behavior, demographics, or preferences enables businesses to customize marketing efforts, enhance customer service, and develop products that better cater to each segment's needs. The segments were divided into six categories: Lost Customers, Slipping Away, Can't Lose, New Customers, Potential Churners, and Active and Loyal.


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
**Answer:**


_This screenshot is only for reference and doesn’t contain all entries due to the issue of space_


![customer segmentation](https://github.com/user-attachments/assets/d1d5c7d7-5d7f-46f2-b4a3-52f53b3bf480)





**Step 12. The Two Most Frequently Purchased Product Pairings**

Objective: It's about uncovering which products are often bought together. This information can help with cross-selling strategies, enhancing product recommendations, and improving store layouts.

````sql
SELECT
 DISTINCT OrderNumber, 
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

**Answer:**

_This screenshot is only for reference and doesn’t contain all entries due to the issue of space_

![Product pairing](https://github.com/user-attachments/assets/20b6cb0c-57b9-4d68-8125-6f46137b6077)


- A total of 19 product pairings were ordered, with the most frequent pairings being S18_1342 and S18_1367, as well as S18_2325 and S24_1937.


  ## Visualization
  

  ![viz 1](https://github.com/user-attachments/assets/365ff51b-ce72-4882-b430-fa86ce2c1e4d)
**This dashboard provides a comprehensive overview of sales performance**
[overview dashboard link](https://public.tableau.com/app/profile/kosemani.babajide/viz/GlobalVehicle1/OVERVIEW)
  





    ![viz 2](https://github.com/user-attachments/assets/0445c551-1403-4424-8b80-2352fd89e10b)
  **This dashboard highlights our top and bottom customers**
[dashboard link](https://public.tableau.com/app/profile/kosemani.babajide/viz/GlobalVehicle2/NEXT)



## Recommendations


1. Leverage Top Customers
Recommendation: Develop loyalty programs and personalized marketing campaigns targeting your top customers. They generate the most revenue and have the highest purchase frequency, so keeping them engaged is crucial.

Action: Implement exclusive offers, early access to sales, and personalized communication to maintain their loyalty.

2. Optimize Product Pairings
Recommendation: Create promotional bundles for the most frequently purchased product pairings.

Action: Offer discounts on bundled products S18_1342 & S18_1367 and S18_2325 & S24_1937 to encourage customers to purchase them together.

3. Enhance Customer Segmentation
Recommendation: Tailor marketing strategies based on customer segments such as Lost Customers, Slipping Away, Can't Lose, New Customers, Potential Churners, and Active and Loyal.

Action: Develop specific campaigns for each segment, like win-back strategies for Lost Customers or reward programs for Loyal Customers.

4. Focus on High-Performing Regions
Recommendation: Strengthen marketing efforts in regions with the highest revenue, such as EMEA.

Action: Allocate more resources to these regions and analyze what contributes to their success to replicate it in other areas.

5. Address Low-Performing Areas
Recommendation: Investigate why certain areas, like Japan, have lower sales.

Action: Conduct market research to understand barriers and adjust your strategies accordingly.

6. Analyze Sales Trends
Recommendation: Pay attention to the sales trends, such as the peak in November, to plan inventory and marketing campaigns.

Action: Prepare for increased demand in peak months and leverage this period for major promotional activities.

7. Monitor Deal Sizes
Recommendation: Encourage medium and large deal sizes by offering incentives or discounts for bulk purchases.

Action: Implement tiered pricing strategies to encourage customers to increase their order sizes.

8. Improve Order Status Management
Recommendation: Focus on maintaining a high shipped status while addressing issues that lead to cancellations and disputes.

Action: Enhance customer support and streamline the order fulfillment process to reduce cancelled and disputed orders.

These steps should help maximize revenue, improve customer satisfaction, and optimize marketing strategies.


## Limitations

- Incomplete Data: The dataset for 2005 is incomplete, potentially leading to skewed results and inaccurate comparisons with other years.

- Data Quality: The accuracy of the analysis relies heavily on the quality of the data. Missing values, inaccuracies, or inconsistencies can impact the findings.

- Segmentation Boundaries: The choice of segmentation criteria (e.g., RFM scoring) may not capture all nuances of customer behavior, potentially oversimplifying complex patterns.

- Assumptions in Pairings: The analysis of frequently purchased product pairings assumes that all recorded purchases are relevant, without considering seasonal or promotional factors.

- External Factors: The analysis doesn't account for external factors (e.g., economic changes, competitor actions) that may have influenced sales trends.

- Limited Time Frame: With the analysis mainly focused on a few years, long-term trends and patterns might be missed.

- Specific Variables: The focus on specific columns (e.g., sales, order dates) may overlook other important variables (e.g., customer feedback, product returns) that could provide deeper insights.



## References

- Data Science for Business" by Foster Provost and Tom Fawcett
- SQL for Data Analysis" by Cathy Tanimura"Customer Segmentation and Clustering Using SAS Enterprise Miner" by Randall S. Collica
- Marketing Metrics: The Definitive Guide to Measuring Marketing Performance" by Neil T. Bendle, Paul W. Farris, Phillip E. Pfeifer, and David J. Reibstein
- Competing on Analytics: Updated, with a New Introduction: The New Science of Winning" by Thomas H. Davenport and Jeanne G. Harris
- Predictive Analytics: The Power to Predict Who Will Click, Buy, Lie, or Die" by Eric Siegel







  
