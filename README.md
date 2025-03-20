# Supermarket Sales Performance Analysis

This project focuses on conducting a comprehensive analysis of sales data from a retail chain to uncover insights into its operational performance, customer engagement, and product line profitability.

### Step one: Sales Performance Analysis by Branch
This SQL query groups the sales data by branch and calculates the total number of transactions, total sales, average sale per transaction, and total gross income for each branch, ordering the results by total sales in descending order. 

```sql
SELECT 
    Branch,
    COUNT(Invoice_ID) AS NumberOfTransactions,
    SUM(Total) AS TotalSales,
    AVG(Total) AS AverageSalePerTransaction,
    SUM(Gross_income) AS TotalGrossIncome
FROM supermarket_sales
GROUP BY Branch
ORDER BY TotalSales DESC;
```

##### Outcome

| Branch | NumberOfTransactions | TotalSales  | AverageSalePerTransaction | TotalGrossIncome |
|--------|----------------------|-------------|---------------------------|------------------|
| C      | 328                  | 110,568.71  | 337.10                    | 5,265.18         |
| A      | 340                  | 106,200.37  | 312.35                    | 5,057.16         |
| B      | 332                  | 106,197.67  | 319.87                    | 5,057.03         |


### Step two: Customer Engagement Analysis by Customer Type
This SQL query analyzes sales data by customer type to compare the number of transactions, total sales, and average sale per transaction between members and non-members.

```sql
SELECT 
    Customer_type,
    COUNT(invoice_id) AS NumberOfTransactions,
    SUM(Total) AS TotalSales,
    AVG(Total) AS AverageSalePerTransaction
FROM supermarket_sales
GROUP BY Customer_type;
```


##### Outcome

| Customer Type | NumberOfTransactions | TotalSales  | AverageSalePerTransaction |
|---------------|----------------------|-------------|---------------------------|
| Member        | 501                  | 164,223.44  | 327.79                    |
| Normal        | 499                  | 158,743.31  | 318.12                    |

### Step three: Analyzing Top-Performing Product Lines
This analysis aims to identify the top-performing product lines based on gross income. By utilizing a subquery, we will calculate the average gross income for each product line and then select the product lines that have a gross income above the overall average.


```sql
SELECT Product_line, AVG(gross_income) AS AverageGrossIncome
FROM supermarket_sales
GROUP BY Product_line
HAVING AVG(gross_income) > (
  SELECT AVG(gross_income) FROM supermarket_sales
)
ORDER BY AverageGrossIncome DESC;
```
##### Outcome


| Product Line        | AverageGrossIncome |
|---------------------|--------------------|
| Home and lifestyle  | 16.03              |
| Sports and travel   | 15.81              |
| Health and beauty   | 15.41              |

Here's a breakdown:

- `AVG(gross_income)`: Calculates the average gross income for each product line.
- `SELECT AVG(gross_income) FROM ssupermarket_sales`: Calculates the overall average gross income across all product lines and transactions in the dataset.
- `HAVING AVG("gross_income) > (...)`: Filters the results to include only those product lines where the average gross income is greater than the overall average gross income calculated for the entire dataset.


### Step four: Comparing Sales by Gender in Different Cities
This task focuses on comparing sales performance by gender across different cities where the retail chain operates. By joining the sales data with a city table (assuming a separate city table exists or using the same sales data table for simplicity), we can analyze how sales to male and female customers vary by city. This analysis provides insights into gender-specific shopping patterns and preferences in different geographical locations.

```sql
SELECT 
    s.City, 
    s.Gender, 
    COUNT(s.Invoice_ID) AS NumberOfTransactions,
    SUM(s.Total) AS TotalSales
FROM supermarket_sales s
JOIN (SELECT DISTINCT City FROM supermarket_sales) c ON s.City = c.City
GROUP BY s.City, s.Gender
ORDER BY s.City, TotalSales DESC;
```
##### Outcome

| City       | Gender | NumberOfTransactions | TotalSales  |
|------------|--------|----------------------|-------------|
| Mandalay   | Male   | 170                  | 53,269.38   |
| Mandalay   | Female | 162                  | 52,928.30   |
| Naypyitaw  | Female | 178                  | 61,685.46   |
| Naypyitaw  | Male   | 150                  | 48,883.24   |
| Yangon     | Female | 161                  | 53,269.17   |
| Yangon     | Male   | 179                  | 52,931.20   |


##### Conclusion
The consolidated insights reveal that Branch C leads in total sales, demonstrating its superior performance in revenue generation among the branches. The analysis indicates that members engage more frequently with the retail chain than non-members, as seen in the higher number of transactions. However, non-members show slightly lower average spending per transaction. The "Home and lifestyle" product line emerges as the most profitable, suggesting consumer preference and potential areas for inventory expansion. Notably, female customers in Naypyitaw contribute significantly to sales, highlighting geographical and gender-specific purchasing patterns. This comprehensive analysis underscores the importance of targeted strategies for customer engagement, inventory management, and regional marketing to support the retail chain's overall performance.

![Sales Comparison by Gender Across Cities](https://raw.githubusercontent.com/Baharkeen/sales-performance-analysis/main/assets/sales_comparison_by_gender_across_cities.png?raw=True)

![Sales Distribution by Product Line](https://github.com/Baharkeen/sales-performance-analysis/blob/main/assets/sales_distribution_by_product_line.png?raw=True)

![Sales Performance by Branch](https://github.com/Baharkeen/sales-performance-analysis/blob/main/assets/sales_performance_by_branch.png?raw=True)