# Bank Customer Churn Analysis & Dashboard

End-to-end churn analysis for a retail bank (France, Spain, Germany): messy data -> cleaned data → KPIs → insights → Power BI design → interactive Power BI dashboard.

## Business problem

Which customers are leaving, why, and how much money walks out with them? The goal is to find the segments that drive churn and give the retention team a prioritised call list.

## Key findings

Overall churn - **20.4%** (2,037 of 10,000 customers) 
Balance held by churned customers - **€185.6M** (24.3% of all balances) 
Germany - 32.4% churn vs about 16% in France and Spain; 40% of all churners; 52.8% of the balance at risk 
Age 46-55 / 56-65 - 50.6% / 48.3% churn 
Number of products  1 -> 27.7%, **2 -> 7.6%**, 3 -> 82.7%, 4 -> 100% 
Inactive vs active members - 26.9% vs 14.3% 
Credit score, salary, tenure - Almost no effect 
Still-active customers at high or medium risk - 1,325 holding €130.9M, 66% of them in Germany 

## Data

File -> What it is 

"data/raw/Bank_Churn_Data_Messy.xlsx" - Original data: sheets "Customer_Info" and "Account_Info" 
"data/raw/Bank_Churn_Data_Dictionary.csv" -> Column definitions 
"data/cleaned/Bank_Churn_Cleaned.csv/.xlsx" - Cleaned table: 10,000 rows x 24 columns (the ".xlsx" also has a cleaning log)



### Data problems found and fixed

- 3 duplicate rows removed (1 in "Customer_Info", 2 in "Account_Info")
- Geography written 5 ways (FRA / French / France) -> standardised to France, Spain, Germany (3,273 rows)
- Salary and Balance stored as text with a € sign → converted to numbers
- 3 salaries holding the placeholder "-999999" -> set to blank and flagged
- 3 customers missing Surname and Age -> kept, Surname = "Unknown", Age blank and flagged
- Yes/No columns converted to 1/0; duplicated "Tenure" column removed
- "HasCrCard" is identical to "IsActiveMember" on every row (probable source error) -> excluded from analysis


## Project structure


bank-churn-dashboard/
├── README.md
├── LICENSE
├── requirements.txt
├── data/
│   ├── raw/            original messy workbook + data dictionary
│   └── cleaned/        cleaned CSV and Excel
├── power-query/        Step by step guide for data cleaning 
├── powerbi/
│   ├── theme/          Power BI theme JSON (light and dark)
│   └── backgrounds/    page backgrounds (PNG)
├── images/             screenshots used in this README



## Dashboards

Power BI dashboard -> bank_churn_dashboard_ : three tabs (Overview, Light Versions, Drivers), clickable filters


## Tools

 Power Query
 Excel
 DAX 
 Power BI 


## Author

**Md. Omor Faruk** · [LinkedIn](https://linkedin.com/in/omorfarukmd)

Licensed under the [MIT License](LICENSE).
