# Elite-Store- Four-Year Performance Analysis of the Haulage Department (2009-2012): A Data Analytics Perspective

### Project Overview


Elite Stores, a leading retailer based in Canada, aims to optimize its haulage department's performance from 2009 to 2012. Challenges include limited revenue visibility, customer focus issues, inefficient order handling, pricing strategy gaps, and a lack of regional performance insights. The project involves comprehensive data analysis to address these challenges, improve operational efficiency, and drive strategic decision-making, ultimately enhancing customer satisfaction and profitability.

[VIEW DASHBOARD](https://public.tableau.com/views/Elitestoreportfolioproject/Dashboard1?:language=en-GB&:sid=BFADC53B5D4245B29F1127889DF0690C-0:0&:display_count=n&:origin=viz_share_link)

![Dashboard 1 (2)](https://github.com/j7878/Elite-Store-/assets/58298723/4a1047a3-27ad-4176-8073-ac1a58100dfd)

### Tools

- Excel - Data Cleaning
- MYSQL - Data Analysis
- Tableau - Creating reports

    ### Data cleaning and Preparation

1. Data loadiing and inspection
2. Handling missing values.
3. Data cleaning and formatting.


  ### Exploratory Data Analysis


  - Total sales and profit

  ```sql
SELECT 
    YEAR([Order Date]) AS Order_Year, 
    ROUND(SUM([Sales]), 2) AS Total_Sales,
    ROUND(SUM([Profit]), 2) AS Total_Profit
FROM 
    [dbo].[Orders$]
WHERE 
    [Order Date] IS NOT NULL AND [Sales] IS NOT NULL AND [Profit] IS NOT NULL
GROUP BY 
    YEAR([Order Date])
ORDER BY 
    Order_Year;
 ```
Total number of received and returned goods for the past 4 years

 ```sql

SELECT 
    COALESCE(r.[Status], 'Received') AS Status,
    COUNT(r.[Order ID]) + (COUNT(o.[Order ID]) - COUNT(r.[Order ID])) AS Product_units
FROM 
    [dbo].[Orders$] o
LEFT JOIN 
    [dbo].[Returns$] r ON o.[Order ID] = r.[Order ID]
GROUP BY 
    COALESCE(r.[Status], 'Received');
 ```
product without discount for 4 years

 ```sql

SELECT *
	FROM [dbo].[Orders$] 
WHERE[Discount]= 0;
 ```
regions with highest sales

 ```sql

SELECT TOP 8
    Province,
    ROUND(SUM(Sales), 2) AS Total_sales
FROM 
    [dbo].[Orders$]
GROUP BY
  Province
ORDER BY 
    Total_sales DESC;
 ```
Total sales for each product ordered
 ```sql

SELECT 
    [Product Sub-Category],
    [Order Quantity],
    [Unit Price],
    [Shipping Cost],
    [Discount],
    ROUND(([Order Quantity] * [Unit Price] + [Shipping Cost]) - ([Order Quantity] * [Unit Price] + [Shipping Cost]) * [Discount], 2) AS Total_sales
FROM 
    [dbo].[Orders$];
 ```

method of delivery that spent the most money

 ```sql

SELECT 
    [Ship Mode] AS Delivery_Method,
    ROUND(AVG([Shipping Cost]), 2) AS Average_Shipping_Cost
FROM 
    [dbo].[Orders$]
WHERE  [Shipping Cost] IS NOT NULL
GROUP BY 
    [Ship Mode]
ORDER BY 
    Average_Shipping_Cost DESC;
 ```

most affordable method of delivery and top customers

```sql

WITH DeliveryCosts AS (
    SELECT 
       [Ship Mode], 
        SUM([Shipping Cost]) AS total_shipping_cost
    FROM 
        [dbo].[Orders$]
    GROUP BY 
        [Ship Mode]
),
TopCustomers AS (
    SELECT TOP 1
        [Customer Name],  
        SUM([Unit Price]) AS total_expenses
    FROM      [dbo].[Orders$]  
  GROUP BY 
        [Customer Name]
    ORDER BY 
        total_expenses DESC
)
SELECT 
    dc.[Ship Mode] AS Most_Affordable_Delivery_Method,
    tc.[Customer Name] AS Top_Customer,
    tc.total_expenses AS Total_Expenses
FROM 
    DeliveryCosts dc
CROSS JOIN
    TopCustomers tc
WHERE 
    dc.total_shipping_cost = (SELECT MIN(total_shipping_cost) FROM DeliveryCosts);
```







  


