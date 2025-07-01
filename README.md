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


``` sql
Create Database DSA_KMS

Select * from [KMS_Store ]

----CASE SCENARIO 1
-----QUE1..WHICH PRODUCT CATEGORY HAD THE HIGHEST SALES
Select Product_Category, sum(Sales) AS TotalSales
From KMS_Store
Group by Product_Category
Order by TotalSales desc

Select TOP 1 Product_Category, sum(Sales) AS TotalSales
From KMS_Store
Group by Product_Category
Order by TotalSales desc


---QUE2...WHAT ARE THE TOP 3 AND BOTTOM 3 REGION IN TERM OF SALES
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


----QUE3...WHAT WERE THE TOTAL SALES OF APPLIANCE IN ONTARIO
Select Region, Sum (Sales) AS TotalSales
From KMS_Store
Where Region = 'Ontario'
Group by Region


Select Sum (Sales) AS TotalApplianceSales_In_Ontario
From KMS_Store
Where Product_Category = 'Appliances' And Region = 'Ontario'


----Ques 4...Advise the management of KMS on what to do to increase the revenue from the 
----bottom 10 customers
Select TOP 10 Customer_Name, Product_Category, Product_Sub_Category, Sum (Sales) As TotalSales
From KMS_Store
Group by Customer_Name, Product_Category, Product_Sub_Category
Order by TotalSales ASC

-- Recommendations:
--1) Offer targeted promotions or loyalty rewards.

--2) Analyze their purchasing patterns and suggest suitable bundles.

--3) Improve delivery time or customer experience.

---4) Upsell higher-margin items they haven�t tried yet.

-----Ques5... KMS Incurred the most Shipping Cost using which shipping method
Select Ship_Mode, Sum (Shipping_Cost) AS Most_Ship_Cost
From KMS_Store
Group by Ship_Mode
Order by MOst_Ship_Cost Desc

Select TOP 1 Ship_Mode, Sum (Shipping_Cost) AS Most_Ship_Cost
From KMS_Store
Group by Ship_Mode
Order by MOst_Ship_Cost Desc

----CASE SCENARIO II
----QUE6...WHO ARE THE MOST VALUABLE CUSTOMERS, AND WHAT PRODUCTS OR SERVICE DO THEY TYPICALLY PURCHASE
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


------QUE7...WHICH SMALL BUSINESS CUSTOMER HAD THE HIGHEST SALES
Select Customer_Name, Sum (Sales) As Totalsales
FROM KMS_Store
Where Customer_Segment = 'Small Business'
Group by Customer_Name
Order by Totalsales Desc


-----Que8...WHICH CORPORATE CUSTOMER PLACED THE MOST NUMBER OF ORDERS IN 2009-2012
Select Customer_Name, Count(Order_ID) AS OrderCount
From KMS_Store
Where Customer_Segment = 'Corporate'
And Order_Date Between '2009-01-01' And '2012-12-31'
Group by Customer_Name
Order by OrderCount Desc

-----QUE9...WHICH CUSTOMER WAS THE MOST PROFITABLE ONE
Select Customer_Name, sum(Profit) AS TotalProfit
From KMS_Store
Where Customer_Segment = 'Consumer'
Group by Customer_Name
Order by TotalProfit Desc


-----QUE10...WHICH CUSTOMER RETURNED ITEMS AND WHAT SEGMENT DO THEY BELONG TO





------QUE11...If the delivery truck is the most economical but the slowest shipping method and 
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

