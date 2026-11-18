Power Query: cleaning the Bank Churn data 

Works the same in Power BI Desktop and Excel (Data > Get Data). Menu names below are Power BI Desktop's.

Target result: one table, "BankChurn_Cleaned", with 10,000 rows, 24 columns and a 20.37% churn rate.



A. Load the file

1. Home > Get data > Excel workbook and pick "Bank_Churn_Data Messy.xlsx".
2. In the Navigator tick "Customer_Info" and "Account_Info", then click "Transform Data".
3. In the left "Queries" pane, rename (right-click > Rename): "Customer_Info" to "Customers", "Account_Info" -> "Accounts".



B. Clean the "Customers" query -> Steps


1. Headers -> Use First Row as Headers
2. Drop stray empty columns -> Click the "CustomerId" header, then Ctrl+click the other 7 real columns (Surname, CreditScore, Geography, Gender, Age, Tenure, EstimatedSalary). Right-click a selected header > "Remove Other Columns".
3. Remove duplicates -> Click the "CustomerId" header > right-click > "Remove Duplicates". (1 row goes.) 
4. Clean Geography text -> Click "Geography" > Transform > Format > Trim. 
5. Fix France (part 1) -> Select "Geography" > Home > Replace Values. Value To Find: "FRA". Replace With: "France". Open Advanced options and tick "Match entire cell contents" > OK. 
6. Fix France (part 2) -> Repeat: Value To Find "French", Replace With "France", Match entire cell contents ticked > OK. 
7. Salary: remove euro sign -> Select EstimatedSalary > Replace Values. Value To Find: "euro sign". Leave Replace With "empty" > OK. 
8. Salary: make it a number -> Click the "ABC" icon at the left of the EstimatedSalary header > Decimal Number. 
9. Flag bad salaries -> Add Column > Conditional Column. New column name: "SalaryMissingFlag". If EstimatedSalary is <= "0" Then "1", Else "0" > OK. Set its type to Whole Number.
10. Blank out the -999999 -> Select EstimatedSalary > Replace Values. Value To Find: "-999999". Replace With: "null" > OK. (3 rows.)
11. Flag missing Age -> Add Column > Conditional Column**. Name: "AgeMissingFlag". If Age = "null" Then "1", Else "0" > OK. Whole Number.
12. Missing Surname -> Select Surname > Replace Values. Value To Find: "null". Replace With: "Unknown" > OK.
13. Set data types -> CustomerId, CreditScore, Age, Tenure = Whole Number. Surname, Geography, Gender = Text.


C. Clean the "Accounts" query


1. Keep only needed columns | Click CustomerId, Ctrl+click Balance, NumOfProducts, HasCrCard, IsActiveMember, Exited. Right-click > Remove Other Columns. This drops the duplicate Tenure column. 
2. Remove duplicates -> Click CustomerId > right-click > Remove Duplicates. (2 rows go.) 
3. Balance: remove euro sign -> Select Balance > Replace Values: find "€", replace with empty > OK. 
4. Balance: number -> Decimal Number (using the same locale fallback as above if needed). 
5. Yes/No to 1/0 -> Select HasCrCard and IsActiveMember together (Ctrl+click). Replace Values: find "Yes", replace with "1", tick Match entire cell contents > OK. Repeat: find "No", replace with "0", same tick > OK.
6. Set data types -> CustomerId, NumOfProducts, HasCrCard, IsActiveMember, Exited = Whole Number.


D. Join the two tables

1. Home > Merge Queries > Merge Queries as New.
2. Top table: Customers, click its CustomerId column. Bottom table: Accounts, click its CustomerId column.
3. Join Kind: Inner (only matching rows) > OK. A new query appears, called Merge1.
4. Click the expand icon (two arrows) on the "Accounts" column header. Untick CustomerId and untick "Use original column name as prefix", leave the rest ticked > OK.
5. Rename the query to BankChurn_Cleaned.
6. Click the CustomerId header > Home > Sort Ascending



E. Add analysis columns (inside "BankChurn_Cleaned")

For each row of the table below: Add Column > Conditional Column, type the name, then Add Clause for each line. 

"AgeGroup" (Text)
If Age 
equals “null” Unknown

is less than or equal to 25 -> 18-25

is less than or equal to 35 -> 26-35

is less than or equal to 45 -> 36-45

is less than or equal to 55 -> 46-55

is less than or equal to 65 -> 56-65

Else -> 66+


"AgeGroupOrder" (Whole Number): same rules, outputs 7, 1, 2, 3, 4, 5, Else 6. This lets Power BI sort the age bands in logical order.

"CreditScoreBand" (Text)

If CreditScore -> Then

is less than or equal to 579 Then Poor (<580)

is less than or equal to 669 Then Fair (580-669)

is less than or equal to 739 Then Good (670-739)

is less than or equal to 799 Then Very Good (740-799)

Else -> Exceptional (800+)

"CreditBandOrder" (Whole Number): same rules, outputs 1, 2, 3, 4, Else 5.

"BalanceBand" (Text)

If Balance

equals 0 Then Zero

is less than or equal to 50000 Then 1-50K

is less than or equal to 100000 Then 50-100K

is less than or equal to 150000 Then 100-150K

Else -> 150K+

"BalanceBandOrder" (Whole Number): same rules, outputs 1, 2, 3, 4, Else 5.

"HasBalance" (Whole Number): If Balance is greater than 0 Then 1, Else 0.

"ChurnStatus" (Text): If Exited equals 1 Then Churned, Else Retained.

"ActiveStatus" (Text): If IsActiveMember equals 1 Then Active, Else Inactive.

Optional: drag column headers into the order CustomerId, Surname, CreditScore, CreditScoreBand, CreditBandOrder, Geography, Gender, Age, AgeGroup, AgeGroupOrder, Tenure, Balance, BalanceBand, BalanceBandOrder, HasBalance, NumOfProducts, HasCrCard, IsActiveMember, ActiveStatus, EstimatedSalary, Exited, ChurnStatus, AgeMissingFlag, SalaryMissingFlag.



F. Load only the final table

1. In the Queries pane, right-click "Customers" > untick Enable load. 
Do the same for "Accounts". (They are staging steps only.)
2. Home > Close & Apply.



G. Check the data 

In Power Query go to View and tick Column quality, Column distribution and Column profile, then check "BankChurn_Cleaned":


Row count (status bar at the bottom) -> 10,000 
CustomerId distinct / unique -> 10,000 / 10,000 
Geography distinct values -> 3 (France, Germany, Spain) 
Errors in any column -> 0 
Empty values -> Age: 3, EstimatedSalary: 3, 
AgeMissingFlag sum / SalaryMissingFlag sum -> 3 / 3 