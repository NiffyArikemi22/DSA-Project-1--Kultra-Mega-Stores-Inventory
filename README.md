# DSA-Project-1--Kultra-Mega-Stores-Inventory
DSA Data Analysis Capstone Project
### Company Overview 
Kultra Mega Stores (KMS), headquartered in Lagos, specialises in office supplies and 
furniture. Its customer base includes individual consumers, small businesses (retail), and 
large corporate clients (wholesale) across Lagos, Nigeria.
### Dataset File
Excel File (folder containing the dataset) 
[Uploading KMS Sql Case Study Edited.csv…]()

### Goal of the Case Study
The goal is to analyze historical sales and order data (2009–2012) from KMS’s Abuja division and generate actionable business insights to support decision-making in areas such as:

Sales performance
Customer segmentation and profitability
Shipping cost optimization
Regional performance
Product and category analysis
The ultimate aim is to help increase revenue, reduce costs, and improve operational efficiency.
### Methodology
The following methodology using SQL and data analysis skills was applied
1. Data Understanding & Cleaning:
* Loaded and inspected the Excel dataset.
* Checked for missing values, duplicates, and data inconsistencies.
* Normalized where necessary (e.g., formatting of dates, removing trailing spaces in categories).
2. Exploratory Data Analysis (EDA):
* Grouped, aggregated, and filtered data using SQL queries to identify trends and patterns.
* Use key metrics such as total sales, number of orders, profits, costs, and customer segments.
3. Segmentation & Ranking:
* Rank customers, regions, and products based on performance.
* Segment by customer type (consumer, small business, corporate), region, and product categories.
4. Insights Generation:
* Drew conclusions for each question based on analysis.
* Compared high-performing vs. underperforming entities (e.g., customers, regions, products).
4. Recommendation:
* Provided evidence-based suggestions to improve revenue (e.g., focusing on top products, optimizing shipping methods, targeting specific customers).
### Data Structure
| Field Name        | Description                                                   |
| ----------------- | ------------------------------------------------------------- |
| `OrderID`         | Unique identifier for each order                              |
| `OrderDate`       | Date the order was placed                                     |
| `CustomerID`      | Unique customer identifier                                    |
| `CustomerName`    | Name of the customer                                          |
| `Segment`         | Consumer, Small Business, or Corporate                        |
| `Region`          | Geographical region (e.g., Ontario, Abuja)                    |
| `ProductCategory` | Category of the product (e.g., Appliances, Furniture)         |
| `ProductName`     | Specific product or item                                      |
| `Sales`           | Total sales amount (revenue)                                  |
| `Profit`          | Profit generated from the order                               |
| `Quantity`        | Number of units sold                                          |
| `ShippingCost`    | Cost incurred for shipping that order                         |
| `ShippingMethod`  | Delivery Truck, Express Air, etc.                             |
| `OrderPriority`   | High, Medium, Low, etc.                                       |
| `Returned`        | Flag/indicator showing whether the item was returned (Yes/No) |


#### Case Scenario I – Sales, Regions, Products, and Shipping
1. Which product category had the highest sales?

``` sql
Select Product_Category, sum(Sales) AS TotalSales
From KMS_Store
Group by Product_Category
Order by TotalSales desc

Select TOP 1 Product_Category, sum(Sales) AS TotalSales
From KMS_Store
Group by Product_Category
Order by TotalSales desc
```

2. Top 3 and Bottom 3 regions in terms of sales
``` sql
----Top 3 Region
Select Top 3 Region, Sum (Sales) AS TOP_3_SALES
From KMS_Store
Group by Region
Order by TOP_3_SALES desc

----Bottom 3 Region
Select Top 3 Region, Sum (Sales) AS Bottom_3_SALES
From KMS_Store
Group by Region
Order by Bottom_3_SALES ASC

Select Region, Sum (Sales) AS TOP_3_SALES
From KMS_Store
Group by Region
Order by TOP_3_SALES desc
```

3. Total sales of appliances in Ontario
``` SQL
Select Region, Sum (Sales) AS TotalSales
From KMS_Store
Where Region = 'Ontario'
Group by Region


Select Sum (Sales) AS TotalApplianceSales_In_Ontario
From KMS_Store
Where Product_Category = 'Appliances' And Region = 'Ontario'
```

4. Advice to increase revenue from bottom 10 customers
```SQL
----bottom 10 customers
Select TOP 10 Customer_Name, Product_Category, Product_Sub_Category, Sum (Sales) As TotalSales
From KMS_Store
Group by Customer_Name, Product_Category, Product_Sub_Category
Order by TotalSales ASC
```
Advice (based on query):
* Offer personalized discounts or bundled offers.
* Follow up with targeted marketing campaigns.
* Investigate if service/shipping issues led to low purchases.
* Cross-sell products frequently bought by similar high-value customers.

5. KMS incurred the most shipping cost using which shipping method?
``` SQL
Select Ship_Mode, Sum (Shipping_Cost) AS Most_Ship_Cost
From KMS_Store
Group by Ship_Mode
Order by MOst_Ship_Cost Desc

Select TOP 1 Ship_Mode, Sum (Shipping_Cost) AS Most_Ship_Cost
From KMS_Store
Group by Ship_Mode
Order by MOst_Ship_Cost Desc
```
Case Scenario II – Customer and Segment Analysis
6. Most valuable customers and what they purchase
```SQL
Select Customer_Name, Sum(Sales) As TotalSales
from KMS_Store
Group by Customer_Name
Order by TotalSales Desc

-----MOST PURCHASED PRODUCTS
Select Customer_Name, Product_Category, count(*)
AS MOST_PURCHASED_PRODUCTS
FROM KMS_Store
Group by Customer_Name, Product_Category
Order by Customer_Name,MOST_PURCHASED_PRODUCTS Desc

------TOP 10 MOST PURCHASED PRODUCTS
Select TOP 10 Customer_Name, Product_Category, count(*)
AS MOST_PURCHASED_PRODUCTS
FROM KMS_Store
Group by Customer_Name, Product_Category
Order by Customer_Name,MOST_PURCHASED_PRODUCTS Desc
```

7. Which small business customer had the highest sales?
```SQL
Select Customer_Name, Sum (Sales) As Totalsales
FROM KMS_Store
Where Customer_Segment = 'Small Business'
Group by Customer_Name
Order by Totalsales Desc
```

8. Corporate customer with most orders (2009–2012)
```SQL
Select Customer_Name, Count(Order_ID) AS OrderCount
From KMS_Store
Where Customer_Segment = 'Corporate'
And Order_Date Between '2009-01-01' And '2012-12-31'
Group by Customer_Name
Order by OrderCount Desc
```
9. Most profitable consumer customer
``` SQL
Select Customer_Name, sum(Profit) AS TotalProfit
From KMS_Store
Where Customer_Segment = 'Consumer'
Group by Customer_Name
Order by TotalProfit Desc
```

10. Which customer returned items, and what segment do they belong to?




11. Evaluate shipping method vs. Order Priority
 ``` SQL    
---Express Air is the fastest but the most expensive one, do you think the company 
--appropriately spent shipping costs based on the Order Priority

---a) Total Shipping Cost by Method and Priority
Select Ship_Mode, Order_Priority, Sum(Shipping_Cost) AS TotalCost
From KMS_Store
Group by Ship_Mode, Order_Priority
Order by Order_Priority, Ship_Mode


-----b. Recommendation
---If high-priority orders mostly use Express Air and low-priority orders use Delivery Truck, the spending is justified.

--If not:

--Suggest policy changes to align shipping cost with urgency.

--Use cheaper methods for standard/low priority unless otherwise justified.
```
