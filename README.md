# Credit_Card_Financial_Dashboard

**Project Objective:** To develop a comprehensive credit card weekly dashboard that provides real-time insights into key performance metrics and trends, enabling stakeholders to monitor and analyze credit card operations effectively.

**Import Data to SQL Database**:
  - Extracted relevant data from a SQL database using optimized SELECT statements to efficiently import data into Power BI.
  - Leveraged Power Query Editor to cleanse and transform raw data from the SQL database, including data type conversions and null value handling.
  - Created calculated columns within Power BI to generate additional insights from the imported SQL data.
  - Successfully connected to a SQL Server database using ODBC connection to import large datasets into Power BI for analysis.

**Project Insights(Success Citriea)- Week 53 (31st Dec)**

Week Over Week (WoW) change:
  - Revenue increased by 28.8%,
  - Total Transaction Amt & Count increased by xx% & xx%
  - Customer count increased by xx%

Overview YTD:
 - Overall revenue is 57M
 - Total interest is 8M
 - Total transaction amount is 46M
 - Male customers are contributing more in revenue 31M, female 26M
 - Blue & Silver credit card are contributing to 93% of overall transactions
 - TX, NY & CA is contributing to 68%
 - Overall Activation rate is 57.5%
 - Overall Delinquent rate is 6.06%

**Data Analysis Expressions(DAX)**:

**1) Conducted an age-based segmentation analysis of credit card users to gain insights into the demographic characteristics of this consumer group.**
```
AgeGroup = SWITCH(
 TRUE(),
 'public cust_detail'[customer_age] < 30, "20-30",
 'public cust_detail'[customer_age] >= 30 && 'public cust_detail'[customer_age] < 40, "30-40",
 'public cust_detail'[customer_age] >= 40 && 'public cust_detail'[customer_age] < 50, "40-50",
 'public cust_detail'[customer_age] >= 50 && 'public cust_detail'[customer_age] < 60, "50-60",
 'public cust_detail'[customer_age] >= 60, "60
 "unknown"
 )
```

**2) Developed and implemented an income-based segmentation model using the SWITCH function to categorize customers into three distinct groups: Low, Med, and High.**

**- Created a new column IncomeGroup to classify customers based on their income levels, with specific thresholds for each group** 
```
IncomeGroup = SWITCH(
 TRUE(),
 'public cust_detail'[income] < 35000, "Low",
 'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] <70000, "Med",
 'public cust_detail'[income] >= 70000, "High",
 "unknown"
 )
```
**3) Utilized the WEEKNUM function to extract the week number from a date column (week_start_date) in the cc_detail table, creating a new column week_num2 to facilitate date-based analysis and reporting.**

```week_num2 = WEEKNUM('public cc_detail'[week_start_date]```

**4)Designed and implemented a dynamic revenue calculation measure Current_week_Revenue using DAX, which leverages the CALCULATE and FILTER functions to sum up revenue for the current week, by filtering the entire dataset to only include records with the maximum week number (week_num2), ensuring accurate and up-to-date revenue reporting.** 

```
Current_week_Reveneue = CALCULATE(
 SUM('public cc_detail'[Revenue]),
 FILTER(
 ALL('public cc_detail'),
 'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])))
```

**5) Developed a measure Previous_week_Revenue to calculate the revenue for the previous week.**

```
Previous_week_Reveneue = CALCULATE(
 SUM('public cc_detail'[Revenue]),
 FILTER(
 ALL('public cc_detail'),
 'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])-1))
```
