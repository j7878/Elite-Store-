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

  


